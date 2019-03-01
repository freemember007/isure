# isure

关于断言和验证，你不需要动则几百k的joi，数十k的validator，你需要的只是一个函数。isure是一个Gzip后仅0kb的极简断言/验证库(函数)，兼容浏览器与Node端，可广泛应用于浏览器表单验证，服务端请求参数验证。

```bash
npm install isure
# or
yarn add isure
```

1. 基本使用示例：

```javascript
/**
 * 登录接口
 */
const isure = require('isure')
module.exports = async (req) => {
  isure(req.query.hos_id, '医院ID不能为空')
  isure(req.query.dept_id, '科室不能为空')
  isure(req.query.pat_id, '患者ID不能为空')

  function yourSomeAwesomeFunc (foo, bar) {
    isure(typeof foo === 'number', 'foo必须为数字', 500) //服务端可传stateCode
    isure(foo > bar, 'foo必须大于bar', 500)
    // .... some logic
  }
  yourSomeAwesomeFunc(1, 2)

  return { user: {} }
}
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