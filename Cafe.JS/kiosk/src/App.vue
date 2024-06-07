<script setup>
import Video from "./components/video/video.vue";
import Category from "./components/category/Category.vue";


</script>

<template>  
    <hearder>
      <div>
        <h1 class="custom-font"> Cafe.js </h1>
      </div> 

    </hearder>

    <div>
        <Video />
    </div>

    <div>
      <Category/>
    </div>
    
     
  
</template>

<style scoped>



  @font-face{
    font-family:'DNFBitBitv2';
    font-style: normal; 
    font-weight:400; 
    src:url('//cdn.df.nexon.com/img/common/font/DNFBitBitv2.otf')format('opentype')
  }


  .custom-font{
    font-family: 'DNFBitBitv2', sans-serif;
    color: #FFB834;
    text-shadow: 4px 4px 0 #783E19; /* X, Y, blur-radius, color */   
    text-align: center;
    
  }




  body {
    background-color: #BA9470;
  }

</style>