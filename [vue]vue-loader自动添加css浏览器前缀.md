
#### 修改vue-loader自动添加css浏览器前缀  
> build / vue-loader.conf.js
```js
module.exports = {
    postcss: [
        require('autoprefixer')({
          browsers: ['last 20 versions']  //向前兼容的版本
        })
    ]
}
```

同样可以在vscode中安装配置 **Autoprefixer** 插件添加css浏览器前缀 ( 支持 .css | .scss | .less 文件 )