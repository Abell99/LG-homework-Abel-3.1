> 1~Basic review of VueJs阶段的随堂笔记

# Basic review of Vue.js

> Vue.js的基础回顾

- 课程介绍
  - 快速回顾Vue.js基础语法
  - Vue Router原理分析与实现
  - 虚拟DOM库Snabbdom源码解析
  - 响应式原理分析与实现
  - Vue.js源码分析


# Ⅰ ~ Vue.js 框架基础回顾

> Vue.js 的基础结构

## Ⅰ ·  Ⅰ ~ Vue基础结构

## Ⅰ · Ⅱ ~ Vue的生命周期

## Ⅰ · Ⅲ ~ Vue语法和概念

- 插值表达式
- 指令
- 计算属性和侦听器
- Class和Style绑定
- 条件渲染/列表渲染
- 表单输入绑定
- 组件
- 插槽
- 插件
- 混入mixin
- 深入响应式原理
- 不同构件版本的Vue

# Ⅱ ~ Vue-Router原理实现

> - Vue Router基础回顾
> - Hash模式和History模式
> - 模拟实现自己的Vue Router

## Ⅱ · Ⅰ ~ Vue Router基础回顾

> 回顾vue-router的基础功能

- 创建一个含有vue-router功能的vue项目
  - `vue create 项目名称`
  - 勾选配置选项
    - Router

- router/index.js的编写
  - 注册路由插件

    - ```js
      import VueRouter from 'vue-router'
      // Vue.use是用来注册插件，他会调用传入对象的install方法
      Vue.use(VueRouter)
      ```

  - 编写路由规则

    > 路由的运行逻辑

  - 创建router对象并导出

    - ```js
      const router = new VueRouter({
          routes
      })
      export default router
      ```

- main.js的编写

  - 导入router

    - ```js
      import router from './router'
      ```
      
      

  - 注册router对象

    > 挂载到Vue下两个属性
    >
    > - $route：路由规则
    > - $router：vue-router的实例

    - ```js
      new Vue({
          router,
          render: h => h(app)
      }).$mount('#app')
      ```
      
      
    
  - ##### 路由加载规则

    - 动态路由
    - 嵌套路由
    - 编程式导航
    - Hash模式和History模式

- App.vue的编写

  - 创建路由组件的占位

    - ```html
      <router-view></router-view>
      ```
    
      

  - 创建路由链接
  
    - ```html
      <router-link to="/"></router-link>
      <router-link to="/blog"></router-link>
      <router-link to="/photo"></router-link>
      ```

## Ⅱ · Ⅱ ~ 模拟实现VueRouter

- 分析VueRouter插件
- VueRouter类图
  
- ![image-20200719001652849](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200719001652849.png)
  
- 实现过程

  1. 判断当前插件是否已经被安装

     > _install(Vue):void

     1. 把Vue构造函数记录到全局变量

     2. 把创建Vue实例时候传入的router对象注入到Vue实例上

        > **混入**

  2. 构造函数的实现

     >+Constructor(Options):VueRouter

     1. 定义属性

        - options

        - routeMap

        - data

     2. 定义方法

        - 遍历路由规则并存储

          > +createRouteMap():void

        - 渲染router-link中的实际内容,处理视图跳转

          > +initComponents():void

        - 记录历史

          > +initEvent():void

        - 包装初始化方法

          > +init():void

# Ⅲ ~ 模拟Vue.js响应式原理

> 课程目标
>
> - 模拟一个最小版本的vue
> - 响应式原理在面试中的常问问题
> - 学习别人的优秀的经验，转换成自己的经验
> - 实际项目中除问题的原理层面的解决
>   - 给Vue实例新增一个成员是否是响应式的
>   - 给属性重新赋值成对象，是否是响应式的
> - 为学习vue源码做铺垫

- 准备工作

  - 数据驱动

  - 响应式的核心原理

    > vue.js 2.x 基于defineProperty
    >
    > vue.js 3 基于Proxy(es6新增对象)

  - 发布订阅模式和观察者模式

- 数据驱动

  - 数据响应式、双向绑定、数据驱动
  - 数据响应式
    - 数据模型仅仅是普通的JavaScript对象，而当我们修改数据时，视图会进行更新，避免了繁琐的DOM操作，提高开发效率
  - 双向绑定
    - 数据改变，视图改变；视图改变，数据也随之改变
    - 我们可以使用v-model在表单元素上创建双向数据绑定

  - 数据驱动时Vue最独特的特性之一
    - 开发过程中仅需要关注数据本身，不需要关心数据时如何渲染到视图

