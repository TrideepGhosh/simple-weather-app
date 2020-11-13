<template>
  <div>
      <div class="text-center wrapper p-3">
          
          <h3 class="my-1">{{ cityName }}</h3>
          <div class="bigInfo d-flex justify-content-around my-3">
              <img :src="weatherImgUrl" alt="weather Image" style="width:80px;height:80px">
              
              <p style="font-size:40px "><span>{{ farenheit }}</span> °F</p>
          </div>
          <hr>
          <div class="d-flex justify-content-center">
              <div class="littleInfo">
                  <img src="@/assets/sun.png" alt="sun" class="smallImage">
                  <p><span>{{ temperature }}</span> ℃</p>
              </div>
              <div class="littleInfo">
                  <img src="@/assets/wind.png" alt="wind" class="smallImage">
                  <p><span>{{ windSpeed }}</span> km</p>
              </div>
          </div>
          <p @click="remove" id="removingBox">Remove</p>
      </div>
  </div>
</template>

<script>

export default {
    name:'Boxes',
    props:['weatherImgUrl','cityName','temperature','farenheit','windSpeed','index'],
    data(){
        return{
            boxIndex:null
        }
    },
    methods:{
        remove(){
            this.boxIndex = this.index
            this.$emit('removed',this.boxIndex)
        }
    }
}
</script>

<style scoped>
.wrapper{
    border: 3.5px solid lightgray;
    border-radius: 30px;
    margin: 5px;
   
}
    p,h3{
        color: #776791;
        font-weight: 300;
    }
    .littleInfo{
        display: flex;
        margin-right:10px;
    }
    img{
        margin-right: 10px;
    }
    .smallImage{
        width: 35px;
        height: 35px;
    }
    #removingBox{
        cursor: pointer;
        font-size: 15px;
    }
    hr{
        width:70%;
        
    }
   
</style>