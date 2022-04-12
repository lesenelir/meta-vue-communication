<template>
  <Parent></Parent>
  <hr>
  <BrotherSend></BrotherSend>
  <hr>
  <BrotherReceive></BrotherReceive>
</template>

<script>
import Parent from '@/views/Parent'
import BrotherSend from '@/views/BrotherSend'
import BrotherReceive from '@/views/BrotherReceive'

export default {
  name: 'APP',
  components: {
    Parent,
    BrotherSend,
    BrotherReceive
  }
}
</script>

<style scoped>

</style>
