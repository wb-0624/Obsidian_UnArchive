# 应用开始

通过`createApp`方法创建应用实例，并可以通过应用API来进行一些配置。

应用实例通过mount挂在到DOM节点后，返回值就不是应用实例了，而是组件实例。


```js
const app = Vue.createApp({})
app.component('SearchInput', SearchInputComponent)
app.directive('focus', FocusDirective)
app.use(LocalePlugin)
```

另外，由于 `createApp` 方法返回应用实例本身，因此可以在其后链式调用其它方法。允许使用链式的方式来创建。

```js
Vue.createApp({})
  .component('SearchInput', SearchInputComponent)
  .directive('focus', FocusDirective)
  .use(LocalePlugin)
```

## 应用和组件

组件也是应用实例。一般情况下，只有根实例叫做应用实例，其余都算组件。

# 应用配置

# 应用API

作用在应用实例上。

[应用 API | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/api/application-api.html#component)

## component

+ 参数：
    + `{String} name`，可以自动设置为组件的name。
    + `{Function | Object} [definition]`

    > definition，描述组件的内容，包括template，data，computed，methods，props，watch，emits，expose。


+ 返回值：
    + 传入`definition`，返回应用实例。用于注册。
    + 不传入 `difinition` ，返回组件定义。用于搜索。

```js
const app = createApp({})

// 注册一个选项对象
app.component('my-component', {
  /* ... */
})
```



## config
## directive
## mixin
## mount
## provide
## unmount
## use
## version

# 选项

# 组件实例property

# 实例方法

# 指令

作用在html标签内部，更为简便的实现一些功能。

## v-text

## v-slot

## v-pre

## v-cloak

## v-once

## v-memo

## v-is

## v-if
```js
<p v-if="seen">能看见<p>
```

根据seen的真假值来决定是否插入`p`元素。

还有`<v-else>`、`<v-else-if>`，等。

## v-show

```js
<p v-show="seen">能看见<p>
```

根据seen的真假值来决定是否显示`p`元素。

> 同：在展示效果上，二者相同。为真时能看见，假时看不见。
> 异：v-if为假时，元素不会被插入DOM。但是v-show为假时，元素是存在的只是不可见。

> 应用场景：使用`<v-if>`，进行元素的插入和移除时，必须生成元素内部DOM，在某些时候工程量会很大。如果希望内容会被频繁的切换，那么选择`<v-show>`可以减少性能开销。
> 即使是未被展示的，`<v-show>`内部也不能引用不存在的属性，会报错。但是`<v-if>`则可以，因为不会生成元素的内部内容。

## v-html

```vue
<div id="app">
    <div v-html="message"></div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: '<h1>标题1</h1>'
  }
})
</script>

//message中可以放html语句。输出html代码。
```

如果使用{{message}}的格式输出，本会将html代码都以字符串的形式进行展示。而现在则相当于html代码起了作用。

## v-on

缩写为`@`。

用于事件处理，更常用的是点击事件。

```html
<div id="event-with-method">
  <!-- `greet` 是在下面定义的方法名 -->
  <button @click="greet">Greet</button>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      name: 'Vue.js'
    }
  },
  methods: {
    greet(event) {
      // `methods` 内部的 `this` 指向当前活动实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM event
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
}).mount('#event-with-method')
```

多事件处理

按键修饰

## v-for

可以遍历数组或对象，循环输出到页面上。

通过属性方式可以获取具体内容。

```js
<div id="app">
  <ol>
    <li v-for="site in sites"> //循环遍历
      {{ site.name }}//要展示为列表的内容
    </li>
  </ol>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    sites: [
      { name: 'Runoob' },
      { name: 'Google' },
      { name: 'Taobao' }
    ]
  }
})
</script>

```

通过index，key，name。获取键值对。

```vue
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
</head>
<body>
<div id="app">
  <ul>
    <li v-for="(value, key, index) in object">
     {{ index }}. {{ key }} : {{ value }}
    </li>
  </ul>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    object: {
      name: '菜鸟教程',
      url: 'http://www.runoob.com',
      slogan: '学的不仅是技术，更是梦想！'
    }
  }
})
</script>
</body>
</html>


```

显示效果

+   0.   name : 菜鸟教程
+   1.   url : http://www.runoob.com
+   2.   slogan : 学的不仅是技术，更是梦想！


## v-model

数据的双向绑定

### 文本(Text)

```html
<div id="v-model-basic" class="demo">
  <input v-model="message" placeholder="edit me" />
  <p>Message is: {{ message }}</p>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      message: ''
    }
  }
}).mount('#v-model-basic')
```

### 多行文本(Textarea)

```html
<div id="v-model-textarea" class="demo">
  <span>Multiline message is:</span>
  <p style="white-space: pre-line;">{{ message }}</p>
  <br />
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      message: ''
    }
  }
}).mount('#v-model-basic')
```

### 复选框(Checkbox)

```html
<div id="v-model-checkbox" class="demo">
  <input type="checkbox" id="checkbox" v-model="checked" />
  <label for="checkbox">{{ checked }}</label>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      checked: false
    }
  }
}).mount('#v-model-checkbox')
```


### 单选框(Radio)

```html
<div id="v-model-radiobutton">
  <input type="radio" id="one" value="One" v-model="picked" />
  <label for="one">One</label>
  <br />
  <input type="radio" id="two" value="Two" v-model="picked" />
  <label for="two">Two</label>
  <br />
  <span>Picked: {{ picked }}</span>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      picked: ''
    }
  }
}).mount('#v-model-radiobutton')
```

### 选择框(Select)

```html
<div id="v-model-select" class="demo">
  <select v-model="selected">
    <option disabled value="">Please select one</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

如果 `v-model` 表达式的初始值未能匹配任何选项，`<select>` 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 `change` 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。

```js
Vue.createApp({
  data() {
    return {
      selected: ''
    }
  }
}).mount('#v-model-select')
```


## v-bind

简写为`:`

**对象语法**

表示 `active` 这个 class 取决于 data property `isActive` 的truthiness

表示 `text-danger` 这个 class 取决于 data property `hasError` 的truthiness

```html
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```

```js
data() {
  return {
    isActive: true,
    hasError: false
  }
}
```

**数组语法**

```html
<div :class="[activeClass, errorClass]"></div>
```

```js
data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}
```

# 特殊attribute



# 内置组件

# 组件注册





