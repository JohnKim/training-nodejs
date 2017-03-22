# Express 로 API 서버 개발

이번 장에서는 이전에 개발한 Express로 만든 웹서버에 간단한 Restful API 를 추가해 보도록 하겠습니다.

RESTful 에서의 REST 는 REpresentational State Transfer 의 약자로 일반적으로 HTTP URI 와 HTTP Method 의 구분자를 사용하여 API 를 호출하는것으로 HTTP URL 는 API 의 resource(자원)을 의미하고, HTTP Method 는 자원에 대한 행위를 지정하는 방식으로 동작합니다.

HTTP Method 는 CRUD(Create, Read, Update, Delete)에 해당하는 4가지가 사용됩니다.
* POST : Create
* GET : Read
* PUT : Update
* DELETE : Delete

테스트를 위하여 server.js 파일에 아래 코드를 추가 합니다.
```js
app.get('/', function (req, res) {
  res.send('Hello World!')
})

app.post('/', function (req, res) {
  res.send('Got a POST request')
})

app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})

app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```
