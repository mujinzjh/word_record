<template>
  <div class="root-node">
    <section class="card-container">
      <div class="common" v-for="item in list" :key="item">
        {{ item }}
      </div>
    </section>
    <div class="loading-more">
      <button @click="loadMore">加载更多</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, reactive } from "vue";

const list = reactive<number[]>([]);
let pageSize = 12;
const getListData = () => {
  for (let i = 0; i < pageSize; i++) {
    list.push(i);
  }
};
const handleScroll = ()=>{
  const scrollY = window.scrollY;
  const windowHeight = window.innerHeight;
  console.log(scrollY);
  console.log(windowHeight);
}
const loadMore = () => {
  pageSize = pageSize + 12;
  getListData();
};
onMounted(() => {
  getListData();
});
window.addEventListener('scroll',handleScroll,true)

</script>

<style>
.root-node {
  height: 100%;
  width: 100%;
}
.card-container {
  display: flex;
  width: 100%;
  height: 100%;
  flex-wrap: wrap;
}
.common {
  width: 300px;
  height: 100px;
  text-align: center;
  vertical-align: middle;
  box-shadow: inset;
  border-radius: 6px;
  border: 1px solid #333;
  margin-right: 20px;
}
</style>