# vue2-学习
### Vue学习笔记


# VUE模板
```
<template></template>    ----html
<script></script>		 ----js
<style></style>          ----css
```

# VUE实例
```
var vm = new Vue({
	el："#root"        ----挂载点
	data:{},            ----实例的属性数据
	methods:{},			-----实例的方法
	computed：{},        -----实例的计算属性
	watch:{}           -------侦听属性（不推荐：推荐使用计算属性代替）
})
```

# VUE模板 实例的书写规范
```
export default{
	name:''     	//模板实例的名字
	data:{},    	//模板实例的属性数据
	methods:{}, 	//模板实例的方法集合
	computed:{}, 	//模板实例点计算属性方法集合   
	watch:{}        //侦听属性（不推荐：推荐使用计算属性代替）
}
```





# 模板语法
*	文本
	>	Mustache语法 （双花括号）
	>	<span>Message:{{msg}}</span>
	>	注：双花括号只能用在标签中，不能用在标签属性中
	>	只能写单行语句（let a = 10就不可以）
```
{{mas}}
{{ "我是花括号文本" }}
{{ 1+1 }}
{{ 0>10 ? "true" : "false"}}
```

# 指令
* v-once : 能执行一次性的插值，当数据改变时，插值处点内容不会更新
	```
	<p v-once>{{name}}</p>
	```
* v-text : 只能解析纯文本
	```
	<div v-text="data.data1"></div>
	```
* v-html : 可以解析HTML(标签)
	```
	<div v-html="data.data2"></div>
	```
* v-bind ： 绑定标签属性
	```
	<p v-bind:class="{redClass:isMyClass,blueClass:!isMyClass}" v-bind:title="show">{{show}} 
	//简写 (直接省略v-bind) 	
		<p :class="{redClass:isMyClass,blueClass:!isMyClass}" :title="show">{{show}} 
	```
##	条件判断
* v-if   :	相当于if
	```
	<p v-if="show">我是v-if 为true 时能被看到</p>
	```
* v-else : 常与v-if连用
	```
	<p v-else>我是v-if 为false 时能被看到</p>
	```

##	监听DOM事件
* v-on 	 : 参数是监听点事件名称
	```
	<input type="button" value="改变颜色" v-on:click="changeColor">
	changeColor 定义在methods中（json格式）
	//简写 （将v-on简写成@）
		<input type="button" value="改变颜色" @:click="changeColor">
	```
##	修饰符
```
* .prevent    <==>  event.preventDefault()
	<form v-on:submit.prevent="onSubmit"></form>
```
## 计算属性

* 计算属性常用于模板内点简单计算，太多的逻辑反而使模板过重且难以维护
	```
	{{msg.split('').reverse().join('')}}   --不推荐
	```
	> 推荐写法
	```
	//计算属性
	computed:{
		//计算属性的 getter
		reverseName:function(){
			//this 指向当前 Vue 实例 
			return this.name.split('').reverse().join('')
		}
	},
	//调用计算属性
	<p>{{reverseName}}</p>
	```
	> 推荐写法2 （写成方法）
	```
	//方法
	methods:{
		changeColor:function(){
			this.isMyClass = !this.isMyClass
		},
		reverseName1:function(){
			return this.name.split('').reverse().join('')
		}
	}
	//调用计算属性方法（方法的调用模式）
	<p>{{reverseName1()}}</p>
	```
	> 方法与计算属性的区别：
		> 计算属性 ：是基于他们的依赖进行缓存的，只要相关依赖不改变，多次访问计算属性会立即返回之前的计算结果
		> 方法 ：是只要触发重新渲染时，调用方法 总会再次执行函数

## 侦听器（侦听属性）
* watch 
	> 功能：观察和响应 Vue实例上的数据变动（当一些数据需要随着其他数据变动而发生变动的时候）
	> 注: 官方并不推荐使用 watch,而推荐使用 计算属性代替侦听属性
	>推荐用法：
	```
	//计算属性
	computed:{
		fullName1:function(){
			return this.firstName + " " + this.lastName
		}
	},
	//调用计算属性
	<div>我是计算属性值{{fullName1}}</div>
	```
	> watch/computed 完整对比代码		
