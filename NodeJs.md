# NodeJS学习笔记
## 模块
模块分为两类：原生模块和文件模块，原生模块是NodeJs提供的模块，它在启动的时候已经被加载，文件模块是通过原生模块来实现和完成的，加载的时候需要调用require
方法来实现加载
### 原生模块的调用
使用require来加载相应的模块，加载成功后悔返回一个模块对象，该对象拥有该模块的所有属性和方法。比如：http模块
```
var http = require('http');
http.createServer(function(req,res){
  res.writeHead({'Content-Type':'text/plain'});
  res.end('Hello');
}).listen(3000,'127.0.0.1');
consoe.log('Server running at http://127.0.0.1:3000');
```
### 文件模块调用
文件模块调用与原生模块调用方式是一样的，但是在加载上有不同，原生模块不需要指定模块路径，但是文件模块需要指定模块路径，并且在文件模块中，只有通过exports和
module.exports对象将属性和方法暴露给外部的属性和方法，才能通过返回的rquire模块对象进行调用，其他方法和属性无法获取。 
