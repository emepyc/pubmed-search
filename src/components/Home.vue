<template>
  <div><!-- Root -->
    <div class="salute vertical-middle" style="width:500px">

      <div>
        <h3>Pubmed Search</h3>
      </div>
      <div>
        <searchbox></searchbox>
      </div>

      <div class="explain">
        Search for any term in <a href="https://www.ncbi.nlm.nih.gov/pubmed">PubMed</a>.
      </div>
      <!--<div class="explain">-->
        <!--<div>Examples:</div>-->
        <!--<div>-->
          <!--<ul>-->
            <!--<li class="tip">You can search for multiple terms joining them with <span class="keyword">AND</span>, for-->
              <!--example: <span class="keyword">BRAF AND melanoma</span></li>-->
            <!--<li class="tip">If a term has more than one word, use quotes. For example: <span class="keyword">"Alzheimer disease"</span>-->
            <!--</li>-->
            <!--<li class="tip">Use quotes as well if the term has non alpha-numerical values. For example: <span-->
              <!--class="keyword">"K-RAS"</span></li>-->
          <!--</ul>-->
        <!--</div>-->
      <!--</div>-->
    </div>
  </div>
</template>

<script>
  import Search from './Search.vue';

  export default {
    name: 'home',
    components: {
      searchbox: Search,
    },
  };
</script>

<style lang="scss">
  .salute {
    width: 60%;
    margin: auto;
    padding: 15px;
    margin-top: 50px;

    .explain {
      margin-top: 20px;
    }

    .keyword {
      font-size: 0.9em;
      background: #f4f4f4;
      color: steelblue;
      padding: 2px;
    }
    .subheader {
      margin-bottom: 10px;
    }
    .tip {
      margin-bottom: 10px;
    }
  }

</style>
