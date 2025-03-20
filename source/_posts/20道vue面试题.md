---
title: 20道vue面试题
date: 2022-02-20 01:10:01
tags: ['面试', 'vue']
---
常见的vue面试题，整理如下：
<!-- more -->
### 单页应用(spa)
单页Web应用，就是只有一张Web页面的应用。浏览器一开始会加载必需的HTML、CSS和JavaScript，之后所有的操作都在这张页面完成，这一切都由JavaScript来控制。

单页应用得优势：

操作体验流畅，媲美本地应用的感觉，切换过程中不会频繁有被“打断”的感觉。
因为界面框架都在本地，与服务端的通讯基本只有数据，所以便于迁移，可以用比较小的代价，迁移成桌面产品，或者各种移动端Hybrid产品。
完全的前端组件化，前端开发不再以页面为单位，更多地采用组件化的思想，代码结构和组织方式更加规范化，便于修改和调整；
API 共享，如果你的服务是多端的（浏览器端、Android、iOS、微信等），单页应用的模式便于你在多个端共用 API，可以显著减少服务端的工作量。容易变化的 UI 部分都已经前置到了多端，只受到业务数据模型影响的 API，更容易稳定下来，便于提供更棒的服务；
组件共享，在某些对性能体验要求不高的场景，或者产品处于快速试错阶段，借助于一些技术（Hybrid、React Native），可以在多端共享组件，便于产品的快速迭代，节约资源。

单页应用得缺点：

首次加载大量资源，要在一个页面上为用户提供产品的所有功能，在这个页面加载的时候，首先要加载大量的静态资源，这个加载时间相对比较长；
对搜索引擎不友好，因为界面的绝大部分都是动态生成的，所以搜索引擎很不容易索引它。
开发难度相对较高，开发者的JavaScript技能必须过关，同时需要对组件化、设计模式有所认识，他所面对的不再是一个简单的页面，而是一个运行在浏览器环境中的桌面软件。

### MVVM
MVVM全名是 Model View View-Model的缩写
Model(模型)：是用于处理应用程序数据逻辑部分。
View(视图)：是应用程序中处理数据显示的本分。通常视图是依据模型数据创建的
ViewModel层：做了两件事达到了数据的双向绑定，一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面。 实现的方式时：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转换成后端的数据。实现的方式是：DOM事件监听。

### Vue 的响应式原理（双向数据绑定）
整体思路是数据劫持 + 观察者模式

对象内部通过 defineReactive 方法，使用 Object.defineProperty 将属性进行劫持（只会劫持已存在的属性），数组则是通过重写数组来实现。当页面使用对应属性时，每个属性都拥有自己的 dep 属性，存在它所依赖的 watcher （依赖收集）get，当属性变化后会通知自己对应的 watcher 去更新（派发更新）set。

1、Object.defineProperty 数据劫持
2、使用 getter 收集依赖 ，setter 通知 watcher派发更新。
3、watcher 发布订阅模式。
### data 为什么是函数

组件的data写成一个函数，数据以函数返回值形式定义，这样每复用一次组件，就会返回一分新的data，类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据。而单纯的写成对象形式，就使得所有组件实例共用了一份data，就会造成一个变了全都会变的结果

### v-model 的原理
v-model 只是语法糖而已。
v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件。
text 和 textarea 元素使用 value property 和 input 事件；
checkbox 和 radio 使用 checked property 和 change事件；
select 字段将 value 作为 prop 并将 change 作为事件。
注意：对于需要使用输入法的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。
在普通元素上：
input v-model='sth'
input v-bind:value='sth' v-on:input='sth = $event.target.value'

### v-if 和 v-show 的区别
控制手段不同
编译过程不同
编译条件不同

控制手段：v-show隐藏则是为该元素添加css--display:none，dom元素依旧还在。v-if显示隐藏是将dom元素整个添加或删除

编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换

编译条件：v-if是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。只有渲染条件为假时，并不做操作，直到为真才渲染

v-show 由false变为true的时候不会触发组件的生命周期

v-if由false变为true的时候，触发组件的beforeCreate、create、beforeMount、mounted钩子，由true变为false的时候触发组件的beforeDestory、destoryed方法

性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗；


### computed、 watch 区别

