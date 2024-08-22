<template>
  <v-app>
    <v-main>
      <SixthBarCode/>
    </v-main>
  </v-app>
</template>

<script>
import SixthBarCode from './components/SixthBarCode.vue';


export default {
  name: 'App',

  components: {
    SixthBarCode,
  },

  data: () => ({
    //
  }),
}
</script>
