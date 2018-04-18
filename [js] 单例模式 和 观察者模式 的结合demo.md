1. 使用单例模式统一管理页面
2. 使用观察者模式提高单例模式扩展性
- Page.js
```js
function Page(option) {
	this.customEvents = {};
	this.init(option)
}

Page.prototype.init = function(option) {
	option && Object.assign(this, option);
	// 根据option初始化的结果 设置初始化状态
	var initInfo = { status: 'success' }
	if (!this.dom) initInfo = { status: 'warming', 'message': 'option.dom is undefined' };

	// 对Page.data中的数据创建监听
	initData(this);

	// 初始化完成时触发自定义事件inited 返回初始化的结果
	var _this=this
	setTimeout(function() {
		_this.trigger('inited', initInfo);
	})
};
//使用uniqueId作为标识将callback绑定customEvent上
Page.prototype.listen = function(customEventName, uniqueId, callback) {
	if (!this.customEvents[customEventName]) this.customEvents[customEventName] = {};

	if (this.customEvents[customEventName][uniqueId]) {
		console.error('uniqueId ' + uniqueId + ' already defined,please use Page.off(uniqueId) to unbind')
	}

	this.customEvents[customEventName][uniqueId] = callback.bind(this);
};
//触发指定customEvent 执行绑定到该customEvent下的所有callback
Page.prototype.trigger = function(customEventName, payload) {
		if (!this.customEvents[customEventName]) return;
		for (var x in this.customEvents[customEventName]) {
			this.customEvents[customEventName][x](payload);
		}
	}
	//根据指定uniqueId解除对应的callBack对某个customEvent的绑定
Page.prototype.off = function(uniqueId) {
	for (var x in this.customEvents) {
		for (var y in this.customEvents[x]) {
			if (this.customEvents[x][y]) {
				delete this.customEvents[x][y];
				return
			}
		}
	}
}

function initData(app) {
	// 遍历app对象的data属性 创建访问器
	Object.keys(app.data).forEach(function(key) {
		Object.defineProperty(app, key, {
			//访问器拦截对变量的获取 获取指定的结果
			get: function() {
				return this.data[key];
			},
			//访问器拦截对变量的修改 可以在此完成与数据对应的DOM渲染 也可以设置回调
			set: function(val) {
				var newVal = val;
				var oldVal = this.data[key];
				if (newVal == oldVal) return
				this.data[key] = newVal;
				// 同步监听回调
				this.trigger && this.trigger(key, { newValue: newVal, oldValue: oldVal });
			}
		})
	})
}
```

- demo.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script type="text/javascript" src="Page.js"></script>
</head>
<body>
	<div id="result0"></div>
	<div id="result1"></div>
	<script type="text/javascript">
	var _page=new Page({
		data:{
			num1:0,
		}
	})
	
	_page.listen('inited','inited_1',function(){
		document.getElementById('result0').innerText=this.num1;
		document.getElementById('result1').innerText=this.num1;
	})

	_page.listen('num1','num1_0',function({oldValue,newValue}){
		// console.log(oldValue,newValue);
		setTimeout(_=>{
			document.getElementById('result0').innerText=newValue;
		},1000)
	})
	_page.listen('num1','num1_1',function({oldValue,newValue}){
		// console.log(oldValue,newValue);
		setTimeout(_=>{
			document.getElementById('result1').innerText=newValue;
		},2000)
	})
	setTimeout(_=>{
		_page.num1=2
	},1000)
	</script>
</body>
</html>
```
