## Ajax
```
function httpreq(method,url,is){
  var xhr = null;
  if(window.XMLHttpRequest){
    xhr = new XMLHttpRequest();
  }else if(window.ActiveeXObject){
    xhr = new ActiveXObject("Microsoft.XMLHTTP");
  }
  if(xhr!=null){
    xhr.open(method,url,is);
    xhr.onreadyStateChange = function(){
      if((xhr.readyState == 4) && ((xhr.status>=200&&xhr.status<300)||xhr.status==304)){
        return xhr.responseText;
      }
      
    };
    xhr.send(null);
  }
  else{
    console.log("不支持AJAX");
  }

}
```

## 函数柯里化
```
console.log(add(1,2)(3)(4));
function add(){
  let sum = 0;
  sum = [].slice.call(arguments).reduce(function(a,b){return a+b;},sum);
  var _add = function(){
    if(arguments.length==0){
      return sum;
    }else{
      sum = [].slice.call(arguments).reduce(function(a,b){return a+b;},sum);
      return _add;
    }
  };
  _add.toString=function(){
    return sum;
  };
  _add.valueOf=function(){
    return sum;
  }
  return _add;
  
}
```

## undefined && null
### undefined
* 表示一个空值，访问未初始化的变量、缺省的参数、不存在的属性都会返回undefined
* 从null演变而来，null表示‘没有对象’，与c中null相同，转化为数字时为0，但是不想让值有明确的指向，因为不仅仅是对象，而且强制类型转化不应该为0.因此，定义了undefined类型，强制类型转化为**NAN**。
* 强制类型转化为boolean为false
### null
* 表示“没有对象”
* 强制类型转化为数字为0.强制类型转化为boolean为false
### 强制类型转化
#### 转化为boolean值Boolean()：
 * undefined/null/NaN/0/''/false 都转化为false
#### 转化为数字Number():
* null为0 / undefined为NaN / false为0 / true为1 
#### valueOf() && toString()
* valueOf(): 返回的是对象或者数组本身，如果是一个Data类型调用，返回的是毫秒数
* toString(): 返回的是字符串，如果是对象调用，返回的是“[object Object]”

## typeOf 和 instanceOf
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


## js中String、Array、Math常用的方法
### String
* String.prototype.charAt(pos):获取字符串第pos位置的字符（从0开始） 例：`'abc'.charAt(1); //b`
* String.prototype.slice(start,end?):截取字符串，从start开始(包含),到end结束(不包含)
* String.prototype.substring(start,end?)与slice()方法相同
* String.prototype.splite(sep):分割字符串
* String.prototype.trim():去除字符串开头和结尾的空格
* String.prototype.concat(str1,str2,str3,...):连接字符串
* toLowerCase():字符串转化为小写
* toUpperCase():字符串转化为大写

### Array
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
### Math
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
