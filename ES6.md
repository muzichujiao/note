# ES6学习笔记

## let && const
### let 
* 全局变量不再在顶层对象上
### const
* 不是值不变，是地址不变，向const定义的对象中加属性时时允许的。


## 字符串模板
* 主要是为了可以定义多行字符串。并且可以在其中嵌入变量。

## 函数扩展
### rest参数
* 用于获取函数的剩余参数
* 函数的length不包括rest参数
* arguments为类数组，不能使用数组的方法，转化为数组：Array.prototype.slice.call(arguments)
### 箭头函数
*注意：
* 内部的this指向定义时的对象。
* 不能作为构造函数，遇到new报错。
* 不能使用arguments，可以使用rest参数代替
* 不能使用yield命令，因为箭头函数不可以作为Generator函数。

### 尾调用
在函数的最后一步调用另一个函数。
#### 尾调用优化
由于尾调用时函数的最后一步，所以可以不用保存外层函数的调用帧，直接用内存函数的调用帧。
#### 尾递归
尾调用自身，就是尾递归。
* 递归耗内存，需要保存成千上万的调用帧，容易发生栈溢出。对于尾递归来说，只保存一个调用帧，不会发生溢出。
* 尾调用优化只在严格模式下才会有，普通模式下可用func.caller来监控调用帧。

## 数组扩展
### 扩展运算符
* 将数组转化为用逗号分隔的参数序列。
* Array.from() :将类数组转化为数组
* Array.prototype.slice.call() :将类数组转化为数组

## Async && Await
是Generator的语法糖，是将Generator函数中\*换成async，yield换成await。
```
async function quits(name){
	var val1 = await func1();
	var val2 = await func2();
	return result;	
}
//调用
quits(name).then(function(res){
	console.log(res);
})
async函数内部return语句返回的值，会成为then方法回调函数的参数。
```
### 与Generator不同点
* 他是自动执行的，当调用函数时，无需调用next()函数一步一步执行，自带内置执行器
* 返回结果是Promise对象，可以根据结果进一步操作

### 注意：
* await后面是Promise对象，运行结果可能是reject，所以把await命令放在try...catch中。只要有一个await命令返回reject，async函数就会中断执行，如果想要后面的代码继续执行，将await命令放到try'cache中。

## 异步编程的解决方法
### 回调函数
传统的异步编程解决方法
### 事件监听
### 发布/订阅
### Promise
#### 介绍
* Promise对象像一个容器放着未来才会结束的事件。
* Promise对象的状态不受外界的影响，只有异步操作的结果能够决定状态的变化。有三种状态：进行中、已成功、已失败
* 一旦状态变化，就不会再变
#### 基本用法
```
const p = new Promise(function(resolve,reject){
	//... ...
	if(成功){
		resolve(value);
	}else{
		reject(error);
	}
})
```
* 异步加载图片
```
function imgload(url){
	return new Promise(function(resolve,reject){
		var img = new Image();
		img.onload = function(){
			resolve(img);
		}
		img.onerror = function(){
			reject(new Error("Can't find url."));
		}
		img.src = url;
	})
}

imgload('......').then(function(value){
	document.getElementById('root').append(value);
})

```
* 异步AJAX
```
function ajaxAysn(url){
  const promise = new Promise(function(resolve,reject){
    const xhr = new XMLHttpRequest();
    const handler = function() {
      if((xhr.readyState == 4) && (xhr.status == 200)){
        resolve(xhr.response);
      }else{
        reject(xhr.statusText);
      }
    }
    xhr.open("GET",url);
    xhr.onreadyStateChange = handler;
    xhr.send();
  })
}
```
#### Promise包含的方法
##### Promise.prototype.then()：
* then()方法主要是为Promise实例添加状态改变的回调函数
* 它接收两个函数为参数，第一个参数为成功时调用，第二个参数是失败时调用.
##### Promise.prototype.catch() / Promise.prototype.finally():无论结果是成功还是失败都会执行
#### Promise.all()
* Promise.all([p1,p2,p3]): 参数是多个Promise实例，只有全部为成功状态，才表示成功，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
* Promise.race([p1,p2,p3]): 只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

