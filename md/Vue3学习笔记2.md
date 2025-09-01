# 条件渲染

条件渲染：根据条件渲染不同的视图

在vue中提供了以下的条件渲染语句，类似`JavaScript`中的条件语句。

- `v-if`

- `v-else`

- `v-else-if`

- `v-show`

## 1. v-if

`v-if`指令用于条件性第渲染一块内容。这块内容只会在指令的表达式返回为真时才会被渲染。

下面的代码中，当`flag`值为`true`时，整个`div`才会被渲染可见。

```html
<template>
  <div v-if="flag">你能看见我么？</div>
</template>

<script>
export default {
  data() {
    return {
      flag:false,
    }
  }
}
</script>
```

## 2. v-else

`v-else`紧跟着上面的`v-if`，形成了`if-else`两个代码块。根据`flag`值的不同，分别各自渲染对应的`div`块。

```html
<template>
  <div v-if="flag">flag值为true</div>
  <div v-else>flag值为false</div>
</template>

<script>
export default {
  data() {
    return {
      flag:true,
    }
  }
}
</script>
```

## 3. v-else-if

`v-else-if`提供相应于`v-if`的`else-if`块，可以接多个`v-else-if`块。

```html
<template>
  <div v-if="type === 'A'">A</div>
  <div v-else-if="type === 'B'">B</div>
  <div v-else-if="type === 'C'">C</div>
  <div v-else>Not A/B/C</div>
</template>

<script>
export default {
  data() {
    return {
      type:'ABC',
    }
  }
}
</script>
```

## 4. v-show

`v-if`后面能接多个判断语句，`v-show`只能判断当前标签自身能否被显示

```html
<template>
  <div v-show="flag">v-show是否被渲染</div>
</template>

<script>
export default {
  data() {
    return {
      flag:true,
    }
  }
}
</script>
```

`v-show`与`v-if`的区别：

- `v-if`是真实的按条件渲染，因为它确保了在切换时，条件区块内的事件监听器和子组件都会被销毁与重建；

- `v-if`也是**惰性**的，如果在初次渲染时条件值为`false`，则不会做任何事情。只有当条件首次变为`true`时才会被渲染；

- `v-show`简单许多，元素无论初始条件如何，始终会被渲染，只有CSS的`display`属性会被切换；

总结：`v-if`有更高的切换开销，而`v-show`有更高的初始渲染开销。因此，**如果需要频繁切换，则使用`v-show`。如果在运行时绑定条件很少改变，则`v-if`更合适**。

# 列表渲染

## 1. `v-for`指令
我们可以使用一个`v-for`指令基于一个数组来渲染一个列表。`v-for`指令的值需要使用`item in items`形式的特殊语法，其中`items`是源数据的数组，而`item`是迭代项的别名。

```html
<template>
  <div>
    <p v-for="item in names" :key="item.name">{{ item }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      names:['Alice', 'Bob', 'Cherry', 'Dick']
    }
  }
}
</script>
```

生成的网页源码为：

```html
<div>
    <p>Alice</p>
    <p>Bob</p>
    <p>Cherry</p>
    <p>Dick</p>
</div>
```

## 2. 复杂数据

```html
<template>
  <div>
    <div v-for="item in result" :key="item.id">
      <p>ID:{{ item.id }}</p>
      <p>Title:{{ item.title }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result:[
        {
          "id":2025090100,
          "title":"xxx",
        },
        {
          "id":2025090101,
          "title":"yyy",
        },
        {
          "id":2025090102,
          "title":"zzz",
        }
      ]
    }
  }
}
</script>
```

浏览器显示：

```
ID:2025090100

Title:xxx

ID:2025090101

Title:yyy

ID:2025090102

Title:zzz
```

注意：`item in items`形式也可以用`item of items`来代替

## 3. `v-for`遍历对象属性

```html
<template>
  <div>
    <p v-for="(value, key, index) in userInfo" :key="key">
      {{ value }} -- {{ key }} -- {{ index }}
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {
        name:"Alice",
        age:20
      }
    }
  }
}
</script>
```

## 4. 通过`key`来管理状态

Vue默认按照就地更新的策略来更新通过`v-for`渲染的元素列表。当数据项的顺序改变时，Vue不会随之移动DOM元素的顺序，而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。

为了给Vue一个提示，以便它可以跟踪每个节点的标识，从而重用和重新排序现有元素，你需要为每个元素对应的块提供一个唯一的`key`属性。

```html
<template>
  <div>
    <p v-for="(name, index) in names" :key="index">{{ name }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      names:["Alice", "Bob", "John"],
    }
  }
}
</script>
```

注意：

- `key`在这里是一个通过`v-bind`绑定的特殊属性；

- 推荐在任何可行的时候为`v-for`提供一个`key`属性；

- `key`绑定的值期望是一个基础类型的值，例如字符串或者number类型；

- `key`的来源：请不要使用`index`作为`key`的值，我们要确保每一条数据的唯一索引不会发生变化；

```html
<template>
  <div>
    <!--使用唯一不变的id作为key-->
    <div v-for="item in result" :key="item.id">
      <p>ID:{{ item.id }}</p>
      <p>Title:{{ item.title }}</p>
    </div>
  </div>
</template>
```

