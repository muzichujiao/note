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
