# 数组变化侦测

## 1. 变更方法

Vue能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新。这些变更方法包括：

- `push()`

- `pop()`

- `shift()`

- `unshift()`

- `splice()`

- `sort()`

- `reverse()`

## 2. 替换一个数组

变更方法，顾名思义，就是会对调用它们的原数组进行变更。相应地，也有一些不可变方法，例如`filter()`、`concat()`和`slice()`，这些都不会更改原数组，而总是返回一个新数组，当遇到的时非变更方法时，我们需要将旧的数组替换为新的。

```html
<template>
  <h3>数组变化侦听</h3>
  <button @click="addListHandler">添加数据</button>
  <ul>
    <li v-for="(item, index) of names" :key="index">{{ item }}</li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      names: ["Alice", "Bob", "Clark"]
    }
  },
  methods: {
    addListHandler() {
      // push方法可引起UI自动更新
      // this.names.push("Frank")
      
      // concat方法无法引起UI自动更新，因为其是返回一个新数组
      // this.names.concat("Frank")

      // 新数组重新赋值给原数组即可
      this.names = this.names.concat("Frank")
    }
  }
}
</script>
```

# 计算属性

模板中的表达式虽然方便，但也只能用来做简单的操作。如果在模板中写太多逻辑，会让模板变得臃肿，难以维护。因此我们推荐使用计算属性来描述响应式状态的复杂逻辑。

比如：

```html
<template>
  <h3>{{ myDict.name }}</h3>
  <p>{{ myDict.content.length > 0 ? 'Yes' : 'No' }}</p>
</template>

<script>
export default {
  data() {
    return {
      myDict: {
        name: 'Names',
        content: ["Alice", "Bob", "Clark"]
      }
    }
  }
}
</script>
```

推荐使用计算属性来处理上面的逻辑：

```html
<template>
  <h3>{{ myDict.name }}</h3>
  <p>{{ nameContent }}</p>
</template>

<script>
export default {
  data() {
    return {
      myDict: {
        name: 'Names',
        content: ["Alice", "Bob", "Clark"]
      }
    }
  },
  // 计算属性
  computed: {
    nameContent() {
      return this.myDict.content.length > 0 ? "Yes" : "No"
    }
  }
}
</script>
```

## 1. 计算属性缓存 vs 方法

```html
<template>
  <h3>{{ myDict.name }}</h3>
  <p>{{ nameContent }}</p>
  <p>{{ nameContent2() }}</p>
</template>

<script>
export default {
  data() {
    return {
      myDict: {
        name: 'Names',
        content: ["Alice", "Bob", "Clark"]
      }
    }
  },
  // 计算属性
  computed: {
    nameContent() {
      return this.myDict.content.length > 0 ? "Yes" : "No"
    }
  },
  // 函数
  methods: {
    nameContent2() {
      return this.myDict.content.length > 0 ? "Yes" : "No"
    }
  }
}
</script><template>
  <h3>{{ myDict.name }}</h3>
  <p>{{ nameContent }}</p>
  <p>{{ nameContent2() }}</p>
</template>

<script>
export default {
  data() {
    return {
      myDict: {
        name: 'Names',
        content: ["Alice", "Bob", "Clark"]
      }
    }
  },
  // 计算属性
  computed: {
    nameContent() {
      return this.myDict.content.length > 0 ? "Yes" : "No"
    }
  },
  // 函数
  methods: {
    nameContent2() {
      return this.myDict.content.length > 0 ? "Yes" : "No"
    }
  }
}
</script>
```

在表达式中调用一个函数也会获得和计算属性相同的结果，两者之间的区别：

- 计算属性：计算属性的值会基于其响应式依赖被缓存，一个计算属性仅会在其响应式依赖更新时才会重新计算；

- 方法：方法调用总是会在重渲染发生时再次执行函数；

