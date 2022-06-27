Vue本质上就是JS的框架，在实际web中表示出来的还是Js代码，Vue只是帮助我们对某些功能的实现进行了简化和扩展。

# 组件

![](img/组件的组织.png)

组件类比为HTML中的标签。可以把组件认为是Vue下的标签，支持自定义。

组件是对特点功能代码(html,css,js)的封装, 通过组件的名字可以重复利用该组件中的代码。也可以认为是一种代码式的模板。

Vue内置了几种组件。`<keep-alive>`，`<transition>`，`<transition-group>`，`<teleport>`。

## 内置组件

导入后可直接使用

```js
import { KeepAlive, Teleport, Transition, TransitionGroup } from 'vue'
```



## 自定义组件

通过`Vue.component("name",{config})`进行注册。

`name`: 名称，之后可以通过名称直接使用组件。
`config`:关于组件中需要定义的东西。

### 全局组件

### 局部组件






