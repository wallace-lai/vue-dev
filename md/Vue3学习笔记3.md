# 事件处理

在Vue中，我们可以使用`v-on`指令（简写为`@`）来监听DOM事件，并在事件触发时执行对应的JavaScript代码。比如：

```
v-on:click="methodName"
@click="handler"
```

事件处理器可以是：

- 内联事件处理器：事件被触发时执行内联JavaScript语句（与`onclick`类似）；

- 方法事件处理器：一个指向组件上定义的方法的属性名或路径

## 1. 内联事件处理器

```html
<template>
  <button @click="count++">Add 1</button>
  <button v-on:click="count += 2">Add 2</button>
  <p>Count is : {{ count }}</p>
</template>

<script>
export default {
  data() {
    return {
      count:0
    }
  }
}
</script>
```

## 2. 方法事件处理器

```html
<template>
  <button @click="addCount2">Add 2</button>
  <p>Count is : {{ count }}</p>
</template>

<script>
export default {
  data() {
    return {
      count:0
    }
  },
  methods: {
    addCount2() {
      this.count += 2
      console.log("Add 2 done.")
    }
  }
}
</script>
```

# 事件传参

事件参数可以获取`event`对象和通过事件传递数据。

## 1. 获取对象

获取对象并打印对象内容：

```html
<template>
  <button @click="addCount2">Add 2</button>
  <p>Count is : {{ count }}</p>
</template>

<script>
export default {
  data() {
    return {
      count:0
    }
  },
  methods: {
    // Vue中的event对象，就是原生JS的Evet对象
    addCount2(e) {
      // 打印event对象
      console.log(e)
      // 修改event内容
      e.target.innerHTML = "Add " + this.count
      this.count += 2
    }
  }
}
</script>
```

打印event内容如下：
```
{
    "isTrusted": true,
    "_vts": 1756732195978
}
```

## 2. 传递参数

获取当前点击的名字，并向事件处理函数传参：

```html
<template>
  <h3>事件传参</h3>
  <p @click="NameHandler(item)" v-for="(item, idx) in names" :key="idx">
    {{ item }}
  </p>
</template>

<script>
export default {
  data() {
    return {
      names:["iwen", "ime", "frank"]
    }
  },
  methods: {
    NameHandler(name) {
      console.log(name)
    }
  }
}
</script>
```

## 3. 传递参数过程中同时获取`event`

处理函数没有参数时，直接在处理函数里使用`event`即可。否则，需要在传参时给`event`加上`$`符号。

```html
<template>
  <h3>事件传参</h3>
  <p @click="NameHandler(item, $event)" v-for="(item, idx) in names" :key="idx">
    {{ item }}
  </p>
</template>

<script>
export default {
  data() {
    return {
      names:["iwen", "ime", "frank"]
    }
  },
  methods: {
    NameHandler(name, e) {
      console.log(name)
      console.log(e)
    }
  }
}
</script>
```

# 事件修饰符

在处理事件时调用`event.preventDefault()`或者`event.stopPropagation()`是很常见的。尽管我们可以直接在方法内调用，但如果方法能够**更专注于数据逻辑而不用去处理DOM事件的细节**会更好。

为了解决这一问题，Vue为`v-on`提供了事件修饰符，常用有以下几个：

- `.stop`

- `.prevent`

- `.once`

- `.enter`

## 1. 阻止默认事件

一般而言，我们可以在事件调用`event`相关方法阻止默认事件。

```html
<template>
  <h3>事件修饰符</h3>
  <a @click="clickHandler" href="https://cn.bing.com">必应搜索</a>
</template>

<script>
export default {
  data() {
    return {

    }
  },
  methods: {
    clickHandler(e) {
      // 阻止默认事件（跳转新页面）
      e.preventDefault()
      console.log("点击了")
    }
  }
}
</script>
```

使用Vue以后，可以直接用以下的方式达到同样的效果。

```html
<template>
  <h3>事件修饰符</h3>
  <a @click.prevent="clickHandler" href="https://cn.bing.com">必应搜索</a>
</template>
```

## 2. 阻止事件冒泡

正常情况下，下面的代码中如果点击测试冒泡这四个字，控制台会先打印`P`，然后再打印`DIV`。即点击事件会往上冒泡。

```html
<template>
  <h3>阻止事件冒泡</h3>
  <div @click="clickDiv">
    <p @click="clickP">测试冒泡</p>
  </div>
</template>

<script>
export default {
  data() {
    return {

    }
  },
  methods: {
    clickDiv() {
      console.log("DIV")
    },
    clickP() {
      console.log("P")
    }
  }
}
</script>
```

要想阻止事件冒泡，可以使用：

（1）方法一：事件里调用`stopPropagation`

```js
    clickP(e) {
      e.stopPropagation()
      console.log("P")
    }
```

（2）方法二：使用修饰符

```html
<p @click.stop="clickP">测试冒泡</p>
```
