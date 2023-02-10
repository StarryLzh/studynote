# ES6

## 1、Babel转码器

 Babel 把ES6转为ES5

## 2、Let命令

1. 1. var关键字：函数级的作用域
   2. let关键字：

1. 1. 1.  let是块级作用域(花括号级的作用)
      2.  let是不存在变量提升的
      3.  let是不能重复声明

1. 1. 在for循环中：

1. 1. 1. 每次循环var i 都是同一个i在for{ }内（作用域为函数级）
      2. 每次循环let i  都是生成一个新的let i在{ }内。 （作用域只在块级）

## 3、Const命令

1. const 声明一个只读的常量。**一旦声明，常量的值就不能改变**
2. const 声明的变量不得改变值，这意味着， const **一旦声明变量，就必须立即初始化**，不能留到以后赋值
3. const 的作用域与let命令相同：只在声明所在的**块级作用域内有效**
4. const 命令**声明**的常量也是**不存在提升**
5. const 声明的常量，也与 let 一样**不可重复声明**

## 4、（对象的）解构赋值

解构可以用于对象，在声明大括号中声明对象的属性。

> **温馨提示** 
>
> 对象的属性没有次序，变量必须与属性同名，才能取到正确的值

```javascript
let {name,age} ={name:"iwen",age:20};
```

#### **对象解构**

``` html
<script>   
    var user={     
        name:"iwen",     
        age:20   
    }
    const {name,age} = user;
    console.log(name,age);
    //简化console   
    const {log} =console;
    log(name,age);
</script>
```

对象的解构赋值，可以很方便地将现有对象的方法，赋值到某个变量

```javascript
let {random,floor} = Math;
let {log} =console;
```

注意事项，如果要将一个**已经声明的变量用于解构赋值，必须非常小心**

<u>**声明重复 **</u>

```javascript
let hello = "Hello"; 
let { hello } = {hello:"hello"}; // 报错  ,重复声明
let hello = "Hello"; 
({ hello } = {hello:"hello"}); // 正确 
```

## 5、字符串扩展

#### **字符串Unicode 表示法**

ES6 加强了对 Unicode 的支持，允许采用 \uxxxx 形式表示一个字符。其中 xxxx 表示字符的 Unicode 码点。

统一码（Unicode），也叫万国码、单一码，是计算机科学领域里的一项业界标准，包括字符集、编码方案等。Unicode是为了解决传统的字符编码方案的局限而产生的，它为每种语言中的每个字符设定了统一并且唯一的二进制编码，以满足跨语言、跨平台进行文本转换、处理的要求.字符串遍历器接口

#### **for...of** **循环遍历**

```javascript
for (let i of 'itbaizhan') {
	//以前的for 前置i的初始化0后，是用下标寻找字符
	console.log(i);
} 
```

#### **模板字符串**

​	模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来**定义多行字符串**，或者在**字符串中嵌入变量。**

```javascript
let url = "www.itbaizhan.com"
let h1 = "<a href='"+ url +"'>itbaizhan</a>"
let h2 = `<a href='${url}'>itbaizhan</a>`
```

#### **includes(),startsWith(),endsWith()**

​	传统上，JavaScript 只有 indexOf 方法，可以用来确定一个字符串是否 包含在另一个字符串中。ES6 又提供了三种新方法。

1. **includes()**：返回布尔值，表示是否找到了参数字符串
2. **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部
3. **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部

```javascript
let s = 'Hello world!';
s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```

这三个方法都支持第二个参数，表示开始搜索的位置

```javascript
let s = 'Hello world!';
s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

#### **repeat()**

repeat 方法返回一个**新字符串**，表示将原字符串重复 n 次。

```javascript
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```

#### **padStart()，padEnd()**

ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。 padStart() 用于头部补全， padEnd() 用于尾部补全。

```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

#### **trimStart()，trimEnd()**

ES2019对字符串实例新增了 trimStart() 和 trimEnd() 这两个方法。它们的行为与 trim() 一致， trimStart() 消除字符串头部的空格， trimEnd() 消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。

```javascript
const s = ' itbaizhan ';
s.trim() // "itbaizhan"
s.trimStart() // "itbaizhan "
s.trimEnd() // " itbaizhan"
```

#### **at()**

at() 方法接受一个整数作为参数，返回参数指定位置的字符，支持负索引（即倒数的位置）。

```javascript
const str = 'hello';
str.at(1) // "e"
str.at(-1) // "o"
```

> **温馨提示** 
>
> 如果参数位置超出了字符串范围， at() 返回 undefined

## 6、数组扩展

#### 扩展运算符

扩展运算符（spread）是三个点（ ... ）。将一个数组转为用逗号分隔的参数序列

```javascript
console.log(...[1, 2, 3])
// 1 2 3
console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5
```

**替代函数的 apply 方法**

由于扩展运算符可以展开数组，所以不再需要 apply 方法，将数组转为函数的参数了

```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77])
// ES6 的写法
Math.max(...[14, 3, 77])
// 等同于
Math.max(14, 3, 77);
```

**合并数组**

扩展运算符提供了数组合并的新写法

