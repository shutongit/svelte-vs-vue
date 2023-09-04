

# Svelte vs Vue 语法异同

## 简介



### 官网地址

Vue: https://cn.vuejs.org/

Svelte: https://svelte.dev/



### 下载量

从下图可以看到目前vue的下载量存在量级的差别

![image-20230904105900536-3796345](/Users/shutong/Documents/文档/TyporaImage/image-20230904105900536-3796345-3816084.png)

![image-20230904110012023](/Users/shutong/Documents/文档/TyporaImage/image-20230904110012023-3816102.png)





### 是什么

什么是Vue

> Vue (发音为 /vjuː/，类似 **view**) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。无论是简单还是复杂的界面，Vue 都可以胜任。



 什么是 Svelte？

> Svelte 是一个构建 web 应用程序的工具。
>
> Svelte 与诸如 React 和 Vue 等 JavaScript 框架类似，都怀揣着一颗让构建交互式用户界面变得更容易的心。
>
> 但是有一个关键的区别：Svelte 在 *构建/编译阶段* 将你的应用程序转换为理想的 JavaScript 应用，而不是在 *运行阶段* 解释应用程序的代码。这意味着你不需要为框架所消耗的性能付出成本，并且在应用程序首次加载时没有额外损失。
>
> 你可以使用 Svelte 构建整个应用程序，也可以逐步将其融合到现有的代码中。你还可以将组件作为独立的包（package）交付到任何地方，并且不会有传统框架所带来的额外开销。



### <u>网上都说Svelte比Vue快和小</u>

- Vue

![image-20230904133034678](/Users/shutong/Documents/文档/TyporaImage/image-20230904133034678-3816189.png)

![image-20230904133115607-3805476-3805479-3805479](/Users/shutong/Documents/文档/TyporaImage/image-20230904133115607-3805476-3805479-3805479-3816196.png)



- Svelte

![image-20230904133225157](/Users/shutong/Documents/文档/TyporaImage/image-20230904133225157-3816149.png)

![image-20230904140828566](/Users/shutong/Documents/文档/TyporaImage/image-20230904140828566-3816168.png)





> svelte打包还有待研究





------























































## 1. 文件名

- Vue

文件以后缀`.vue`命名

- Svelte

文件以后缀`.svelte`命名



## 2. 语法格式

- Vue	

```vue
<script setup>
操作...
</script>

<template>
视图...
</template>

<style>
全局...
</style>

<style scope>
局部...
</style>
```

- Svelte

> 默认style只影响当前组件；
>
> 需要设置全局css可以用`:global(...)`标记

```svelte
<script>
操作...
</script>

视图...

<style>
局部...
  p {
    color: #1893fe;
  }
  
  /* 全局 */
  div :global(strong) {
		/* 这里表示全局应用于被<div>包裹的<strong>标签 */
		color: goldenrod;
	}
</style>


```



## 3. 组件

- Vue

1. 声明 一个`Component.vue`的文件，在里面处理视图和逻辑
2. 引入

```vue
<script setup>
	import Component from './Component.vue'
</script>

<template>
	<Component></Component>
</template>

```

- Svelte

1. 声明 一个`Component.Svelte`的文件，在里面处理视图和逻辑
2. 引入

```vue
<script>
	import Component from './Component.Svelte'
</script>

<Component></Component>
```



## 4. HTML标记

- Vue
```svelte
<script setup>
	const string = `here's some <strong>HTML!!!</strong>`;
</script>

<!-- 使用v-html标记 -->
<p v-html="string"></p>
```


- Svelte

```svelte
<script>
	let string = `here's some <strong>HTML!!!</strong>`;
</script>

<!-- 使用@html标记 -->
<p>{@html string}</p>
```



## 5. 响应式数据

- Vue

```vue
<script setup>
  import {ref} from 'vue'
	const count = ref(0)
  const handleClick = () => {
    count.value += 1
  }
</script>
<template>

{{ count }}

<button @click="handleClick">
	点我  {{ count }}
  </button>
