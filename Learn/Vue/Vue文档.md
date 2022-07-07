# 构造参数

使用构造参数，可以来创建Vue实例。

## Data

### data

`data : Object|Function()`

用来存放一个应用实例会用到的数据，变量，常量等。

```js
// 直接创建一个实例
const data = { a: 1 }

// 这个对象将添加到组件实例中
const vm = createApp({
  data() {
    return data
  }
}).mount('#app')

console.log(vm.a) // => 1
```

如果将一个实例理解为一个对象，那么data里的key值就是它的属性。通过`.`的方式可以很便利的进行调用。

### props

自定义的一些值，可以在标签上赋值，在组件内的模板可以进行使用。

### method

定义一些实例可以调用的方法。
```
method:{
 funcA(){
 },
 funcB{
 }
}
```
### computed

也是定义一些函数。

和method的区别在于，只要用来计算的值不变，就只是从缓存中读取计算结果而已。类似于，刷新页面时，只要值不变，就不会重新调用计算，只是把之前的缓存结果重新读取。而method内的函数调用，在刷新后，就会被重新调用执行。

### watch

用于监听数据的变动。

### emits



# 应用API

由实例对象调用的API。

## component

`app.component(name, definition)`
+ 参数：
    + `{String} name`，可以自动设置为组件的name。
    + `{Function | Object} [definition]`

> `definition`，描述组件，包括`template`，`data`，`computed`，`methods`，`props`，`watch`，`emits`，`expose`等。
> `definition`，可以是一些构造参数，也可以是一个[单文件组件](Vue理解.md##单文件组件)。通常使用单文件组件的方式。
> 把`definition`描述的组件注册到父级app实例中，并可以通过`<name>`调用。


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

`app.config()`

+ 参数
    + 应用配置对象，里面定义了关于应用实例的配置。

```js
import { createApp } from 'vue'
const app = createApp({})

app.config = {...}
```

[应用配置有哪些？]



## directive
## mixin
## mount
## provide
## unmount
## use
## version


# 应用配置


# 指令

# 方法


