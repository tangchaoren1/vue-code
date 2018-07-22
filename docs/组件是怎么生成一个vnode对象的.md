# 创建一个组件  createComponent
1.    在创建一个vnode的时候，会调用_createElement方法，其中有一段逻辑是对tag的判断，如果是一个普通的HTML标签，则会实例化一个普通VNode节点，否则通过_createElement方法创建一个组件VNode.


2.  针对组件vnode的渲染，主要就3个关键步骤：
	1. 构造子类构造函数
	
	2. 安装组件钩子函数
	3. 实例化vnode
	
###  构造子类构造函数
1. mergeOptions对options做了一层合并，把组件传入的option和vue实例上的option合并到vm.$options。


2. 调用vue.extend函数。作用就是构造一个vue的子类。如何构造的呢？利用非常经典的原型继承的方式把一个纯对象转换成一个继承于vue的构造器Sub并返回，然后对Sub这个对象本身扩展了一些属性，如扩展options，添加全局API等。并且对配置中的props和computed做了初始化工作。最后针对这个Sub构造函数做了缓存，避免多次执行vue.extend的时候，对同一个子组件重复构造。
3. 执行Sub的_init函数，实际上就是执行vue的init函数。执行初始化逻辑。


### 安装组件钩子函数
1. 虚拟DOM的snabbdom开源库在patch流程中对外暴露了各种时机的钩子函数，Vue也正是借用了这一点。在初始化一个component类型的vnode的过程中实现了几个钩子函数。


2. 实现钩子函数是在installComponentHooks 整个方法中。整个installComponentHooks的过程就是把componentVNodeHooks的钩子函数合并到data.hook中。vnode在执行patch的过程中会执行相应的钩子
### 实例化VNode
1. 这是组件解析成一个VNode 的最后一步，就是通过new VNode 实例化一个vnode并返回。