</template>
```

- Svelte

```svelte
<script>
  let count = 0;

	function handleClick() {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>
```



## 6. 计算属性

- Vue

> 引入` computed` 进行计算

```vue
<script setup>
  import { computed, ref } from 'vue'
  const count = ref(1)
  const doubleCount = computed(() => {
    return count.value * 2
  })
  const handleClick = () => {
    count.value += 1
  }
</script>

<template>
  <button @click="handleClick">点我{{ count }}</button>
  结果：{{ doubleCount }}
</template>

```

- Svelte

> 引入 `$:`进行计算

```svelte
<script>
  let count = 1
  $: doubleCount = count * 2
  function handleClick() {
    count += 1
  }
</script>

<button on:click={handleClick}>点我{count}</button>
count * 2 = {doubleCount}

```



## 7. 动态数据监听

- Vue

> 使用 `{{ }}`标记进行判断；也可以使用`watch`进行监听

```vue
<script setup>
  import { watch, ref } from 'vue'

  let count = ref(0)
  watch(count, () => {
    if (count.value > 9) {
      count.value = 9
      alert('count is dangerously high!')
    }
  })
  function handleCick() {
    count.value += 1
  }
</script>

<template>
  <button @click="handleCick">点我 {{ count }}</button
  >{{ count >= 9 ? 'count is dangerously high!' : count }}
</template>

```

- Svelte

```svelte
<script>
  let count = 0

  $: if (count > 9) {
    count = 9
    alert(`count is dangerously high!`)
  }

  function handleCick() {
    count += 1;
  }
</script>

<button on:click={handleCick}>点我 {count}</button> {count >= 9 ? "count is dangerously high!" : count}

```



## 8. Props 外部传入的值

- Vue

> 1. 使用 `defineProps()`标记外部传入的属性名；
>
> 2. 在组件中使用`:属性名="值"`赋值；
>
> 3. 也可以默认当前属性的值;

```vue
// Compontent 组件声明属性
//  defineProps(['receiveCount'])

  defineProps({
    receiveCount: Number,
    receiveStr: {
      type: String,
      default: 'is a string',
    },
  })

// App组件中引入 Compontent组件
<Compontent :receiveCount="99" ></Compontent>

```

- Svelte

> 1. 使用`export let name`标记外部传入的属性名；
> 2. 在组件中使用`对象名={值}`赋值;
> 3. 可以默认当前属性的值；

```svelte
// Compontent 组件声明属性
  export let receiveCount;
  export let receiveStr = 'is a string'

// App组件中引入 Compontent组件
<Compontent receiveCount={99}></Compontent>
```



## 9. If语句

- Vue

> 在标签中使用 `v-if="条件1"`决定当前内容是否显示，可以直接接着写入`v-else-if=" 条件2"`或者`v-else`来判断显示其他内容

```vue
<div v-if="count < 3">我是count小于3的时候显示的</div>
  <div v-else-if="count >= 3 && count <= 6">
    我是count 大于等于 3 并且 小于等于 6的时候显示的
  </div>
  <div v-else>我是不符合的时候显示的</div>
```

- Svelte

> 1. 首先使用` {#if 条件1}`标记判断开始，使用` {/if }`标记判断结束;在标记中间显示内容
> 2. 可以在`{#if 条件1}{/if}`中间可以使用 `{:else }`或者`{:else if  条件3}`来依次判断内容显示

```svelte
{#if count < 3}
    <p>我是count小于3的时候显示的</p>
{:else if count >= 3 && count <= 6} 
    <p>我是count 大于等于 3 并且 小于等于 6的时候显示的</p>
{:else}
    <p>我是不符合的时候显示的</p>
{/if}
```



## 10 循环



- Vue

> 1. 在被循环的节点上使用 `v-for="元素对象 in 元素数组"` 
> 2. 循环创建的节点是需要添加唯一标志的，这里的唯一标志是通过在节点上添加`:key="唯一标志"`来标记的

```vue
<script setup>
 let cats = [
    { id: 'J---aiyznGQ', name: 'Keyboard Cat' },
    { id: 'z_AbfPXTKms', name: 'Maru' },
    { id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' },
  ]
</script>

<template>

    <ul>
        <li v-for="{id, name} in cats" :key="id">
            {{ name }}的id是：{{ id }}
        </li>
    </ul>
</template>
```

- Svelte

> 1. 在要循环的节点外围使用`{#each 元素数组 as 元素 (唯一标识)} {/each}`标记包含住，然后在标记内写入要循环的节点
> 2. 这里注意Each循环需要添加一个唯一标志，这里的唯一标志是在 `{#each 元素数组 as 元素 (唯一标识)}`标记的开始处使用

```svelte
<script>
  let cats = [
    { id: 'J---aiyznGQ', name: 'Keyboard Cat' },
    { id: 'z_AbfPXTKms', name: 'Maru' },
    { id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' },
  ]
</script>

<ul>
  {#each cats as { id, name } (id)}
    <li>
      {name}的id是：{id}
    </li>
  {/each}
</ul>

```



## 11. #await的不同状态 

- Vue

> ​	vue中可以使用 `async`标记方法，然后使用`await`进行处理异步的方法。
>
> 但是没有一种类似于 `#await`的语法糖

- Svelte

> `#await`可以监听到一个异步的变量状态：pending、fulfilled和rejected；
>
> `#await`有多种使用
>
> ```svelte
> 1. 监听pending、fulfilled、rejected三种状态
> {#await expression}...{:then name}...{:catch name}...{/await}
> 2. 监听pending、fulfilled两种状态
> {#await expression}...{:then name}...{/await}
> 3. 监听fulfilled两种状态
> {#await expression then name}...{/await}
> 4. 监听rejected两种状态
> {#await expression catch name}...{/await}
> ```
>
> 

```svelte
<script>
  let request = loadData()

  /**
   * 开始网络请求
   */
  async function loadData() {
    const res = await sleep()
    return res
  }
  /**
   * 模拟网络请求
   */
  function sleep() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        const count = Math.ceil(Math.random() * 10)
        count % 2 ? resolve(count) : reject('error')
      }, 2e3)
    })
  }

  function handleClick() {
    request = loadData()
  }
</script>

<button on:click={handleClick}>开始网络请求</button>
{#await request}
  <p>loading....</p>
{:then name}
  <p>value的值是：{name}</p>
{:catch}
  <p>哎呀出错了</p>
{/await}

```



## 12. Event

- Vue

> 1. 使用`v-on:事件名`和`@事件名`都可以给标签绑定事件
> 2. 在事件名后添加`.事件限制`,`@click.once`

```vue
<script setup>
  import { reactive } from 'vue'
  const m = reactive({
    x: 0,
    y: 0,
  })
  function handleMove(event) {
    m.x = event.clientX
    m.y = event.clientY
  }
  function handleClick() {
    alert('only can click once!')
  }
</script>

<template>
  <div class="box" v-on:mousemove="handleMove" @click.once="handleClick">
    The mouse position is {{ m.x }} x {{ m.y }}
  </div>
</template>

<style scoped>
  .box {
    width: 100%;
    height: 100px;
    border: 1px solid #ccc;
  }
</style>

```



- Svelte

> 1. 使用`on:事件名`给标签绑定事件
> 2. 在事件名后添加`|事件限制`,`on:click|once`

```svelte
<script>
  let m = {
    x: 0,
    y: 0,
  }
  function handleMove(event) {
    m.x = event.clientX
    m.y = event.clientY
  }
  function handleClick() {
    alert('only can click once!')
  }
</script>

<div class="box" on:mousemove={handleMove} on:click|once={handleClick}>
  The mouse position is {m.x} x {m.y}
</div>

<style>
  .box {
    width: 100%;
    height: 100px;
    border: 1px solid #ccc;
  }
</style>

```



### 12.1 子向父传递数据

> 之前vue中使用`defineProps`，svelte使用`export let 对象`可以实现父标签向子标签传递数据。
>
> 这里我们实现子标签向父标签传递数据

- Vue

1. 子标签，创建和分发事件

> 使用`defineEmits`注册事件，在特定情况下触发事件传递数据

```vue
<script setup>
  const emits = defineEmits(['message'])

  function handleMessageBtn() {

    emits('message', { text: 'hello, i am message' })
  }
</script>
<template>
  <button @click="handleMessageBtn">message btn</button>
</template>

```

2. 父标签接收数据和响应事件

> 在添加子标签的地方使用`@事件名=响应事件`

```vue
<script setup>
  import { reactive, ref } from 'vue'
  import EventChildComponent from './EventChildComponent.vue'
  const message = ref('')
  function handleMessageClick(value) {
    message.value = value?.text
    alert(message.value)
  }
</script>
<template>
  <EventChildComponent @message="handleMessageClick"></EventChildComponent>
</template>
```





- Svelte

1. 子标签

> 使用`createEventDispatcher`创建事件分发器，在特定的情况下触发事件传递数据

```svelte
<script>
  import { createEventDispatcher } from 'svelte'

  // 创建事件分发器
  const dispatch = createEventDispatcher()

  function handleMessageBtn() {
    dispatch('message', {
      text: 'hello, i am message',
    })
  }
</script>

<button on:click={handleMessageBtn}>message btn</button>

```

2. 父标签

> 在添加子标签的地方使用`on:事件名={响应事件}`

```svelte

<script>
  import EventChildComponent from './EventChildComponent.svelte'
  let message = ''
  function handleMessageClick(event) {
    console.log(event)
    // event.detail 表示分发出来的数据
    message = event?.detail?.text
    alert(message)
  }
</script>
<EventChildComponent on:message={handleMessageClick} />

```

### 12.2 <u>孙子组件向上传递数据</u>

- Vue

> 使用`provide`为后代提供一个`key`和值，值可以是变量、常量或回调事件；
>
> 使用`inject`接收祖先组件注入的`key`和值，并返回一个对象

1. 祖先组件

```vue
<script setup>
	 import { provide } from 'vue'
  
    provide('gradesun-message', (val) => {
      alert(val)
    })
 </script>
```

2. 后代组件

```vue
<script setup>
  import { inject } from 'vue'
  const gradesunFunc = inject('gradesun-message')

  function handleClickCC() {
    gradesunFunc('我是孙子组件')
  }
</script>
<template>
  <button @click="handleClickCC">gradesun btn</button>
</template>
```



- Svelte

  [^1]: 目前没有看到跨组件通信的工具；目前想象到的是使用`store`进行数据同步

> 跨组件通信，目前需要一级一级的向上传递；但是在中间组件中不需要进行实现；

1. 孙子组件

```svelte
<script>
  import { createEventDispatcher } from 'svelte'

  // 创建事件分发器
  const dispatch = createEventDispatcher()

  function handleClickCC() {
    dispatch('gradesun-message', '我是孙子组件')
  }
</script>

<button on:click={handleClickCC}>gradesun btn</button>

```

2. 父组件

```svelte
<script>
  import EventChildComponent from './EventChildComponent.svelte'
</script>

<EventChildComponent on:gradesun-message />

```

3. 爷爷组件

```svelte
<script>
  import EventComponent from '../components/EventComponent.svelte'

  function handleClickCC(event) {
    // 点击相应孙子组件
    alert(event?.detail)
  }
</script>

 <EventComponent on:gradesun-message={handleClickCC} />
```

### 12.3 自定义事件,并在外部实现

- Vue

> 使用`defineEmits()`声明事件名，在特定情况下响应

```vue
<script setup>
  // 声明
  const emits = defineEmits(['message'])
  
  function handleClick () {
    // 响应
    emits('message')
  }
</script>

<template>
<button @click="handleClick">
  click
  </button>
</template>
```



- Svelte

> 需要使用`on:事件名`进行标记

1. 子组件

```svelte

<button on:click>click me</button>
```

2. 父组件

```svelte
<script>
    import DomEventForwardingComponent from '../components/DomEventForwardingComponent.svelte'

  
function handleClickDomEvent () {
    console.log(123);
  }
</script>

      <DomEventForwardingComponent on:click={handleClickDomEvent}></DomEventForwardingComponent>

```



## 13. Bind

> 数据双向绑定实现了视图影响数据，数据也影响视图。

- Vue

> 实现双向绑定简单的说就是使用`v-on`监听变化，`v-bind`绑定数据。
>
> 提供了一个语法糖：`v-model`

```vue
<script setup>
import { ref } from 'vue'
const name = ref('')
const scoops = ref(1)
</script>

<template>
  <input v-model="name" type="text" placeholder="请输入内容" />
  <p>Hello {{ name || 'Vue' }}!</p>

  <!-- 多选 -->
  <label>
    <input type="radio" v-model="scoops" :value="1" />
    One scoop
  </label>

  <label>
    <input type="radio" v-model="scoops" :value="2" />
    Two scoops
  </label>
  <p>scoops 的值是： {{ scoops }}</p>
</template>

```



- Svelte

```svelte
<script>
  let name = ''
  let scoops = 1
</script>

<input bind:value={name} type="text" placeholder="请输入内容" />
<p>Hello {name || 'Svelte'}!</p>
<!-- 多选 bind:group -->
<label>
  <input type="radio" bind:group={scoops} value={1} />
  One scoop
</label>

<label>
  <input type="radio" bind:group={scoops} value={2} />
  Two scoops
</label>
<p>scoops 的值是： {scoops}</p>

```



