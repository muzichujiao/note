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
## HTTP服务器
### GET方法获取参数
```
/*获取get请求的url的参数*/
var http = require('http'),
	url = require('url'),
	querystring = require('querystring');

http.createServer(function(req,res){
	var pathname = url.parse(req.url).pathname;
	var canshu = url.parse(req.url).query;
	var param = querystring.parse(canshu);
	if(pathname == '/favicon.ico')
		return;
	console.log(pathname);
	console.log(canshu?canshu:'no');
	console.log(param);
	res.writeHead(200,{'Content-Type':'text/plain'});
	res.end('success');
}).listen(3001);
```
### POST方法获取
NodeJs为了使整个过程非阻塞，会将POST数据拆分成很多小的数据块，然后通过触发特定的事件，将这些小数据块有序传递给回调函数。request中有一个方法addListener，该方法有两个参数：'data'表示数据传输开始，'end'表示数据传输失败。
```
var http=require('http'),
	fs = require('fs'), //读取页面
	url = require('url'),
	querystring = require('querystring');

http.createServer(function(req,res){
	var pathname = url.parse(req.url).pathname;
	switch(pathname){
		case '/add' : resAdd(req,res);break;
		default : resDefault(res);break;
	}
}).listen(3000);

function resDefault(res){
	var path = __dirname + '/'+url.parse('index1.html').pathname;
	var readPath = fs.readFileSync(path);
	res.writeHead(200,{
		'Content-Type':'text/html'
	});
	res.end(readPath);
}
function resAdd(req,res){
	var data1='';
	req.setEncoding('utf8');
	req.addListener('data',function(postData){
		data1 += postData;
	});
	req.addListener('end',function(){
		var dataFin = querystring.parse(data1);
		console.log( data1);
		console.log(dataFin);
		res.writeHead(200,{
			'Content-Type':'text/plain'
		});
		res.end('success');
	})
}
```

## NodeJs静态资源管理
### 根据文件的后缀名，简单的处理几类文件
```
var http = require('http'),
	url = require('url'),
	fs = require('fs');

http.createServer(function(req,res){
	var pathname = url.parse(req.url).pathname;
	var path = __dirname+pathname;
	if(pathname == 'favicon.ico'){
		return;
	}
	else if(pathname == '/index' || pathname == '/'){
		var pathp = __dirname + '/' + url.parse('index2.html').pathname;
		var readPath = fs.readFileSync(pathp);
		res.writeHead(200,{
			'Content-Type':'text/html'
		});
		res.end(readPath);
	}
	else{
		dealStatic(pathname,path,res);
	}
}).listen(3000);
console.log('Server running at 3000……');

function dealStatic(pathname,path,res){
	 fs.exists(path,function(exists){
	 	if(!exists){
	 		res.writeHead(404,{
	 			'Content-Type':'text/plain'
	 		});
	 		res.end('未找到请求的文件');
	 	}else{
	 		//如果存在文件，就需要找出后缀名，设置返回类型
	 		var lastName = pathname.lastIndexOf('.'); //指定字符串出现的最后位置
	 		var type = pathname.substring(lastName+1); //获取文件类型
	 		var endType;
	 		switch(type){
	 			case 'css' : endType='text/css';break;
	 			case 'png' : endType='image/png';break;
	 			default : endType='text/plain';break;
	 		}
	 		fs.readFile(path,'binary',function(err,file){
	 			if(err){
	 				//500 表示服务器太忙
	 				res.writeHead(500,{'Content-Type':'text/plain'});
	 				res.end(err);
	 			}else{
	 				res.writeHead(200,{'Content-Type':endType});
	 				res.write(file,'binary');
	 				res.end();
	 			}
	 		})
	 	}
	 })
}
```
