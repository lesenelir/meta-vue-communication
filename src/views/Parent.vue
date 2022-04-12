<template>
  <!-- @change为监听事件 -->
  <Child :msg="pMsg" @change="getChildInfo" ref="refChild"></Child>
  <button @click="sendValue">获取子组件数据</button>
</template>

<script>
import Child from '@/views/Child'
import { provide, reactive, ref } from 'vue'

export default {
  name: 'parent-component',
  components: {
    Child
  },
  setup (props, context) {
    const pMsg = ref('传给子组件的数据')
    const getChildInfo = (v) => {
      console.log(v)
    }

    const car = reactive(['Audi', 'Porsche'])
    provide('carList', car)

    // ref and refs
    const refChild = ref(null)
    const sendValue = () => {
      console.log(refChild.value.childData)
    }

    return {
      pMsg,
      getChildInfo,
      refChild,
      sendValue
    }
  }
}
</script>

<style scoped>

</style>
