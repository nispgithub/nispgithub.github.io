# 基础

## 创建一个 Vue 应用

每个 Vue 应用都是通过 `createApp`函数创建一个新的 **应用实例**；

我们传入 `createApp` 的对象实际上是一个组件，每个应用都需要一个“根组件”，其他组件将作为其子组件。

**挂载应用**：应用实例必须在调用了 `.mount()` 方法后才会渲染出来

```vue
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

const app = createApp(App)

app.use(router)

app.mount('#app')
```

## 模板语法

Vue 使用一种基于 HTML 的模板语法，使我们能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。所有的 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。

### 文本插值

最基本的数据绑定形式是文本插值，它使用的是“Mustache”语法 (即双大括号)：

template

```vue
<span>Message: {{ msg }}</span>
```

### 原始 HTML

双大括号会将数据解释为纯文本，而不是 HTML。若想插入 HTML，你需要使用 **v-html 指令**：

template

```
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

以下是几个常用的 Vue 指令：

| 指令      | 描述                                                         |
| :-------- | :----------------------------------------------------------- |
| `v-bind`  | 用于将 Vue 实例的数据绑定到 HTML 元素的属性上。              |
| `v-if`    | 用于根据表达式的值来条件性地渲染元素或组件。                 |
| `v-show`  | v-show 是 Vue.js 提供的一种指令，用于根据表达式的值来条件性地显示或隐藏元素。 |
| `v-for`   | 用于根据数组或对象的属性值来循环渲染元素或组件。             |
| `v-on`    | 用于在 HTML 元素上绑定事件监听器，使其能够触发 Vue 实例中的方法或函数。 |
| `v-model` | 用于在表单控件和 Vue 实例的数据之间创建双向数据绑定。        |

## 响应式基础

声明响应式状态的两种方式：

### ref()

在组合式 API 中，推荐使用 [`ref()`] 函数来声明响应式状态：

js

```vue
import { ref } from 'vue'

const count = ref(0)
```

在 `setup()` 函数中手动暴露大量的状态和方法非常繁琐。我们可以使用 `<script setup>` 来大幅度地简化代码

`<script setup>`中的顶层的导入、声明的变量和函数可在同一组件的模板中直接使用。你可以理解为模板是在同一作用域内声明的一个 JavaScript 函数——它自然可以访问与它一起声明的所有内容。

**Ref 可以持有任何类型的值，包括深层嵌套的对象、数组或者 JavaScript 内置的数据结构，比如 `Map`。**

非原始值将通过 [`reactive()`] 转换为响应式代理

### reactive()

与将内部值包装在特殊对象中的 ref 不同，`reactive()` 将使对象本身具有响应性：

js

```
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