## Ⅲ · Ⅰ ~ 数据响应式的核心原理

> 响应式原理是Vue最独特的特性之一，使其非侵入式的响应式系统。数据模型仅仅是普通的JavaScript对象。而当你修改它们的时候，视图会进行更新。这使得状态管理非常简单直接，

### ㈠ Vue 2.x

> Object.defineProperty

- **基于Object.defineProperty来实现的**

  > - Object.defineProperty的作用
  >
  >   > 当我们把一个普通的JavaScript对象传入Vue实例作为data选项，Vue将**遍历此对象所有的属性**，并使用**Object.defineProperty把这些属性全部转为getter/setter**。
  >
  > - P.S  关于浏览器兼容问题
  >
  >   > **Object.defineProperty是ES5中一个无法shim(降级处理)的特性**，这也就是Vue不支持IE8以及更低版本浏览器的原因。

### ㈡Vue 3.0

> Proxy

- **基于Proxy实现**
  - 直接监听对象，而非属性
  - ES 6 中新增，IE不支持，性能由浏览器优化

## Ⅲ · Ⅱ ~ 数据响应式的事件模型 

> 关于数据响应的思想构建

### ㈠ 发布/订阅模式

> 我们假定，存在一个“信号中心”，某个任务执行完成，就会向信号中心“发布”一个信号，其他任务向信号中心“订阅”这个信号，从而知道什么时候自己可以开始执行。这就叫做“发布/订阅模式”

- 订阅者
- 发布者
- 信号中心

### ㈡ 观察者模式

> 观察数据变化从而做出反应

- 观察者(订阅者)--Watcher
  - update(): 当事件发生时，具体要做的事情
- 目标(发布者)--Dep
  - subs数组：存储所有的观察者
  - addSub()：添加观察者
  - notify()：当事件发生，调用所有观察者的update()方法
- 没有事件中心

### ㈢ 比较

- 观察者模式是由具体目标调度，比如当事件触发，Dep就会去调用观察者的方法，所有观察者模式的订阅者与发布者之间是存在依赖的
- 发布/订阅模式由统一调度中心调用，因此发布者和订阅者不需要知道对方的存在1

![image-20200723032125059](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723032125059.png)

## Ⅲ · Ⅲ ~ Vue响应式原理模拟

> 模拟一个最低限度的Vue响应式

- 整体分析

  - Vue基本解构

  - 打印Vue实例观察

  - 整体解构

    ![image-20200723032708945](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723032708945.png)

### ㈠ Vue类的实现

> 把data中的成员注入到Vue实例，并且把data中的成员转成getter/setter

- 功能需求

  - 负责接收初始化的参数(选项)
  - 负责把data中的属性注入到Vue实例，转换成getter/setter
  - 负责调用observer监听data中所有属性的变化
  - 负责调用compiler解析指令/插值表达式

- 结构

  ![image-20200723033938892](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723033938892.png)

### ㈡ Observer类的实现   

> 能够对数据对象的所有属性进行监听，如有变动可以拿到最新值并通知Dep

- 功能

  - 负责把data选项中的属性转换为响应式 数据
  - data中的某个属性也是对象，把该属性转换成响应式数据
  - 数据变化发送通知

- 结构

  ![image-20200723041031860](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723041031860.png)

### ㈢ Compiler

> 操作DOM

- 功能

  - 负责编译模板，解析指令/插值表达式
  - 负责页面的首次渲染
  - 当数据变化后重新渲染视图

- 结构

  ![image-20200723061409464](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723061409464.png)

### ㈣ Dep类的实现

>  发布者

- 功能

  - 收集依赖，添加观察者
  - 通知所有观察者

- 结构

  ![image-20200723070340649](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723070340649.png)

### ㈤ Watcher类的实现

> 观察者

![image-20200723224010059](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723224010059.png)

- 功能
  - 当数据变化触发依赖，dep通知所有的Watcher实例更新视图
  - 自身实例化的时候往dep对象中添加自己

- 结构

  ![image-20200723225746871](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200723225746871.png)

### ㈥ 双向绑定

> 使用input事件来实现

- 在v-model事件中，绑定元素事件

### 总结

![image-20200724033925451](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200724033925451.png)

# Ⅳ ~ Virtual DOM

> 虚拟DOM,[github开源库](www.github.com/Matt-Esch/virtual-dom)

- 课程目标
  - 了解什么是虚拟DOM,以及虚拟DOM的作用
  - Snabbdom的基本使用
  - Snabbdom的源码解析

## Ⅳ · Ⅰ~ 初识虚拟DOM

> 关于虚拟DOM的使用

### ㈠什么是虚拟DOM?

