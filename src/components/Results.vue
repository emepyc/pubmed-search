<template>

  <div> <!-- root -->
    <div class="row justify-center">
      <div class="spinner" v-show="abstracts.length === 0">
        <i class="fa fa-spinner fa-2x fa-spin"></i>
      </div>
      <div class="width-2of3" v-show="abstracts.length>0">
        <div><a href="/">Back</a></div>
        <h2>
          <span class="lighter-header">Results for query</span> {{term}}
        </h2>
        <div class="total-msg">
          We have found <b>{{total}}</b> articles matching your query. Showing the first 20:
        </div>
        <div v-for="abstract in abstracts">
          <publication-card :abstract="abstract"></publication-card>
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
        abstracts: [],
      };
    },
    watch: {
      // '$route.query.q'() {
      //   console.log('route changed!');
      // },
    },
    mounted() {
      this.term = this.$route.query.inputQuery;

      // Build the query with the pubmedBaseUrl + the term to search for
      this.query = `${pubmedBaseUrl}${this.term}`;

      // call pubmed with the term:
      axios.get(`${pubmedBaseUrl}/esearch.fcgi`, {
        params: {
          db: 'pubmed',
          retmax: 20,
          retmode: 'json',
          term: this.term,
        },
      })
        .then(resp => {
          this.total = resp.data.esearchresult.count;
          return axios.get(`${pubmedBaseUrl}/esummary.fcgi`, {
            params: {
              db: 'pubmed',
              retmode: 'json',
              id: resp.data.esearchresult.idlist.join(','),
            },
          });
        })
        .then(resp => {
          /* eslint no-param-reassign: 0 */
          // There is an entry called "uids" in the response that doesn't correspond to an article.
          // but just the passed uids. I remove it from the response...
          delete resp.data.result.uids;
          this.abstracts = _.values(resp.data.result);
        });
    },
  };
</script>

<style lang="scss">
  .lighter-header {
    color: #777777;
  }
  .total-msg {
    font-size: 0.8em;
    margin-bottom: 20px;
  }

  .spinner {
    margin-top:50px;
  }
</style>
