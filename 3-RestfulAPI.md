# Express 로 API 서버 개발

이번 장에서는 이전에 개발한 Express로 만든 웹서버에 간단한 Restful API 를 추가해 보도록 하겠습니다.

RESTful 에서의 REST 는 REpresentational State Transfer 의 약자로 일반적으로 HTTP URI 와 HTTP Method 의 구분자를 사용하여 API 를 호출하는것으로 HTTP URL 는 API 의 resource\(자원\)을 의미하고, HTTP Method 는 자원에 대한 행위를 지정하는 방식으로 동작합니다.

HTTP Method 는 CRUD\(Create, Read, Update, Delete\)에 해당하는 4가지가 사용됩니다.

* POST : Create
* GET : Read
* PUT : Update
* DELETE : Delete

테스트를 위하여 `server.js` 파일에 아래 코드를 추가 합니다.

HTTP Method 별 라우팅하는 함수는 `app.METHOD(PATH, HANDLER)` 입니다.

```js
app.post('/user', function (req, res) {
  res.send('POST (Create) ');
});

app.get('/user/:userId', function (req, res) {
  res.send('GET (Read) ');
});

app.put('/user/:userId', function (req, res) {
  res.send('PUT (Update) ');
});

app.delete('/user/:userId', function (req, res) {
  res.send('DELETE (Delete) ');
});
```

개발한 서버에 직접 API 를 호출하여 테스트 하기 위해서 API 테스트 도구를 활용하면 어렵지 않게 할 수 있습니다.

* [https://advancedrestclient.com/](https://advancedrestclient.com/)
* [https://www.getpostman.com/](https://www.getpostman.com/)

Chrome 앱 또는 Desktop 어플리케이션으로 설치하여 HTTP Method 를 변경해가면 테스트 할 수 있습니다.

### request.params

URI 에서 포함된 대입된 값을 가져오기 위하여 request.params 를 사용할 수 있습니다.

예를 들어  
`[GET]`[`http://localhost:3000/user/13579`](http://localhost:3000/user/13579) 를 호출하여 User ID 가 13579 인 사용자 정보를 조회하는 경우라면, '/user/:userId' 의 userId 는 request.params.userId 로 가져올 수 있습니다.

```js
app.get('/user/:userId', function (req, res) {

  console.log(req.params.userId + '의 정보를 가져옵니다');

  // TODO 실제로 DB 에서 userId 에 해당하는 사용자 정보를 가져오는 로직을 개발해야 함
  var user = {
    userId: 13579,
    name: 'John',
    email: 'yohany_AT_gmail.com',
    company: 'KossLAB'
  };

  res.send(user);
});
```

### body-parser 모듈

POST 요청 처리시 데이터를 JSON 형식으로 파싱하여 req.body 를 통해 사용할 수 있도록 해주는 NPM 모듈입니다.

모듈 설치를 위하여 아래와 같이 합니다.

```
$ npm install --save body-parser
```

그리고 server.js 에서 미들웨어를 추가 설정하고 PUT 메소드 처리 부분을 아래와 같이 수정해서 테스트 합니다.

```js
var bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({ extended: false }));

...

app.post('/user', function (req, res) {

  console.log('데이터 확인', req.body);

  // TODO 실제로 DB 데이터를 저장하는 로직을 개발해야 함.

  res.send({state: 'OK', data: req.body});
});
...
```

PUT, DELETE 역시 같은 방식으로 데이터를 전달 받을 수 있습니다.

### request.query

`[GET] http://localhost:3000/user/search?name=John` 과 같이 검색기능은 일반적으로 `GET` 으로 Query 파라미터를 사용합니다.

Query 파라미터를 받아 사용하는 부분은 아래와 같이 할 수 있습니다.

```js
app.get('/user/search', function (req, res) {

  console.log('데이터 확인', req.query.name);

  // TODO 실제로 DB 데이터를 조회하는 로직을 개발해야 함.

  var user = {
    userId: 13579,
    name: 'John',
    email: 'yohany_AT_gmail.com',
    company: 'KossLAB'
  }

  res.send(user);

});
```



