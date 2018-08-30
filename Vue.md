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

