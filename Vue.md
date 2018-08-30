# Vue学习
## 组件
### 组件注册：全局注册/局部注册一个
### 数据传递：props / 通信 / slot
#### 父组件向子组件传值Props
##### props注意
* 子组件希望从父组件传递下来的数据
* 使用v-bind将动态的props绑定到父组件的数据，每当父组件数据发生改变时，子组件接收的数据也会发生改变。
* 在子组件中不能修改props中接收的值，如果需要修改：
  1. 可以定义一个局部变量，并用props值初始化它
  2. 定义一个计算属性用来处理props值，再返回处理之后的值
```
<div id="app">
		<div>
			<input type="text" v-model='msg' >
			<button v-on:click='submitmsg'>提交</button>
		</div>
		<ul>
			<todo-list 
				v-for='item in list'
				v-bind:pmsg='item'

			></todo-list>
		</ul>
	</div>
  
Vue.component('todo-list',{
			props:['pmsg'],
			template:'<li>{{pmsg}}</li>'
		})
		new Vue({
			'el':'#app',
			data:{
				'msg':'',
				'list':[]
			},
			methods:{
				submitmsg:function(){
					this.list.push(this.msg);
					this.msg='';
				}
			}
		});
```
##### props验证：
* 组件给别人使用时，确保组件正确使用，此时props值是一个对象
* type可以是：Number/String/Boolean/Object/Function/Array 也可以自定义

#### 子组件向父组件通信
* 由于vue规定只允许单向的数据传递，子组件向父组件通信是通过发出事件来改变数据
* Vue实例的事件触发器：$on() / $emit()事件沿作用域向上派发 / $dispatch()事件沿着父链冒泡 / $broadcast()事件向下传给所有后代
* 子组件可以使用v-on监听自定义事件


## 与服务端通信 vue-resource
### 介绍
* vue-resource是通过XMLHttpRequest或JSONP技术实现的异步加载服务端数据的插件
* 该插件提供了两种请求接口，一个是http请求，一个是RESTFUL架构请求接口
#### Http请求接口
* 一般http请求接口按照调用的便捷程度分为底层方法和便捷方法，便捷方法是对底层方法的封装。
* 全局调用和组件实例调用http请求
	1.  全局调用：Vue.http() / Vue.$http.get() / Vue.$http.post() / Vue.$http.jsonp()
	2. 组件实例调用 this.$http() / this.$http.get() / this.$http.post() / this.$http.jsonp()
* 全局调用和组件实例调用区别主要是全局调用方法中this指向window对象，而组件实例调用方法中指向组件实例
* 执行结果都返沪一个Promise对象，可以使用Promise语法来注册成功和失败的回调方法
##### 底层方法
他们都是根据options参数的method属性来判断请求方法是GET还是POST
```
new Vue({
	data:{},
	ready: function() {
		this.$http({
			url:'/http',
			method:'POST',
			data:{
				cat:'1'
			},
			header:{
				'Content-Type':'x-www-from-urlencoded'
			}
		}).then(function(res){
			//请求成功
		},function(res){
			//请求失败
		})
	}
})
```
##### 便捷方法
* 便捷丰富是对底层方法的封装
* get(url,data,options)
```
new Vue({
	ready:function() {
		this.$http.post(
			'/http',
			{
				cat:'1',
				name:'newbook'
			},{
				'headers':{'Content-Type':'x-www-form-urlencoded'}
			}
		).then(function(res){
			//请求成功
		},function(res){
			
		})
	}
})
```
* 在调用http请求方法时，this.$http(options),options参数对象的属性如下：
1. url
2. method 
3. data	 对于GET请求会将data中数据加到url上
4. params 用来替换url中的模板变量，模板变量中未匹配的直接加到url后面去
5. headers
6. jsonp jsonp请求中回调函数的名称
7. crossOrigin 表示是否跨域，如果没有设置该属性，内部会自动判断是否跨域

#### RESTful架构请求
* 通过http动词来表示对服务器数据的增删查改操作的
* 方法：Vue.resource()或者this.$resource(url,[params],[actions],[options])

#### Http请求接口和RESTful架构请求不同点：
* http请求是对数据的请求获取，对数据的改变是在服务端后端所进行操作的，他们知识获取数据
* RESTful架构请求是直接使用http动词直接对数据进行修改。
#### 跨域的AJAX请求
* 在以前AJAX是不支持跨域请求的，但是在XMLHttpRequest2规范中添加了一些特性来支持跨域的资源请求。
* 在进行跨域时，首先需要判断当前浏览器是否支持XMLHttpRequest2的特性，判断方法是使用in操作符检测XMLHttpRequest对象是否包含withCredentials属性，如果包含则表示支持。`var cros = 'withCredentials' in new XMLHttpRequest();`
* 支持情况下还需要在服务端启用跨域资源的请求，在响应头中添加允许跨域请求的域，`Access-Control-Allow-Origin:...`
* 服务端开启跨域资源支持后就可以提交AJAX跨域请求了，与普通的AJAX请求一样。