computed
1、computed是计算属性，也就是依赖某个值或者props通过计算得来得数据；
2、 computed的值是在getter执行之后进行缓存的，只有在它依赖的数据发生变化，会重新调用getter来计算；
3、 不支持异步，当computed内有异步操作时无效，无法监听数据的变化；

watch
1、watch是监听器，可以监听某一个数据，然后执行相应的操作；
2、不支持缓存，数据变直接会触发相应的操作；
3、监听的函数接收两个参数，第一个参数是最新的值；第二个参数是输入之前的值；
4、支持异步操作；

### Vue 的生命周期
beforeCreate 在实例初始化之后，数据观测（data observe）和 event/watcher 事件配置之前被调用。在当前阶段 data、methods、computed 以及 watch 上的数据和方法都不能被访问。

created 实例已经创建完成之后被调用。在这一步，实例已经完成以下的配置：数据观测（data observe ），属性和方法的运算，watch/event 事件回调。这里没有 $el，如果非要想与 DOM 进行交互，可以通过vm.$nextTick 来访问 DOM。

beforeMount 在挂载开始之前被调用：相关的 render 函数首次被调用。

mounted 在挂载完成后发生，在当前阶段，真实的 Dom 挂载完毕，数据完成双向绑定，可以访问到 Dom节点。

beforeUpdate 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁 （patch）之前。可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

updated 发生在更新完成之后，当前阶段组件 Dom 已经完成更新。要注意的是避免在此期间更新数据，因为这个可能导致无限循环的更新，该钩子在服务器渲染期间不被调用。

beforeDestroy 实例销毁之前调用。在这一步，实力仍然完全可用。我们可以在这时进行 善后收尾工作，比如清除定时器。

destroy Vue实例销毁后调用。调用后，Vue实例指示的东西都会解绑定，所有的事件监听器会被移除，左右的子实例也会被销毁，该钩子在服务器端渲染不被调用。

activated keep-alive 专属，组件被激活时调用

deactivated keep-alive 专属，组件被销毁时调用

### 父子组件生命周期顺序
加载渲染过程
父beforeCreate -> 父created -> 父beforeMount -> 子beforeCreate -> 子created -> 子beforeMount -> 子mounted -> 父mounted
子组件更新过程
父beforeUpdate -> 子beforeUpdate -> 子updated -> 父updated
父组件更新过程
父beforeUpdate -> 父updated
销毁过程
父beforeDestroy -> 子beforeDestroy -> 子destroyed -> 父destroyed
### Vue 组件间通信方式
1、props 和 $emit。父组件向子组件传递数据是通过props传递的，子组件传递给父组件是通过$emit触发事件来做到的。

2、$parent 和 $children 获取单签组件的父组件和当前组件的子组件。

3、$attrs 和 $listeners A -> B -> C。Vue2.4开始提供了$attrs和$listeners来解决这个问题。

4、父组件中通过 provide 来提供变量，然后在子组件中通过 inject 来注入变量。（官方不推荐在实际业务中适用，但是写组件库时很常用。）

5、$refs 获取组件实例。

6、envetBus 兄弟组件数据传递，这种情况下可以使用事件总线的方式。

7、vuex 状态管理。
### Vue 的单项数据流
数据总是从父组件传到子组件，子组件没有权利修改父组件传过来的数据，只能请求父组件对原始数据进行修改。这样会防止从子组件意外改变父组件的状态，从而导致你的应用的数据流向难以理解

### keep-alive 组件
keep-alive 是 Vue 内置的一个组件，可以实现组件缓存，当组件切换时不会对当前组件进行卸载
### slot 插槽
vue里提供了一种将父组件的内容和子组件的模板整合的方法：内容分发，通过slot插槽来实现。

在组件标签内部写入的内容默认的会被替换掉，如果想要在组件的模板里使用这些内容，就在对应的位置写上slot标签，这个slot标签就代表着这些内容
### Vue 检测数组或对象的变化
对象是用defineProperty setter。
数组考虑性能原因没有用 defineProperty 对数组的每一项进行拦截，而是选择对7种数组（push,shift,pop,splice,unshift,sort,reverse）方法进行重写（AOP 切片思想）。

