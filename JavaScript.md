# JS合集
## 重点
### 1.数据类型
- null、undefined、string、number、bool、symbol(独一无二的值)
- object

### 2.数据隐式转化
- `console.log([1,2] + 4)  // 1,24`
- `console.log([1,2] - 4)  // NaN`
- `console.log([1,2] + [3,4])  // 1,23,4`
- `console.log([] + {})  // [Object Object]`
- `console.log({} + 1)  // [Object Object]1`
- `console.log(undefined + 1) // NaN`
- `console.log([] == []) // false`
- `console.log(null > 0) // false`

### 3. 如何判断数据类型
- Object.prototype.toString().call()
- instanceOf Array
### 4. var、let、const区别
- **var**
    * 不存在块级作用域；
    * 存在变量提升
    * 可重复声明
- **let\const** 
    * 存在块级作用域
    

### 5. 延迟加载js的方法
- defer ： 立即下载，待页面解析完成后执行，顺序执行
- async ： 下载完成立即执行，不保准顺序执行
- 延迟加载js有助于提高页面加载速度
### 6. Null\undefined区别
- Null ：空的对象，Number(null) 为 0
- undefined ：为空的值，只定义了，但是未初始化，Number(undefined) 为 NAN
### 7. 事件循环机制
- **执行过程**
    * js是单线程的，同一个时间只能执行一个事件
    * js中将任务分为两种，同步任务和异步任务，同步任务执行完成才会执行异步任务
    * 同步代码直接进行主线程执行，宏任务进入宏任务队列，微任务进入到微任务队列
    * 主线程内任务执行完成后，检查微任务列表，如果有依次进入主线程执行，直到全部执行完成
    * 之后就会去检查宏任务列表，依次执行宏任务
- 微任务： promise.then / process.nextTick
- 宏任务： setTimeout / setInterval
- **延时队列：用于存放计时器到达后的回调任务**



### 8. 深拷贝和浅拷贝
- 浅拷贝 
    * Object.assign
- 深拷贝 
```
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            //判断ojb子元素是否为对象，如果是，递归复制
            if(obj[key]&&typeof obj[key] ==="object"){
                objClone[key] = deepClone(obj[key]);
            }else{
                //如果不是，简单复制
                objClone[key] = obj[key];
            }
        }
    }
    return objClone;
}    
```

### 9. 数组上的方法
- pop / push / shift / unshift / concat / splice / slice / indexOf / find / reverse / sort / join /forEarch
- includs 返回查找的元素在数组的位置，找到返回true
- some 遍历每一项，只要有一项满足条件，返回true
    ```
        let is = arr.some((item,index) => item > 2);
    ```
- every 遍历每一项都要满足条件，返回true
- filter
- map 每一项运行回调函数，返回由每次调用结果组成的数组
    ```
    let numbers = [1,2,3,4,5];
    let result = numbers.map((item,index) => item*item);
    console.log(result) // [1,4,9,16,25]
    ```
- reduce：用于归类的方法，常用的场景就是求和
    * 第一个参数：是一个函数，每个项都会执行，接收四个参数：上次执行的返回值、当前项、索引、数组对象，返回的值会传给下次遍历的第一个参数
    * 第二个参数：归并的初始值
    * 求数组每项的和
    ```
        arr.reduce((pre,item) => pre + item ,0)
    ```
    * 计算每个元素出现的次数
    ```
        let arr = ["a","b","c","q","a","w","q","w","s","b"];
        arr.reduce((pre,item) => {
            if(item in pre){
                pre[item]++;
            }else{
                pre[item] = 1;
            }
            return pre;
        },{})
    ```

### 10.数组扁平化
```
function flatten(arr) {
  return arr.reduce((pre, current) => {
    return pre.concat(Array.isArray(current) ? flatten(current) : current)
  }, [])
}
```
### 11. 数组去重
- 方法一 ：`let uniqueArr = [... new Set(arr)]`
- 方法二
```
    let uniqueArr = arr.reduce((pre,item,index) => {
        pre.includes(item) ? pre : pre.push(item);
        return pre;
    },[])
```

### 2. call、apply、bind函数内部实现
- 三者都是改变函数this的指向
- call、apply都是立即执行，bind返回的是一个函数，需要调用执行
- apply第二个参数是数组，call和bind参数是顺序传入
- 手写bind
```
    Function.prototype.myBind(content,...rest){
        let newThis = content || window;
        newThis.fn = this;
        return function (){ //返回一个函数
            let result = newThis.fn(...rest);
            delete newThis.fn;
            return result;
        }
    }
```
- 手写call
```
    Function.prototype.myCall = function(content,...rest){
        let newThis = content || window;
        newThis.fn = this;
        let result = newThis.fn(...rest);
        delete newThis.fn;
        return result;
    }
```
- 手写apply
```
    Function.prototype.myApply = function(content,arr = []){
        if(!Array.isArray(arr)) throw "参数必须是数组";

    }
```

