<script>
import Video from "./components/video/video.vue";
import Category from "./components/category/Category.vue";
</script>
<template>
  <div class="body">
    <header>
      <div>
        <h1 class="custom-font"> Cafe.js </h1>
      </div> 

    </header>

    <div>
        <Video/>
    </div>

    <div>
      <Category/>
    </div>

    <div class="appContainer">
      <template v-if="!viewingState">
        <div v-for="wine in pageWine" :key="wine.id" class="wine-card">
          <img class="image" :src="wine.image" alt="wine image" />
          <p>{{ wine.wine }}</p>
          <button v-if="!buy.includes(wine.id)" @click="likeWine(wine.id)">담기</button>
          <button v-if="buy.includes(wine.id)" @click="hateWine(wine.id)">빼기</button>
        </div>
      </template>
      <template v-else>
        <div class="wine-card" v-for="wine in totalbuy" :key="wine.id">
          <p>{{ wine.wine }}</p>
          <img class="image" :src="wine.image" alt="wine image" />
        </div>
      </template>
    </div>
    <div v-if="buy.length !== 0">
      <div>
        <p>장바구니 : {{ buy }}</p>
        <button @click="showbuyWines">{{ viewingState ? "돌아가기" : "장바구니" }}</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// State variables
const pageWine = ref([]);
const index = ref(0);
const next = ref(9);
const loadingState = ref(false);
const buy = ref([]);
const totalbuy = ref([]);
const viewingState = ref(false);
let allWines = null;

// Fetch all wines data
const fetchAllWines = async () => {
  const response = await fetch("https://api.sampleapis.com/wines/reds");
  const data = await response.json();
  allWines = data;
  updatePageWine(index.value, next.value);
};

// Update the pageWine list
const updatePageWine = (index, next) => {
  if (allWines) {
    pageWine.value = allWines.slice(index, next);
  }
};

// Navigate to the previous page
const goBack = () => {
  if (index.value > 0) {
    index.value -= 9;
    next.value -= 9;
    updatePageWine(index.value, next.value);
  }
};

// Navigate to the next page
const nextPage = () => {
  if (allWines && next.value < allWines.length) {
    index.value += 9;
    next.value += 9;
    updatePageWine(index.value, next.value);
  }
};

// Like a wine
const likeWine = (id) => {
  if (!buy.value.includes(id)) {
    buy.value.push(id);
  }
};

// Hate a wine
const hateWine = (id) => {
  buy.value = buy.value.filter(item => item !== id);
};

// Show buy wines
const showbuyWines = () => {
  loadingState.value = true;
  viewingState.value = !viewingState.value;
  if (viewingState.value && allWines) {
    totalbuy.value = allWines.filter(obj => buy.value.includes(obj.id));
  }
  loadingState.value = false;
};

// Fetch wines data when component is mounted
onMounted(fetchAllWines);
</script>

<style scoped>
.header {
  text-align: center;
}
.video {
  height: 150px;
}
.category {
  height: 50px;
  display: flex;
  text-align: center;
  justify-content: space-evenly;
}
.selCate {
  background-color: rgb(153, 114, 31);
}
button {
  background-color: rgb(226, 204, 0);
}
.appContainer {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  border: 1px dotted black;
  height: 100%;
  justify-content: center;
  align-items: center;
}
.wine-card {
  display: flex;
  flex-direction: column;
  width: 25%;
  height: 200px;
  border: 1px solid black;
  justify-content: center;
  align-items: center;
  margin-top: 5px;
  margin-bottom: 5px;
  margin-left: 5px;
  margin-right: 5px;
  background-color: white;
  overflow: hidden;
}
.image {
  width: 50px;
  height: 100px;
}

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