---
title: javascript欺骗词法
date: 2018-06-11
comment: true
---

### 欺骗词法
js词法作用域是在编译词法阶段就确定的作用域，所以根据代码所处的位置，一般就可以确定其作用域。但有些方法可以欺骗词法作用域。
什么是欺骗词法，即能改变当前变量所处的作用域。
1. eval()
eval()函数能够将字符串解析成代码块执行。如下：
```js
var a = 1;
function test(str) {
    eval(str);
    console.log(a);
}
test();//1
test("var a = 2");//2
```
`test("var a = 2")`时，`console.log(a)`的答应结果为2，因为函数内执行`eval("var a = 2")`，相当于在函数内部声明了变量a，在调用a时在函数作用内a的声明。就不会往外层作用于查找。所以a的用于域动态发什么变化。并没有确定下来。
`use strict` 模式下，eval()函数会有其自己的词法作用域，那么无法修改所在位置的作用域

2. with()
with()函数 可以根据传入的参数创建一个封闭的作用域。但如果在起作用域赋值一个对象中不存在的属性，其作用域就会泄漏到外层作用域上。
```js
var obj = {
    a:1,
    b:2,
}
with(obj) {
    a = 2;
    b = 3;
    c = 3;
}
console.log(obj.a);//2
console.log(obj.b);//3
console.log(obj.c);//undefined
console.log(c);//3
```
在以上例子中 obj中没有c属性，所以以obj属性创建的作用域中没有c变量申明。所以`c = 2`提升到全局变量中。
`use strict` 模式下，with函数不可用。

### 欺骗词法影响性能
js在编译的词法分析阶段可以知道全部词法标识符位置，js引擎在编译时对作用域查找有优化。而eval()可以改变词法作用域，with是在运行时根据入参对象动态创建新的词法作用域。因此js编译时的优化都不起作用。因此大量使用eval和with会影响代码效率。
