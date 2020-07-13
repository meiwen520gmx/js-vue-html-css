## Js的几座大山

* 数据类型/堆栈内存
* 闭包作用域
* 面向对象
* 任务队列和同步异步编程
* ES6
* 事件及设计模式
* HTTP
* AJAX和跨域处理
* 数据可视化

## Js的数据类型

### 1.基本数据类型

number ,string,null,undefined,boolean,symbol,bigint

### 2.引用数据类型

object ,function

## 数据之间类型转换规则

* 对象--------数字：先对象.toString为字符串，再字符串转数字
* 只要+左右任何一边有字符串，那么就会变成字符串拼接（+左右任何一边出现对象，也会变成字符串拼接）
* +null =  +0
* +undefined =  +NaN
* 任何数+NaN = NaN
* ({}).toString = "[Object Object]"
* ([]).toString = " "

### 1.把其他数据类型转换为Number类型

* (1)特定需要转换为Number的
  * +Number([value])  
  * `console.log(Number("12px"))//NaN`
  * `console.log(Number(null))//0`
  * `console.log(Number(undefined))//undefined`
  * +parseInt/parseFloat([val])
* (2)隐式转换（浏览器内部默认要先转换为Number再进行计算）
     * +isNaN([val])
     * +数学运算（特殊情况下：+在出现字符串的情况下不是数学运算，是字符串拼接）
     * +在==比较的时候，有些值需要转换为数字再进行比较

### 2.把其他数据类型转换为字符串

* (1)能使用的方法
  * +toString()
  * String()
* (2)隐式转换（一般都是调用其toString）
  * +加号运算的时候，如果某一边出现字符串，则是字符串拼接
  * +把对象转换为数字，需要先toString()转换为字符串，再去转换为数字
  * +基于alert/confirm/prompt/document.write...这些方式输出内容，都是把内容转换为字符串，然后再输出

### 3.把其他数据类型转换为布尔

* (1)基于以下方式可以把其它数据类型转换为布尔
  * ！转换为布尔值后取反
  * ！！转换为布尔类型
  * Boolean([val])
* (2)隐式转换
  * 在循环或者条件判断中，条件处理的结果就是布尔类型值
  * 只有0，NaN,null,undefined,空字符串，五个值会变成布尔的false,其余都是true

### 4.在==比较的过程中，数据转换的规则：

* 【类型一样的几个特殊点】
  + {} == {}  //false 对象比较的是堆内存的地址
  + [] == []// false
  + NaN == NaN//false
  + Number('as') == NaN//false
* 【类型不一样的转换规则】
     * null==undefined//true,但是换成===结果是false(因为类型不一样),剩下null/undefined和其他任何数据类型值都不相等
     * 字符串==对象，要把对象转换为字符串
     * 剩下如果==两边数据类型不一致，都是需要转换为数字再进行比较

## 堆栈内存

* js代码之所以能在浏览器运行是因为有一个ECStack执行环境栈
* 所有的赋值都是先创建值，再创建变量，最后用指针关联
* 所有的引用类型都要开辟一个堆内存来存储
* 带成员访问的要优先处理 比如：a.x = a = {n:2}   先创建值{n:2}存堆里面，   然后a.x=xxx，最后a=xxx
* 一个变量只能跟一个值进行关联

<img src="C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200629100358552.png" alt="image-20200629100358552" />

<img src="C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200629100358552.png" alt="image-20200629100358552" style="zoom:80%;" />

* 变量提升：当前上下文代码执行之前，会把var/function声明或者定义，带var的只声明，带function声明+定义，如果遇到了{}新老浏览器会表现不一致（兼容ES3,ES6），声明就是创建变量，定义就是给变量赋值
* 【IE浏览器<=IE10】 不管{}，还是一如既往的function声明+定义，而且也不会存在块级作用域
* 【新版本浏览器】{}中的function在全局下只声明不定义，{}出现function/let/const会创建一个块级上下文

![image-20200629112833892](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200629112833892.png)

## vue核心基础知识

### 说说你对SPA单页面的理解，它的优缺点分别是什么？

![image-20200630092232690](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200630092232690.png)

* iframe本质上是MPA,但是它能实现SPA效果

### vue-router有哪几种导航守卫？

#### （1）全局守卫：

* router.beforeEach 全局前置守卫，进入路由之前
* router.beforeResolve 全局解析守卫（2.5.0+）在beforeRouteEnter调用之后调用
* router.afterEach 全局后置钩子，进入路由之后

`

router.beforeEach((to, from, next) => {

  //next()  next(false)  next('/')

]})

`

#### （2）路由独享守卫：

* beforeEnter

#### （3）路由组件内守卫：

* beforeRouteEnter 进入路由前，在路由独享守卫后调用，不能获取组件实例this,组件实例还没被创建
* beforeRouteUpdate(2.2) 路由复用同一个组件时，在当前路由改变，但是该组件被复用时调用
* beforeRouteLeave 离开当前路由时，导航离开该组件的对应路由时调用

`

beforeRouteEnter(to, from, next) {

​	next(vm => {

​			//通过“vm”来访问组件实例

​     })

}

`

回答：路由导航守卫有很多种：全局守卫和路由独享守卫，还有组件内守卫，但是我在项目中做权限校验的时候使用的是beforeEach，其他的守卫只是知道，但是很少用。

### 真实项目中的权限校验

#### （1）登录态校验

* 会话机制
* token机制（接口权限）
* beforeEach(登录态存储在vuex中)
* 向服务器发送请求----
  * 登录的时候，登录成功后会在vuex中存储已经登录isLogin
  * 为了防止页面刷新过程中vuex存储信息消失，服务器是记录登录态的
  * 第一种：会话方式 cookie session
  * 第二种：token方式  后端用JWT生成token  客户端把token存储起来（本地存储）