- Virtual DOM（虚拟DOM）,是由**普通的JS对象来描述DOM对象**，因为不是真实的DOM对象，所以叫做Virtual DOM

> P.S. 为什么使用虚拟DOM，而不是用真实DOM?
>
> - 创建一个真实的DOM成本过高
>
>   > 使用js获取页面DOM元素，打印出它的成员即可了解

### ㈡为什么使用虚拟DOM?

> 使用虚拟DOM还有什么好处？

- MVVM框架解决了视图和状态的同步问题，避免了DOM操作

  > 手动操作DOM比较麻烦，还要考虑浏览器兼容性问题，虽然有jQuery等库简化DOM操作，但是随着项目的复杂，DOM操作的复杂度也随之提升

  - 虚拟DOM可以维护程序的状态，跟踪上一次的状态，通过对比前后状态的差异更新真实的DOM

    > Virtual DOM的好处是当状态改变时不需要立即更新DOM,只需要创建一个虚拟树来描述DOM,Virtual DOM内部将弄清楚如何有效的更新DOM

- 虚拟DOM开源库的描述

  > 手动DOM操作比较麻烦，并且很难跟踪以前的DOM状态。解决此问题的方法就是编写代码，就像在状态更改时重新创建整个DOM一样。当然，如果您每次更新应用程序状态时实际上都是重新创建了整个DOM,则应用程序将会非常缓慢，并且输入字段将会失去焦点。Virtual-dom是模块的集合，旨在提供声明性的方式来表示应用程序的DOM。因此，无需再应用程序状态更改时更新DOM,只需要创建一个虚拟树或VTree,它看起来像你想要的DOM状态，然后，Virtual-dom将弄清楚如何有效的使DOM看起来像这样，而无需重新创建所有的DOM节点。virtual0dom允许您在状态变化时更新视图，方法是创建视图的完整VTree,然后有效地修补DOM以使其完全符合您的描述。这样可以避免在应用程序代码中手动进行DOM操作和先前状态跟中，从而为Web应用程序提供简洁且可以维护的呈现逻辑。

### ㈢虚拟DOM的作用

- 维护视图和状态的关系

- 复杂视图情况下提升渲染性能

- 除了渲染DOM外，还可以实现SSR(Nuxt.js/Next.js)、原生应用(Weex/React Native)、小程序(mpvue/uni-app)等

  ![image-20200724043851217](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200724043851217.png)

## Ⅳ · Ⅱ ~  虚拟DOM的使用

> 虚拟DOM的正确打开方式

- Virtual DOM库

  - Snabbdom

    > [github库](www.gihhub.com/snabbdom/snabbdom)
    >
    > 注重简单性、模块化、强大特性和性能的虚拟DOM库

    - Vue 2.x 内部使用的Virtual DOM就是改造的Snabbdom
    - 大约200SLOC(single line of code)
    - 通过模块可扩展
    - 源码使用TypeScript开发
    - 最快的Virtual DOM之一

  - virtual-dom

### ㈠Snabbdom的基本使用

> Snabbdom的初步认识

#### ①创建项目

- 打包工具为了方便使用parcel

- 创建项目，并安装parcel

  ```js
  # 创建项目目录
  md snabbdom-demo
  # 进入项目目录
  cd snabbdom-demo
  # 创建package.json
  yarn init -y
  # 本地安装parcel
  yarn add parcel-bundler
  ```

- 配置package.json的scripts

  ```js
  "scripts": {
      "dev": "parcel index.html --open"
      "build": "parcel build index.html"
  }
  ```

- 创建目录结构

  ```js
  | index.html
  | package.json
  |-src
  	01-basicusage.js
  ```

#### ②导入Snabbdom

> 关于导入的一点小问题

- Snabbdom官网中demo导入使用的是commonjs模块化语法，但我们使用更流行的ES6模块化语法import

- ES6模块与CommonJS的差异

  ```js
  import { init, h, thunk } from 'snabbdom'
  ```

- Snabbdom的核心仅提供最基本的功能，只导出三个函数init()、h()、thunk()

  - h()返回虚拟节点VNode.这个函数在我们使用Vue.js的时候会见到

    ```js
    new Vue({
        roter,
        store,
        render：h => h(App)
    }).$mount('#app')
    ```

> P.S. 使用import导入的时候不能使用`import snabbdom from 'snabbdom'`进行导入
>
> 原因：
>
> node_modules/src/snabbdom.ts末尾导出使用的语法时export导出API,没有使用`export default`导出默认输出

#### ③三个函数方法的用途

> Snabbdom的原生方法

