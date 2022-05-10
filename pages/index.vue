<template>
  <v-container fluid class="pa-0">
     <v-img src="/logo.jpeg" height="600" class="" gradient="to top right, rgba(100,115,201,.33), rgba(0,0,0,0.9)">
      <v-sheet
        color="transparent"
        height="85vh"
        class="d-flex justify-start align-center pl-16"
      >
        <hero />
      </v-sheet>
    </v-img>
   <div align="center">
     <!-- <hero-about /> -->
    </div>
  <footer-all />
  </v-container>
</template>

<script>
import Hero from '../components/home/Hero.vue'
import HeroAbout from '../components/home/HeroAbout.vue'

export default {
  components: { Hero,  HeroAbout },

}
</script>
<style>
</style>