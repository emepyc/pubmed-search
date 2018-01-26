<template>

  <div> <!-- root -->
    <div class="publications-container row justify-center sm-column">

      <!-- Spinner -->
      <div class="spinner" v-show="abstracts.length === 0 && !noResults">
        <i class="fa fa-spinner fa-2x fa-spin"></i>
      </div>

      <!-- No results message -->
      <div v-show="noResults">
        <h4>
          <span class="lighter-header">No results matching your query</span> {{term}}
        </h4>
        <button class="primary" @click="$router.push('/')">Search again</button>
      </div>

      <!-- Results -->
      <div class="width-2of3" v-show="abstracts.length>0">
        <div class="back"><a href="/">Back</a></div>
        <h2>
          <span class="lighter-header">Results for query</span> {{term}}
        </h2>
        <div class="total-msg">
          Records <b>{{(page * 20) + 1}}</b> to <b>{{((page * 20) + 20) > total ? total : (page * 20) + 20 }}</b> of <b>{{total}}</b>
        </div>
        <div v-for="abstract in abstracts">
          <publication-card :abstract="abstract"></publication-card>
        </div>
        <div>
          <button class="primary" :disabled="page === 0" @click="prevPage">Previous</button>
          <button class="primary" :disabled="page === ~~(total / 20)" @click="nextPage">Next</button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import * as _ from 'lodash';
  import PublicationCard from './PublicationCard';

  const pubmedBaseUrl = '//eutils.ncbi.nlm.nih.gov/entrez/eutils';
  export default {
    components: {
      'publication-card': PublicationCard,
    },
    data() {
      return {
        query: '',
        term: '',
        total: 0,
        page: 0,
        abstracts: [],
        noResults: false,
      };
    },
    methods: {
      nextPage() {
        this.page += 1;
      },
      prevPage() {
        this.page -= 1;
      },
      getRecords() {
        this.abstracts = [];
        this.term = this.$route.query.inputQuery;

        // Build the query with the pubmedBaseUrl + the term to search for
        this.query = `${pubmedBaseUrl}${this.term}`;

        // call pubmed with the term:
        axios.get(`${pubmedBaseUrl}/esearch.fcgi`, {
          params: {
            db: 'pubmed',
            retstart: this.page * 20,
            retmax: 20,
            retmode: 'json',
            term: this.term,
          },
        })
          .then(resp => {
            this.total = resp.data.esearchresult.count;
            return resp.data.esearchresult.idlist.join(',');
          })
          .then(ids => {
            if (ids.length) {
              axios.get(`${pubmedBaseUrl}/esummary.fcgi`, {
                params: {
                  db: 'pubmed',
                  retmode: 'json',
                  id: ids,
                },
              })
                .then(resp => {
                  /* eslint no-param-reassign: 0 */
                  // There is an entry called "uids" in the response
                  // that doesn't correspond to an article.
                  // but just the passed uids. I remove it from the response...
                  delete resp.data.result.uids;
                  this.abstracts = _.values(resp.data.result);
                });
            }
            else {
              this.noResults = 1;
            }
          });
      },
    },
    watch: {
      page() {
        this.getRecords();
      },
    },
    mounted() {
      this.getRecords();
    },
  };
</script>

<style lang="scss">
  .publications-container {
    margin-left: 20px;
    margin-right: 20px;
  }
  .lighter-header {
    color: #777777;
  }
  .total-msg {
    font-size: 0.8em;
    margin-bottom: 20px;
  }

  .back{
    margin-top: 10px;
  }

  .spinner{
    margin-top: 50px;
  }
</style>
