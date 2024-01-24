<template>
  <div class="infinite-scroll">
    <ul>
      <li v-for="item in list" :key="item">{{ item }}</li>
    </ul>
    <div id="loading">正在加载中...</div>
  </div>
</template>


<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
const currentPage = ref(1)
const pageSize = 12
const list = ref([])
const loading = ref(false)


const loadData = async () => {
  loading.value = true // 开始加载数据
  const response = getList()
  list.value = [...list.value, ...response] // 将数据添加到列表中
  currentPage.value++ // 增加页码
  loading.value = false // 加载完成
}
const getList = ()=>{
  let result = [];
  for (let i = 0; i < pageSize; i++) {
    result.push(i)
  }
  return result;
}
const handleScroll = () => {
  // 监听滚动事件
  const container = document.querySelector('.infinite-scroll') // 获取滚动容器
  const { scrollTop, scrollHeight, clientHeight } = container // 获取滚动高度和内容高度
  if (scrollTop + clientHeight >= scrollHeight && !loading.value) {
    loadData() // 滚动到底部，加载更多数据
  }
}
onMounted(() => {
  loadData();
  const container = document.querySelector('.infinite-scroll')
  const ob = new IntersectionObserver(async (entries)=>{
    const entry = entries[0];
    console.log(entry);
    if (!entry.isIntersecting) {
      return;
    } 
    await loadData();
    // console.log('12313');
  }, {
    root: container,
    threshold: 0,
  });
  const loadMode = document.getElementById('loading');
  ob.observe(loadMode);
  // 
  // container.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  const container = document.querySelector('.infinite-scroll')
  container.removeEventListener('scroll', handleScroll)
})
</script>
<style>
.infinite-scroll {
  height: 200px;
  overflow: auto;
}
</style>