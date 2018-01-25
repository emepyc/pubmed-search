<template>
  <q-layout>
    <div id="searchbox" class="layout-view bg-light" ref="wrap">
      <router-view></router-view>
    </div>
  </q-layout>
</template>

<script>
  export default {
    name: 'app',
  };
</script>
