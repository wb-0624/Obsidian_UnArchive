# 前置基础

在开始之前，先确保有关于html，css，JavaScript的基础。

# Vue实例对象

Vue实例对象是Vue.js中最基本的单元。由`Vue(构造参数)`构造得到。创建得到的实例对象也就是所谓的Vue应用。可以用来注册为组件。

使用`Vue.createApp(构造参数)`来创建一个Vue实例。
```js
const app = Vue.createApp({
  /* 选项 */
})
```

[构造参数有哪些？](Vue文档.md##构造参数)

# Vue组件

![](img/组件的组织.png)

Vue中最重要的概念就是组件。组件之间的层级关系就反映了页面中的嵌套关系。如上图所示。

> 最外层的，最底层的白色页面 -> 最上层的根组件。
> 三个灰色的部分 -> 第二层的三个组件。
> 黑色的部分 -> 对应着最下层的组件。

在实际生产中，主要使用单文件组件。即一个.vue来表示一个组件。

在以上介绍后，还需要了解的东西。


[最上层的组件是什么？](##根组件)
[单文件组件。](##单文件组件)
[如何实现组件的层级关系。](##组件注册)

## 根组件
通常在项目当中，需要通过Vue实例对象来创建一个根组件，其余的组件使用单文件组件的方式创建。即根组件表示整个页面app，其余的页面都只是在app下进行跳转到相应的组件罢了。

## 单文件组件

单文件组件就是以一个.vue文件来作为一个单一组件。内部定义了组件的模板，样式，脚本等。

一个单文件组件内部通常由三部分组成。

-   `<template>` 部分定义了组件的模板。组件的内容。类似于html部分。
-   `<script>` 部分是一个标准的 JavaScript 模块。它应该导出一个 Vue 组件定义作为其默认导出。
-   `<style>` 部分定义了与此组件关联的 CSS。组件的样式。

可以发现一个.vue文件的结构和内容和.html还是非常类似的。

```html
<template>
  <div v-loading="loading"></div>
</template>

<script setup>

import {ref,onMounted } from 'vue';
import {useRouter} from "vue-router";
import {useLoading} from "/lib/service";
  
const router = useRouter()
const loading = ref(false)

onMounted(useLoading(loading, async ()=>{
}))

</script>



<style scope>

.bg-black{
backgroud:black;
}

</style>

```

暂时只注重结构的样子，无需在意内部的内容是什么。

### template

模板很好理解，定义了一个组件的结构，包含什么内容等。类似于html。

### script

通常定义导入和导出。
导入很好理解，就是import，和其他语言的导入基本没有区别。
注意[import和import{}](注意事项及问题.md#import%20和%20import{})的区别。
注意第三个import，from的路径原本应该填单个文件的，但是这里是一个文件夹。
[import一个文件夹的作用是什么？](注意事项及问题#import%20{xxx%20as%20yyy}%20from%20"path"%20文件夹)

注意导出。
[导出和默认导出](注意事项及问题.md#export%20和%20export%20default)

但是注意会发现，上述例子中，好像并没有export的部分，这是因为使用了[setup语法糖](注意事项及问题.md#setup 语法糖)，默认导出了除import的所有部分，使得程序更简洁而已，并没有什么特殊的语法。


现在知道了单文件组件应该是什么样子的，并将其导出，那么如何实现组件之间的层级关系？或者说如何在组件中引入其他组件。

## 组件注册

首先当然是需要在`<script></script>`中，导入需要用到的组件。其次就是在export中进行注册。

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  }
  // ...
}
```

然后就能在改.vue文件的 `<template></template>`中像使用其他组件一样，使用`<ComponetA></ComponentA>`。

但是注意到之前介绍`<script setup>`语法糖的时候，可以省略导出的定义，那么在哪里注册组件呢？

setup 语法糖还包含了自动注册的功能，使用时只需要负责把需要用到的组件导入即可。就可以在模板中使用`<ComponentA></ComponentA>`来直接使用。

## 组件挂载

通常结构下，每个组件都注册到自己的父组件当中，层层往上，最终都在根组件之下。

也就是说只需要让根组件在页面中进行显示，自然而然地所有的组件都会显示在页面当中。

在基础页面框架(index.html)下的DOM引入脚本(main.js)即可。

index.html
```html
<html>
  <head>
      ....
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

main.js
```js
import App from "./App.vue"//导入App.vue并命名App

const app = createApp(App)//使用App的结构，创建一个应用实例，作为根组件app
app.mount("#app")//将app挂载到id="app"的DOM之上
```

虽然`<div></div>`下面没有内容，但是实际在项目启动后，会把app根组件下的内容全都显示。

## 第三方组件

简单来说就是引入第三方库。通过包管理，导入相关依赖包。在需要使用第三方组件库的文件内使用import导入需要的组件，就可以直接使用了。（使用setup语法糖后，不需要自己手动注册，导入后就可以直接使用）

# Vue路由

此部分主要介绍，如何通过路由来实现，对于不同的路径显示不同的页面。

一般情况下，关于路由的配置放在`src/router/index.js`当中。

![](img/页面.png)

如图所示，页面的逻辑应该是随着区域1内不同的选项被点击，区域3应该展示不同的内容。区域2不变化。

所以组件之间的关系为1，2，3是同级的，但是如何实现区域3会根据点击不同的选项从而发生变化呢？

区域3的位置用`<router-view/>`来占位，当访问路径符合路由下的path后，会用相应的组件来代替展示。

```js
{

    path: "/",
    name: RouteName.index,
    component: () => import("../../lib/components-layout/kit-layout-admin.vue"),
    meta: {
      menu: true,
    },
    children: [
      {
        path: '/monitor',
        name: 'web:monitor',
        component: () => import('../views/monitor/monitor.vue'),
        meta: {
          menuTitle: "现场监控",
          menuIcon: "fee",
        },
      },
      {
          //实验室管理
      },
      {
          //物料仓储管理
      }
}
```

也就是说，路由其实就是说明占位需要被什么所代替。

可以看到上面的路由中有一个children，上面还有一个layout的路由，这是什么？

layout代表的是整个页面的结构，如区域123的划分。从命名中不难看出，是关于admin管理员的页面，也就是说在用户以普通身份登录是其实看到的应该是另一个layout页面。

那children也不难理解了，可以想象到整个页面应该是有两层 `<router-view/>`，外面一层由不同的layout来代替，里面一层由不同的模块来代替。那如果同一个模块下，增加其他选项来控制模块内不同的页面呢？

继续嵌套 `<router-view/>` 并在相应模块路由下，继续增加children属性即可。

# 项目结构例子

[Vue项目结构理解](Vue项目理解--以webkit-sample为例)

项目源码在文件中。

