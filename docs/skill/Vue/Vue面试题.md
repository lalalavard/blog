---
id: vue-interview
slug: /vue-interview
title: Vue 面试题
date: 2022-11-25
authors: keqing
tags: [vue]
keywords: [vue]
sidebar_position: 4
---

<!-- truncate -->

:::tip Vue2 面试题

:::

## Vue 目录结构

| 目录名       |    作用    |
| ------------ | :--------: |
| assets       | 放静态资源 |
| components   |   放组件   |
| app.vue      |   根组件   |
| main.js      |  入口文件  |
| router(可选) |    路由    |
| store(可选)  |  状态管理  |

## 什么是 SPA(单页面应用)

SPA(Single-Page-Application) 单页面应用，就是在页面初始化时，一同加载 HTML、CSS、JavaScript，一旦页面加载完成，SPA 不会因为用户操作而进行页面跳转和重新加载(例如 a 标签跳转)，而是采用路由进行组件间的切换，实现 html 内容的变化。可以给客户更好的用户体验，内容更改无需重新加载页面。 tip 优缺点

- 优点:
  - SPA 相较于 SSR 对服务器的压力更小<br />
- 缺点:
  - 是首次加载时间过长
  - 不利于 SEO
  - 不支持浏览器的前进后退(这个问题被路由解决了)

## CSR(Client-Side-Rendering)

SPA(Single-Page-Application) 客户端渲染模式下，服务端会把渲染需要的静态文件发送给客户端，客户端加载过来之后，自己在浏览器里跑一遍 JS，根据 JS 的运行结果(比如 ajax 请求,浏览器爬虫不会等你请求结果返回后再爬取)，生成相应的 DOM tip 你右键查看源代码,页面上呈现的内容，在源代码文件里是找不到的

---

## 什么是 SSR(服务端渲染),优缺点?

SSR(Server-Side-Rendering) 顾名思义是将渲染的工作放在 Server 端进行。服务器渲染`完整`的 HTML 后返回给客户端。客户端拿到的内容包括首屏及完整 spa 结构，应用激活后依然按照 spa 方式运行。整个过程只向服务端发起一次请求

> **相对的 CSR ，客户端渲染，也就是目前 Web 应用中主流的渲染模式，一般由 Server 端返回的初始 HTML 页面，然后再由 JS 去异步加载数据，然后完成页面的渲染**。 tip 优缺点

- 优点:
  - 在服务端就返回渲染好的 html 页面给客户端,有更好的 SEO 表现
  - 更快的首屏加载速度
- 缺点:
  - 占用服务器资源
  - 不利于前后端分离
  - 部署比较麻烦...

## Vue2 生命周期(全)

生命周期生命周期就是 Vue 实例从创建到销毁的过程，过程分为八个钩子，分别是**创建前后、挂载前后、更新前后、销毁前后**。即`指从创建、初始化数据、编译模板、挂载到DOM上和渲染、更新到渲染、销毁的一系列过程`。

warning 生命周期中比较重要的钩子

- **created()**

  - 这时候数据初始化完成，可以在这里发 ajax 请求等等。也就是 created 在组件实例一旦创建完成后就立即调用 created()方法，这时候页面 dom 节点还没生成。

- **mounted()**

  - 是在 dom 节点渲染完成之后立刻调用的，所以可以获取访问数据和 dom 元素。

- **beforeDestory()**

  - 销毁前需要清除定时器清除缓存,防止内存泄漏

- 还有 **Keep-alive** 带来的三个额外钩子 1.`activated` 2.`deactivated`
- 如果采用 Nuxt.js 做 SSR,`beforeMounted()`之后的钩子都不会被调用,同时在`beforeCreate()`之前拓展了许多服务器端执行的钩子.

## 为什么说 Vue 是单向数据流？

tip 单向数据流

- 子组件无权修改父组件传递的数据，防止子组件意外变更父组件的状态，导致数据流向难以理解，所以说 Vue 是单向数据流。<br />

参考父子组件通信之**子组件传值给父组件**，子组件无法直接传值给父组件，需要触发父组件的事件去让父组件自己去修改值的变化。

## 组件中的 data 为什么是一个函数？

多个组件实例的 data 是独立的

- 一个组件可能会实例多次,也就是组件复用
- 对象在栈中存储的都是地址，函数的作用就是属性私有化，保证组件修改自身时不会影响其他复用组件。**如果是对象的话，一个组件修改 data 的值，其他复用组件都会跟着修改**，不方便数据维护。

