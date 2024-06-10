<script setup>
import Name from '../components/cafeName/Name.vue';
import Video from '../components/video/Video.vue';
import Category from "../components/category/Category.vue";
import Wine from "../components/wine/Wine.vue";
</script>
<template>
  <div class="body">
    <header>
      <div>
        <Name/>
      </div> 
    </header>

    <div>
        <Video/>
    </div>

    <div> 
      <Category/>
    </div>
    <div>
      <Wine/>
      <!-- 팝업 안 데이터 전달은 props -->
    </div>
  </div>
</template>

<style scoped>

.body {
  background-color: #BA9470;
}

</style>