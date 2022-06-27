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

应用：用过createApp创建的一个Vue实例，应用实例
组件：就是标签。标签是内置的，而组件是可以自定义的。将应用挂载之后就可以变成组件。

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

```js
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Something'
  }
})
</script>
//输入框默认有Something，随着自己改变，message值也会改变，同时反映在上面的<p>{{message}}</p>
```
## v-bind

# 特殊attribute

# 内置组件

# 组件注册





