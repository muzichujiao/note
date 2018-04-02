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

