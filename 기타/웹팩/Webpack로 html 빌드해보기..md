
yarn init -y 이후 

```cli
$ yarn add -D @babel/core @babel/preset-env @babel/preset-react babel-loader clean-webpack-plugin css-loader html-loader html-webpack-plugin mini-css-extract-plugin node-sass react react-dom sass-loader style-loader webpack webpack-cli webpack-dev-server
```

위의 패키지들을 설치, 이유는 후에 서술

### 웹팩으로 자바스크립트 파일 필드
src 디렉터리를 만들고 test.js로 다음과 같이 작성하기.

```js
console.log("test");
```

이후 최상위 디렉터리로 이동해 webpack.config.js를 작성해준다.

entry는 웹팩이 빌드할 파일을 알려주는 역할을 함.
src/test.js 파일을 기준으로 import 되어있는 모든 파일들을 찾아서 하나의 파일로 합치게 된다

output은 웹팩에서 빌드를 완료하면 output에 명시되어 있는 정보를 통해서 빌드 파일을 생성함.
mode는 웹팩 빌드 옵션임, production은 최적화되어 빌드되어지는 특징을 가지고 있고
development는 빠르게 필드하는 특징, none 같은 경우에는 아무 기능없이 웹팩으로 빌드한다.



```js
const path = require("path");

module.exports = {
  entry: "./src/test.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname + "/build")
  },
  mode: "none"
};
```

이후 package.json에 build 스크립트를 작성하고 확인해보자.

```js
"scripts": {
"build": "webpack"
},

/******/ (() => { // webpackBootstrap
var __webpack_exports__ = {};
console.log("test");
/******/ })()
;
```

### 웹팩으로 HTML 빌드해보기
웹팩은 자바스크립트 파일뿐만 아니라 자바스크립트가 아닌 파일들도 모듈로 관리할 수 있다.
어떻게 자바스크립트가 아닌 파일들도 관리할 수 있을까?

loader 라는 기능을 통해서 자바스크립트 파일이 아닌 파일을 웹팩이 이해할 수 있게 해준다.

```
module : {
	rules: {
		test: '가지고올 파일 정규식',
		use: [
			{
				loader: '사용할 로더 이름',
				options: { 사용할 로더 옵션 }
			}
		]
	}
}
```

public 디렉터리를 만들고 그 안에 index.html을작성한다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>webpack-prac</title>
</head>
<body>
<noscript>스크립트가 작동되지 않습니다.</noscript>
<div id="root"></div>
</body>
</html>
```

이후 webpack.config.js에 html loader 부분에 코드를 추가해주자.

```
module: {
rules: [
{
test: /\.html$/,
use: [
{
loader: "html-loader", //loader html
options: { minimize: false }, //한줄로?
},
],
},
],
},
plugins: [
new HtmlWebpackPlugin({
template: "./public/index.html", //public/index.html 파일을 읽음.
filename: "index.html", // output으로 출력할 파일은 index.html이다.
}),
],
```

코드의 의미는 이해했지만, 구조는 잘 이해를 못하겠다 [작성 방법이라던지.. roules?]
모듈을 이용한다는 것 같고, html-loader을 로더에 삽입해주는 것 같다.
