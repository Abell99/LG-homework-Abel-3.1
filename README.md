# 拉钩大前端第一期学员-职铭想

# 第三模块第一部分作业

## 一、简答题

### 1、当我们点击按钮的时候动态给 data 增加的成员是否是响应式数据，如果不是的话，如果把新增成员设置成响应式数据，它的内部原理是什么。

```
let vm = new Vue({
 el: '#el'
 data: {
  o: 'object',
  dog: {}
 },
 method: {
  clickHandler () {
   // 该 name 属性是否是响应式的
   this.dog.name = 'Trump'
  }
 }
})
```

>答：
>
>**不是响应式数据**
>
>> 理由：响应式对象和响应式数组是指在vue**初始化**时期，利用**Object.defineProperty()方法**对其进行监听，这样在修改数据时会及时体现在页面上。
>
>**Vue中将新增成员设置为响应式数据的两种方法**
>
>- 给dog的属性name设置一个初始值
>
>  > e.g.
>  >
>  > ```js
>  > name: '' || undefined
>  > ```
>
>  > P.S.
>  >
>  > **使其在初始化期间就存在**
>
>- 使用Vue.set(target, key, value)时，target为需要添加属性的对象，key是要添加的属性名，value为属性key对应的值
>
>  > e.g.
>  >
>  > ```js
>  > this.$set(this.data.dog, name, 'Trump')
>  > ```
>  >
>  > P.S
>  >
>  > **手动调用方法使其成为响应式对象**

### 2、请简述 Diff 算法的执行过程

> 答：
>
> **Diff算法的执行过程**
>
> >  diff是一个算法逻辑，diff算法思想在调用patch中体现
>
> - patch方法的执行逻辑图
>
>   ![image-20200731042746692](C:\Users\Abel\Desktop\image-20200731042746692.png)
>
>   - 在新旧节点都有子节点的时候执行：**upadteChildren**
>
>     > **使用diff算法对比子节点，更新子节点**
>
>     ![image-20200731120359166](C:\Users\Abel\Desktop\image-20200731120359166.png)

## 二、编程题

### 1、模拟 VueRouter 的 hash 模式的实现，实现思路和 History 模式类似，把 URL 中的 # 后面的内容作为路由的地址，可以通过 hashchange 事件监听路由地址的变化。

> - 实现将URL 中的 # 后面的内容作为路由的地址
>
>   > 对应的源代码文件：code/vue-router
>   
>   ```js
>   clickHandler (e) {
>     location.hash = this.to
>     this.$router.data.current = this.to
>     e.preventDefault()
>   }
>   window.addEventListener('hashchange', () => {
>     this.data.current = location.hash.slice(1)
>   }
>   ```

### 2、在模拟 Vue.js 响应式源码的基础上实现 v-html 指令，以及 v-on 指令。

> - 实现v-html指令
>
>   > 对应的源代码文件：code/minVue
>
>   ```js
>   // 处理 v-html 指令
>   htmlUpdater (node, value, key) {
>     node.innerHTML = value
>     new Watcher(this.vm, key, (newVal) => {
>       node.innerHTML = newVal
>     })
>   }
>   ```
>
> - 实现v-on指令
>
>   > 对应的源代码文件：code/minVue
>
>   ```js
>   // 判断元素属性是否是事件
>   isEvent(attrName) {
>     return attrName.includes(':')
>   }
>   // 编译元素节点，处理指令
>   compileElement(node) {
>     // 遍历所有的属性节点
>     Array.from(node.attributes).forEach(attr => {
>       // 判断是否是指令
>       let attrName = attr.name
>       if (this.isDirective(attrName)) {
>         // v-text --> text
>         attrName = attrName.substr(2)
>         let key = attr.value
>         // 新增判断是否为 v-on 指令
>         if (this.isEvent(attrName)) {
>           let name = attrName.replace('on:', '')
>           this.addHandler(node, name, key)
>           return 
>         }
>         
>         this.update(node, key, attrName)
>       }
>     })
>   }
>   // 添加事件
>   addHandler (node, eventName, key) {
>       let keyArr = key.split('(')
>       let eventFn = keyArr[0]
>       let eventArg = keyArr[1].match(/(?<=').*?(?=')/g)[0]
>       node.addEventListener(eventName, () => {
>         this.vm.$options.methods[eventFn](eventArg)
>       })
>     }
>   ```

