<template>
  <div>
    <BaseLoading v-if="isLoading" />
    <template v-if="profileData !== null && !isLoading">
      <MainBlock :profile-data="profileData" />
    </template>
  </div>
</template>

<script>
// Traemos el mixin
import setError from "@/mixins/setError";
import { getApiAccount } from "@/api/search";
import BaseLoading from "@/components/BaseLoading";
import MainBlock from "./MainBlock/Index";

export default {
  name: "ProfileView",
  // Lo damos de alta
  mixins: [setError],
  components: { BaseLoading, MainBlock },
  data() {
    return {
      isLoading: false,
      profileData: null
    };
  },
  created() {
    // llamada a la API
    this.isLoading = true;
    this.fetchData();
  },
  methods: {
    fetchData() {
      const { region, battleTag: account } = this.$route.params;
      getApiAccount({ region, account })
        .then(({ data }) => {
          // Guardamos los datos en una variable local
          this.profileData = data;
        })
        .catch(err => {
          this.profileData = null;
          // Creamos el objeto error
          const errObj = {
            routeParams: this.$route.params,
            message: err.message
          };
          if (err.response) {
            errObj.data = err.response.data;
            errObj.status = err.response.status;
          }
          // Hacemos uso del Mixin
          // Guardamos el objeto en el Store
          this.setApiErr(errObj);
          // Navegamos a la ruta de nombre 'Error'
          this.$router.push({ name: "Error" });
        })
        .finally(() => {
          this.isLoading = false;
        });
    }
  }
};
</script>
