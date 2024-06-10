<template>
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
    <div class="cart" v-if="buy.length !== 0">
    <div v-for="wine in totalbuy" :key="wine.id" class="cart-item">
      <img class="cart-image" :src="wine.image" alt="wine image" />
      <button class="remove-btn" @click="hateWine(wine.id)">X</button>
    </div>
    <div>
      <!-- <p>장바구니 : {{ buy }}</p> -->
      <button class="buy" @click="showbuyWines">{{ viewingState ? "돌아가기" : "장바구니" }}</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// State variables
const pageWine = ref([]);
const index = ref(0);
const next = ref(pageWine.length);
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
    updateTotalbuy();
  }
};

// Hate a wine
const hateWine = (id) => {
  buy.value = buy.value.filter(item => item !== id);
  updateTotalbuy();
};

const updateTotalbuy = () => {
  if (allWines) {
    totalbuy.value = allWines.filter(obj => buy.value.includes(obj.id));
  }
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
.selCate {
  background-color: rgb(153, 114, 31);
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
  cursor: pointer;
}
.image {
  width: 50px;
  height: 100px;
}

.cart {
  position: fixed; /* 장바구니를 고정 위치에 놓기 위해 사용합니다. */
  bottom: 0; /* 화면 하단에 위치시킵니다. */
  width: 100%; /* 장바구니가 화면 너비 전체를 차지하도록 설정합니다. */
  background-color: #5E7153;
  border-top: 1px solid black; /* 상단에 경계를 추가합니다. */
  padding: 1%; /* 패딩을 추가하여 내용물에 여유를 줍니다. */
  box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.1); /* 상단 그림자를 추가하여 시각적 깊이를 줍니다. */
  z-index: 10; /* 다른 요소들보다 위에 배치되도록 z-index를 설정합니다. */
  display: flex; /* 플렉스 박스를 사용하여 내부 요소를 정렬합니다. */
  /*justify-content: space-between;  내부 요소를 양 끝에 배치합니다. */
  align-items: center; /* 내부 요소를 세로로 중앙 정렬합니다. */
  
}

.cart-image {
  width: 50px; /* 이미지 너비를 설정합니다. */
  height: 50px; /* 이미지 높이를 설정합니다. */
  /* object-fit: cover; 이미지를 박스에 맞게 잘라냅니다. */
}

.buy{
  height: 85px;
  width: 85px;
  font-size: 200%;
  font-family: 'BMJUA';
  background-color: #FFB834;
  color: #783E19;
  border-radius: 10px;
  cursor: pointer;
  position: absolute; /* 오른쪽에 고정 */
  top: 1px;
  right: 3%;
}

.cart-item {
  position: relative;
  display: inline-block;
  margin-right: 10px;
}

.remove-btn {
  position: absolute;
  top: -10px;
  right: -10px;
  background: #D9D9D9;
  color: black;
  border: none;
  border-radius: 1%;
  width: 20px;
  height: 20px;
  font-size: 12px;
  line-height: 20px;
  cursor: pointer;
}
</style>