## 组件中的 data 什么时候可以使用对象?

data 使用对象

- 根组件,因为根组件只会实例化一次
- 组件复用时所有组件实例都会共享 data，如果 data 是对象的话，就会造成一个组件修改 data 以后会影响到其他所有组件，所以需要将 data 写成函数，每次用到就调用一次函数获得新的数据。

## v-if 和 v-show 有什么区别？

:::tip v-if 是销毁代码，v-show  则是将代码注释掉

:::

> **两者都是控制元素显示和隐藏，v-if 是控制元素本身的增加或者删除，而 v-show 控制的是元素的 CSS 属性(display:none)**。 换种问法:为什么在$refs 中拿不到 v-if 的,能拿到 v-show 的

- 也就是说 ，**v-if 控制的是整个 DOM 节点的渲染与否**，DOM 节点的切换会有更大的性能消耗(重排 reflow)，如果频繁切换的话使用 v-show，一次切换的话就可以考虑 v-if。
- v-show 相比与 v-if 的性能优势是在组件的更新阶段,如果仅仅是在初始化阶段,v-if 的下性能还有高于 v-show,因为 v-if 只会渲染命中的一个分支,而 v-show 会渲染 2 个分支,通过 display 来控制显示与否

- 还有一个区别就是**v-show 由 flase 变为 true 的时候不会触发组件的生命周期**，而 v-if 由 false 变为 true 的时候则会触发组件的 beforeCreate、created、beforeMount、mounted

## v-if 和 v-for 一起使用会有什么问题？如何解决？

优先级和性能开销大

- 性能开销变大，每次渲染都要先循环再进行条件判断，其次是优先级的问题，Vue2 中 v-for 优先级比 v-if 高，Vue3 中 v-if 比 v-for 高。

- 解决的办法之一是采用计算属性替代，先在计算属性里面对数据做一个筛选，然后直接 for 循环筛选后的数据，就不需要 if 进行判断了。

## computed 和 watch 有什么区别

> **computed 和 watch 本质都是通过实例化 Watcher 实现，最大区别就是使用场景不同**。

1. computed 也就是计算属性，**依赖其他值且值具备缓存的特性，只有它依赖的值发送发生改变，下一次获取的值才会重新计算**，适用于复杂的数值计算并且依赖于其他属性时，就可以利用缓存特性，避免每次获取值都需要重新计算。

1. watch 侦听器，**监听属性值的变化，每当属性值发送变化，都会执行相应的回调函数**。适用于数据变化时执行异步或者开销比较大的操作
   > computed 好处

- 依赖于数据，数据更新，处理结果自动更新
- 常用的 getter 方法，获取数据，也可以使用 set 方法改变数据
- 相较于 methods，不管依赖的数据变不变，methods 都会重新计算，但 依赖数据不变的时候，computed 从缓存中获取， 不会重新计算

## vue 中的 ref 是什么？

> **ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上**。 ref

- 如果在普通的 DOM 元素上使用，**引用指向的就是 DOM 元素**；
- 如果用在子组件上，**引用就指向组件实例**。

## Vue 常用修饰符

.precent：提交事件不在重载页面 .stop： 组织但即使单击事件冒泡 .self：当事件发生在该元素本身而不是子元素的时候会触发 .capture: 事件侦听, 事件发生的时候会调用

## keep-alive 内置组件有什么作用?

