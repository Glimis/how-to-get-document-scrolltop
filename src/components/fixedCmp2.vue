<template>
  <div class="fixed">
    <p>
     处理后：当前滚动信息：{{top}} -- 当前进入导航：{{active}}
    </p>
   <p>
     <span v-for="item,index in arr" :key='index'>
        导航{{index}}位置:{{item.value}} --</span>
    </p>
  </div>
</template>

<script setup>
import { ref,onUnmounted,watch,computed,toRaw, reactive } from 'vue'

// utils 。。。
/**
 * 
 * 实时获取 el 相对于app的 offsetTop
 * 
 * @param el {document}  dom节点
 * @param app {document} 关联容器
 * @return {ref}
 * 
 */
const getOffsetTop = (el,app = document.getElementById('app'))=>{
  // 响应式属性
  const rs = ref(el.offsetTop) 
   // 修正函数
  const fn = ()=>rs.value = el.offsetTop
  // 注入生命周期
  // app.addEventListener('DOMNodeInserted',fn)
  // onUnmounted(()=>app.removeEventListener('DOMNodeInserted',fn))
  
  const observer = new MutationObserver(fn);
  observer.observe(app, { attributes: true, childList: true, subtree: true });

  onUnmounted(()=>observer.disconnect())
  return rs
}

/**
 * 
 * 实时获取 el 相对于app的 scrollTop
 * 
 * @param el {document}  dom节点
 * @param app {document} 关联容器
 * @return {ref}
 * 
 */
const getScrollTop = ( el = document.querySelector('html') ,app = document)=>{
  // 响应式属性
  let rs = ref(el.scrollTop);
  // 刷新滚动信息
  const fn = ()=> rs.value = el.scrollTop
  // 注入生命周期
  app.addEventListener('scroll',fn)
  onUnmounted(()=>app.removeEventListener('scroll', fn))
  return rs;
}

/**
 * 获取value 在 从小到大数组arr 中的插入序号
 * @example
 * getAcsIndex([3,5,6],1) 
 * returns 0
 * @example
 * getAcsIndex([3,5,6],4) 
 * returns 1
 * 
 * @param arr {Array<number>}  顺序数组
 * @param value {number} 对比数字
 * @return {number}
 * 
 */
const getAscIndex = (arr,value)=>{
  let nextIndex = arr.findIndex(item=> item>value) 
  return (nextIndex + arr.length + 1)%(arr.length+1) 
}


/**
 * 通过id，获取监听锚定的集合
 * 
 * @param doms {Array<document>}
 */
const getAscAnchor = (...doms)=>doms.map((dom)=>getOffsetTop(dom))


// 业务。。

// 1. 获取所有锚定集合属性
const arr = getAscAnchor(document.getElementById('h1'),
                        document.getElementById('h2'),
                        document.getElementById('h3'))

// 2. 获取滚动属性
const top = getScrollTop();// 刷新滚动信息


// 3. 获取集合属性
const active = computed(()=> getAscIndex(arr.map(item=>item.value),top.value)) 
</script>

<style scoped>
.fixed{
  position: fixed;
  top:100px;
  left:0;
  background: #fff;
}
</style>
