<template>
  <div> <!-- Root -->

    <!-- Search container -->
    <div class="search-container" @keyup.enter="doSearch">
      <div class="search-bar floating-label">
        <input v-model="inputQuery" required class="full-width">
        <label>Search for a term</label>
      </div>

      <div class="buttons">
        <button class="primary" @click="doSearch">Search</button>
        <button class="primary" @click="discardSearch">Reset</button>
      </div>

    </div>

    <div class="main-container">
    </div>
  </div>
</template>

<script>
  import router from '../router';

  export default {

    name: 'search',
    data() {
      return {
        inputQuery: '',
      };
    },
    methods: {
      doSearch() {
        const currQuery = Object.assign({}, this.$route.query);
        router.push({
          path: 'results',
          query: {
            ...currQuery,
            inputQuery: this.inputQuery,
          },
        });
      },
      discardSearch() {
        // Changing the Vuex store...
        this.inputQuery = '';
        this.removeAllFilters();
      },
    },
  };
</script>

<style lang="scss">
  .buttons {
    margin-top: 20px;
  }

  .search-container {
    margin-top: 20px;
  }
</style>
