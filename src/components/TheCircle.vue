<template>
  <div>
    
      <div class="outer-circle d-flex justify-content-center align-items-center">
        <div class="circle d-flex justify-content-center align-items-center flex-column">
          <p>{{ weather_descriptions }}</p>
              
          <div class="temp d-flex justify-content-center align-items-center">
            <h1>{{ temperature }}</h1>
            <p style="color:#776791">&#8451;</p>
          </div>
        </div>
      </div>
    </div>
  
</template>

<script>
export default {
  name: 'TheCircle',
  props:['temperature','weather_descriptions']
}
</script>


<style scoped>
.outer-circle{
  width: 300px;
  height: 300px;
  background-color: rgb(230, 230, 230);
  border-radius:50% ;
  margin: 25px;
}
.circle{
  width: 270px;
  height: 270px;
  border: 2px solid #776791;
  border-radius: 50%;
  background-color: white;
}
h1{
  font-size: 60px;
  font-weight: 200;
  color:#776791
}
p{
  color: rgb(144, 144, 144);
  font-size: 15px;
  letter-spacing: 6px;
  text-align: center;
  
}

</style>