所以在 Vue 中修改数组的索引和长度无法监控到。需要通过以上7种变异方法修改数组才会触发数组对应的watcher进行更新
### 虚拟dom
由于在浏览器中操作DOM是很昂贵的。频繁操作DOM，会产生一定性能问题。这就是虚拟Dom的产生原因。Vue2的Virtual DOM 借鉴了开源库 snabbdom 的实现。Virtual DOM本质就是用一个原生的JS对象去描述一个DOM节点，是对真实DOM的一层抽象。
优点：
1、保证性能下限：框架的虚拟DOM需要适配任何上层API可能产生的操作，他的一些DOM操作的实现必须是普适的，所以它的性能并不是最优的；但是比起粗暴的DOM操作性能要好很多，因此框架的虚拟DOM至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，既保证性能的下限。
2、无需手动操作DOM：我们不需手动去操作DOM，只需要写好 View-Model的 代码逻辑，框架会根据虚拟DOM和数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率。
3、跨平台：虚拟DOM本质上是JavaScript对象，而DOM与平台强相关，相比之下虚拟DOM可以进行更方便地跨平台操作，例如服务器端渲染、weex开发等等。
缺点：
1、无法进行极致优化：虽然虚拟DOM + 合理的优化，足以应对大部分应用的性能需要，但在一些性能要求极高的应用中虚拟DOM无法进行针对性的极致优化。
2、首次渲染大量DOM时，由于多了一层DOM计算，会比innerHTML插入慢。
### Vue 中 key 的作用
如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。key 是为Vue中Vnode的唯一标识，通过这个key，我们的diff操作可以更准确、更快速。
更准确：因为带key就不是就地复用了，在sameNode函数 a.key === b.key 对比中可以避免就地复用的情况。所以更加准确。
更快速：利用key的唯一性生成map对象来获取对应节点，比遍历方式块。
### nextTick 的原理
nextTick 中的回调是在下次 DOM 更新循环结束之后执行的延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。主要思路就是采用微任务优先的方式调用异步方法去执行 nextTick 包装的方法
### Vuex
vuex 是专门为 vue 提供的全局状态管理系统，用于多个组件中数据共享、数据缓存

主要包括以下几个模块：

State:定义了应用状态的数据结构，可以在这里设置默认的初始化状态。
Getter:允许组件从Store中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。
Mutation:是唯一更改 store 中状态的方法，且必须是同步函数。
Action:用于提交 mutation，而不是直接变更状态，可以包含任意异步请求。
Module:允许将单一的 Store 拆分更多个 store 且同时保存在单一的状态树中。
### vue-router 的两种模式
hash 模式
1、location.has 的值实际就是 URL 中 # 后面的东西。它的特点在于：hash虽然出现 URL 中，但不会被包含在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

2、可以为 hash 的改变添加监听事件
window.addEventListener("hashchange",funcRef,false)
每一次改变 hash (window.location.hash)，都会在浏览器的访问历史中增加一个记录，利用hash的以上特点，就可以实现前端路由“更新视图但不重新请求页面”的功能了
特点：兼容性好但是不美观

history 模式
利用 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。

这两个方法应用于浏览器的历史记录站，在当前已有的 back、forward、go 的基础上，他们提供了对历史记录进行修改的功能。这两个方法有个共同点：当调用他们修改浏览器历史记录栈后，虽然当前 URL 改变了，但浏览器不会刷新页面，这就为单页面应用前端路由“更新视图但不重新请求页面”提供了基础

特点：虽然美观，但是刷新会出现 404 需要后端进行配置
### vue-router 有几种导航钩子
路由钩子的执行流程，钩子函数种类有：全局守卫、路由守卫、组件守卫。
完整的导航解析流程：
1、导航被触发。
2、在失活的组件里调用 beforeRouterLeave 守卫。
3、调用全局的 beforeEach 守卫。
4、在重用的组件调用 beforeRouterUpdate 守卫（2.2+）。
5、在路由配置里面 beforeEnter。
6、解析异步路由组件。
7、在被激活的组件里调用 beforeRouterEnter。
8、调用全局的 beforeResolve 守卫（2.5+）。
9、导航被确认。
10、调用全局的 afterEach 钩子。
11、触发 DOM 更新。
12、调用 beforeRouterEnter 守卫中传给next的回调函数，创建好的组件实例会作为回调函数的参数传入。



