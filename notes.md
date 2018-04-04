# overflow
* visible:内容不会被修剪，会显示到外面
* hidden：超出的内容被隐藏
* scroll：不管内容是否超出，都有滚动条
* auto：有超出内容才会有滚动条
* inherit：继承父元素的overflow值

# 立即执行函数this指针
```
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log(this.foo);   // bar
        console.log(self.foo);   // bar
        (function() {
            console.log(this.foo);  // undefined
            console.log(self.foo);  // bar
        }());
    }
};
myObject.func();
```
* 第一个输出：this指针指向myObject
* 第二个输出：self是this指针的副本，同样指向myObject
* 第三个输出：立即执行函数中this指针指向window
* 第四个输出：在立即执行函数中没有self，所以通过作用域链向上查找，self指向myObject

# 基本数据类型和复杂数据类型存储
* 基本数据类型存在栈中，引用数据类型存在堆上，但是会在栈上存储一个地址，这个地址就是指向这个对象在堆上的起始位置，当查找这个对象时，先从栈中找出对象在堆上的地址，再通过地址从堆上查找。
* 基本数据类型使用8Byte的内存存储基本数据类型值。

# 排序 
## 快排
```
function quickSort(arr){
  if(arr.length<=1){
    return arr;
  }
  var index = Math.floor(arr.length/2);
  var mid = arr.splice(index,1);
  var left=[],right=[];
  for(var i=0;i<arr.length;i++){
    if(arr[i]>mid)
      right.push(arr[i]);
    else
      left.push(arr[i]);
  }
  return quickSort(left).concat(mid,quickSort(right));
}
```
## 冒泡
```
function maopao(arr){
  for(var i=0;i<arr.length;i++){
    for(var j=i+1;j<arr.length;j++){
      if(a[i]>a[j])
        swap(a[i],a[j]);
    }
  }
  return arr;
}
```
# JS全局函数
* decodeURI():解析编码某个URI
* decodeComponent():解码一个编码的URI组件
* ecodeURI():将字符串编码为URI
* ecodeComponent():将字符串解析为URI组件
* escape()：对字符串进行编码
* eval():参数为一个字符串，将字符串转化为js代码执行
* getClass()	返回一个 JavaObject 的 JavaClass。
* isFinite()	检查某个值是否为有穷大的数。
* isNaN()	检查某个值是否是数字。
* Number()	把对象的值转换为数字。
* parseFloat()	解析一个字符串并返回一个浮点数。
* parseInt()	解析一个字符串并返回一个整数。
* String()	把对象的值转换为字符串。
* unescape()	对由 escape() 编码的字符串进行解码。
# 字符串'+'和加法符号'+'
字符串连接符'+'优先级高于加法运算符'+'
```
console.log(1+ "2"+"2");
console.log(1+ +"2"+"2");
console.log("A"- "B"+"2");
console.log("A"- "B"+2);
```
* 遇到'+'前后有字符串的，是做字符串连接，所以第一个是‘122’
* 第二个'+'，即+'2'是一元操作符，将字符串'2'变为数字，所以单价于3+'2',结果为'32'
* 第三个，在遇到'-'时，先将转化为数值，结果不能转化，故为NAN，再进行字符串连接运算，结果为'NAN2'
* 第四个，为NAN

