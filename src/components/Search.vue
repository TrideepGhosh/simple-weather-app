<template>
  <div>
      <form class="form-inline">
        <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" v-model = "address">
        <button class="btn btn-outline-primary my-2 my-sm-0" type="submit" @click="geolocation">Search</button>
      </form>
  </div>
</template>



<script>
import axios from 'axios'
export default {
    name:'SearchBar',
    data(){
      return{
        info:null,
        address:null,
        name:null
      }
    },
   
    methods:{
      geolocation(event){
        event.preventDefault();
        axios.get('https://api.mapbox.com/geocoding/v5/mapbox.places/'+ encodeURIComponent(this.address) +'.json?access_token=pk.eyJ1IjoidHJpZGVlcGdob3NoIiwiYSI6ImNrYjZ0MDl2czAxdmcyeGwzZ255cHRldmsifQ.KifyKS7cY30GCq8gTqK9IA&limit=1')
      .then(response => { 
        this.nameAndId = response.data.features[0]
        axios.get('http://api.weatherstack.com/current?access_key=8e60607362643be9d5c4814273cf80f7&query=' + response.data.features[0].center[1] + ',' + response.data.features[0].center[0])
        .then(response => {
        this.info = response.data
        this.$emit('clicked', this.info,this.nameAndId)
      })
      })
      this.address = ''
      }
    },
}
</script>

<style>

</style>