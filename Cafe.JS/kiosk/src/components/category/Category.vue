<script setup>
import { ref } from 'vue';

const buttons = ref([
  { label: '추천메뉴!', color: 'yellow' },
  { label: '커피', color: 'brown' },
  { label: '디카페인', color: 'brown' },
  { label: '스무디', color: 'brown' },
  { label: '티&에이드', color: 'brown' },
  { label: '디저트', color: 'brown' }
]);

const changeColor = (index) => {
  // 모든 버튼의 색상을 갈색으로 초기화
  buttons.value.forEach(button => {
    button.color = 'brown';
  });
  // 클릭된 버튼의 색상을 노란색으로 변경
  buttons.value[index].color = 'yellow';
};
</script>

<template>
     <div class="categoryMenu">
    <div v-for="(button, index) in buttons" :key="index" @click="changeColor(index)" :class="['menuBtn', { yellow: button.color === 'yellow' }]">
      <h4>{{ button.label }}</h4>
    </div>
  </div>

</template>


<style scoped>

    @font-face {
    font-family: 'BMJUA';
    src: url('https://fastly.jsdelivr.net/gh/projectnoonnu/noonfonts_one@1.0/BMJUA.woff') format('woff');
    font-weight: 900;
    font-style: normal;
    }

    .categoryMenu{
        display: flex;
        padding-top: 10px;
        justify-content: center; 
    }
    
    .menuBtn{
        cursor: pointer;
        display: flex;
        font-family: 'BMJUA';
        font-size: 1.15em;
        cursor: pointer;
        color: white;
        background-color: #783E19;
        width: 100px;
        height: 50px;
        border-radius: 10px;
        margin: 5px;        
        
        /* justify-content: center; 좌우 가운대 */
        justify-content: center; 
        /* align-items: center; 상하 가운대 */
        align-items: center;
        /* float: left; 왼쪽부터 시작해서 가로로 나열 */
        float: left;            

    }

    .yellow {
        background-color: #FFB834;
        color: #783E19;
    }



</style>