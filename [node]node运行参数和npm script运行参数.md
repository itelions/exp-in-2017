node 运行参数  
- 定义  
```js
node server.js --port:8080
```
- 获取
```js
process.argv
```  
  
npm script 运行参数  
- 定义  
```js
npm run dev --port:8080
```
- 获取
```js
JSON.parse(process.env.npm_config_argv).original
```