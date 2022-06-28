Vue本质上就是JS的框架，在实际web中表示出来的还是Js代码，Vue只是帮助我们对某些功能的实现进行了简化和扩展。

![](img/组件的组织.png)

组件类比为HTML中的标签。可以把组件认为是Vue下的标签，支持自定义。

组件是对特点功能代码(html,css,js)的封装, 通过组件的名字可以重复利用该组件中的代码。也可以认为是一种代码式的模板。

Vue内置了几种组件。`<keep-alive>`，`<transition>`，`<transition-group>`，`<teleport>`。

# 内置组件

导入后可直接使用

```js
import { KeepAlive, Teleport, Transition, TransitionGroup } from 'vue'
```

`<component>` ，`<slot>` 并不是真正的组件，并且也无法像组件那样被导入。

## component

+ `Props`: `is` -`String | Component | Vnode`

根据`is`的值来决定哪个组件被渲染，is的值是字符串，可以是HTML标签也可以组件名称。

```HTML
<!--  动态组件由 vm 实例的 `componentId` property 控制 -->
<component :is="componentId"></component>

<!-- 也能够渲染注册过的组件或 prop 传入的组件-->
<component :is="$options.components.child"></component>

<!-- 可以通过字符串引用组件 -->
<component :is="condition ? 'FooComponent' : 'BarComponent'"></component>

<!-- 可以用来渲染原生 HTML 元素 -->
<component :is="href ? 'a' : 'span'"></component>
```

## slot

+ `Props:` `name`-`String`

`<slot>` 元素作为组件模板之中的内容分发插槽。`<slot>` 元素自身将被替换。

## transition

## transition-group

## keep-alive

## teleport

# 自定义组件

通过`Vue.component("name",{config})`进行注册。

`name`: 名称，之后可以通过名称直接使用组件。
`config`:关于组件中需要定义的东西。

## 全局注册

```js
const app = Vue.createApp({})

app.component('component-a', {
  /* ... */
})
app.component('component-b', {
  /* ... */
})
app.component('component-c', {
  /* ... */
})

app.mount('#app')
```

全局注册就是把注册后的应用实例挂载到根组件上。

## 局部注册

全局注册所有的组件意味着即便你已经不再使用其中一个组件了，它仍然会被包含在最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。所以就需要局部注册。

在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：

```js
const ComponentA = {
  /* ... */
}
const ComponentB = {
  /* ... */
}
const ComponentC = {
  /* ... */
}
```


注意**局部注册的组件在其子组件中不可用**。例如，如果你希望 `ComponentA` 在 `ComponentB` 中可用。

```js
const ComponentA = {
  /* ... */
}

const ComponentB = {
  components: {
    'component-a': ComponentA
  }
  // ...
}
```

局部注册的组件只能在注册它的Vue实例中使用

或者如果你通过 Babel 和 webpack 使用 ES2015 模块，那么代码看起来像这样：

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  }
  // ...
}
```

# 组件使用

注册过的组件，只需要像标签一般用尖括号+组件名即可使用。

# 组件选项

## Data

## DOM
## 生命周期钩子

## 选项/资源

## 组合

## 其他

# 实例属性

# 实例方法