响应式对象是 [JavaScript 代理](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为就和普通对象一样。不同的是，Vue 能够拦截对响应式对象所有属性的访问和修改，以便进行依赖追踪和触发更新。

值得注意的是，`reactive()` 返回的是一个原始对象的 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，它和原始对象是不相等的：

js

```
const raw = {}
const proxy = reactive(raw)

// 代理对象和原始对象不是全等的
console.log(proxy === raw) // false
```

只有代理对象是响应式的，更改原始对象不会触发更新。因此，使用 Vue 的响应式系统的最佳实践是 **仅使用你声明对象的代理版本**。

为保证访问代理的一致性，对同一个原始对象调用 `reactive()` 会总是返回同样的代理对象，而对一个已存在的代理对象调用 `reactive()` 会返回其本身：

js

```
// 在同一个对象上调用 reactive() 会返回相同的代理
console.log(reactive(raw) === proxy) // true

// 在一个代理上调用 reactive() 会返回它自己
console.log(reactive(proxy) === proxy) // true
```

有一些局限性：

1. **有限的值类型**：它只能用于对象类型 (对象、数组和如 `Map`、`Set` 这样的[集合类型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections))。它不能持有如 `string`、`number` 或 `boolean` 这样的[原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)。

2. **不能替换整个对象**：由于 Vue 的响应式跟踪是通过属性访问实现的，因此我们必须始终保持对响应式对象的相同引用。这意味着我们不能轻易地“替换”响应式对象，因为这样的话与第一个引用的响应性连接将丢失：

   js

   ```
   let state = reactive({ count: 0 })
   
   // 上面的 ({ count: 0 }) 引用将不再被追踪
   // (响应性连接已丢失！)
   state = reactive({ count: 1 })
   ```

3. **对解构操作不友好**：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接：

   js

   ```
   const state = reactive({ count: 0 })
   
   // 当解构时，count 已经与 state.count 断开连接
   let { count } = state
   // 不会影响原始的 state
   count++
   
   // 该函数接收到的是一个普通的数字
   // 并且无法追踪 state.count 的变化
   // 我们必须传入整个对象以保持响应性
   callSomeFunction(state.count)
   ```

由于这些限制，我们建议使用 `ref()` 作为声明响应式状态的主要 API

## 计算属性

### computed方法

`computed()` 方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**。

示例：

vue

```vue
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 一个计算属性 ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>
```

### 缓存

你可能注意到我们在表达式中像这样**调用一个函数**也会获得和计算属性相同的结果：

```vue
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 定义方法
function calculateBooksMessage() {
  return author.books.length > 0 ? 'Yes' : 'No'
}
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage() }}</span>
</template>
```

若我们将同样的函数定义为一个方法而不是计算属性，两种方式在结果上确实是完全相同的，然而，不同之处在于**计算属性值会基于其响应式依赖被缓存**。一个**计算属性**仅会在其响应式依赖**更新时**才重新计算。这意味着只要 `author.books` 不改变，无论多少次访问 `publishedBooksMessage` 都会立即返回先前的计算结果，而不用重复执行 getter 函数。

**数据未更新时：计算属性走缓存，方法调用一次执行一次！！！**

这也解释了为什么下面的计算属性永远不会更新，因为 `Date.now()` 并不是一个响应式依赖：

js

```
const now = computed(() => Date.now())
```

## 指令

示例

```vue
<template>
      <div>
        <!-- 插入语法 -->
        <h1>{{ msg }}</h1>
        <!-- v-text -->
        <p v-text="text"></p>
        <!-- v-html -->
        <p v-html="html"></p>
        <!-- v-bind button禁用-->
        <button v-bind:disabled="isButtonDisabled">Button1</button>
        <button :disabled="isButtonDisabled">Button2</button>
        <!-- v-on -->
        <button v-on:click="addCount">v-on1</button>
        <button @click="addCount">v-on2</button>
        <!-- v-show -->
        <p v-show="show1">v-show1</p>
        <p v-show="show2">v-show2</p>
        <!-- v-if -->
        <div v-if="type==='A'">优秀</div>
        <div v-else-if="type ==='B'">良好</div>
        <div v-else-if="type==='C'">一般</div>
        <div v-else>差</div>
        <!-- v-for -->
        <ul v-for="a in arr" :key="a">
            <li>{{a}}</li>
        </ul>
        <ul v-for="item in list" :key="item.id">
            <li>姓名是：{{item.name}}</li>
        </ul>
        <ul v-for="(item,index) in list" :key="index">
            <li>姓名是：{{item.name}}, 索引是：{{ index }}</li>
        </ul>
        <!-- v-model -->
        <input v-model="message" placeholder="edit me" @input="message=$event.target.value">
        <p>Message is: {{ message }}</p>
        <!-- 在“change”时而非“input”时更新 -->
        <input v-model.lazy="modelmsg">
        <input v-model.number="age" type="number">
        <input v-model.trim="trim">
        <!-- <footers></footers> -->
    </div>
</template>

<script setup>
 import { ref } from 'vue';
    const  msg='插入语法练习',
      text='v-text',
      html='<h2 style="color:red;">v-html</h2>',
      isButtonDisabled= true,
      show1=true,
      show2=false,
      type='B',
      arr=['a','b','c'],
      list=[//列表数据
        { id: 1, name: 'zs' },
        { id: 2, name:'ls' }
        ];
    const  message=ref('v-model'),
      modelmsg='modelmsg',
      age='18',
      trim='   去空格';

      function addCount(e){
            e.target.style.backgroundColor = 'red';
        }
</script>

<style lang="scss" scoped>

</style>
```

## 组件

在实际应用中，组件常常被组织成层层嵌套的树状结构。

### 定义一个组件

当使用构建步骤时，我们一般会将 Vue 组件定义在一个单独的 `.vue` 文件中，这被叫做[单文件组件](https://cn.vuejs.org/guide/scaling-up/sfc.html) (简称 SFC)：

vue

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```

### 使用组件

第一步：导入。

要使用一个子组件，我们需要在父组件中导入它（script setup中import）

第二步：使用组件（template中添加<ButtonCounter />）

```vue
<script setup>
import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```

### 组件注册

一个 Vue 组件在使用前需要先被“注册”，这样 Vue 才能在渲染模板时找到其对应的实现。组件注册有两种方式：全局注册和局部注册。

#### 全局注册

我们可以使用 [Vue 应用实例](https://cn.vuejs.org/guide/essentials/application.html)的 `app.component()` 方法，让组件在当前 Vue 应用中全局可用。

js

```
import { createApp } from 'vue'

const app = createApp({})

app.component(
  // 注册的名字
  'MyComponent',
  // 组件的实现
  {
    /* ... */
  }
)
```

#### 局部注册

导入即可使用（import）

请注意：**局部注册的组件在后代组件中并\*不\*可用**。

### props（页面传参）

Props 是一种特别的 attributes，你可以在组件上声明注册。要传递给博客文章组件一个标题，我们必须在组件的 props 列表上声明它。这里要用到 [`defineProps`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits) 宏

在使用 `<script setup>` 的单文件组件中，props 可以使用 `defineProps()` 宏来声明：

vue

```vue
<script setup>
const props = defineProps(['foo'])

console.log(props.foo)
</script>
```

除了使用字符串数组来声明 prop 外，还可以使用对象的形式：

js

```vue
// 使用 <script setup>
defineProps({
  title: String,
  likes: Number
})
```

## DOM 内模板解析注意事项

如果你想在 DOM 中直接书写 Vue 模板，Vue 则必须从 DOM 中获取模板字符串。由于浏览器的原生 HTML 解析行为限制，有一些需要注意的事项。

### 大小写区分

HTML 标签和属性名称是不分大小写的，所以浏览器会把任何大写的字符解释为小写。这意味着当你使用 DOM 内的模板时，无论是 PascalCase 形式的**组件名称**、camelCase 形式的 **prop 名称**还是 v-on 的**事件名称**，都需要转换为相应等价的 kebab-case (短横线连字符) 形式：

js

```
// JavaScript 中的 camelCase
const BlogPost = {
  props: ['postTitle'],
  emits: ['updatePost'],
  template: `
    <h3>{{ postTitle }}</h3>
  `
}
```

template

```
<!-- HTML 中的 kebab-case -->
<blog-post post-title="hello!" @update-post="onUpdatePost"></blog-post>
```

### 闭合标签

我们在上面的例子中已经使用过了闭合标签 (self-closing tag)：

template

```
<MyComponent />
```

这是因为 Vue 的模板解析器支持任意标签使用 `/>` 作为标签关闭的标志。

然而在 DOM 内模板中，我们必须显式地写出关闭标签：

template

```
<my-component></my-component>
```

这是由于 HTML 只允许[一小部分特殊的元素](https://html.spec.whatwg.org/multipage/syntax.html#void-elements)省略其关闭标签，最常见的就是 `<input>` 和 `<img>`。对于其他的元素来说，如果你省略了关闭标签，原生的 HTML 解析器会认为开启的标签永远没有结束，用下面这个代码片段举例来说：

template

```
<my-component /> <!-- 我们想要在这里关闭标签... -->
<span>hello</span>
```

将被解析为：

template

```
<my-component>
  <span>hello</span>
</my-component> <!-- 但浏览器会在这里关闭标签 -->
```

### 元素位置限制

某些 HTML 元素对于放在其中的元素类型有限制，例如 `<ul>`，`<ol>`，`<table>` 和 `<select>`，相应的，某些元素仅在放置于特定元素中时才会显示，例如 `<li>`，`<tr>` 和 `<option>`。

这将导致在使用带有此类限制元素的组件时出现问题。例如：

template

```
<table>
  <blog-post-row></blog-post-row>
</table>
```

自定义的组件 `<blog-post-row>` 将作为无效的内容被忽略，因而在最终呈现的输出中造成错误。我们可以使用特殊的 [`is` attribute](https://cn.vuejs.org/api/built-in-special-attributes.html#is) 作为一种解决方案：

template

```
<table>
  <tr is="vue:blog-post-row"></tr>
</table>
```