### 响应式编程 Reactor

#### 前言
> 了解响应式编程之前,应当对[`函数式编程`](https://blog.csdn.net/weixin_39847945/article/details/110816323)和 `Stream`流 有所了解


##### 参考
- https://juejin.cn/post/6844903858599428110

#### 概念

> 响应式编程是对异步事件的流编程.<br>
响应式编程是一种关注于数据流（data streams）和变化传递（propagation of change）的异步编程方式。 这意味着它可以用既有的编程语言表达静态（如数组）或动态（如事件源）的数据流。<br>
响应式编程通常作为面向对象编程中的“观察者模式”（Observer design pattern）的一种扩展。 <br>
响应式流（reactive streams）与“迭代子模式”（Iterator design pattern）也有相通之处， 因为其中也有 Iterable-Iterator 这样的对应关系。主要的区别在于，Iterator 是基于 “拉取”（pull）方式的，而响应式流是基于“推送”（push）方式的。

- iterator 是一种“命令式”（imperative）编程范式，即使访问元素的方法是 Iterable 的唯一职责。关键在于，什么时候执行 next() 获取元素取决于开发者。
- 响应式流中，相对应的角色是 Publisher-Subscriber，但是当有新的值到来的时候 ，却反过来由发布者（Publisher）通知订阅者（Subscriber），这种“推送”模式是响应式的关键.


