<template>
  <div id="app">
    <LoadLayout v-if="isLoading">
      <BaseLoading />
    </LoadLayout>

    <MainLayout v-else />
  </div>
</template>

<script>
import MainLayout from "@/layouts/MainLayout";
import LoadLayout from "@/layouts/LoadLayout";
import BaseLoading from "@/components/BaseLoading";

export default {
  name: "App",
  components: {
    MainLayout,
    LoadLayout,
    BaseLoading
  },
  computed: {
    isLoading() {
      return this.$store.state.loading.isLoading;
    }
  }
};
</script>

<style lang="stylus">
#app
  padding 60px 0
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #ffffff // Le ponemos un color de letra blanco para que resalte
  background-color #15202b // Le cambiamos el color de fondo por un azul marino oscuro
</style>
