# ES6学习笔记

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
* 静态方法：不必创建实例就可以调用，一般通过类名进行调用，静态方法只能调用静态变量。所以类中定义的方法都会被实例继承，但是加上static的方法不会被实例继承，而是直接通过类调用。
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
