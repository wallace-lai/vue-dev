# Vue简介

Vue是：**渐进式的JavaScript框架**

Vue的设计特点：
- 灵活性；

- 可被逐步集成；

你可以以不同的方式使用Vue：

- 无需构建步骤，渐进式增强静态的HTML；

- 在任何页面中作为Web Components嵌入；

- 单页面应用（SPA）；

- 全栈 / 服务端渲染（SSR）；

- Jamstack / 静态站点生成（SSG）；

- 开发桌面端、移动端、WebGL，甚至时命令行终端中的界面；

# Vue API风格

由于Vue2和Vue3两大版本的存在，造成了两种不同的API风格：

- 选项式API（Vue2）；

使用选项式API，我们可以**用包含多个选项的对象来描述组件的逻辑**。例如`data`、`methods`和`mounted`。选项所定义的属性都会暴露在函数内部的`this`上，它会指向当前的组件实例。


```html
<script>
export default {
    data() {
        return {
            count: 0
        }
    },
    methods: {
        increment() {
            this.count++
        }
    },
    mounted() {
        console.log(`The initial count is ${this.count}.`)
    }
}
</script>

<template>
    <button @click="increment">Count is : {{ count }}</button>
</template>
```

- 组合式API（Vue3）；

通过组合式API，我们可以**使用导入的API函数来描述组件逻辑**。

```html
<script setup>
import { ref, onMounted } from 'vue'

<!-- data -->
const count = ref(0)

<!-- methods -->
function increment() {
    count.value++
}

<!-- mounted -->
onMounted(() => {
    console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
    <button @click="increment">Count is : {{ count }}</button>
</template>
```

# Vue3的安装

（1）先下载Node.js并安装

安装好后，打开CMD查看node和npm的版本号

```
C:\Windows\system32>node -v
v22.19.0

C:\Windows\system32>npm -v
10.9.3

C:\Windows\system32>
```

（2）使用Vue CLI的方式安装Vue 3

- 使用npm安装Vue CLI

```
npm install -g @vue/cli
```

- 创建Vue 3项目

```
vue create my-vue3-app
```

# Vue项目结构

```
node_modules        --- Vue项目运行依赖
public              --- 资源文件
src                 --- 源码
.gitignore
babel.config.js     --- 
jsconfig.json
package-lock.json
package.json        --- 信息描述文件
README.md
vue.config.js
```