```javascript
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

#### 新增方法

**Array.from()**

Array.from 方法用于**将类数组转为真正的数组**

> **温馨提示** 
>
> 常见的类数组有三类：
>
> 1. arguments 
> 2. 元素集合 
> 3. 类似数组的对象

**arguments**

```javascript
function add(){
    let collect = Array.from(arguments);
    collect.push(40);
    console.log(collect);
}
add(10,20,30)
```

**元素集合**

```javascript
let divs = document.querySelectorAll('div');
console.log(Array.from(divs));
```

**类似数组的对象**

```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
let arr = Array.from(arrayLike);
console.log(arr);
```

**Array.of()**

Array.of() 方法用于**将一组值，转换为数组**

```javascript
1 Array.of(3, 11, 8) // [3,11,8]
```

## 7、对象的扩展

#### **属性的简洁表示法**

​	ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```javascript
let name = "iwen"
const user = {
    name,
    age:20
}
```

除了属性简写，方法也可以简写

```javascript
const o = {
  method() {
    return "Hello!";
 }
};
// 等同于
const o = {
  method: function() {
    return "Hello!";
 }
};
```

这种写法用于函数的返回值，将会非常方便

```javascript
function getPoint() {
  const x = 1;
  const y = 10;
  return {x, y};
}
getPoint() // {x:1, y:10}
```

#### **属性名表达式**

ES6 允许字面量定义对象时，用表达式作为对象的属性名，即把表达式放在方括号`[ ]`内

```javascript
let propKey = 'itbaizhan';
let obj = {
 [propKey]: true,
 ['a' + 'bc']: 123
};
```

#### **对象的扩展运算符**

ES2018 将这个运算符引入了对象

```javascript
let z = { a: 3, b: 4 };
let n = { ...z };
console.log(n);
{...{}, a: 1}
// { a: 1 }
```

## 8、函数的扩展_箭头函数

#### **基本用法**

ES6 允许使用“箭头”（ => ）定义函数

```javascript
var add = (x) => x;

// 等同于
var add = function (x) {
    return x;
};
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分

```javascript
var add = (x,y) => x+y;
// 等同于
var add = function (x,y) {
   return x+y;
};


var add = () => 100;
// 等同于
var add = function () {
    return 100;
};
```

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用 `return `语句返回

```javascript
var add = (x,y) => {
    var z = 10;
    return x+y+z
};
// 等同于
var add = function (x,y) {
    var z = 100
    return x+y+z
};
```

由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

``` javascript 
var add = (x,y) => ({x:10,y:20});
```

箭头函数的一个用处是简化回调函数（匿名函数）

```javascript
var arr = [10,20,30]
arr.map(item =>{
    console.log(item);
})
```

#### **使用注意点**

对于普通函数来说，内部的` this `指向函数运行时所在的对象，但是这一点对箭头函数不成立。它没有自己的` this` 对象，内部的 `this` 就是定义时上层作用域中的 `this`

``` javascript
var name = "itbaizhan"
var user = {
   name:"iwen",
   getName(){
       setTimeout(() =>{
           console.log(this.name);// iwen
      })
   }
}
user.getName()
```

> **温馨提示**
>
> 箭头函数里面根本没有自己的 this ，而是引用外层的 this

## 8、Set数据解构

#### **基本用法**

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

`Set `**本身是一个构造函数**，用来生成 Set 数据结构。

``` javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
    console.log(i);
}
// 2 3 5 4
```

通过` add() `方法向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值。



`Set `函数可以接受一个数组作为参数

```javascript
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]
```

数组去除重复成员的方法

```javascript
// 去除数组的重复成员
[...new Set(array)]
```

字符串去除重复字符

```javascript
[...new Set('ababbc')].join('')

// "abc"
```

向 Set 加入值的时候，不会发生类型转换，所以` 5` 和` "5" `是两个不同的值。

```javascript
var mySet = new Set();
mySet.add("5")
mySet.add(5)
console.log(mySet); // Set(2) {'5', 5}
```

#### **size属性**

返回 `Set `实例的成员总数

```javascript
const items = new Set([1, 2, 3, 4, 5, 5, 5,5]);
items.size // 5
```

### **Set数据结构方法**

#### **add()**

`set `添加方法

```javascript
var mySet = new Set();
mySet.add("5")
console.log(mySet);
```

#### **delete()**

删除某个值，返回一个布尔值，表示删除是否

```javascript
var mySet = new Set();
mySet.add("5")
var flag = mySet.delete("5");
console.log(flag);  // true
```

#### **has()**

返回一个布尔值，表示该值是否为 `Set` 的成员

```javascript
var mySet = new Set();
mySet.add("5")
var flag = mySet.has("5");
console.log(flag);  // true
```

#### **clear()**

清除所有成员，没有返回值

```javascript
var mySet = new Set();
mySet.add("5")
mySet.clear();
console.log(mySet);  // Set(0) {size: 0}
```



## 9、Promise对象

#### **基本概念**

Promise 是**异步编程的一种解决方案**，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了` Promise` 对象

所谓 `Promise` ，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以**获取异步操作的消息**。`Promise `提供统一的API，各种异步操作都可以用同样的方法进行处理