```
<template>
	<div class="testDemo">
		<p class="redClass">侦听属性</p>
		<div>我是firstName值{{firstName}}</div>
		<div>我是lastName值{{lastName}}</div>
		<div>我是侦听属性值{{fullName}}</div>
		<div>我是计算属性值{{fullName1}}</div>
		<input type="button" value="改变firstName" @click="changeFirstName">
	</div>
</template>
<script>
	export default {
		name:"democ1",

		data(){
			return {
				firstName: 'FirstName123',
				lastName:'lastName',
				//侦听属性需要给定默认值
				fullName:'FirstName lastName'
			}
		},
		//计算属性
		computed:{
			//计算属性不需要默认值，直接根据给定值计算
			fullName1:function(){
				return this.firstName + " " + this.lastName
			}
		},
		//方法
		methods:{
			changeFirstName:function(){
				return this.firstName = "changeFir"
			}
		},
		//侦听属性
		watch:{
			firstName:function(val){
				// val  <==> this.firstName
				this.fullName = val + " " + this.lastName
			},
			lastName:function(val){
				this.fullName = this.firstName + " " + val
			}
		}
	}
</script>
```
	> watch 优点：(未完待续)
		>	当需要在数据变化时执行异步或者开销较大的操作时，watch 选项提供了一个更通用的方法，来相应数据点变化(即常和axios连用)


## class和style绑定
* 因为class 和 style 都是属性，所以可以使用v-bind做处理

	### 绑定class 

	* 处理动态class 

	* 对象语法 v-bind:class="{class名:数据属性,'class名':数据属性}"
	> class名可以加引号或者不加引号

		* 单纯点为标签添加redClass这个class
		```
		<span class="redClass font24">我是v-bind:class</span>
		```
		* 将数据对象定义在内联模板里 
		> (redClass是否展示取决于 show是true还是false,font24/'text-danger'同理)
		```
		<span v-bind:class="{redClass:show,font24:isMyClass,'text-danger':show}">我是v-bind:class</span>
		data(){return{show:true,isMyClass:false}}
		```
		* 将class绑定在一个返回对象的计算属性
		```
		<span v-bind:class="classObject">我是v-bind:class</span>
		data() {return {show:true,error:null}}
		computed:{
			classObject:function(){
				return {
					redClass : this.show && !this.error,
					'text-danger':this.error && this.error.type === 'fatal'
				}
			}
		}
		```
	* 数组语法 
		* 为标签添加三目计算后的class
		```
		<div :class="[show ? 'redClass' :'blueClass','text-danger']">为div的class添加三目</div>
		
		> 当shou为true时 <div class="redClass text-danger">为div的class添加三目</div>
		> 当shou为false时 <div class="blueClass text-danger">为div的class添加三目</div>
		```
	* 用在组件上
		> 与用在普通标签上相同
		> 如果标签和组件上都有class且满足显示条件，那会一起显示


	### 处理内联样式 style
	* 对象语法 
	```
	v-bind:style="{color:activeColor,fontSize:fontSize + 'rem','background-color':'#eee'}"
		> data(){return{activeColor:'rgba(255,0,0,0.5)',fontSize:0.3}}
	```
	* 绑定样式对象
	```
	<div v-bind:style="styleObj">样式对象</div>	
	data(){retrun {sytleObj{color:'blue',fontSize:'0.4rem',lineHeight:'0.5rem'}}}
	```
	* 数组语法
		> 可以将多个样式对象绑定到同一个元素上
	```
	<div v-bind:style="[styleObj,styleObj1]">我是多个样式对象</div>		
	data(){return{ 
		styleObj{color:'blue',fontSize:'0.4rem',lineHeight:'0.5rem'},
		styleObj1{borderBottom:'0.02rem solid green'}
	 }}
	```
	* 多重值
		> 2.3.0+及以上才支持
		```
		<div :style="{display:['-webkit-box','-ms-flexbox','flex']}">我是style内联的多重值</div>
		 > 当浏览器支持不带前缀的flex时，内联样式显示为 <div style="display:flex">我是style内联的多重值</div>	
		```



### vue2中遇到的问题
> 关于 render:h => h(App) 和 components:{App} 的区别？
> > 注：App是组件
```
import App from "../componendts/App.vue";
//vue1.0写法
new Vue({
  el:"#root",
  router, //router使用
  components:{App},  //注册组件信息
  template:"App"     //调用组件模板的标签
})
//vue2.0写法
new Vue({
  el:"#root",
  router,
  render:h => h(App)  //调用组件（es6写法）
  /*
  * es5写法
  render:function(h){return h(App)}
  * 官方写法
  render:function(createElement) { renturn createElement (App)}
  * 箭头函数的this是[指向] :包裹this所在函数外面的对象上
  * h 是 createElement 的别名，createElement 是用来生成HTML DOM 元素 的脚本，
  * h（Hyperscript）来自 HTML (hyper-text markup language : 超文本标记语言)   
  * 是vue生态系统的通用管理
  */
})
```
