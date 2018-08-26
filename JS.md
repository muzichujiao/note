# 对象序列化

# 深拷贝和浅拷贝

# 数组扁平化

# 数组去重

# 垂直居中

# flex布局

# 两栏布局

# promise

# 跨域
## 同源政策
协议、域名、端口相同，否则容易受到XSS、CSFR等攻击
## 同源政策限制
* Cookie/LocalStorage无法读取
* DOM和JS对象无法获取
* AJAX请求不能发送
## 跨域解决方法
### JSONP
由于script标签中src属性可以链接跨域的文件，所以通过创建script标签进行跨域请求
```
var script = document.createElement('script');
script.type = 'text/javascript';
//传入参数并且指定回调函数
script.src = 'http://www/domain.com:8080/login?user=admin&callback=onBack';
document.head.appendChild(script);
//执行回调函数
function onBack(res){
  alert(JSON.stringify(res));
}

服务端返回如下（返回时即执行全局函数）：
onBack({"status": true, "user": "admin"})
```
使用jQuery方法进行jsonp请求
```
$.ajax({
  url:'http:www.domain.com:8080/login',
  type:'get',
  dataType:'jsonp',
  jsonpCallback:'onBack',
  data:{}
  
})
```
jsonp请求缺点，只能实现get请求

### 跨域资源共享（CORS）
* 只需要服务端设置Access-Control-Allow-Origin即可，前端无需设置。
* 如果需要带cookie请求：前后端都需要设置字段
```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true; // 设置是否携带cookie
xhr.open('post',url,true);
xhr.send('user=admin');
xhr.onreadystatechange = function(){
  if(xhr.readystate == 4 $$ xhr.status == 200{
    alert(xhr.responseText);
  }
}
```


# js中String、Array、Math常用的方法
## Array
* sort(): 升序排序，调用每一项的toString()方法，比较得到的字符串
* reverse()
* indexOf()/lastIndexOf()
* forEach(): arr.forEach(function(item,index,arr){ })
* map():arr.map(function(item){ return...... }) // 对数组每一项给定函数，返回运行结果组成的数组
* every(): 判断数组每一项是否都满足条件，只有都满足才会返回true
```
var arr = [1,2,3,4,5];
var istrue = arr.every(function(x){
  return x<10;
});
console.log(istrue); // true
```
* some(): 判断数组中是否存在满足条件的项，只要有满足条件，就返回true
```
var arr = [1,2,3,4,5];
var istrue = arr.some(function(x){
  return x>4;
});
console.log(istrue); // true
```
## Math
* Math.celi(x): 向上取整
* Math.floor(x): 向下取整
* Math.round(x): 四舍五入
* Math.max([1,2,3,4]): 返回数值中最大值
* Math.min([]): 返回数值最小值
* 产生随机数：Math.floor(Math.random()*(max-min+1)+min) //返回min，max之间的随机数

# bootstrap实现栅格原理

# JS闭包

# JS内存泄漏

开放题；考察前端开发移动端与PC端的熟悉程度；
回答得分占比：
（1）浏览器兼容、手机端兼容及一些常见的兼容问题； 30%
（2）Js事件处理，click事件、Touch事件处理； 30%
（3）布局方面; 30%
（4）开发调试; 10%
（5）其它一些开发技巧。
