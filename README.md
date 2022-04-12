# meta-vue-communication
Component communication in vue

---

### 一、父子组件通信

#### 1.1 父组件向子组件传递数据

子组件中定义props

```vue
// Child
<template>
  <div>{{ msg }}</div>
</template>

<script>
export default {
  name: 'child-component',
  props: {
    msg: String
  },
  setup () {

  }
}
</script>


// Parent
<template>
  <Child :msg="pMsg"></Child>
</template>

<script>
import Child from '@/views/Child'
import { ref } from 'vue'

export default {
  name: 'parent-component',
  components: {
    Child
  },
  setup () {
    const pMsg = ref('传给子组件的数据')

    return {
      pMsg
    }
  }
}
</script>

```

通过在子组件中定义props属性，接受父组件的信息

props中数据是不能被修改的，如果在子组件中要修改props数据，可以定义一个变量用来存放props数据



#### 1.2 子组件向父组件传递数据

子组件向父组件传递数据是通过**自定义事件**

Context.emit绑定一个自定义事件，当语句被执行时，会为父组件绑定监听一个自定义事件

子组件 emit触发事件、父组件v-on监听事件



```vue
<template>
  <div>{{ msg }}</div>
  <button @click="clickBtn">按钮</button>
</template>

<script>
export default {
  name: 'child-component',
  props: {
    msg: {
      type: String,
      default: 'default'
    }
  },
  setup (props, context) {
    console.log(props.msg)
    const clickBtn = () => {
      context.emit('change', '子组件传递给父组件的数据') // 自定义触发事件名为change
    }

    return {
      clickBtn
    }
  }
}
</script>

// Parent
<template>
  <Child :msg="pMsg" @change="emitFn"></Child>
</template>

<script>
import Child from '@/views/Child'
import {reactive, ref } from 'vue'

export default {
  name: 'parent-component',
  components: {
    Child
  },
  setup () {
    const pMsg = ref('传给子组件的数据')
    const emitFn = (v) => {
      console.log(v)
    }
    return {
      pMsg,
      emitFn
    }
  }
}
</script>
```



#### 1.3 深层次父孙组件传递数据

使用provide & inject

```vue
// Parent
<template>
  <Child :msg="pMsg" @change="emitFn"></Child>
</template>

<script>
import Child from '@/views/Child'
import { provide, reactive, ref } from 'vue'

export default {
  name: 'parent-component',
  components: {
    Child
  },
  setup () {
    const pMsg = ref('传给子组件的数据')
    const emitFn = (v) => {
      console.log(v)
    }

    const car = reactive(['Audi', 'Porshe'])
    provide('carList', car)

    return {
      pMsg,
      emitFn
    }
  }
}
</script>

// Child
<template>
  <div>{{ msg }}</div>
  <button @click="clickBtn">按钮</button>
  <GrandSon></GrandSon>
</template>

<script>
import GrandSon from '@/views/GrandSon'
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
      context.emit('change', '子组件传递给父组件的数据') // 自定义触发事件名为on-change
    }

    return {
      clickBtn
    }
  }
}
</script>

// GrandSon
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

```



#### 1.4 ref和refs

ref如果在普通DOM对象上，拿到的值是DOM元素；如果用在子组件上，则引用的是组件实例，可以通过实例直接调用组件的方法或访问数据





### 二、EventBus ｜ mitt（vue3）

中央事件总线，不管是父子组件，兄弟组件，跨层级组件等都可以使用它完成通信操作

主要用来完成兄弟组件的通信

```vue
// BrotherSend
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

// BrotherReceive
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
      emitter.on('brotherChange')
    })

    return {
      info
    }
  }
}
</script>

```

