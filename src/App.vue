<template>
  <v-app id="facebork">
    <v-container fluid>
      <v-row>
        <v-col cols="12">
          <h1 class="display-3">Facebork</h1>
          <h2 class="headline">Doin' 'Em a Big Heckin' Rate</h2>
        </v-col>
      </v-row>
    </v-container>
    <v-content>
      <upload></upload>
      <dog></dog>
    </v-content>
  </v-app>
</template>

<script>
import Upload from './components/Upload.vue';
import Dog from './components/Dog.vue';

export default {
  components: {
    Upload,
    Dog,
  },
};
</script>

<style lang="scss">
  .max-height {
    height: 100%;
  }
</style>