### 3. new操作符具体干了什么事情，new的原理是什么？通过new的方式创建对象和通过字面量创建有什么区别？
- 创建了一个空的对象，this引用了这个对象，且继承了函数的原型
- 属性和方法被添加到this应用的对象上
- 隐式的返回this
### 16. 闭包
- 什么是闭包
    * 闭包是能够访问函数内部变量的函数
    * 可以避免全局变量的污染
    * 希望一个变量长期存储在内存中（缓存变量）
- 缺点
    * 内存泄漏
    * 增加内存使用
- 应用场景
    * 封装一些功能的时候会用到，比如：函数防抖、节流
    * 需要使用到私有的属性和方法


### 6. AMD、CMD
- CommonJS规范 同步加载模块
    * nodejs目前使用它
    * 通过require方法加载模块，通过exports或者module.exports导出模块
    * 特点：
        1. 适用于服务端，因为模块都放在服务器端，对于服务端来说模块加载较快，不适合在浏览器环境中使用，因为同步意味着阻塞加载
        2. 所有代码都运行在模块作用域，不会污染全局作用域
        3. 模块可以多次加载，但只会在第一次运行，结果被缓存，之后去缓存中读取
        4. 按照出现的顺序加载

- AMD 异步加载模块
    * requireJS使用
    * 采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。推崇**依赖前置**
    * 模块必须采用define函数定义
    * 加载方法
    ```
    require(['math'], function (math) {
        math.add(2, 3);
    });
    ```
    * 特点：
        1. 异步并行加载，不会阻塞dom渲染
        2. 推崇依赖前置，也就是提前执行（预执行），在模块使用之前就已经执行完毕。
- CMD 通用模块加载
    * 与依赖的模块执行时机不同，推崇就近依赖
    * 按需执行，异步加载
- UMD 是AMD和commonjs融合
    * 先判断是否支持Node.js模块（exports是否存在），存在则使用Node.js模块模式。
    * 再判断是否支持AMD（define是否存在），存在则使用AMD方式加载模块。
    * 前两个都不存在，则将模块公开到全局（window或global）。

- ES6模块化
    * 编译时加载，异步加载



### 8. 函数节流和防抖
- 函数防抖（debounce）
    * 当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。





### 10. js原型、原型链；prototype 和 proto 区别是什么？
- ​ 当访问一个对象的某个属性时，会先从该函数本身属性上找，如果没有找到，则会在他的 _ proto _隐式属性（即构造函数的prototype）上去找，如果还没有找到的话，则就会再去prototyep的 _ proto _ 中去找，这样一层一层向上查找就会形成一个链式结构，就称之为原型链。

### 11. 取数组最大值

### 12. promise的状态，优缺点？如何实现Promise.all()和Promise.finally()



### 14. cookie与storge区别





### 17. 跨域
- 浏览器的同源策略，协议、端口、域名必须相同
- 跨域资源共享(CORS)
- nginx代理
- jsonp跨域 ： 是通过动态创建script标签，回调函数的方式实现的
- nodejs中间件代理




## 一般问题
### 1. 如何阻止冒泡和默认事件
- 阻止冒泡 `e.stopPropagation()`
- 阻止默认 `e.preventDefault()`
### 1. js垃圾回收机制

### 2. js实现继承的方法

### 3. 见过哪些设计模式

### 4. 常见web安全及防护原理


### 5. 为什么需要同源限制


### 6. 为什么console.log(0.1+0.2 == 0.3) 为false

### 7. JS事件委托是什么


### 8. 如何判断img加载完成


### 9. websocket和socket区别


### 10. 常见状态码

### 11. 跨域问题解决方法


### 12. 文件上传如何做断点续传
- **前端**
    * 核心是利用Blob.prototype.slice方法对源文件进行切割
    * 预先设置好最大的切片数量
    * 借助http的可并发性，同时上传多个切片
    * 由于是并发，传输到服务端的顺序会发生变化，需要给每一个切片记录顺序
- **后端**
    * 前端需要在每个切片中记录最大的切片数量，服务端获取到所有的切片后自动合并
    * 可以使用nodejs中的api fs.appendFileSync()，它可以同步的将数据追加到指定的文件中。


### 13. 封装一个异步加载图片的方法


### 14. 使用promis实现红绿灯交替重复亮


### 15. 使用promise实现每隔1s输出1、2、3


### 16. 防抖和节流区别及实现

