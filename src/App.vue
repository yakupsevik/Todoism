<template>
  <Loading v-if="isLoading" />
  <router-view v-else />
</template>

<script>
import Loading from "./components/Loading.vue";

export default {
  name: "App",

  components: {
    Loading,
  },

  data() {
    return {
      isLoading: true,
    };
  },
  created() {
    setTimeout(() => (this.isLoading = false), 1500);
  },
};
</script>

<style>
#app {
  background-image: linear-gradient(rgb(255, 0, 106), rgb(255, 0, 128));
  background-repeat: no-repeat;
}
</style>