## Generator
### 介绍
* Generator相当于一个状态机，函数内部封装了许多状态。调用的时候返回一个遍历器对象，可以依次遍历内部的状态。
* 调用时候并不会执行，返回一个指向内部状态的指针，当调用next()方法时，会依次遍历内部状态。
```
function *gen(){
	yield 'one';
	yield 'two;
	return 'three';
}
const g = gen();
g.next()  //{value:'one',done:false}
g.next()  //{value:'two',done:false}
g.next()  //{value:'three',done:true}
g.next()  //{value:undefined,done:true}
```
### for...of...循环
```
function *num(){
	yield 1;
	yield 2;
	yield 3;
	yield 4;
	return 5;
}
for(let i of num()){
	console.log(i);
}
//1,2,3,4
```
一旦done的属性值为true，就会终止循环，因此没有输出5


## 模块 Module
### CommonJS和AMD区别
#### CommonJS
* nodeJS和webpack模块系统就是使用的commonJS规范
* 有一个全局的require()函数用来加载模块
* CommonJs定义的模块分为三部：使用require()函数来引用模块、模块的定义使用exports对象用于导出当前模块的方法和变量、模块的表示module对象代表模块本身
* CommonJs用于服务端，采用的是同步加载的方式，因为模块文件都放在服务器的硬盘上，实际的加载时间是硬盘文件的读取时间
#### AMD
* 异步加载模块，使用在浏览器端，由于加载的js文件都放在服务端，从服务器加载文件受到网络等各种环境的限制，如果采用同步加载，一旦一个文件加载受阻，后面排队执行的js语句就会执行错误，导致页面“假死”，用户无法交互。
* 采用异步加载，模块的加载不影响后面语句的运行，所有依赖这个模块的语句都定义在一个回调函数中
* 模块引用require([module],callback)、模块定义 define(modulename,dependence,fun)

### ES6模块
* CommonJs模块就是对象，加载模块时生成对象，然后从对象上读取方法，这种加载称为在运行时加载
```
// CommonJs模块
let {stat,exists,readFile} = require('fs');
//等价于
let _fs = require('fs');
let _stat = _fs.stat;
let _exists = _fs.exists;
let _readFile = _fs.readFile;
```
* ES6模块是在编译时完成模块加载，模块不是对象，通过export显示输出代码，使用import输入
```
import {stat,exists,readFile} from 'fs';
```
#### ES6模块语法
* ES6模块中，禁止this指向全局对象，顶层this指向undefined。
* exoport命令：用于规定模块对外的接口。export语句输出的接口，对应的值是动态绑定关系，通过该接口，可以取到模块内部实时的值。CommonJS输出的值是缓存，不存在动态更新。export命令必须存在模块顶层，不能处于块级作用域中
* import命令：用于加载这个模块。 模块接口都只是只读的，不允许在加载模块脚本中改写接口。 import命令有提升效果，提升到模块头部

### 浏览器加载
#### 传统浏览器加载script
* 传统浏览器加载js使用script标签，进行的是同步加载js文件。如果想要异步加载在script标签中加上defer关键字(渲染完成再执行)或者asyn关键字(下载完成就执行)
#### 浏览器加载ES6模块
* 也使用script标签，但是需要加入type="module"属性。加载是异步加载，等同于页面渲染完成再执行脚本
* 注意
1. 模块采用严格模式
2. 模块中，顶层的this关键字返回undefined，不是指向window
3. 同一模块如果加载多次，只会执行一次
* ES6模块与CommonJS模块差异：
1. CommonJS模块输出的是一个拷贝值，一旦输出，模块内部变化不会影响。ES6模块输出的是值的引用
2. CommonJS模块是运行时加载，ES6是编译时输出接口。原因是Commonjs加载的是一个对象，该对象只有在脚本运行完成之后才会生成
3. ES6顶层的this指向undefined。CommnJS模块顶层this对象指向当前模块
#### 循环加载
循环加载：指的是a脚本执行依赖b脚本，b脚本执行依赖a脚本
##### CommonJS循环加载：
* 加载原理： 
require第一次加载这个模块时，执行这个脚本，生成一个对象，对象中有模块的名称、模块输出的接口。以后再执行require命令时，不会再执行脚本，而是从缓存中取值
* 循环加载： Commonjs模块在require时会全部执行，但是一旦有循环加载，只会输出已经执行的部分，还未执行的部分不会输出
##### ES6模块加载


