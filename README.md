# 只有出去面试，才知道自己到底有多菜

# javascript部分

## var, let, const的区别
声明方式 | 变量提升 | 暂时性死区 | 重复声明 | 初始值 | 作用域
--- | --- | --- | --- | --- | --- |
var | 允许 | 不存在 | 允许 | 不需要 | 非块级
let | 不允许 | 存在 | 不允许 | 不需要 | 块级
const | 不允许 | 存在 | 不允许 | 需要 | 块级

### 变量提升
指变量可以在声明之前使用，但是值为undefined
```
    console.log(a) //undefined
    var a = 1;

    console.log(b) //报错， b is not defined
    let b = 1;

    console.log(c) //报错， c is not defined
    const c = 1;
```
### 暂时性死区
在块级作用域中，如果使用了let/const,他们所声明的变量就会绑定在这个区域，不受外界影响
```
var temp = 1;
if(true) {
    temp = 3; //报错
    let temp;
}
```
### 重复声明
在相同作用域内，重复声明同一个变量（ES6规定let和const命令不允许重复声明同一个变量）
```
function func() {
    let a = 1;
    var a = 2; //报错
}
```
### 初始值
ES6规定，const是一个只读常量，一旦声明，就必须赋值，且不能修改（对象数据类型的变量，不可以修改指向该地址的指针，可以修改里边的值）

### 作用域
ES5中，只有全局作用域跟函数作用域，ES6中有块级作用域，即花括号内为一个块

## i++和++i的区别
i++：先进行表达式的运算，然后再自加1  
++i：先自加1，然后再进行表达式的运算

## 根据上面问题提出来的延伸
```
for(var i = 0; i <= 3; i++) {
    setTimeout(function() {
        console.log(i)
    }, 0)
}
// 求打印出来的值： 4个4
```
我的理解是，setTimeout并没有执行，当i=3时，执行了一遍i++后，然后才运行的setTimeout
### 考校的知识点
 * 关于for循环的执行顺序
    > 1. var i = 0;
    > 2. 进行判断，如果为真，执行循环体里的内容
    > 3. i++
 * 关于setTimeout 0秒的理解
    > 我的理解是：最后执行，最后执行，最后执行，因为javascript是单线程

## 闭包
闭包是指在一个函数内，访问外部函数内的变量，并把这个函数返回到外面
### 例子
```
function fn() {
    var temp = 1;
    return function() {
        temp++;
    }
}
```
内部函数调用了fn函数内部的变量，待fn执行完成并销毁后，变量temp并没有被销毁，而是被内部函数携带到了外部

### 闭包的用处
1. 私有化属性
2. 有些计算会用到闭包

# vue部分

## v-if和v-show的区别以及各自的应用场景
### 区别
v-if的显示隐藏是把dom元素整个的渲染或者删除，所以每次v-if控制的显示或者隐藏，都会伴随着大量的性能消耗
v-show的显示隐藏是控制元素的display属性
### 应用场景
如果需要频繁切换元素的显示隐藏，可以用v-show, 如果少量显示隐藏或者只显示一次，可以选择用v-if

## vue-router中push/replace/go的区别
### router.push：等同于router-link，调用时，会在history栈添加一条新纪录，点击回退按钮，可以回到上一个页面
举个例子：假如从a -> b之后，又想从b -> c，调用push方法会向history栈中添加一条记录，然后跳转到c页面，在当前路由点击浏览器的回退按钮， 则回到b
### router.replace：不会向history栈中添加记录，如同这个名字一样，替换掉当前的路由地址
举个栗子：假如从a -> b之后，又想从b -> c，调用replace方法后，内部直接将当前页面的路由地址替换成c的地址，在当前路由页点击回退按钮，会直接回到a页面
### router.go：接受一个整型参数，正数为前进n步，负数为回退n步
