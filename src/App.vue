<template>
  <div id="content">
    <AppHeader />
    <div class="container mt-3 row m-0">
      <router-view />
    </div>
  </div>
</template>

<script>
  import AppHeader from "@/components/AppHeader.vue";
  export default {
    components: {
      AppHeader,
    },
  };
</script>

<style>
</style>
