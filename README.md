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

