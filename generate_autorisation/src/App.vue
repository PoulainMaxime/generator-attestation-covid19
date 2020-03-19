<template>
  <v-app id="main">
    <v-container>
      <p class="text-center attestation">ATTESTATION DE DÉPLACEMENT DÉROGATOIRE</p>
      <p class="text-center law">
        En application de l’article 1er du décret du 16 mars 2020 portant
        réglementation des déplacements dans le cadre de la lutte contre la
        propagation du virus Covid-19:
      </p>
      <p class="text-center dear">Je soussigné(e)</p>
    </v-container>
    <v-content>
      <Form />
    </v-content>
    <v-footer dark padless>
      <v-card class="flex" flat tile>
        <v-card-text class="py-2 white--text text-center">
          {{ new Date().getFullYear() }} —
          <strong>© Made by Maxime POULAIN</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>
import Form from "./components/Form";

export default {
  name: "App",
  components: {
    Form
  },
  data: () => ({})
};
</script>
<style lang="scss">
.v-input--selection-controls .v-input__slot > .v-label,
.v-input--selection-controls .v-radio > .v-label {
  color: black !important;
}
#main {
  margin: 0 auto;
  .attestation {
    font-weight: bold;
    font-size: 20px;
  }
  .law {
    font-weight: 900;
  }
  .dear {
    font-weight: 300;
  }
}
</style>
