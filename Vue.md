# Vue学习
## 计算属性
#### 特性
* 计算属性基于他们之间的依赖进行缓存的，只有当依赖发生改变时才会重新计算。
* 计算属性默认只有一个getter函数，但是你可以定义一个setter函数
```
new Vue({
	'el':'#app',
	data:{
		firstname:'Li',
		lastname:'Yu'
	}
	computed:{
		fullName:{
			//get
			get:function(){
				return this.firname+" "+this.lastname;
			}
			//set
			set:function(newValue){
				var names = newValue.splite(' ');
				this.firstname = names[0];
				this.lastname = names[1];
			}
		}
	}
})
```
## Class与Style的绑定
### Class绑定
#### 对象语法
* 我们可以给v-bind:class传递一个对象，用来动态切换class
```
<div class='root' v-bind:class='{on:isOn,active:isActive}'></div>
new Vue({
	'el':'#root',
	data:{
		isOn:true,
		isActive:false
	}
});
//渲染为
<div class='root on' ></div>
```
当isOn和isActive变化时，class列表相应的变化
* 可以绑定一个返回对象的计算属性
```
<div class='root' v-bind:class='compClass'></div>
new Vue({
	'el':'#root',
	data:{
		isOn:4,
		isActive:false
	},
	computed:{
		compClass:function(){
			return {
				'on':isOn>3 ? true:false,
				'active': isActive
			}
		}
	}
});
//渲染为
<div class='root on' ></div>
```
#### 数组语法
可以把一个数组传递给v-bind:class
```
<div class='root' v-bind:class='[isOn,isActive]'></div>
new Vue({
	'el':'#root',
	data:{
		isOn:on,
		isActive:active
	}
});
//渲染为
<div class='root on active' ></div>
```
### 内联样式绑定
#### 对象语法

#### 注意
**当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。**
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

### 动态组件和异步组件
#### 动态组件 keep-alive
当我们进行组件之间切换的时候通常使用is属性，但是如果我们想要保存这些组件的状态，以避免重复渲染



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

## 过渡
### CSS过渡
#### 应用过渡效果，需要在目标元素上使用transition特性，当插入删除的元素或是组件带有transition特性时，会执行以下操作：
* 自动探测目标元素是否有CSS过渡或者是动画，如果有会在合适的时候添加/删除CSS类名
* 看过渡组件是否提供了合适的JS钩子函数，这些钩子函数在适当的时候执行。
* 如果没有CSS过渡和JS钩子函数，DOM操作会在下一帧中立即执行。
#### 过渡的类名
在进入/删除的过渡中，有6个类名class的切换
* v-enter：定义进入过渡的开始状态，插入之前生效，在元素被插入的下一帧失效
* v-enter-active：定义进入过渡生效时状态，在整个进入的过渡状态中应用，在元素被插入前生效，完成后移除。可以用来定义过渡的时间、延迟和曲线函数。
* v-enter-to：定义进入过渡的结束状态，在元素插入后下一帧生效，与此同时v-enter移除，过渡完成之后移除
* v-leave：
* v-leave-active：
* v-leave-to：




