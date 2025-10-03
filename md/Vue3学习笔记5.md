# Class绑定

数据绑定的一个常见需求场景是操纵元素的CSS class列表，因为`class`是属性attribute，我们可以和其他属性一样使用`v-bind`将它们和动态字符串绑定。但是，在处理比较复杂的绑定时，通过拼接生成字符串是麻烦且易出错的。因此，Vue专门为`class`的`v-bind`用法提供了特殊功能增强。除了字符串外，表达式的值也可以是对象或数组。

举例，如果使用传统的`v-bind`，那么只能绑定简单的字符串：

```html
<template>
  <p :class="myClass">Class样式绑定</p>
</template>

<script>
export default {
  data() {
    return {
      myClass: "demo"
    }
  }
}
</script>
```

## 1. 绑定对象

使用class绑定可以绑定复杂对象，比如绑定active和text-danger两个对象：

```html
<template>
  <p :class="{ 'active': isActive, 'text-danger': hasError }">Class样式绑定</p>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: true,
    }
  }
}
</script>

<style>
.active {
  font-size: 30px;
}

.text-danger {
  color: red;
}
</style>
```

## 2. 绑定多个对象

将多个对象包装在一个对象中：

```html
<template>
  <p :class="{ 'active': isActive, 'text-danger': hasError }">Class样式绑定</p>
  <p :class="classObject">Class样式绑定</p>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: true,

      // 多个对象封装在一个对象中
      classObject: {
        'active': true,
        'text-danger': true,
      }
    }
  }
}
</script>

<style>
.active {
  font-size: 30px;
}

.text-danger {
  color: red;
}
</style>
```

## 3. 绑定数组

```html
<template>
  <p :class="[arrActive, arrHasError]">Class样式绑定</p>
</template>

<script>
export default {
  data() {
    return {
      arrActive: "active",
      arrHasError: "text-danger",
    }
  }
}
</script>
```

数组里也可以使用表达式：

```html
<template>
  <p :class="[isActive ? 'active' : '', hasError ? 'text-danger' : '']">Class样式绑定</p>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: true,
    }
  }
}
</script>

<style>
.active {
  font-size: 30px;
}

.text-danger {
  color: red;
}
</style>
```

## 4. 数组嵌套对象

必须是数组嵌套对象，不能反过来对象里嵌套数组！

```html
<template>
  <!--方括号（数组）里嵌套大括号（对象）-->
  <p :class="[{'active':isActive}, hasError]">Class样式绑定</p>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: 'text-danger'
    }
  }
}
</script>

<style>
.active {
  font-size: 30px;
}

.text-danger {
  color: red;
}
</style>
```

# Style绑定

数据绑定的一个常见需求场景是操纵元素的CSS style列表，因为`style`是属性attribute，我们可以和其他属性一样使用`v-bind`将它们和动态字符串绑定。但是，在处理比较复杂的绑定时，通过拼接生成字符串是麻烦且易出错的。因此，Vue专门为`style`的`v-bind`用法提供了特殊功能增强。除了字符串外，表达式的值也可以是对象或数组。

```html
<template>
  <!--直接绑定-->
  <p :style="{color: 'red'}">Style绑定1</p>
  <!--动态绑定-->
  <p :style="{color: activeColor, fontSize:fontSize+'px'}">Style绑定2</p>
  <!--绑定对象-->
  <p :style=styleObject>Style绑定3</p>
  <!--绑定数组-->
  <p :style=[styleObject]>Style绑定4</p>
</template>

<script>
export default {
  data() {
    return {
      activeColor: 'green',
      fontSize: 30,
      styleObject: {
        color: "red",
        fontSize: "30px",
      }
    }
  }
}
</script>
```

# 侦听器

我们可以使用`watch`选项在每次响应式属性发生变化时触发一个函数。

注意：触发函数必须和被监听的对象同名

```html
<template>
  <h3>侦听器</h3>
  <p>{{ message }}</p>
  <button @click="updateHandler">修改数据</button>
</template>

<script>
export default {
  data() {
    return {
      // message为响应式数据，数据改变页面内容会同步发生改变
      // 侦听器监测的就是这类响应式数据
      message: "hello"
    }
  },
  methods: {
    // 点击按钮修改数据的这个变化过程是可以被监听的
    updateHandler() {
      this.message = 'world'
    }
  },
  watch: {
    // 使用watch选项监听message的变化，函数名与要监听的对象名同名
    message(newValue, oldValue) {
      console.log(newValue, oldValue);
    }
  }
}
</script>
```

# 表达输入绑定

在前端处理表单时，我们常常需要将表单输入框的内容同步给JavaScript中相应的变量。手动连接值绑定和更改事件监听器可能会很麻烦，`v-model`指令帮我们简化了这一步骤。

## 1. 绑定输入框
```html
<template>
  <h3>表单输入绑定</h3>
  <form>
    <input type="text" v-model="message">
    <p>{{ message }}</p>
  </form>
</template>

<script>
export default {
  data() {
    return {
      message: ""
    }
  }
}
</script>
```

## 2. 绑定复选框

```html
<template>
  <h3>表单输入绑定</h3>
  <form>
    <input type="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
  </form>
</template>

<script>
export default {
  data() {
    return {
      checked: true
    }
  }
}
</script>
```

## 3. 修饰符

- `.lazy`

默认情况下，`v-model`会在每次`input`事件后更新数据，你可以添加`.lazy`修饰符来改为在每次`change`事件后更新数据。

```html
<template>
  <h3>表单输入绑定</h3>
  <form>
    <input type="text" v-model.lazy="message">
    <p>{{ message }}</p>
  </form>
</template>

<script>
export default {
  data() {
    return {
      message: ""
    }
  }
}
</script>
```

- `.number`：只接收输入的数字


- `.trim`：去掉输入前后的空格

# 模板引用

虽然Vue的声明式渲染模型为你抽象了大部分对DOM的直接操作，但在某些情况下，我们仍然需要直接访问底层DOM元素。要实现这一点，我们可以使用特殊的`ref`属性。

挂载结束后引用都会被暴露在`this.$refs`之上。这里的模板引用指的是在Vue中直接读取DOM元素。

```html
<template>
  <div ref="container" class="container">{{content}}</div>
  <button @click="getElementHandle">获取元素</button>
</template>

<script>
/**
 * 改变DOM的几种情况：
 * （1）内容改变，使用：{{模板语法}}
 * （2）属性改变，使用：v-bind: 指令
 * （3）事件改变，使用：v-on:click 指令
 * （4）直接操作DOM，使用ref属性
 * 
 * 如果没有特殊要求，不要直接操作DOM
 */
export default {
  data() {
    return {
      content: "内容"
    }
  },
  methods: {
    getElementHandle() {
      // 直接读取container这个div
      // innerHTML是原生JS的属性
      console.log(this.$refs.container.innerHTML = "哈哈哈")
    }
  }
}
</script>
```


