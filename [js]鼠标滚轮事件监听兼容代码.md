#### 鼠标滚轮事件监听兼容代码

```js
//添加滚轮事件监听
function addMouseWheelWatcher(ele,callback){
    if(document.attachEvent){ 
    	ele.attachEvent('onmousewheel',callback);  
    }
    if(document.addEventListener){ 
      ele.addEventListener('DOMMouseScroll',callback,false); 
      ele.addEventListener('mousewheel',callback,false); 
    }
}
//移除滚轮事件监听 
function removeMouseWheelWatcher(ele,callback){
    if(document.attachEvent){ 
    	ele.detachEvent('onmousewheel',callback);  
    }
    if(document.addEventListener){ 
      ele.removeEventListener('DOMMouseScroll',callback,false); 
      ele.removeEventListener('mousewheel',callback,false); 
    }
}
//获取滚轮上下滚动的状态(大于0为向上滚动)
//event.detail||event.wheelDelta
```

##### 参考资料  

[html中鼠标滚轮事件onmousewheel的处理方法](http://www.jb51.net/article/97033.htm)