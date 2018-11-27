# React学习
## 虚拟DOM
真实页面对应DOM树，每次更新页面都需要手动操作DOM更新，DOM更新操作昂贵。React把真实DOM转化为*JS对象树*，也就是虚拟DOM。每次跟新数据，重新计算虚拟DOM，并和上一次生成的虚拟DOM做对比，将发生变化的部分进行批量更新。

## props
* props值不能改变，一定来自于默认属性或者是父组件传递过来的。
* React为props提供了默认配置，通过defaultProps静态变量的方法来定义，当组件调用时，默认值保证渲染后始终有值
```
static defaultProps = {
  color:'red',
  value:'hi'
}
```
* propTypes用于规范props的类型与必须的状态，会对组件的props进行类型检查，如果传入的props不匹配，在开发环境下会报warning。
```
static propTypes = {
  color:React.PropTypes.string,
  value:React.PropTypes.string
}
```

## 组件的生命周期
React组件生命周期分为两类：
* 组件挂载和卸载
* 组件的更新
##### 组件挂载
* componentWillMount：在render()之前执行
* componentDidMount：在render()之后执行
##### 组件卸载
* componentWillUnmount：进行事件回收或者清除定时器
##### 组件更新
* shouldComponentUpdate：`shouldComponentUpdate(nextProps,nextState)//接收需要更新的props和state` 开发者在里面增加判断条件，让其在需要时更新，不需要时不更新，当返回false时，组件不会向下执行生命周期函数
* componentWillUpdate
* componentDidUpdate
