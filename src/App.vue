<template>
  <div id="app" class="main">
    <div class="d-flex container-fluid align-items-center flex-column">
      <h1 class="my-5" style="color:#776791">A Weather App</h1>
      <SearchBar @clicked="onClickChild" />

      <div v-if="temp!=null" class="d-flex justify-content-center row my-5 container-fluid">
      
        <div class="col-sm-12 col-md-3 d-flex align-items-center flex-column">
          <h3 style="color:#776791">{{box[0].cityName}}</h3>
          <TheCircle :weather_descriptions="weather" :temperature="temp" />
        </div>
      
      
        <div class="col-sm-12 col-md-3 d-flex  align-items-center flex-column">
          <h3 class="mx-3" style="color:#776791">Details</h3>
          <Details v-for="(image,index) in img" :key="index" 
        :imgName="require('@/assets/'+image.imgName)" 
        :heading="image.name" 
        :value="image.value"/>
        </div>
      <div class="col-sm-12 col-md-6">
        <div class="container-fluid my-2">
        <h3 style="color:#776791" class="mx-4 text-center" v-if="temp!=null">Cities</h3>
        <div class="row d-flex justify-content-center">
          <div class="col-sm-12 col-md-4" v-for="(boxInfo,index) in box" :key="index">
            <Boxes  
          :weatherImgUrl="boxInfo.weatherImgUrl"
          :cityName="boxInfo.cityName" 
          :temperature="boxInfo.temp"
          :farenheit="boxInfo.farenheit" 
          :windSpeed="boxInfo.windSpeed"
          :index="index" @removed="removed"/>
          </div>
          
      </div>
      </div>
      </div>
    </div>
    
    
    </div>
  </div>
</template>

<script>
import TheCircle from './components/TheCircle'
import Details from './components/Details'
import SearchBar from './components/Search'
import Boxes from './components/Boxes'


export default {
  name: 'App',
  components: {
   TheCircle,
   Details,
   SearchBar,
   Boxes
  },
  data(){
        return{
            img:[
                {
                    name:'N',
                    imgName:'wind-speed.png',
                    value: null
                },
                {
                    name:'Real Feel',
                    imgName:'real-feel.png',
                    value: null
                },
                {
                    name:'Uv Index',
                    imgName:'uv-index.png',
                    value: null
                },
                {
                    name:'Pressure',
                    imgName:'pressure.png',
                    value:null
                }],
                box:[],
                weather:null,
                temp:null,
                offlineImgvalue:[],
                offlineWeather:[],
                offlineTemp:[],
        }
    },
    mounted(){
      if(localStorage.box){
        this.box = JSON.parse(localStorage.box)

        

      
      this.offlineTemp = JSON.parse(localStorage.offlineTemp)
      this.temp = this.offlineTemp[0]
      
      this.offlineWeather = JSON.parse(localStorage.offlineWeather)
      this.weather = this.offlineWeather[0]
        
        }
        this.offlineImgvalue = JSON.parse(localStorage.offlineImgvalue)
      
        if(this.offlineImgvalue.length>=1){
          
          
        let tempVariable = this.offlineImgvalue[0]
        this.img = this.img.map(item => {
          item.value = tempVariable[item.name]
          return item
       })
        }

      
    },
    watch:{
      box(BI){
        localStorage.box = JSON.stringify(BI)
      },
      offlineImgvalue(imgV){
        localStorage.offlineImgvalue = JSON.stringify(imgV)
      },
      offlineTemp(t){
        localStorage.offlineTemp = JSON.stringify(t)
      },
      offlineWeather(w){
        localStorage.offlineWeather = JSON.stringify(w)
      }
    },
    methods:{
     onClickChild (value,nameAndId) {
      const mapp = {
        'N':value.current.wind_speed + ' km/h',
        'Real Feel':value.current.feelslike + ' ℃',
        'Uv Index':value.current.uv_index,
        'Pressure':value.current.pressure  + ' mb'
      }
      this.offlineImgvalue.unshift(mapp)
      this.weather = value.current.weather_descriptions[0]
      this.offlineWeather.unshift(this.weather)
      this.temp = value.current.temperature
      this.offlineTemp.unshift(this.temp)
      
      this.img = this.img.map(item => {
        item.value = mapp[item.name]
        return item
      })
      
      const boxesOb = {
        cityId:nameAndId.context[0].id,
        cityName:nameAndId.text,
        temp:this.temp,
        farenheit:(this.temp * 9/5) +32,
        windSpeed:value.current.wind_speed,
        weatherImgUrl:value.current.weather_icons[0]
      }
      
      
    this.box.unshift(boxesOb)
   
    let jsonObject = this.box.map(JSON.stringify);
    //converting the array of objects in json string
    let uniqueSet = new Set(jsonObject); 
    //creating a set with jsonobject and storing in the uniqueSet
    let uniqueArray = Array.from(uniqueSet).map(JSON.parse);  
    //converting this set to array of objects and storing in the uniqueArray 
    this.box = uniqueArray
    this.name = this.box[0].cityName
        
    },
    removed(boxIndex){

      
      
      if (boxIndex > -1) {
        this.box.splice(boxIndex, 1)
        this.offlineImgvalue.splice(boxIndex, 1)
        this.offlineWeather.splice(boxIndex, 1)
        this.offlineTemp.splice(boxIndex, 1)
      }

      
      
       
      if(this.box.length == 0){
          this.temp = null
      }else{
          this.img = this.img.map(item => {
          item.value = this.offlineImgvalue[0][item.name]
          return item
          })
          this.temp = this.offlineTemp[0]
          this.weather = this.offlineWeather[0]
      }
    }
    },
}
</script>

<style>
p{
  font-size: 25px;
  
}
.main{
  
  /* background-color: #f7fbd8; */
  max-height: 100vh;
  
}
</style>




