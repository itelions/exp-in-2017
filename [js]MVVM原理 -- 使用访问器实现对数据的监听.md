## [js]MVVM原理学习 -- 使用访问器实现对数据的监听

> 模仿vue数据结构的demo ( 不包含dom节点的绑定 )

参考资料 : [Vue.js双向绑定的实现原理](http://www.cnblogs.com/kidney/p/6052935.html?utm_source=gold_browser_extension)

```js
const App={
    // 需要创建访问器的数据
	data:{
		text:'测试',
		message:'old message'
	},
	// 方法
	methods:{
		sendMessage(){
			console.log('发送了'+this.message)
		}
	},
	// 监听回调
	watch:{
		message(newVal,oldVal){}
	}
}

createApp(App,document.getElementById('App1'));
App.message='new message';

function createApp(app,node) {
    // 遍历app对象的data属性 创建访问器
    Object.keys(app.data).forEach(function(key) {
    	Object.defineProperty(app,key,{
    	    //访问器拦截对变量的获取 获取指定的结果
    		get:function(){
    			return this.data[key];
    		},
    		//访问器拦截对变量的修改 可以在此完成与数据对应的DOM渲染 也可以设置回调
    		set:function(val){
    			var newVal=val;
    			var oldVal=this.data[key];
    			if(newVal==oldVal)return
    			this.data[key]=newVal;
    			// 同步监听回调
    			this.watch[key]&&this.watch[key](newVal,oldVal);
    		}
    	})
    })
}
```
