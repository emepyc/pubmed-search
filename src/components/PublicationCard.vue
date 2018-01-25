<template>
  <div> <!-- root -->
    <div class="card">
      <div class="paper-title section">
        <a target="_blank" :href="pubmedLink" class="epmc_citation_link">{{title}}</a>
      </div>

      <div class="section">
        <span class="paper-author">{{author}}
        </span>
      </div>

      <div class="paper-journal section">
        <span class="paper-journal-abbrev">{{journal}}</span>
        <span class="paper-year">{{year}}</span>
        <span class="paper-volume">{{volume}}</span>
        <span class="paper-issue">{{issue}}</span>
        <span class="paper-pages">{{pages}}</span>
      </div>

      <div class="paper-pmid section">
        PMID: {{pmid}}
      </div>

    </div>
  </div>
</template>

<script>
  import * as _ from 'lodash';

  /* eslint no-underscore-dangle: 0 */
  export default {
    name: 'publication-card',
    props: ['abstract'],
    data() {
      return {
        // Authors
        noMoreAuthors: false,

        // Abstract
        showFull: false,
        loadingAbstract: false,
        abstractDomId: '',
        abstractText: '',
      };
    },
    computed: {
      title() {
        return this.abstract.title;
      },
      pubmedLink() {
        return `//www.ncbi.nlm.nih.gov/pubmed/${this.abstract.uid}`;
      },
      author() {
        if (!this.abstract.authors || !this.abstract.authors.length) {
          return '';
        }
        const authorNames = this.abstract.authors.map(d => d.name);
        if (authorNames.length === 1) {
          return authorNames[0];
        }
        if (authorNames.length === 2) {
          return authorNames.join(' and ');
        }
        return `${authorNames.slice(0, authorNames.length - 1).join(', ')} and ${authorNames[authorNames.length - 1]}`;
      },
      journal() {
        return _.unescape(this.abstract.fulljournalname);
      },
      year() {
        return new Date(this.abstract.pubdate).getFullYear();
      },
      volume() {
        return this.abstract.volume || '';
      },
      issue() {
        return this.abstract.issue ?
          `(${this.abstract.issue})` : '';
      },
      pages() {
        return this.abstract.pages || '';
      },
      pmid() {
        return this.abstract.uid;
      },
    },
  };
</script>

<style lang="scss">
  $lighter-color: #777777;

  .card {
    padding: 10px;

    .section {
      margin-top: 10px;
      margin-bottom: 10px;
    }

    .paper-show-more-or-less {
      color: #2e9dfd;
      font-size: 0.8em;
      cursor: pointer;
      &.inactive {
        color: inherit;
        cursor: text;
      }
    }
    .paper-pmid {
      font-size: 0.8em;
      color: $lighter-color;
      margin-bottom: 1em;
    }
    .paper-authors {
      margin-top: 0.5em;
    }
    .paper-journal {
      .paper-year {
        font-weight: bold;
        margin-left: 5px;
        margin-right: 5px;
      }
      font-size: 0.8em;
      margin-top: 0.2em;
      margin-bottom: 0.2em;
    }
  }
</style>
