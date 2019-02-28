# isure

关于断言和验证，你不需要动则几百k的joi，数十k的validator，你需要的只是一个函数。isure是一个Gzip后仅0kb的极简断言/验证库(函数)，兼容浏览器与Node端，可广泛应用于浏览器表单验证，服务端请求参数验证。

```bash
npm install isure
# or
yarn add isure
```

1. 基本使用示例：

```javascript
isure(req.query.hos_id, '医院ID不能为空')
isure((foo > bar), 'foo必须大于bar')
isure(foo.toString().match(/.../), 'foo格式不匹配') 
isure((typeof foo === 'number'), '函数bar的参数foo的类型必须为数字', 500) //服务端可传stateCode
```

2. 服务端错误捕获与处理建议：

```javascript
try {
    result = await handler(request)
    ... // finish your response
} catch (err) {
    response.writeHead(err.statusCode, {"Content-Type": "application/json charset=utf-8"})
    err.statusCode === 400 
      && response.write(JSON.stringify({ 
        message: `400请求错误，${err.message}`
      })) 
      || response.write(JSON.stringify({ 
        message: `500服务器错误，${err.message}`
      }))
    response.end()
}
```

3. 客户端错误捕获与处理建议：

```javascript
 window.onerror = (err) => {
   alert(err.message) // or use yourself's awesome toast UI
 }
```