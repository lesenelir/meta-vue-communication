<template>
  <div>{{ msg }}</div>
  <button @click="clickBtn">按钮</button>
  <GrandSon></GrandSon>
</template>

<script>
import GrandSon from '@/views/GrandSon'
import { ref } from 'vue'
export default {
  name: 'child-component',
  components: {
    GrandSon
  },
  props: {
    msg: {
      type: String,
      default: 'default'
    }
  },
  setup (props, context) {
    console.log(props.msg)
    const clickBtn = () => {
      context.emit('change', '子组件传递给父组件的数据') // 自定义触发事件名为change - 自定义名称+值
    }

    const childData = ref('ref Data from Child')

    return {
      clickBtn,
      childData
    }
  }
}
</script>

<style scoped>

</style>
