<template>
  
  <h1 id="h1" @click="go">1. 导航a</h1>
  <div id="h2">
    <h1>2. 导航b - 动态添加 - 鬼知道他会变多少次 </h1>
    <p v-if="show">{{h2}}</p>
  </div>
  <h1 id="h3">3. 导航c</h1>
  <fixedCmp1></fixedCmp1>
  <fixedCmp2></fixedCmp2>
</template>

<script setup>
import fixedCmp1 from './components/fixedCmp1'
import fixedCmp2 from './components/fixedCmp2'
import {ref} from 'vue'
import { setTimeout } from 'timers';


let show = ref(true);
// 模拟异步，如详细介绍，通常为富文本界面
let h2 = ref("")

setInterval(()=>{
  h2.value += ' x' 
})

setTimeout(()=>{show.value = false;},100000)
</script>

<style>
#app{
  text-align:center;
}
#h1,#h3{
  height:500px;
}
</style>