:::warning [源码: src/core/components/keep-alive.ts](https://github.com/vuejs/vue/blob/main/src/core/components/keep-alive.ts)

:::

- **当进行组件的切换的时候,如果需要保持原组件的状态,则可以使用`keep-alive`包裹要缓存的组件**
- **keep-alive 有两个独有的生命周期钩子 1.actived 2.deactivated**
- **用 keep-alive 包裹的组件在切换时不会进行销毁，而是缓存到内存中并执行 deactivated 钩子函数，命中缓存渲染后会执行 actived 钩子函数**

## keepAlive 中的 include,excluede 有了解吗?

- include: 只有匹配的组件才会被缓存
- excluede: 匹配到的组件不会被缓存

## Keep-alive 的实现原理和缓存策略

实现原理 keep-alive 的实现正是用到了 LRU 策略, 将最近访问的组件 push 到 this.keys 最后面, this,keys[0] 也就是最久没有被访问到的组件, 当缓存实例超 过 max 设置值, 删除 this.keys[0] LRU 缓存淘汰算法: LRU 算法根据数据的历史访问记录来进行记录, 其核心 思想是”如果数据最近被访问过, 那么将来被访问的几率也更高”

## 什么是$nextTick?

> 一**次完整的 DOM 更新称为 Tick，nextTick 也就是在下次 DOM 更新循环 j 结束之后执行延迟回调，常用于修改数据后获取更新后的 DOM**。Tick 是一次数据渲染的周期，nextTick 就是指渲染后执行

:::warning [源码: vue/src/core/util/next-tick.js](https://github.com/vuejs/vue/blob/main/src/core/util/next-tick.ts)

:::

$nextTick 其实就是运用异步锁的的概念，保证同一时刻任务队列只有一个 flushCallbacks，当 pending 为 false 的时候，表示浏览器任务队列中没有 flushCallbacks 函数，当 pending 为 true 的时候，表示浏览器任务队列中已经放入 flushCallbacks，待执行 flushCallbacks 函数时，pending 为被再次置位 false，表示下一个 flushCallbacks 可进入任务队列，环境能力检测选择可选中效率最高的(宏任务/微任务)进行包装执行，保证是在同步代码都执行完成后，再去执行修改 DOM 等操作，flushcallbacks 先拷贝再清空，为了防止 nextTick 嵌套 mextTick 导致循环不结束

- `$nextTick`是什么？vue 实现响应式并不是数据发生变化后 dom 立即变化，而是按照一定的策略来进行 dom 更新。`$nextTick` 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM

## v-for 中 key 的作用，为什么要加 key？为什么不推荐用 index 作 key？

tip key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速 v-for 中 key 的作用

1. key 是 Vue 中 Vnode 的唯一标记，diff 算法中，sameVnode 和 updateChildren 中就使用了 key。sameVnode 用来判断是否为同一节点，常见的业务常见是一个列表，若 key 值是列表索引，在新增或删除的情况下就会存在**就地复用**的问题(就是复用了上一个在当前位置元素的状态)。所以 key 值的唯一确保 diff 更准确。

1. updateChildren 保证新开老开是同一节点，新开老结是同一节点，老开新结是同一节点，老结新开是同一节点。如果都未匹配，就需要依赖老节点的 key 和索引创建关系映射表，再用新节点的 key 去关系映射表，再用新节点的 key 去关系映射表，去寻找索引进行更新。这保证 diff 算法快速而准确

1.虚拟 DOM 变化时，两个虚拟 DOM 节点的 key 如果一样就不会重新创建节点，而是修改原来的节点，如果是分页，表格展示这种数据只是作展示的话，可以使用 index 作为 key，因为 index 值是一样的，不会出现重新创建节点的情况。其他情况下，key 值不要使用可能重复或者可能变化的 key 值，也可用 symbol 或者 id 作为 key 值保证是 key 是唯一的。

> 为什么不用 index 作为 key 呢，是因为，如果在一个 v-for list 里面，删除中间的一个 item，这个时候这个 item 后面所有的 index 都会变化，那么 diff 就会计算出后面的 item 的 key-index 映射都发生了变化，就会全部重新渲染，大大影响了性能。而且这也会导致一些 bug，比如当删除了 item2 的时候，再选中 item3 就会变成选中 item4

## v-model 双向绑定是如何实现的

v-model 双向绑定

> v-model 指令用于实现 input、select 等表单元素的双向绑定，实际上是@input 和 value 的语法糖。

- 原生 input 元素若是 text/textarea 类型，使用 value 属性和 input 事件。
- 原生 input 元素若是 radio/checkbox 类型，使用 checked 属性和 change 事件。
- 原生 select 元素，使用 value 属性和 change 事件。

**input 元素上使用 v-mode 等价于**

```html
<input :value="messgae" @input="message = $event.target.value" />
```

## 什么是虚拟 DOM？好处？

virtual DOM

- 实质上是一个 JavaScript 对象去描述真实的 DOM 或者叫视图结构，
- React 的说法是叫 UI tree。因为它不一定描述的是 DOM，描述的是视图结构。

> 好处

- 一个真实的 DOM 元素有非常多属性，直接去操作 DOM 元素会造成非常多不必要的重绘与回流
- 将真实元素节点抽象成 vNode，有效减少直接操作 DOM 的次数，起到缓冲的作用。
- 同时也利于跨平台及服务端渲染，可以是浏览器的 dom 节点，也可以是移动端相应的控件
- 能够提升开发者效率同时提供了不错的性能，并不是性能绝对比操作 DOM 好

## vNode 是如何生成的？

vNode 生成过程

1. 在 Vue 中需要编写模板-template(你还可以在 Vue 中写 JSX)
1. 这个模板会被 编译器-compile 编译为 渲染函数-h()
1. 在 挂载-mount 的过程中会调用 render 函数，返回的对象就是 虚拟 DOM
1. 它们会在 patch 的过程中转化为真实的 DOM，
1. 挂载完成后 如果 组件的响应式数据发送变化 就会 重新渲染 - re render，生成**新的虚拟 DOM** 与 **旧的虚拟 DOM** 进行比对，更新修改的地方，转换为最小量的 DOM 操作，高效更新视图。

> template 的 compile 编译过程

- 比如 `<div>content</div>` ，会转化成 ast (抽象语法树)
- 根据 AST 优化(静态内容提取等)
- AST 转换为 render 函数

## Vue 的编译/渲染过程

V 浏览器只能解析 html 文档,Vue 模板显然需要通过编译才能在浏览器中运行渲染

- **通过 compile 编译器把 template 编译成 AST 语法树（abstract syntax tree)**
- **优化 AST(节点进行了静态内容提取，也就是将永远不会变动的节点提取了出来，实现复用 Virtual DOM，跳过对比算法的功能。)**
- **将 AST 转换为 render 函数**

## Vue 的 diff 算法

- 不优化的 virtual Dom 算法,就是新旧模板进行完整的比对,每次更新都是全量更新
- 而优化后的则是 对模板的编译时优化生成 render function
- vue 3 静态节点 动态节点 patch flag 带 patch flag 的节点才会被真正追踪 就是更新的时候只会跟踪 **真正会变的东西**
- v-for 循环加 key, key 可以唯一标识一个循环个体，可以使用例如  item.id  作为 key。在列表数据进行遍历渲染时，给每一项 item 设置唯一 key 值，会方便 vue 内部机制精准找到该条列表数据。当 state 更新时，新的状态值和旧的状态值对比，较快地定位到 diff

## 什么是响应式？

数据响应式就是数据变化能被检测且对变化做出相应操作

> 数据响应式就是数据变化能被检测且对变化做出相应操作 如 视图更新

- 常见于 MVVM 框架，实现数据驱动，意思是开发人员只需要关心数据，不必像之前那样进行大量的 DOM 操作。
- 每个框架的响应式原理都有所不同，因为其中涉及到的核心问题就是如何**链接 数据层 和 视图层**。

## Vue 的响应式原理

:::warning [ 源码: src/core/observer](https://github.com/vuejs/vue/tree/main/src/core/observer)

:::

三个步骤

- 数据劫持
- 收集依赖
- 派发更新

**数据分为两类，数组和对象**。

- 当遍历对象的时候，通过 Object.defineProperty 为每个属性添加 getter 和 setter 进行数据劫持，getter 函数用于在数据读取时进行收集依赖，在对应的 dep 中存储所有的 watcher，setter 则是在数据更新后通知所有的 watcher 进行更新

- 当遍历数组的时候，用数组增强的方式，覆盖原属性上默认的数组方法，保证在新增或删除数据时，通过 dep 通知所有的 watcher 进行更新

- 运用观察者模式或者发布-订阅模式，当一个对象被修改时会自动通知依赖它的对象

> 响应式更新对象的源码下图

```js
function defineReactive(obj,key,val,shallow){
    //  实例化一个dep. 一个key对应一个dep
    const dep = new Dep()
    //获取属性描述符
    const getter = property && property.get
    const setter = property && property.set
    if((!getter || setter )) && arguments.length === 2){
        val = obj[key]
    }
    //通过递归的方式处理val为对象的情况，即处理嵌套对象
    let childOb = !shallow && observe(val)
    Object.defineProperty(obj,key,{
        enumerable:true,
        configurable:true,
        // 拦截obj.key，进行依赖收集
        get: function reactiveGetter(){
            const value = getter ? getter.call(obj) : val
            //Dep.target 是当前组件渲染的watcher
            if(Dep.target){
                //将dep添加到watcher中
                dep.depend()
                if(childOb){
                    //嵌套对象依赖收集
                    childOb.dep.depend()
                    //响应式处理value值为数组的情况
                    if(Array.isArray(value)){
                        dependArray(value)
                    }
                }
            }
            return value
        },
        set: function reactiveSetter(newVal){
            //获取旧值
            const value = getter ? getter.call(obj) : val
            //判断新旧值是否一致
            if(newVal === value || (newVal !== newVal && value !== value)){
                return
            }
            if( process.env.NODE_ENV !== 'production' && customSetter){
                customSetter()
            }

            if(getter && !setter ) return
            //如果是新值，用新值替换旧值
            if(setter){
                setter.call(obj,newVal)
            }else{
                val = newVal
            }
            //新值做响应式处理
            childOb = !shallow && observe(newVal)
            //当响应式数据更新，依赖通知更新
            dep.notify()
        }
    })
}
```

> 响应式更新数组源码如下图

```js
const arrayProto = Array.prototype;
//基于数组原型对象创建1个新的对象
export const arrayMethods = Object.create(arrayProto);
const methodsToPatch = [
  'push',
  'pop',
  'shift',
  'unshift',
  'splice',
  'sort',
  'reverse',
];
methodsToPatch.forEach(function (method) {
  const original = arrayProto[method];
  // 分别在arrayMethods 对象上定义7个方法
  def(arrayMethods, method, function mutator(...args) {
    //先执行原生的方法
    const result = original.apply(this, args);
    const ob = this.__ob__;
    let inserted;
    switch (method) {
      case 'push':
      case 'unshift':
        inserted = args;
        break;
      case 'splice':
        inserted = args.slice(2);
        break;
    }
    //针对新增元素进行响应式处理
    if (inserted) ob.observeArray(inserted);
    //数据无论是新增还是删除都进行派发更新
    ob.dep.notify();
    return result;
  });
});
```

> 观察者模式和发布订阅模式

```js
let uid = 0;
class Dep {
  constructor() {
    this.id = uid++;
    //存储所有的watcher
    this.subs = [];
  }
  addSub(sub) {
    this.subs.push(sub);
  }
  removeSub(sub) {
    if (this.subs.length) {
      const index = this.subs.indexOf(sub);
      if (index > -1) return this.subs.splice(index, 1);
    }
  }
  notify() {
    this.subs.forEach((sub) => {
      sub.update();
    });
  }
}

class Watcher {
  constructor(name) {
    this.name = name;
  }
  update() {
    console.log('更新');
  }
}
```

## Vue2 数组响应式失效原因

vue 引用类型响应式失效

> **JavaScript 的限制，Vue 不能检测数组和对象的变化**

- 数组

  - Vue2 改写了数组的 7 个方法,`'push','pop','shift','unshift','splice','sort','reverse'`
  - 这 7 个方法是可以检测到变化的.
  - **但是使用索引赋值或者改变数组的长度,响应式是会失效**

- 对象
  - **Vue2 无法检测对象属性的增加或者删除**,这是因为 Vue 会在初始化实例时对 property 执行 getter/setter 转化，所以 property **必须在 data 对象上存在才能让 Vue 将它转换为响应式的**。
  - 这个问题仅存在于 Vue2.x,**因为 Vue3 使用了 Proxy 直接劫持整个对象,而不是对象的属性**,所以不存在这个问题

tip 解决方法

- 如果是数组, 增添元素可以使用改写过后的 splice()方法,或者下面的 Vue.set()方法
- 如果是对象, 使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上。 还可以使用 vm.$set 实例方法，这也是全局 Vue.set 方法的别名。

```tsx
var a = [1, 2, 3, 4];
var b = [1, 2, 3, 4];
delete a[0];
console.log(a); //[empty,2,3,4]
this.$delete(b, 0);
console.log(b); //[2,3,4]
```

## 彻底理解 Vue2 响应式原理

1. vue 会遍历此 data 中对象的所有属性

2. 并使用 Object.defineProperty 把这些属性全部转为 getter/setter

3. 每个组件实例都有 watcher 对象

4. 它会在组件渲染的过程中吧属性记录为依赖

5. 之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而使它关联的组件得以更新

> **三个最重要的对象**

- **Observer、Watcher、Dep**

1. **Observer 对象: vue 中的数据对象在初始化过程中转换为 Observer 对象**

1. **Watcher 对象: 将模板和 Observer 对象结合在一起生成 watcher 实例，Watcher 是订阅者中的订阅者。**

1. **Dep 对象: Watcher 对象和 Observer 对象之间纽带，每一个 Observer 都有一个 Dep 实例，用来存储订阅者 Watcher**

- **当属性变化会执行主体对象 Observer 的 dep.notify()方法**，这个方法会遍历订阅者 Watcher 列表向其发送信息，Watcher 会执行 run 方法去更新视图。模板编译过程中的指令和数据绑定都会生成 watcher 实例，实例中的 watch 属性也会生成 Watcher 实例

- **在生命周期的 initState 方法中将 data，prop，method,computed, watch 中的数据劫持，通过 observe 方法与 Object.defineProperty 方法将相关对象转为换 Observer 对象**。
- 然后在 initRender 方法中解析模板，通过 Watcher 对象，Dep 对象与观察者模式将模板中的指令与对象的数据建立依赖关系，**使用全局对象 Dep.target 实现依赖收集**。
- 当数据变化时，setter 被调用，**触发 Object.defineProperty 方法中的 dep.notify 方法**，遍历该数据依赖列表，执行器 update 方法通知 Watcher 进行视图更新。

## Vue2 组件间通信

如果组件 A 引入了 B,那么 A 就是 B 的父亲,同级则是兄弟

- 父传子: props
- 子传父: $emit
- 事件总线: EventBus: $emit,$on,
- VueX
- provide/inject
- $parent/$children
- $attrs/$listeners
- ref

details Vue2 组件间通信代码有空补充...

## mixin 和 mixins

mixin

> 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项

- 其实就是把共用的代码抽出来,不用每个组件都写一遍,复用代码逻辑
- mixin 用于全局混入，会影响到每个组件实例，会看到插件是这么做初始化的

```js
Vue.mixin({
  beforeCreate() {
    // ...逻辑
    // 这种方式会影响到每个组件的 beforeCreate 钩子函数
  },
});
```

tip mixins

- mixins 应该是我们最常使用的扩展组件的方式了。如果多个组件中有相同的业务逻辑，就可以将这些逻辑剥离出来，通过 mixins 混入代码，比如上拉下拉加载数据这种逻辑等等。

- 另外需要注意的是 mixins 混入的钩子函数会先于组件内的钩子函数执行，如果组件内有同名方法,会覆盖 mixin 中的方法

## 父子组件创建销毁过程

先创建父组件,在父组件挂载前,创建并挂载子组件,在父组件销毁前,销毁子组件

## Vue 性能优化

Vue 性能优化参考文章[揭秘 Vue.js 九个性能优化技巧](https://juejin.cn/post/6922641008106668045)

- 开启 SSR 服务端渲染，加快首页时间
- 代码优化，抽取公共函数复用逻辑
- keep-alive 缓存组件,避免重新渲染组件的性能开销
- Virtual scrolling 虚拟滚动组件
- 第三方库/ UI 库组件 按需引入
- cdn 存放静态资源
- 纯展示的数据可以取消响应式 (Object.freeze() 冻结一个对象)
- 用 v-show 复用 DOM
- v-for 使用:key,避免和 v-if 同时使用
- 路由懒加载，组件的异步加载（按需加载组件）
- 组件细分,当数据变更时，由于组件代码比较庞大，vue 的数据驱动视图更新会比较慢，造成渲染过慢,用户体验也不好,所以需要将大组件拆分成小组件
- 组件销毁前清除不用的事件和定时器，避免内存泄露
- 长列表使用虚拟滚动，避免页面卡顿
  - 参考[vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)、[vue-virtual-scroll-list](https://github.com/tangbc/vue-virtual-scroll-list)

---

- Webpack 优化
- 开启 gzip 压缩
- 合理利用浏览器缓存
- cdn 加速
- 使用 Chrome 的 Performance 查找性能瓶颈

## 看过 Vue 的源码吗

Vue 的源码

- 实际上这个回答的重点，并不是你看过什么源码，而是看你怎么看源码的。因为工作原因，我看过 XXXX 的源码，分析过里面的功能，但是只看功能是第一步的，我原先总结的看代码目标是：

* 自动机中功能性的划分，能迅速找到功能划分的位置。
* 针对某些异常情况的处理，能迅速找到在不同步骤出异常或问题时的处理流程。
* 系统竞争和瓶颈所在地，能迅速找到可能导致并发 /新建问题出现的地方。
* 多线程或多进程同步的地方，能迅速分析清楚具体是哪种同步模型。
* 能够将模块中可拆分 /变化的部分迅速找到，并零件化

# Vue-Router

## Vue-Router 有什么标签？

tip 当加入 Vue Router 时，我们需要做的就是将我们的组件映射到路由上，让 Vue Router 知道在哪里渲染它们。

- **Router-Link** 和 **Router-View**，Link 执行导航的组件，View 显示组件的内容
- **路由最终会被渲染成 a 标签**

```vue{7,12}
<template>
  <h1>Hello App!</h1>
  <p>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</template>
```

## vue-router 路由模式区别？

## tip 常用的是 history 和 hash 模式，还有一种是 abstract 模式

路由不同模式的区别

> hash 模式

1. url 路径会出现“#”号字符，使用 URL hash 值来作路由，location.hash 的值就是 URL 中 # 后面的内容
2. hash 值不包括在 Http 请求中，它是交由前端路由处理，所以改变 hash 值时不会刷新页面，也不会向服务器发送请求
3. 通过 onHashChange()事件监听 url 哈希值变化则跳转页面渲染
4. hash 虽然在 URL 中，但不被包括在 HTTP 请求中；用来指导浏览器动作，对服务端安全无用，hash 不会重加载页面

> history 模式

1. 使用 HTML5 History API 来实现，
1. 需要配置下 nginx 防止页面 404 的问题
1. 前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误
1. history 采用 HTML5 的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及 popState 事件的监听到状态变更
1. window.history(可直接写成 history)指向 History 对象，它表示当前窗口的浏览历史。History 对象保存了当前窗口访问过的所有页面网址
1. historyAPI 常见属性和方法 - length：记录当前窗口访问过的页面数量 - go(n)：1 前进, -1 后退, 0 刷新 - forward()：前进 - back()：后退 - pushState()

## vue-router 如何响应路由参数的变化

> 为什么要响应参数变化？

- 切换路由，路由参数发生了变化，但是页面数据并未及时更新，需要强制刷新后才会变化。
- 不同路由渲染相同的组件时（组件复用比销毁重新创建效率要高），在切换路由后，当前组件下的生命周期函数不会再被调用。

> 解决方案：

- **使用 watch 监听**

```jsx
watch: {
    $route(to, from){
        if(to != from) {
            console.log("监听到路由变化，做出相应的处理");
        }
    }
}
```

- **向 router-view 组件中添加 key**
  - `<router-view :key="$route.fullPath"></router-view>`
  - $route.fullPath 是完成后解析的 URL，包含其查询参数信息和 hash 完整路径

## $route和$router 的区别？

- $route 是“路由信息对象”，包括 path，params，hash，query，fullPath，matched，name 等路由信息参数。
- $router 是'路由实例'对象包括了路由的跳转方法，钩子函数等。
- meta 是路由元信息

## 路由钩子有哪些？路由守卫

> 3 种，全局的、组件的、单个路由独享的 执行顺序：全局 > 路由 > 组件

1. 全局守卫：beforeEach,afterEach
2. 路由独享守卫：beforeEnter
3. 组件级别的守卫 beforeRouteEnter,beforeRouteUpdate,beforeRouteLeave

- 全局前置守卫可以用来进行拦截，（登录拦截）,除了 afterEach 全局后置外，其他的守卫中务必要调用 next(),否则无法完成导航

## 路由传参，query 和 params 的区别

tip 详情传递规则可参考 [Vue-Router v4 官方文档](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html)

- 使用 name 和 path 传参注意各自的区别
- 1.使用 query 方法传入的参数使用 this.$route.query 接受
- 2.使用 params 方式传入的参数使用 this.$route.params 接受

## 路由懒加载如何实现？

- Vue 是单页面应用，由路由去加载不同组件，默认会加载所有路由
- 这样使用 webpcak 打包后的文件很大，当进入首页时，加载的资源过多，页面会出现白屏的情况，不利于用户体验。
- 可以实现路由懒加载，当路由被访问的时候才加载对应的组件，大大提高首屏显示的速度

> 路由懒加载：

```js{4}
const Foo = () => import('./Foo.vue')
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

## Vue 中想要监听一个对象的属性，要怎么操作

- **watch 开启 deep:true 深度监听选项**

```js{7}
watch: {
	search: {
		handler(newVal, oldVal) {
			//todo
		},
		immediate: true,
     	deep: true // 可以深度检测到 person 对象的属性值的变化
	}
}
```

## Vue3 新特性

Vue3 新特性

- 响应式机制变化，**Proxy 代替 Object.defineProperty**
- **diff 算法优化**，加入预编译和 flag 标记，进一步提升对比效率。
- 作用域插槽改成了函数的方式，这样只会影响子组件的重新渲染，提升了渲染的性能
- webpack/rollup 都有**tree shaking**的功能，但前提是你用 esmodule Import 去写
- teleport 对标 react 的`<Portal>`
- `<suspense>` 管理嵌套组件的异步加载
- 新的构建工具 Vite，Pinia 为新一代官方状态管理库，实际上的 VueX5

# VueX 状态管理(整理和实现中....)

> 不再推荐使用 VueX，建议使用 Pinia

- VueX 五部分
  - state: () => ({ ... }),
  - mutations: { ... },
  - actions: { ... },
  - module
  - getters: { ... }
- VueX 数据在页面刷新后数据消失
  - 用 sessionstorage 或者 localstorage 存储数据
  - 存储： sessionStorage.setItem( '名', JSON.stringify(值) )
  - 使用： sessionStorage.getItem('名') ---得到的值为字符串类型，用 JSON.parse()去引号；

<!-- ## Vue项目封装axios？封装哪方面的？
## 说说你对slot的理解？slot使用场景有哪些？(整理和实现中....) -->

## Seo 优化终极方案

tip SEO（Search Engine Optimization）：搜索引擎优化是一种利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。目的是让其在行业内占据领先地位，获得品牌收益。很大程度上是网站经营者的一种商业行为，将自己或自己公司的排名前移。

- 开启 SSR 服务器渲染/静态化 SSG（Static Server Generation）
- SPA 应用开启预渲染 prerender-spa-plugin
- 使用 Phantomjs 针对爬虫做处理
- 合理的 title、description、keywords(TDK)
- 语义化的 HTML 代码，即语义化标签，重要的内容放在前面
- 非装饰性图片必须加 alt
- 友情链接,页面使用外链
- 向各大搜索引擎提交收录自己的站点(站长)
- 避免使用 iframe：iframe 的内容不会被爬取
- 提高网站速度：这也是搜索引擎排序的一个重要指标
- 网站流量：访问你的网站的人越多，排名也会越靠前
- 竞价排名，国内百度

[//]: # '## 如何给axios携带添加请求头'
[//]: # '对于**loader**，它是一个转换器，将A文件进行编译形成B文件，这里操作的是文件，比如将A.scss转换为A.css，单纯的文件转换过程'
[//]: #
[//]: # '**plugin**是一个扩展器，它丰富了webpack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务'

---

:::tip Vue3 面试题

:::

## CompositionAPI 和 OptionsApi 的区别?

CompositionAPI

- CompositionAPI 组织代码更加集中，不会 optionsAPI 那么分散
- 能更方便的抽取公共逻辑，复用代码

## WatchEffect 和 Watch 的区别?

## 解构的对象如何保持响应式?

- Vue3 的响应式是用 Proxy 去做的，解构后的 Proxy 对象会失去响应式
- toRefs

## Vue3.0 性能提升主要是通过哪几方面体现的？

- diff 的优化(预编译 + flag 标记)
- tree-shaking(没有用到的代码自动剔除)
- 响应式解耦

## Vue3.0 里为什么要用 Proxy API 替代 defineProperty API

- Vue2 的响应式无法监听对象/数组的 动态改变，必须在 data 中存在去遍历劫持对象的属性，而 Proxy 直接劫持整个对象，可以有 13 种拦截对象的方法。

## 说说 Vue 3.0 中 Treeshaking 特性？举例说明一下？

- 需要使用 ESmodule(import 、export)
- 比如你使用 Vue，但是在你的项目中没有使用 v-model，keep-alive 这些 api 或者指令，关于这些的代码打包就不会被打包进去

## Vite 快在哪里？

- Vite 主要快在开发阶段的 1. 启动项目快 2. 热更新快,因为 Vite 开发阶段是基于 esbuild 的,Go 写的一个打包比 JS 快上一个数量级,而生产阶段是使用 roll up,相比 webpack 也并没有快多少.
- 在 Webpack 中,当模块达到几十个以上的时候,修改其中之一的模块,都需要全部去打包一边再让服务器请求,在项目变得越来越大的时候需要非常长的时间,而 Vite 会让浏览器做更多的事情,怎么做呢? Webpack 是先打包再服务器去请求,而 Vite 直接不打包了
- (快是有前提的,要采用 esbuild,import 的导入方式)Vite 会先启动开发服务器,对文件代码不打包，服务器会根据客户端的请求加载不同的模块处理,浏览器只会对用到的模块发起 HTTP 请求,Vite 也只会编译浏览器发起 HTTP 请求的模块,实现了按需加载。

## Vue3 性能监控

- 对于性能监控来说，其实我们只需要调用` performance.getEntriesByType('navigation')`,，一行代码我们就可以获得页面中各种详细的性能相关信息.

## 有了解 suspense 组件吗?

- 就是组件还没完全加载完成的过渡动画展示组件
- 可以做 加载动画 loading，骨架屏 skeleton 这些
