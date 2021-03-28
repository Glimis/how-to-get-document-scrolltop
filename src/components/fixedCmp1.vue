<template>
  <div class="fixed">
    <p>
      处理前：当前滚动信息：{{top}} -- 当前进入导航：{{active}} 
    </p>
    <p>
      <span v-for="item,index in arr" :key='index'>
        导航{{index}}位置:{{item.value}} --</span>
    </p>
  </div>
</template>

<script setup>
import { ref,onUnmounted, computed } from 'vue'


const getScrollTop = ()=>{
  // 注意，获取的是html标签的滚动位置
  const scrollDom = document.querySelector('html')
  let rs = ref(0);
  
  // 刷新滚动信息
  const getScroll = ()=> rs.value = scrollDom.scrollTop
  document.addEventListener('scroll',getScroll)
  onUnmounted(()=>document.removeEventListener('scroll', getScroll))
  return rs;
}



/**
 * 获取锚点信息
 */
const arr = 
[
  ref(document.getElementById('h1').offsetTop),
  ref(document.getElementById('h2').offsetTop),
  ref(document.getElementById('h3').offsetTop)
]
const top = getScrollTop();
const active = computed(()=> {
  let nextIndex = arr.findIndex(item=> item.value>top.value) // 当前top的下一级菜单序号
  return (nextIndex + arr.length + 1)%(arr.length+1)
} ) 
</script>

<style scoped>
.fixed{
  position: fixed;
  top:0;
  left:0;
  background: #fff;
}
</style>
