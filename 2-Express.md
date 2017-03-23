# Express

> Fast, unopinionated, minimalist web framework for Node.js

Express 는 웹서버 및 API 서버를 개발할 수 있는 가장 많이 사용되고 있는 프레임워크 모듈입니다. Express 홈페이지에서 설명하고 있는 특징 몇가지를 살펴보면,

* **Web Applications**  
  Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.

* **APIs**  
  With a myriad of HTTP utility methods and middleware at your disposal, creating a robust API is quick and easy.

* **Performance**  
  Express provides a thin layer of fundamental web application features, without obscuring Node.js features that you know and love.

* **Frameworks**  
  Many popular frameworks are based on Express.

이제부터 Express 모듈을 사용하여 직접 웹서버를 개발해보도록 하겠습니다.

### 1. Repository 생성

Github 에서 New Repository 를 통해 프로젝트 리파지토기를 생성합니다.

![](/images/newRepository.png)

* Repository Name : `sample-server`
* Initialize this repository with a README 체크
* Add .gitignore : `Node`
* Add a license : `MIT`

위와 같이 리파지토리를 생성하면, 자동으로 `.gitignore`, `LICENSE`, `README.md` 파일이 생성됩니다.

이제 `git clone` 으로 로컬 PC 에 복재해 오도록 하겠습니다.

```
$ git clone https://github.com/[본인계정]/sample-server.git
$ cd sample-server
```

`.gitignore` 는 Repository 에 업로드하지 말아야 하는 파일 또는 폴더명을 설정하는 파일입니다. 여기에 기록된 파일 또는 폴더는 업로드 되지 않습니다. `http://gitignore.io` 에서 보다 쉽게 `.gitignore` 파일 내용을 만들 수 있습니다.

### 2. npm init

Node.js 기반의 어플리케이션 또는 모듈을 개발하기 전에 가장 먼저 해야 할 일은 `package.json` 파일을 만드는 것입니다. `package.json` 파일은 Node.js 기반의 모듈을 개발하고 NPM 에 배포 할 때 필요한 파일이기도 하고, 개발하고자 하는 어플리케이션이 사용하는 다양한 NPM 모듈 목록을 정의해 두게 됩니다. Node.js 로 만든 프로그램은 그 무엇이든지 NPM\([https://www.npmjs.com](https://www.npmjs.com)\) 저장소에 올려 공유할 수 있는 모듈이 될 수 있습니다.

package.json 파일을 생성하기 위하여 `npm init` 명령어를 실행합니다. 물론 직접 `package.json` 파일을 만들어도 되겠습니다.

```
$ npm init
```

생성된 `package.json` 파일은 아래와 같습니다.

```js
{
  "name": "sample-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/JohnKim/sample-server.git"
  },
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/JohnKim/sample-server/issues"
  },
  "homepage": "https://github.com/JohnKim/sample-server#readme"
}
```

### 3. Express 설치 및 사용

이제 `npm install` 명령어로 NPM 저장소에 있는 Express 모듈을 설치해보도록 합니다.

```
$ npm install --save express
```

Express 최신 버젼을 node\_modules 라는 폴더에 다운받았습니다. 이제부터 Express 모듈을 사용할 수 있습니다.  
`--save` 옵션은 모듈을 설치할때 모듈의 정보를 `package.json` 에 기록하도록 하는 것입니다. `package.json` 에 기록된 모듈 정보는 나중에 `npm install` 을 실행하면 자동으로 모든 모듈을 설치하게 됩니다.

웹서버를 구현 할 `server.js` 파일을 아래와 같이 작성합니다. 이 파일은 Node.js 의 내장 모듈인 `http` 를 사용하는 대신 Express 모듈을 사용하여 개발한 것입니다.

```js
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!')
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
});
```

`require` 로 모듈을 로딩할 때에는 가장 먼저 현재 폴더에서 express 모듈을 찾고, 없는 경우 node\_modules 폴더에서 express 모듈을 찾아 로딩하게 됩니다.

이제 실행해 보도록 합니다.

```
$ node server.js
```

실행시 확장자\(`.js`\)는 생략할 수 있습니다. 웹브라우저에서 `http://localhost:3000/` 으로 정상적으로 동작하는지 확인합니다.

이제 HTML 파일을 작성하여 서비스 해보도록 합니다.

테스트를 위해 `./public/test.html` 파일을 아래와 같이 간단하게 작성합니다.

```js
<html>
  <head>
    <title></title>
    <link href="/css/main.css" rel="stylesheet" type="text/css" />
    <script src="/js/main.js"></script>
  </head>
  <body>
    Hello World with Express !!
  </body>
</html>
```

`./public/css/main.css`

```java
  body { background-color : yellow; }
```

`./public/js/main.js`

```js
  console.log('Hello World');
```

`public`폴더에 생성한 static 파일을 서비스 하기 위해서는 `server.js` 에 아래와 같은 코드를 추가 합니다.

```js
...
app.use(express.static('public'))
...
```

`use` 함수는 Express 의 middleware 를 사용하는 함수 입니다. 위의 코드는 Express에서 제공하는 `express.static` 미들웨어를 사용하여 static 서비스를 위한 폴더명을 지정하는 것입니다.

### 4. Middleware 구현하기

`express.static` 와 같은 미들웨어를 직접 구현해 보도록 합니다. `server.js` 파일에 아래와 같은 미들웨어를 구현한 후 서버를 재시작하여 확인합니다.

```js
...
app.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})
...
```

이제 모든 요청에 대하여 현재 시간을 서버를 시작한 표준출력으로 보여지게 됩니다.

이외에도 Express 에서 기본으로 제공되는 Middleware 나 직접 Middleware 말고, 3rd party 에서 구현한 다양한 Middleware 도 사용할 수 있습니다.

[http://expressjs.com/en/resources/middleware.html](http://expressjs.com/en/resources/middleware.html)

### 5. Background 로 서버 실행하기

Node.js 기반의 서버를 실행하게 되면, 기본적으로 Foreground 로 실행되므로, 이번에는 Background 로 실행해 보도록 합니다.

NPM 에 등록되어 공유되고 있는 모듈중 `forever` \([https://github.com/foreverjs/forever](https://github.com/foreverjs/forever)\) 를 사용해 보도록 하겠습니다.  
A simple CLI tool for ensuring that a given script runs continuously

`forever` 는 Node.js 어플리케이션이 실행된 인스턴스를 관리하는 모듈로, 인스턴스가 중지되었을 때 자동으로 재시작해주는 기능이 포함되어 있습니다.

이번에는 npm 으로 모듈을 global 로 설치해보도록 합니다. global 로 설치 하게 되면, 모든 디렉토리에서 명령어를 사용할 수 있습니다.

```
$ npm install --global forever
```

global 로 모듈을 설치하게 되면, OS의 PATH 위치에 설치되어 모든 디렉토리에서 `forever` 명령어를 사용할 수 있습니다.

이제 forever 로 서버를 시작해 봅니다.

```
$ forever start server.js
$ forever list
```

forever 로 서버를 중지하도록 해봅니다.

```
$ forever stop 0
```