有了` Promise `对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，` Promise` 对象提供统一的接口，使得控制异步操作更加容易

#### **基本用法**

ES6 规定， `Promise `对象是一个构造函数，用来生成` Promise `实例

``` javascript
const promise = new Promise(function(resolve,
reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
 } else {
    reject(error);
 }
});
```

`Promise `构造函数**接受一个函数作为参数**，**该函数的两个参数**分别是`resolve `和 `reject` 。它们是两个函数，由 JavaScript 引擎提供，不用自己部署

`Promise` 实例生成以后，可以用` then `**方法**分别指定 `resolved `状态和 `rejected`状态的回调函数。

```javascript
promise.then(function(value) {
  // success resolved
}, function(error) {
  // failure rejected
});
```

#### **加载图片资源例子**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

    <div id="box">等待加载</div>

    <script>

        var box = document.getElementById("box")

        function loadImageAsync(url){
            const promise = new Promise(function(resolve, reject){
                // 异步处理：消耗时间的代码
                const image = new Image(); // 创建image对象
                image.src = url
                image.onload = function(){
                    resolve(image)
                }
                image.onerror = function(){
                    reject(new Error('Could not load image at ' + url))
                }
            })

            return promise;
        }

        const promise = loadImageAsync("http://iwenwiki.com/api/musicimg/2.png");
        promise.then(function(data){
            // 成功
            box.appendChild(data)
        },function(error){
            // 失败
            box.innerHTML = "图片加载失败";
            console.log(error);
        })


    </script>
    
</body>
</html>
```

###  Ajax实操

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

    <script>

        // XHR对象

        const getJSON = function(url){
            const promise = new Promise(function(resolve,reject){
                // 异步操作：网络请求代码

                const handler = function(){
                    if(this.readyState !== 4){
                        // 0 1 2 3 4
                        return;
                    }
                    if(this.status === 200){
                        resolve(this.response)
                    }else{
                        reject(new Error(this.statusText))
                    }
                }
                 // XHR对象
                const client = new XMLHttpRequest();
                client.open("GET",url);
                client.onreadystatechange = handler;
                client.responseType = "json";
                client.setRequestHeader("Accept","application/json");
                client.send();
            })
            return promise;
        }

        getJSON("http://iwenwiki.com/api/blueberrypai/getChengpinDetails.php")
        .then(function(data){
            console.log(data);
        },function(error){
            console.log(error);
        })


    </script>
    
</body>
</html>
```

## 10、Async函数

> [技术蛋老师](https://www.bilibili.com/video/BV1tZ4y1Q7Zh)
>
> 用同步编程的方式，去写异步代码
>
> 其他老师说：async函数永远返回一个Promise
>
> 蛋老师：如果函数不返回return，则async没有什么意义。
>
> ​				其实返回的是 return Promise.resolve( code); //执行成功返回的值
>
> await 其实就是把 “异步操作变为了同步”， 让线程等待“异步的结果”

> await 必须和 async一起使用

**示例操作**

``` javascript
function print(){
    //定时器是异步的，第一个参数为函数=需要执行的代码
  setTimeout(() =>{
    console.log("定时器");
   },1000)
  console.log("Hello");
}

print() //希望先定时器，然后hello
		//Hello  定时器
```

#### **基本语法**

```javascript
function timeout(ms) {
    
   	//resolve本身在promise中就是一个函数
 	return new Promise((resolve) => {
  	setTimeout(resolve, ms);

  });
}

async function asyncPrint(value, ms) {
 	//把具有异步的操作放在“await后”
    await timeout(ms);
	console.log(value);
}

asyncPrint('hello world', 50);
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

    <script>

        // function print(){
        //     // 定时器时异步的
        //     setTimeout(()=>{
        //         console.log("定时器");
        //     },10)
        //     console.log("Hello");
        // }

        // print(); // Hello  定时器  

        // async
        function timeout(ms){
            // resolve:是一个函数
            return new Promise((resolve,reject) =>{
                setTimeout(function(){
                    console.log("定时器");
                    resolve();
                    //这句话是为了让promise执行成功函数resolve，才能继续执行
                },ms)
            })
        }

        async function asyncPrint(ms,value){
            // 把具有异步操作的代码前面放入：await
            await timeout(ms);
            console.log(value);
        }

        asyncPrint(100,"hello")

    </script>
    
</body>
</html>
```



#### **异步应用**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./jquery-3.6.0.min.js"></script>
</head>

<body>

    <script>

        // 网络请求之间有没有用依赖关系？  很多接口可能要依赖遇上一个接口的数据才能执行

        function ajax(url) {
            return new Promise(function (resolve, reject) {
                $.getJSON(url, function (result) {
                    resolve(result)
                }, function (error) {
                    reject(error)
                })
            })
        }

        async function getInfo(){
            var id = await ajax("http://iwenwiki.com/api/generator/list.php");
            var info = await ajax("http://iwenwiki.com/api/generator/id.php?id=" +id[0] );
            var result = await ajax("http://iwenwiki.com/api/generator/name.php?name=" +info.name )
            console.log(result);
        }

        getInfo();

    </script>

</body>

</html>
```