### 3、参考 Snabbdom 提供的电影列表的示例，利用Snabbdom 实现类似的效果:如下图

![示例图片](https://yanxuan.nosdn.127.net/d8c0b872629fb2b1fd125e52b1a1a54b.png)

> - 使用Snabbdom实现例图效果
>
>   > 对应的源代码地址：code/snabbdom-demo
>
>   ```js
>   import { h, init } from 'snabbdom'
>   import style from 'snabbdom/modules/style'
>   import eventlisteners from 'snabbdom/modules/eventlisteners'
>   import { originalData } from './originData'
>   
>   let patch = init([style,eventlisteners])
>   
>   let data = [...originalData]
>   const container = document.querySelector('#container')
>   
>   var sortBy = 'rank';
>   let vnode = view(data);
>   
>   // 初次渲染
>   let oldVnode = patch(container, vnode)
>   
>   
>   // 渲染
>   function render() {
>       oldVnode = patch(oldVnode, view(data));
>   }
>   // 生成新的VDOM
>   function view(data) {
>       return h('div#container',
>           [
>               h('h1', 'Top 10 movies'),
>               h('div',
>                   [
>                       h('a.btn.add',
>                           { on: { click: add } }, 'Add'),
>                       'Sort by: ',
>                       h('span.btn-group',
>                           [
>                               h('a.btn.rank',
>                                   {
>                                       'class': { active: sortBy === 'rank' },
>                                       on: { click: [changeSort, 'rank'] }
>                                   }, 'Rank'),
>                               h('a.btn.title',
>                                   {
>                                       'class': { active: sortBy === 'title' },
>                                       on: { click: [changeSort, 'title'] }
>                                   }, 'Title'),
>                               h('a.btn.desc',
>                                   {
>                                       'class': { active: sortBy === 'desc' },
>                                       on: { click: [changeSort, 'desc'] }
>                                   }, 'Description')
>                           ])
>                   ]),
>               h('div.list', data.map(movieView))
>           ]);
>   }
>   
>   // 添加一条数据 放在最上面
>   function add() {
>       const n = originalData[Math.floor(Math.random() * 10)];
>       data = [{ rank: data.length+1, title: n.title, desc: n.desc, elmHeight: 0 }].concat(data);
>       render();
>   }
>   // 排序
>   function changeSort(prop) {
>       sortBy = prop;
>       data.sort(function (a, b) {
>           if (a[prop] > b[prop]) {
>               return 1;
>           }
>           if (a[prop] < b[prop]) {
>               return -1;
>           }
>           return 0;
>       });
>       render();
>   }
>   
>   // 单条数据
>   function movieView(movie) {
>       return h('div.row', {
>           key: movie.rank,
>           style: {
>               display: 'none', 
>               delayed: { transform: 'translateY(' + movie.offset + 'px)', display: 'block' },
>               remove: { display: 'none', transform: 'translateY(' + movie.offset + 'px) translateX(200px)' }
>           },
>           hook: {
>               insert: function insert(vnode) {
>                   movie.elmHeight = vnode.elm.offsetHeight;
>               }
>           }
>       }, [
>           h('div', { style: { fontWeight: 'bold' } }, movie.rank),
>           h('div', movie.title), h('div', movie.desc),
>           h('div.btn.rm-btn', {on: { click: [remove, movie]}}, 'x')]);
>   }
>   // 删除数据
>   function remove(movie) {
>       data = data.filter(function (m) {
>           return m !== movie;
>       });
>       render()
>   }
>   ```