- Init()
  - 返回**patch()**函数
    - 作用是对比两个vnode的差异并更新到真实DOM中去

- h()
  - 创建vnode
    - 虚拟节点

### ㈡Snabbdom中的模块

> Snabbdom的核心库并不能处理元素的属性/样式/事件等，如果需要处理的话，可以使用模块

#### ①常用模块

- 官方提供的六个模块
  - attributes
    - 设置DOM元素的属性，使用setAttribute()
    - 处理布尔类型的属性
  - props
    - 和attributes模块相似，设置DOM元素的属性`element[arrr] = value`
    - 不处理布尔类型的属性
  - class
    - 切换类样式
    - 注意：给元素设置类样式是通过`sel`选择器
  - dataset
    - 设置`data-*`的自定义属性
  - eventlisteners
    - 注册和移除事件
  - style
    - 设置行内样式，支持动画
    - delayed/remove/destroy

#### ②模块的使用

- 模块的使用步骤
  - 导入需要的模块
  - init()中注册模块
  - 使用h()函数创建VNode的时候，可以把第二个参数设置为对象，其他参数往后移

## Ⅳ · Ⅲ ~ Snabbdom源码的学习

> 内置方法的原理学习

### ㈠h函数

> 用于创建VNode

#### ①h()函数介绍

- 在使用Vue的时候见过h()函数

  ```js
  new Vue({
      router,
      store,
      render: h => h(App)
  }).$mount('#app')
  ```
  - h()函数最早见于hyperscript,使用JavaScript创建超文本
  - Snabbdom中的h()函数不是用来创建超文本，而是创建VNode

#### ②函数重载

- 概念
  - 参数个数或类型不同的函数
  - JavaScript中没有重载的概念
  - TypeScript中有重载，不锁重载的实现还是通过代码调整参数

#### ③VNode函数

> 创建节点的执行者

- VNode接口

  - 约束节点对象属性

    - sel选择器

    - data节点数据

    - children子节点

      > 与text属性互斥

    - elm记录vnode对应的真实DOM

    - text节点中的内容，和children智能互斥

    - key优化使用

- 函数主体

  - 根据约定好的属性，返回一个js对象作为虚拟DOM

#### ④VNode渲染真实DOM

> Snabbdom的核心内容

- patch(oldVNode， newVnode)
  - 打补丁，把新节点中变化的内容渲染到真实DOM中，最后返回新节点作为下一次处理的久节点
  - 对比新旧VNode是否具有相同的节点(节点的key和sel是否相同)
  - 如果不是相同节点，删除之前的内容，重新渲染
  - 如果是相同节点，再判断新的VNode是否有text,如果有并且和oldVnode的text不同，则直接更新文本内容
  - 如果新的VNode有children,判断子节点是否有变化，判断子节点的过程使用的就是diff算法
  - diff过程只进行同层级比较 

### ㈡init函数

> 返回一个patch函数方法

- 接收两个参数
  - 模块
  - 指定转换为的dom格式

- 将高频调用率的patch参数减负，高阶函数的作用

### ㈢Patch函数

> Patch的主要方法

#### ①patchVnode

>  找节点的差异并更新DOM

![image-20200727055724779](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200727055724779.png)

#### ②createElm

> 创建vnode对应的DOM元素，并触发init/create钩子函数

![image-20200726072017527](C:\Users\Abel\AppData\Roaming\Typora\typora-user-images\image-20200726072017527.png)

#### ③addVnodes和removeVnodes

> 批量添加新的节点和移除老的节点

## ㈣diff算法

> diff算法在snabbdom中的核心使用:updateChildren

- 功能
  - diff算法的核心，对比新旧界定啊的children，更新DOM

- 执行过程
  - 要对比两颗树的差异，我们可以取第一棵树的每一个节点一次和第二棵树的每一个节点比较，但是这样的事件复杂度为O(n^)
  - 在DOM操作的时候我们很少很少会把一个父节点移动/更新到某一个子节点
  - 因此只需要找同级别的子节点一次比较，然后再找下一级别的节点比较，这样算法的事件复杂度为O(n)
  - 在进行同级别节点比较的时候，首先会对新节点数组的开始和结尾节点设置索引，遍历的过程中移动索引

## ㈤Modules源码的核心总结

- 核心内容
  - patch()->patchVnode()->updateChildren()
  - Snabbdom为了保证核心库的精简，把处理元素的属性/事件/样式等工作，放置到模块中
  - 模块可以按照需要引入
  - 模块实现的核心是基于Hooks
- Hooks
  - 预定义的钩子函数的名称
  - 源码位置：src/hooks.ts