## 数组的扩展
### 扩展运算符(...)
* 扩展运算符将一个数字转化为参数序列
```
console.log(...[1,2,3]); //1 2 3
var arr = [...[1,2,3],[4,5]];
console.log(arr);    //[1,2,3,[4,5]]
```
* 替代数组的apply方法
由于扩展运算可以展开数组，所以不需要使用apply将数组转化为函数参数
```
var arr1 = [1,2,3,4,5,6];
var maxx = Math.max.apply(null,arr1);
console.log(maxx);   // 6
var min = Math.min(...arr1);
console.log(min);   // 1
```
* 将一个数组添加到另一个数组后面
```
var arr2 = [1,2,3],arr3=[4,5,6];
arr3.push(...arr2);
console.log(arr3);   // [4,5,6,1,2,3]
```

## Class基本语法
### 定义类
```
class Person{
	constructor(name,age){
		this.name = name;
		this.age = age;
	}
	sayName(){
		console.log(this.name);
	}
	addAge(num){
		console.log('原来的年龄是'+this.age);
		console.log('修改后的年龄是：'+(this.age+num));
	}
}
var p = new Person('小美',24);
p.sayName();  
p.addAge(2); 
```
* `console.log(typeof Preson);  // "function"` 表明类的数据类型是函数。
* 类的方法都是添加在类的prototype属性上
* 类的方法都不可以枚举
### 类的实例对象
* 类的属性定义在实例对象本身上，方法是定义在原型上。类的所有实例共享一个原型对象。
```
var p = new Person('小美'，23)；
var p2 = new Person('小民'，24);
p.hasOwnProperty('name');   // true
p.hasOwnProperty('sayName');   // false
p.__proto__.hasOwnProperty('sayName');   //true
p.__proto__ === p2.__proto__;  //true
```
### 私有方法
有类的概念就伴随着私有方法，但是ES6不提供，所以只能使用变通的方法来模拟实现。
* 一种方法是在命名的时候加以区别
```
Class Person(){
  sayName(name){ this,_addAge(num); }
  _addAge(num){ …… }
}
```
规定使用以 '_' 开头命名的为私有方法，但是尽管如此，还是可以访问
* 另一种方法是将私有方法移除类中，因为类中的所有方法都可以访问
```
Class Person(){
  sayName(name){ 
    addAge.call(this,name);
  }
  
}
function addAge(name){ retrun console.log('My age is '+ this.age); }
```
sayName是公有方法，内部调用了addAge方法，使得addAge实际上是当前模块的私有方法。
* 还有一种就是利用Symbol值得唯一性将私有方法的名字命名为一个Symbol值
* **this** 默认指向类的实例
### 静态方法(static)
* 静态方法：加上static的方法不会被实例继承，而是直接通过类调用。
```
class Person{
	static sayName(){
		console.log('xiaomei');
	}
}
Person.sayName();  //xiaomei
var p = new Person();
p.sayName(); // TypeError
```
由于是静态方法所以可以通过类名来调用，而不是通过实例来调用。
* 父类的静态方法可以被子类继承
* 静态方法也可以从super对象上调用
```
class Animal{
  static sayName(){
    console.log('hello');
  }     
}
class Cat extendsAnimal(){
  static sayName(){
    super.sayName();
  }
}
```
## Class的继承
### 定义
* super() 表示父类的构造函数，子类没有自己的this对象，只能继承父类的this对象，如果不调用super就得不到this对象
* ES6继承与之前继承的不同：
1. ES6的继承实质是：先创建父类的实例对象this，然后再用子类的构造函数修改this
2. ES5的继承实质：构建子类的实例对象this，然后将父类的方法添加到上面。
* 子类的构造函数中只有调用super之后才能使用this关键字
### 类的prototype属性和__proto__属性
Class存在着这两个属性
* 子类的__proto__属性表示构造函数的继承，总是指向父类
* 子类的prototype属性的__proto__属性表示方法的基础，总是指向父类的prototype属性
