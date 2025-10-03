# 组件组成

组件最大的优势就是可复用性。当使用构建步骤时，我们一般会将Vue组件定义在一个单独的`.vue`文件中，这被叫做单文件组件SFC。

## 1. 组件的组成结构

组件一般包含三部分基础内容：

- template承载页面标签；

- script承载操作逻辑；

- style承载样式内容；

对于一个组件来说，最少需要含有template部分，逻辑和样式可以缺失。

```html
<template>
  <div class="container">{{ messages }}</div>
</template>

<script>
export default {
  data() {
    return {
      messages: "组件组成基础"
    }
  }
}
</script>

<!--scoped含义：让当前样式只在当前组件中生效-->
<style scoped>

.container {
  font-size: 30px;
  color: red;
}

</style>
```

## 2. 组件的引用

```html
<template>
  <!--第三步：显示组件-->
  <MyComponent/>
</template>

<script>

// 第一步：引入组件
import MyComponent from "./components/MyComponent.vue"

export default {
  // 第二步：注入组件
  components: {
    MyComponent
  }
}
</script>

<style>

</style>
```
