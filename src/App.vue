<!-- TODO: diff算法、最长递增子序列  调试源码 https://github.com/vuejs/core/blob/main/packages/runtime-core/src/renderer.ts-->
<!-- ref源码：https://github.com/vuejs/core/blob/main/packages/reactivity/src/ref.ts -->
<template>
  <div>
    {{ Man }}
  </div>
  <hr>
  <div>
    customRef: {{ obj }}
  </div>
  <hr>
  <div ref="dom">我是dom</div>
  <hr>
  <button @click="change">修改</button>
</template>

<script setup lang='ts'>
import { ref, reactive, isRef, shallowRef, triggerRef, customRef } from 'vue'


function MyRef<T>(value: T) {
  let timer: any
  return customRef((track, trigger) => {
    return {
      get() {
        // 收集依赖
        track();
        return value
      },
      set(newVal) {
        clearTimeout(timer)
        timer = setTimeout(() => {
          console.log('自定义customRef里，防抖调用接口')
          value = newVal
          timer = null
        // 触发依赖更新
        trigger()
        }, 500)
      }
    }
  })
}
// 这里命名要和上面元素的ref命名一致
const dom = ref<HTMLDivElement>()
const obj = MyRef<string>('hangu')

// ref  shallowRef
// ref 深层次响应   shallowRef浅层次响应
// ref和shallowRef 是不能一块写的，不然会影响shallowRef 造成视图的更新 
// 因为ref底层会调triggerRef

// type M= { name: string}
const Man = ref({name: 'aaa'})
const change = () => {
  Man.value.name = 'bbb';
  console.log(Man)
  obj.value = 'hangu改'
console.log('dom: ', dom.value?.innerText)
}


</script>


<style lang='scss' scoped>

</style>