<template>
  <v-app style="background: white">
    <Toolbar class="front" />
    <v-content>
      <nuxt />
    </v-content>
    <CompanyLogos />
    <Footer />
  </v-app>
</template>

<script>
import Toolbar from '~/components/partials/Menu/Toolbar.vue';
import Footer from '~/components/partials/Footer.vue';
import CompanyLogos from '~/components/partials/CompanyLogos.vue';

export default {
  components: {
    CompanyLogos,
    Footer,
    Toolbar
  }
};
</script>
