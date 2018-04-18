> 通过js创建script标签获取远程js文件  在onload事件中处理回调 

1. 需要在组件销毁前删除创建script标签  
1. 或者在组件创建时判断是否有该标签 避免反复创建

```js
var node1 = document.createElement('script'),
    node2 = document.createElement('script');
    
    node1.src = "/static/jquery-1.11.3.js",
    node2.src = "/static/jquery-ui-1.12.1/jquery-ui.js";
    
    node1.onload = function(){ 
        document.body.appendChild(node2)
    };
    
    node2.onload = function(){
        $( "#resizable" ).resizable();
    };
    
    document.body.appendChild(node1);
```

> 使用script.js引入远程js文件(推荐) 详细用法见[scriptjs](https://github.com/MissLoveWu/scriptjs)
    

```
npm install scriptjs --save //(安装)

import { $script } from 'scriptjs' //(引入)
```

> 在index.html中直接在头部引入 ( 适合全局都要引入的js文件 )