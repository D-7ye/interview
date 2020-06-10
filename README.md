# 只有出去面试，才知道自己到底有多菜

# 专业素养
### 主流浏览器及其内核
1. IE -> Trident
2. Chrome -> 以前是webkit，现在是Blink
3. Safari -> webkit
4. Opera -> 以前是Presto，现在是Blink
5. firefox -> Gecko

# 浏览器兼容问题
## 盒模型
使用box-sizing: border-box;

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
i++：先进行表达式的运算，然后在自加1
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

## Es6中的异步方法的用法？

### promise
```
new Promise((resolve, reject) => {
	resolve()
})
.then((val) => {
	...
})
.catch((error) => {
	...
})
```

### A/A
await调用后，生成一个Promise对象，可以使用.then()/.catch()处理结果


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

# 小程序
## 小程序传递数据的几种方式？

### 页面间传值

#### 通过wx.navigateTo() API
```
// 传值
wx.navigateTo({

      url: '/page/index/index?id=' + id +'&username='+ username+'&password='+ password

})
// 取值
onLoad: function (options) {

    varid = options.id;

    varname = options.name;

    varpassword = options.password;

    this.setData({

      id:id,

      name:name,

      password:password,

    })

}
```
#### 通过 <navigator>组件中的url携带数据来实现
```
//在url中设置定义属性name和值
<navigator url="../login/login?name=郭宝">跳转到登录页</navigator> 
//通过onload方法接受传递过来的数据
onLoad: function (options) {
    console.log(options);
     // {name: "郭宝"} 
}
```

### 通过数据存储传值
```
wx.setStorage、wx.setStorageSync、wx.getStorage、wx.getStorageSync、wx.clearStorage、wx.clearStorageSync、
//根据key获取到缓存的值
varsalemanId= wx.getStorageSync("salemanId");
```
### 通过全局变量传递数据
```
//在项目根目录的app.js文件中的 globalData 对象定义一个全局变量
globalData: {
	itemData: null,
	userInfo: null
}
// 在page中获取应用实例
const app = getApp()
// 通过这个app实例，可以获取、设置全局变量的值
app.globalData.itemData = item //设置
app.globalData.itemData //获取
```
## bindtap和catchtap的区别？
bind事件绑定不会阻止冒泡事件向上冒泡，catch事件绑定可以阻止冒泡事件向上冒泡
## wx.navigateTo、wx.redirectTo、wx.reLaunch、wx.switchTab、wx.navigateBack的区别？
* wx.navigateTo  用于保留当前页面、跳转到应用内的某个页面，使用 wx.navigateBack可以返回到原页面。对于页面不是特别多的小程序，通常推荐使用 wx.navigateTo进行跳转， 以便返回原页面，以提高加载速度。当页面特别多时，则不推荐使用。
* wx.redirectTo  当页面过多时，被保留页面会挤占微信分配给小程序的内存，或是达到微信所限制的 5 层页面栈。这时，我们应该考虑选择 wx.redirectTo。wx.redirectTo()用于关闭当前页面，跳转到应用内的某个页面。这样的跳转，可以避免跳转前页面占据运行内存，但返回时页面需要重新加载，增加了返回页面的显示时间。
* wx.reLaunch   wx.reLaunch()与 wx.redirectTo()的用途基本相同， 只是 wx.reLaunch()先关闭了内存中所有保留的页面，再跳转到目标页面。
* wx.switchTab  对于跳转到 tab bar 的页面，最好选择 wx.switchTab()，它会先关闭所有非 tab bar 的页面。其次，也可以选择 wx.reLaunch()，它也能实现从非 tab bar 跳转到 tab bar，或在 tab bar 间跳转，效果等同 wx.switchTab()。使用其他跳转 API 来跳转到 tab bar，则会跳转失败。
* wx.navigateBack  用于关闭当前页面，并返回上一页面或多级页面。开发者可通过 getCurrentPages() 获取当前的页面栈，决定需要返回几层。这个 API 需要填写的参数只有 delta，表示要返回的页面数。若 delta 的取值大于现有可返回页面数时，则返回到用户进入小程序的第一个页面。当不填写 delta 的值时，就默认其为 1（注意，默认并非取 0），即返回上一页面。
## 如何提高小程序加载速度？
？？？
## 小程序的生命周期
onLoad、onReady、onShow、onHide、onUnload

# 关于项目开发
## 假如数据放在一个数组里，数组里边是对象，如果要删除几条数据，请问用什么方法？
？？？

