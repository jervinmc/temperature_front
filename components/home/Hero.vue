<template>
  <div>
    <v-img align="start" >
      <v-row>
        <v-col cols="8">
          <div class="text-h2 white--text">
            <b>Temperature Monitioring</b>
          </div>
          <div class="text-h5 white--text ">
            <b>
              Sterling Manors Subdivision<br />

            </b>
          </div>
        </v-col>
        <v-col>
            <!-- <v-img src="/car.jpg">
                
            </v-img> -->
        </v-col>
      </v-row>
      
    </v-img>
  </div>
</template>

<script>
export default {
  created(){
    
  },
  methods:{
    goToMarket(){
      window.location.href='/register'
    },
  }
}
</script>

<style>
</style>