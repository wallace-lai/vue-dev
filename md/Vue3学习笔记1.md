总结：

（1）文本的绑定：通过双花括号实现

```html
<p>{{ num + 1 }}</p>

```

（2）标签属性的板顶：通过`v-bind`实现

```html
<div v-bind:id="dynamicId">测试</div>
```


# Vue模板语法

Vue使用一种基于HTML的模板语法，使我们能够声明式地将其组件实例的数据绑定到呈现的DOM上。所有的Vue模板都是语法层面合法的HTML，可以被符合规范的浏览器和HTML解析器解析。

## 1. 文本插值

使用双大括号的形式完成文本插值

```html
<template>
  <p>模板语法</p>
  <p>{{ num + 1 }}</p>
</template>

<script>
export default {
  data() {
    return {
      num:10
    }
  }
}
</script>
```

## 2. 使用JavaScript表达式
每个绑定仅支持**单一表达式**，也就是一段能够被求值的JavaScript代码。比如：

- 表达式；
- 三目运算符；

```html
<template>
  <!-- 表达式 -->
  <p>{{ num + 1 }}</p>
  <!-- 三目运算符 -->
  <p>{{ ok ? "YES" : "NO" }}</p>
  <!-- 表达式 -->
  <p>{{ msg.split(' ').reverse().join(' ') }}</p>
</template>

<script>
export default {
  data() {
    return {
      num:10,
      ok:true,
      msg:"hello world"
    }
  }
}
</script>
```

无效表达式如下：

- 这是语句，而非表达式

```html
{{var a = 1}}
```

- 不支持条件表达式，请使用三元表达式

```html
{{if (ok) {return msg}}}
```

## 3. 插入原始HTML

双大括号将会将数据插值为纯文本，而不是HTML。若想插入HTML，你需要使用`v-html`指令。

```html
<template>
  <p>纯文本：{{ rawHtml }}</p>
  <p>属性：<span v-html="rawHtml"></span></p>
</template>

<script>
export default {
  data() {
    return {
      rawHtml:"<a href='https://cn.bing.com'>必应</a>"
    }
  }
}
</script>
```

显示结果：双花括号里的`rawHtml`会被替换为纯文本

```
纯文本：<a href='https://cn.bing.com'>必应</a>

属性：必应
```

# Vue属性绑定

## 1. 使用Vue可以动态地绑定某个标签的属性。

双大括号不能在HTML attributes中使用。想要响应式地绑定一个attribute，应该使用`v-bind`指令。


假如我们想动态地绑定`div`标签的`class`属性，如下：

```html
<template>
  <div class="{{ attr }}">测试</div>
</template>

<script>
export default {
  data() {
    return {
      attr:"active"
    }
  }
}
</script>
```

从网页源码中可以发现并不能起作用，这个时候如何实现对标签属性的绑定呢？

```html
<div class="{{ attr }}">测试</div>
```

可以使用`v-bind`实现，如下所示：

```html
<template>
  <div v-bind:id="dynamicId" v-bind:class="dynamicClass">测试</div>
</template>

<script>
export default {
  data() {
    return {
      dynamicId:"appid",
      dynamicClass:"appclass"
    }
  }
}
</script>

<style>
.appclass {
  color: red;
  font-size: 50px;
}
</style>
```


从网页中可以看到属性绑定成功：

```html
<div id="appid" class="appclass">测试</div>
```

注意：

- `v-bind`指令指示Vue将元素的`id`属性与`dynamicId`属性保持一致。如果绑定的值是`null`或者`undefined`，那么该属性将会从渲染的元素上移除；

- 因为`v-bind`非常常用，Vue提供了特定的简写语法

```html
  <div :id="dynamicId" :class="dynamicClass">测试</div>
```

## 2. 布尔型属性

布尔型属性依据`true`或`false`值来决定属性是否应该存在于该元素之上，`disabled`就是常见的例子之一。

```html
<template>
  <button :disabled="isButtonDisabled">Button</button>
</template>

<script>
export default {
  data() {
    return {
      isButtonDisabled:true,
    }
  }
}
</script>
```

## 3. 动态绑定多个值

```html
<template>
  <div v-bind="objectofAttrs">测试</div>
</template>

<script>
export default {
  data() {
    return {
      objectofAttrs: {
        id:"appid",
        class:"appclass"
      }
    }
  }
}
</script>
```

网页源码如下：

```html
<div id="appid" class="appclass">测试</div>
```
