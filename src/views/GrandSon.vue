<template>
  <div>
    {{carList}}
  </div>
</template>

<script>
import { inject } from 'vue'

export default {
  name: 'GrandSon',
  setup () {
    const carList = inject('carList')
    return {
      carList
    }
  }
}
</script>

<style scoped>

</style>
