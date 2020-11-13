<template>
  <div>
      <div class="container">
          <div class="d-flex my-2">
              <img :src="imgName" alt="png">
              <div >
                  <p class="my-1 mx-2 " >{{ heading }}</p>
                  <p class="my-1 mx-2 ">{{ value }}</p>
              </div>
          </div>
      </div>
  </div>
</template>

<script>

export default {
    name:'Details',
    props:['imgName','heading','value']
}
</script>

<style scoped>
    img{
        width: 60px;
    }
    p{
        font-size: 15px;
    }
</style>