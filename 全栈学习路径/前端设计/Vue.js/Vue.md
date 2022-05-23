

JavaScript 框架，一种架构，定义实现某种效果的规范。更容易。本质上说相当于地第三方库。



$,Vue针对自己内置定义的属性的标志，用于和用户自定义同名情况下进行区分。或者说，就是Vue内置的键值名。



```javascript
var data = { site: "菜鸟教程", url: "www.runoob.com", alexa: 10000}
var vm = new Vue({
	el: '#vue_det',
	data: data
})
```



如：el，data。

自定义有一个data，new Vue中也有一个data。就用$进行区分。





# Vue对象

## 七个属性

| 属性      | 作用                                                         |
| --------- | :----------------------------------------------------------- |
| el:’#app’ | 和id = app的标签关联                                         |
| data      | 所有的变量放在里面                                           |
| template  | 设置模板，替换页面元素                                       |
| methods   | js方法，函数，引用时{{}}，必须有()，若在标签内引用，则不需要() |
| computed  | 根据已存在属性，计算新的属性，{{}}使用方法时不用()           |
| render    | 创建Virtual Dom                                              |
| watch     | 监听数据的变化                                               |



```vue
<div id="app">
    {{msg}}
      <div>这是模板的第一种使用方法1</div>
</div>
 
<template id="bot">这是模板的第三种使用方法，不常用3</template>
<script>
<div id="bot">模板的第四种创建方法4</div>
</script>
<script>
    var vm1 = new Vue({
        data: {
            msg: "data属性"
        },
        methods: {
            Personal:function () {
                console.log("methods方法存放方法")
            }
        },
        template: ' <div>模板的第二种使用方法2</div> '
        //template："#bot",
        render: (createElement) => {
            return createElement("h1",{"class":"类名"，"style":"color: red"},"这一个render属性创建的虚拟dom")
        },
    })
</script>
```



```vue
<div id="app">
  <p>原始字符串: {{ message }}</p>
  <p>计算后反转字符串: {{ reversedMessage }}</p>
</div>

<script>
var vm = new Vue({
  el: '#app',   					//指定该实例用于哪个元素标签，id = app。
  data: {							//
    message: 'Runoob!'
  },
  method:{							//该标签下，需要用到的函数在这里定义和实现。方法属性。
      								//页面重新渲染时，函数就会被重新调用。刷新？
  }
  computed: {						//计算属性。也是定义函数。和method的区别在于： 
    // 计算属性的 getter	默认		 	//只要data.message的值不变，下面的函数就不会被重新执行。
    reversedMessage: function () {  //即使页面刷新，也只是从缓存重新读取之前的运行结果。
      // `this` 指向 vm 实例		  //computed响应更快，但是有缓存。
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```



### el

```vue
el: '#app',该vue实例对象与 id = app的标签绑定。
```



### data

所有的变量都放在data下。用键值对的方式。

### template



### methods



### computed



### render



### watch

```vue
 watch : {
        kilometers:function(val) {
            this.kilometers = val;
            this.meters = this.kilometers * 1000
        },
        meters : function (val) {
            this.kilometers = val/ 1000;
            this.meters = val;
        }
    }
```





## 八个方法

初始化显示

+   beforeCreate()
+   created()
+   beforeMount() 挂载前
+   mounted() 挂载后

更新状态:this.xxx=value

+   beforeUpdate()
+   updated()

销毁 vue 实例

+   :vm.$destory()
+   beforeDestory()
+   destoryed()

## 七个指令



### v- if

`v-if = "seen"` 

是否插入该标签。

包括v-else，v-if-else。



### v-show

`v-show = "ok"` 

根据条件展示元素



### v-on

`v-on: click = "doSomething"`  

添加事件监听器，

缩写为`@click = "doSomething"`



### v-html

```vue
<div id="app">
    <div v-html="message"></div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: '<h1>菜鸟教程</h1>'
  }
})
</script>

//message中可以放html语句。输出html代码。
```



### v-model

```vue
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





### v-for

```Vue
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



### v-bind

```vue
//根据isActive 和 hasError的布尔值确定使用哪种css样式。
//使用文件内样式
<div class="static"
     v-bind:class="{ 'active' : isActive, 'text-danger' : hasError }">
</div>

//内联样式
<div id="app">
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">菜鸟教程</div>
</div>

v-bind:简写为:
```



# Vue组件



## 全局组件

```vue
<div id="app">
    <runoob></runoob>
</div>
 
<script>
// 注册
Vue.component('runoob', {
  template: '<h1>自定义组件!</h1>'
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```



## 局部组件

```vue
<div id="app">
    <runoob></runoob>
</div>
 
<script>
var Child = {
  template: '<h1>自定义组件!</h1>'
}
 
// 创建根实例
new Vue({
  el: '#app',
  components: {
    // <runoob> 将只在父模板可用
    'runoob': Child
  }
})
</script>
```



## prop

prop 是子组件用来接受父组件传递过来的数据的一个自定义属性。

父组件的数据需要通过 props 把数据传给子组件，子组件需要显式地用 props 选项声明 "prop"：

```vue
<div id="app">
    <child message="hello!"></child>
</div>
 
<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```

>   props，相当于自己声明的自定义组件下可以使用的属性。



## 动态prop

类似于用 v-bind 绑定 HTML 特性到一个表达式，也可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件：

```vue
<div id="app">
    <div>
      <input v-model="parentMsg">
      <br>
      <child v-bind:message="parentMsg"></child>
    </div>
</div>
 
<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app',
  data: {
    parentMsg: '父组件内容'
  }
})
</script>
```

>   前者静态：直接给属性赋值。
>
>   后者动态：由父组件的值来赋值。

注意: prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。



## 自定义事件

prop：父组件向子组件传递数据。
自定义事件：子组件将数据传回父组件。



## 自定义指令



## Vue 路由



## 过渡/动画



## 混入



## 接口
