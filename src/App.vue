<template>
  <v-app>
    <v-content>
      <desafioTero/>
    </v-content>
  </v-app>
</template>

<script>
import desafioTero from './components/desafioTero';


export default {
  name: 'App',
  components: {
    desafioTero,
  },
  data: () => ({
    //
  }),
};
</script>
