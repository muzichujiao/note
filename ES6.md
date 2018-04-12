# note

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
* this默认指向类的实例

