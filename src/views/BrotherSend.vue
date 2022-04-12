<template>
  <button @click="handleClick">兄弟组件发送方 - 发送数据</button>
</template>

<script>
import emitter from '@/util/mitt'

export default {
  name: 'BrotherSend',
  setup () {
    const handleClick = () => {
      // 发射事件总线
      emitter.emit('brotherChange', '通过mitt事件总线来自发送方发送的数据')
    }
    return {
      handleClick
    }
  }
}
</script>

<style scoped>

</style>
