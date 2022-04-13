<template>
  <div>
    mitt 是类似于 Eventbus传递数据
  </div>
  <h2>{{info}}</h2>
</template>

<script>
import emitter from '@/util/mitt'
import { onUnmounted, ref } from 'vue'

export default {
  name: 'BrotherReceive',
  setup () {
    const info = ref('')

    emitter.on('brotherChange', (res) => {
      info.value = res
    })
    onUnmounted(() => {
      emitter.off('brotherChange')
    })

    return {
      info
    }
  }
}
</script>

<style scoped>

</style>
