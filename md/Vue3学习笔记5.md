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


