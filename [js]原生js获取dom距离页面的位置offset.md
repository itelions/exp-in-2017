
#### 获取dom节点相对页面的位置offset

```js
//获取元素相对于html的位置
function getEleOffset(ele) {
	if(ele==document)return null
	 var elePosition=getStyle(ele,'position');
	 var targetNode=ele;
	 if(ele==document.body){
	 	return {
	 		top:getStyle(ele.parentNode,'padding-top',true)+getStyle(ele.parentNode,'border-top-width',true)+getStyle(ele.parentNode,'margin-top',true)+getStyle(ele,'margin-top',true),
	 		left:getStyle(ele.parentNode,'padding-left',true)+getStyle(ele.parentNode,'border-left-width',true)+getStyle(ele.parentNode,'margin-left',true)+getStyle(ele,'margin-left',true)
	 	}
	 }
	 if(elePosition!=='fixed'){
	 	while(targetNode!=document.body&&targetNode.parentNode!=document.body){
	 		targetNode=targetNode.parentNode
	 	}

	 	return {
	 		top:targetNode.offsetTop+getStyle(targetNode,'border-top-width',true)+ele.offsetTop,
	 		left:targetNode.offsetLeft+getStyle(targetNode,'border-left-width',true)+ele.offsetLeft
	 	}
	 }else{
	 	return {
	 		top:ele.offsetTop,
	 		left:ele.offsetLeft
	 	}
	 }
}
//获取元素样式兼容代码
function getStyle(ele,style,isNumber){
	var theStyle=window.getComputedStyle ? window.getComputedStyle(ele,null)[style] : ele.currentStyle[style];
	if(isNumber)return parseFloat(theStyle.replace('px',''));
	return theStyle;
}
```