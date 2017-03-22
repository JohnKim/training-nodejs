# Node.js

> Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://developers.google.com/v8/). Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open source libraries in the world.
>
> * [https://nodejs.org](https://nodejs.org)

Node.js 는 C++ 로 개발된 자바스크립트 엔진인 V8 을 기반으로 JavaScript 를 standalone 으로 실행할 수 있도록 하는 개발 환경입니다. 이전에는 JavaScript 는 당연히 웹브라우저에서 사용되는 스크립트로 여겨졌지만, Chrome 브라우저의 JavaScript 엔진을 웹브라우저가 아닌 단독으로 실행할 수 있게 되면서 JavaScript 가 더이상 웹브라우저에서만 동작하지 않고 standalone 으로 실행될 수 있게 된 것입니다.

> V8 is Google's open source high-performance JavaScript engine, written in C++ and used in Chromium, Node.js and multiple other embedding applications.V8 implements ECMAScript as specified in ECMA-262. and runs on Windows XP or later, Mac OS X 10.5+, and Linux systems that use IA-32, ARM or MIPS processors. **V8 can run standalone, or can be embedded into any C++ application.** V8 compiles and executes JavaScript source code, handles memory allocation for objects, and garbage collects objects it no longer needs. V8's stop-the-world, generational, accurate garbage collector is one of the keys to V8's performance.
>
> * [https://developers.google.com/v8](https://developers.google.com/v8)

Node.js 는JavaScript로 네트워크 또는 웹 서버를 개발하거나, 응용 어플리케이션을 개발할 수 있습니다. Node.js 의 가장 큰 장점 중 하나는 풍부한 에코시스템입니다. NPM\(Node.js Package Manager\) 로 수많은 모듈을 다운받아 사용할 수 있습니다.

Node.js 의 사례 중 일부를 아래와 같이 소개 합니다.

* 서버 어플리케이션
  * 웹페이지를 서비스 하는 서버, Restful API 서버 등
  * Express.js \([http://expressjs.com/\](http://expressjs.com/%29\)
* 응용 어플리케이션
  * OSX, Linux, Windows 에서 설치 및 실행되는 Desktop 어플리케이션
  * Electron \([https://electron.atom.io/\](https://electron.atom.io/%29\)
* 개발 지원도구
  * 웹개발 지원도구 : Bower \([https://bower.io/\](https://bower.io/%29\)
  * 모바일 개발 지원도구 : React Native \([https://facebook.github.io/react-native/\](https://facebook.github.io/react-native/%29\)

### Node.js Version

![](https://raw.githubusercontent.com/nodejs/LTS/master/schedule.png)최신 버전을 사용하는 것도 좋지만, 안정적인 운영을 위해서라면, LTS 버전을 선택하는 것이 좋겠습니다.

Node.js의 다양한 버전별 호환되지 않는 모듈을 사용하거나, 새로운 버전에 일부 동작하지 않는 경우가 발생하기도 합니다. 그러므로, 다양한 버전의 Node.js 설치하고 이를 관리하는 Node Version Manager \([https://github.com/creationix/nvm\](https://github.com/creationix/nvm%29\) 를 사용합니다. 다만 NVM 은 공식적으로 Windows 를 지원하지 않습니다.

### 간단한 웹 서버 개발 시작하기

Node.js 는 기본적으로 ES6을 지원합니다. ES6 문법에 대해서는 어렵지 않게 관련 문서를 찾아 학습 할 수 있습니다. \([http://es6-features.org/](http://es6-features.org/) 참조\)

간단한 웹서버 예제를 작성하고 실행해 보도록 합니다.  
`server.js` 파일을 생성하고 아래와 같이 작성합니다.

```js
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

```
$ node server.js
```

서버가 실행되었고, 웹브라우저에서 `localhost:3000` 으로 접속해서 확인합니다.

`http` 는 Node.js 에서 기본적으로 제공하는 기능으로, 내장 모듈이라고도 합니다.  
API 문서를 통해 제공되는 다양한 내장 모듈을 확인합니다.  
[https://nodejs.org/en/docs/](https://nodejs.org/en/docs/)

### Node.js 모듈 개발하기

Node.js 의 내장 모듈외에도, 개발자가 직접 모듈을 개발해 사용할 수 있습니다. 주로, 공통적인 기능을 모듈로 구현해 사용합니다.  
이러한 모듈을 사용자 정의 모듈이라고 부르며, exports 또는 module.exports 를 사용하여 개발합니다.

`hello.js` 파일을 생성하고, 아래와 같이 작성합니다.

```js
module.exports = function () {
  return 'Hello World';
}
```

앞에서 작성했던 `server.js` 파일을 아래와 같이 수정하여 직접 만든 hello.js 모듈을 사용해 봅니다.

```js
const http = require('http');
const hello = require('./hello');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end(hello());
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

만약 여러개의 함수를 제공하는 모듈을 만들기 위해서는 exports 를 사용합니다.

`hello2.js` 파일을 생성하고, 아래와 같이 작성합니다.

```js
exports.sum = function(a, b) {
  return a+b;
}

exports.avg = function(a, b) {
  return (a+b)/2;
}

exports.hello = function() {
  return 'Hello World';
}
```

그리고, hello2 모듈을 가져와 사용하도록 server.js 를 수정합니다.

```js
const http = require('http');
const hello = require('./hello2');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end(hello());
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```