* 接口权限校验   token 

#### （2）菜单和按钮的权限校验(显示隐藏)

* 获取权限字段：登录成功从服务器获取用户的权限字段，存储在本地（本地存储[不安全，可以加密存储] /  [vuex]）
* 使用自定义指令来控制渲染还是不渲染（不建议使用组件内 v-if）
* 有的公司，客户端渲染的菜单，都是服务器返回的html  (服务器压力大，但是绝对安全)
* 不管是否有权限都能看，只不过点击会提示或者不让跳路由，再或者跳转到指定路由（beforeEach,组件内自己判断）
* 数据权限
  * 服务器处理的（保证绝对的安全性）

#### （3）导航守卫

* 登录态校验
* 根据权限控制是否允许进入路由

### 谈谈你对keep-alive的了解？

keep-alive是vue内置的一个组件，可以使被包含的组件保留状态，避免重新渲染，其有以下特征：

* 一般结合路由和动态组件一起使用，用于缓存组件
* 提供include和exclude属性，两者都支持字符串或者正则表达式，include表示只有名称匹配的组件会被缓存，exclude表示任何名称匹配的组件都不会被缓存，其中exclude的优先级比include高
* 对应两个钩子函数activated和deactivated,当组件被激活时，触发钩子函数activated,当组件被移除时，触发钩子函数deactivated

### v-show与v-if有什么区别

* v-show是css切换，v-if是完整的销毁和重新创建
* 使用频繁切换时用v-show,运行时较少改变时用v-if
* v-if="false"  v-if是条件渲染，当false的时候不会渲染

### v-model的原理

`<input type="text" :value="searchtxt" @input="searchtxt = $event.target.value">{{searchtxt}}`

### computed和watch的区别和运用的场景

* computed擅长处理的情景：一个数据受多个数据影响

  <img src="C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200630155643151.png" alt="image-20200630155643151" style="zoom:80%;" />

* watch擅长处理的情景：一个数据影响多个数据

* ![image-20200630155730286](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20200630155730286.png)

* watch监听现有的数据，改变其他值，类似于某些数据的监听回调
* computed创建一个数据，这个数据受到其他状态的影响，并且computed的值有缓存，只有他的依赖属性值发生改变，下一次获取computed的值时才会重新计算
* 运用场景：
* 当我们需要进行数值计算，并且依赖于其他数据时，应该使用computed，因为可以利用computed的缓存特性，避免每次获取值时，都要重新计算；
* 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用watch，使用watch选项允许我们执行异步操作（访问一个API），限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态，这些都是计算属性无法做到的；

### vue的生命周期(问的是各个周期函数在项目中的应用)

* beforeCreate 实例还没有创建完成
* created 实例创建完成，整个东西初始化完成，页面还没加载完成（发送异步数据请求）
* beforeMount
* mounted 第一次页面渲染完了，有真实的DOM
* beforeUpdate
* updated中一定不能再更新状态，会形成一个死循环
* beforeDestroy路由切换会把上一个组件销毁，把一些数据存储起来
* destroyed

### MVVM(vue的原理)

Vue主要通过以下4个步骤来实现数据双向绑定的：

* 实现一个监听器Observer：对数据对象进行遍历，包括子属性对象的属性，利用Object.defineProperty()对属性都加上setter和getter,这样给这个对象的某个值赋值，就会触发setter,那么就能监听到数据的变化
* 实现一个解析器Compile：解析Vue模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新
* 实现一个订阅者Watcher：Watcher订阅者是Observer和Compile之间通信的桥梁，主要的任务是订阅Observer中的属性值变化的消息，当收到属性值变化的消息时，触发解析器Compile中对应的更新函数
* 实现一个订阅器Dep：订阅器采用发布-订阅设计模式，用来收集订阅者Watcher，对监听器Observer和订阅者Watcher进行统一管理

### N种组件间相互通信的方法

* props/$emit适用 父子组件通信
* $refs与$parent/$children适用 父子组件通信
* EventBus($emit/$on)适用于 父子、隔代、兄弟组件通信
* provide/inject 适用于隔代组件通信
* vuex公共状态管理 适用于SPA单页面应用中的各类情况

### Vue的单向数据流

* 所有的prop都使得其父子prop之间形成了一个单向下行绑定：父级prop的更新会向下流动到子组件中，但是反过来则不行
* vue的父组件和子组件生命周期钩子函数执行顺序可以归类为以下四个部分:

                    *  加载渲染过程：`父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted`
                    *  子组件更新过程：`父bedoreUpdate->子beforeUpdate->子updated->父updated`
                    *  父组件更新过程：`父beforeUpdate->子updated`
                    *  销毁过程：`父beforeDestroy->子beforeDestroy->子destroyed->父destroyed`
                    *  每次父级组件发生更新时，子组件中所有的prop都将会刷新为最新的值，这意味着你不应该在一个子组件内部改变prop，如果你这样做了，vue会在浏览器的控制台中发出警告！子组件想修改时，只能通过$emit派发一个自定义事件，父组件接收到后，由父组件修改。
   *  有两种常见的视图改变一个prop的情形：
          *   （1）这个prop用来传递一个初始值，这个子组件接下来希望将其作为一个本地的prop数据来使用，在这种情况下，最好定义一个本地的data属性并将这个prop用作其初始值
          *   （2）这个prop以一种原始的值传入且需要进行转换，在这种情况下，最好使用这个prop的值来定义一个计算属性













