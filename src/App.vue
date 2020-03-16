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
      <upload @runDog="val => runDog()"></upload>
      <dog></dog>
    </v-content>
  </v-app>
</template>

<script>
import Upload from './components/Upload.vue';
import Dog from './components/Dog.vue';

export default {
  data() {
    return {
    }
  },
  components: {
    Upload,
    Dog,
  },
  methods: {
    runDog() {
      let dog = document.getElementById("dog");
      let pos = 0
      let walk = setInterval(() => {
        if (pos === window.innerWidth) {
          pos = -300;
          let returnWalk = setInterval(() => {
            if (pos === 0) {
              clearInterval(returnWalk);
            } else {
              pos++;
              dog.style.left = pos + 'px';
            }
          }, 5);
          clearInterval(walk);
        } else {
          pos++;
          dog.style.left = pos + 'px';
        }
      }, 5);
    }
  }
};
</script>

<style lang="scss">
  .max-height {
    height: 100%;
  }
</style>
