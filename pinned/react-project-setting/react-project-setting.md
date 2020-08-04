React Project 세팅하기
===

## **npm 프로젝트 생성하기**
```
npm init -y
```
  실행하고 나면, ```package.json``` 이 생성된다.  

```
npm install
```
  실행하고 나면, ```package-lock.json``` 이 나온다. ```package-lock.json```에서는 우리가 프로젝트에서 사용하는 것들의 구체적인 버전들 및 정보들이 담겨있다. (이 파일 덕분에 개발환경이 다른 곳에서도 문제를 덜 일으키도록 할 수 있다.)   

```HTML
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <title>Counter</title>
</head>

<body>
  <p>hello, world!</p>
</body>

</html>
```  
HTML 파일을 생성하고 기본적인 코드를 작성해보자.

```
npx server
```
그런 다음 서버에서 실행시켜 보고 싶다면, 위에 코드를 실행시켜 보자.


## **webpack 설치하기**

```
npm i -D webpack webpack-cli webpack-dev-server
```
이 때, ```i``` 는 ```install```의 줄임말이고, ```-D```는 ```--save-dev```의 줄임말이다. 개발환경에서만 사용하는 패키지를 다운받는다는 뜻이다.
위 명령어를 실행하고나면, ```package.json``` 에서 ```devDependencies``` 에 추가된다.  


```
npx webpack-dev-server
```
webpack-dev-server를 실행시켜 보자. (에러가 나오지만, 8080 포트로 들어가면 확인 가능하다) 에러의 내용은 src 폴더가 없다는 것이다. 

```javascript
// src/index.js
console.log('hello, world');
```
src/index.js 만들어보자.  

```HTML
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8">
  <title>Counter</title>
</head>

<body>
  <p>hello, world!</p>
  <script src="main.js"></script>
</body>

</html>
```
HTML 파일을 위와 같이 고쳐보자 (```script``` 부분) 이 코드를 추가해야 index.js 파일 실행된다.
main.js 를 만든적은 없지만, 뭔가 막 있다...(?!)

## **eslint 설치하기**

```
npm i -D eslint
```
위 코드를 통해 eslint를 설치한다. vscode에서 eslint 자동으로 실행하는 것을 설치해도 된다.  

```
npx eslint --init
```
* to check syntax, find problems, and enforce code style 
* javascript modeuls
* project: React
* Typescript : N
* Browser
* Use a popular style guide
* Airbnb style
* JavaScript
* (나머지 질문들) Y

위 설정을 따른다.


```
npx eslint src/index.js
```
**eslint 검사하기1)** src/index.js에 해당하는 code의 문제들 찾아줌  


```
npx eslint .
```
**eslint 검사하기2)** 현재 폴더 안에 있는 모든 파일의 문제들 찾아줌  

```
npx eslint --fix .
```
**eslint 검사하기 & 고치기)** 현재 폴더 안에 있는 모든 애들 문제 찾고 고칠 수 있는 애들 고쳐줌.  


### **[eslint 관련하여 참고하기]**

```javascript
//code 1
console.log("hello world");
```
```javascript
//code 2
const { log } = console;
log("hello world");
```
위 코드는 자꾸에러가 나는데, 위와 같이 하면 lint 에러가 없어진다.

### **[저장할 때 eslint auto fix 실행하도록 설정하기 - 맥 기준]**
```
1. open $HOME/Library/Application Support/Code/User/settings.json

2. add 
 {
   "editor.codeActionsOnSave": {
     "source.fixAll.eslint": true
   },
 }
```

## **webpack-dev-server 자동으로 실행시키기**
```HTML
<body>
  <div id="app"></div>
  <script src="main.js"></script>
</body>
```
(강의 과정중이라서) 우선 살짝 index.html을 수정해주자.

```json
"scripts": {
  "start": "webpack-dev-server",
}
```
```npm start```를 할 때 자동으로 ```webpack-dev-server```가 실행되도록, ```package.json``` 파일을 위와 같이 수정한다.  


## **Babel 설치하기**

```
npm i -D babel-loader
```
위 명령어를 통해, ```babel-loader```를 설치한다.

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      }  
    ],
  },
};
```
webpack에서 babel-loader를 쓰기 위하여, 위 코드를 담은 ```webpack.config.js```를 만든다. 위 파일의 의미는 ```js``` 파일 혹은 ```jsx``` 파일에 대하여 바벨로더를 사용하겠다는 뜻이다. (단, ```node_modules``` 폴더는 제외하겠다는 뜻이다.)


```
npm i -D @babel/core
```
위에서 ```webpack.config.js```를 설정하고나서도 안된다. 그 이유는 ```@babel/core```가 설치되어 있지 않기 때문이다. 위 명령어를 통해 설치를 하자.

```javascript
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          node: 'current',
        },
      },
    ],
    '@babel/preset-react',
  ],
};
```
```babel.config.js``` 을 만들어서 위 코드를 작성해준다.

```
npm i -D @babel/preset-env @babel/preset-react
```
위 명령어대로 설치한다.

React 없이 jsx를 쓰고 싶은 경우, ```index.js```로 가서, ```/* @jsx createElement */```를 제일 위에 써주도록 한다.


## **React 설치하기**


```
npm i react react-dom
```
위 명령어를 통해 react와 react-dom을 설치해준다. 그런 다음, ```index.js```를 ```index.jsx```로 바꾼다.


```javascript 
const path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'src/index.jsx'),
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      }  
    ],
  },
  resolve: {
    extenstions: ['.js', '.jsx'],
  },
};
```
```webpack.config.js```를 위와 같이 바꿔준다. 

1. ```rules```가 의미하는 것은 다음과 같다. 해당 내용은 1주차 강의에서 다룬바가 있다.

    > 'node_modules'내에 있는 모든 파일은 제외하고, "js" 혹은 "jsx"로 되어있는 파일들에 대하여 babel loader를 적용한다. 

2. ```resolve```는 모듈을 import 할 때, 어떤 파일의 형태에서 찾을지 지정해주는 것이다. 예를 들어, ```import App from './App';``` 와 같이 파일이름에서 ```.jsx``` 부분을 빼고 불러오도록 한다. 


## .gitignore 세팅하기

다음과 같은 세팅이면 충분할듯 하다. 특히, ```node_module```과 ```coverage```는 꼭 세팅을 해주자!

```
## Mac OS x
.DS_*

## IntelliJ
.idea

## VS Code
.vscode

## CodeceptJS
output

## Node.js
node_modules

## Jest
coverage
```

## **jest 세팅하기**

1. **jest 설치하기**
    ```
    npm i -D jest babel-jest
    ```
    위 코드를 실행해서, jest와 babel-jest를 설치한다. 그런 다음, ```simple.test.js```를 만들어보자!

2. **@types/jest 설치하기**
    ```
    npm i -D @types/jest
    ```
    위 코드를 실행시켜, types/jest를 설치한다. test 코드를 작성할 때 자동완성 되는 등의 문제를 막아준다.

3. **```.eslintrc.js``` 에 들어가서 jest true 만들어주기**
    ```javascript
    module.exports = {
      env: {
        browser: ... ,
        es6: ... ,
        jest: true,
      },
    }
    ```

4. **```jest```를 실행해보자**
    ```
    npx jest
    npx jest --watchAll
    ``
    ```--watchAll```를 쓰면 이 프로젝트에 있는 테스트 파일을 계속 감사한다.

5. **react 테스팅 라이브러리 설치하기**
    ```
    npm i -D @testing-library/react @testing-library/jest-dom
    ```
    React에서 테스트용으로 렌더링할 수 있도록 도와주는 도구들이다. (조금 더 정확하게 정리하자면, '사용자와 동일한 방식으로 DOM 쿼리를 사용할 수 있게 해준다'고 한다.)

6. **```jest.config.js```로 세팅하기**
    ```javascript
    //jest.cofing.js
    module.exports = {
      setupFilesAfterEnv: [
        './jest.setup',
      ],
    }
    ```
    같은 import 문은 계속 쓰기 힘드니까 위와 같이, 세팅파일을 만들어서 설정해주자.

    ```javascript
    //jest.setup.js
    import '@testing-library/jest-dom';
    ```
    ```jest.setup.js``` 에서 공통으로 import 되는 것들을 작성한다.


----

## **그 외 React 관련**

### **Redux 설치하기**
```
npm i redux react-redux
```

### **Redux Thunk 설치하기**

```
npm i redux-thunk
```

### **react router 설치하기**
```
npm i react-router-dom
```

### **emotion 설치하기**

```
npm i @emotion/core @emotion/styled
```

### **Redux Toolkit 설치하기**
```
npm i @reduxjs/toolkit
npm uninstall redux-thunk
```
@reduxjs/toolkit이 redux-thunk에 의존성을 가지고 있어서 이 안에도 포함되어있음. 

---


### **jest-plugin-context설치하기**

```
npm i -D jest-plugin-context
```

```javascript
// jest.config.js
module.exports = {
  setupFilesAfterEnv: [
    'jest-plugin-context/setup', // 이 부분 추가
    './jest.setup',
  ],
  ...
};
```
```javascript
// .eslintrc.js
module.exports = {
  globals: {
    ...
    context: 'readonly', // 이 부분 추가
    ...
  },
}
```

### **Redux mock store 설치하기**

> A mock store for testing Redux async action creators and middleware. The mock store will create an array of dispatched actions which serve as an action log for tests.

```
npm i -D redux-mock-store
```

### **given2 설치하기**

> Given is a lazy-evaluation system for use in your specs.


```
npm i -D given2
```

```javascript
// jest.
module.exports = {
  setupFilesAfterEnv: [
    'given2/setup',
    'jest-plugin-context/setup',
    './jest.setup',
  ],
  // ...
};
```

### **HtmlWebpackPlugin 설치하기**
> This is a webpack plugin that simplifies creation of HTML files to serve your webpack bundles. This is especially useful for webpack bundles that include a hash in the filename which changes every compilation. 

```
npm i -D html-webpack-plugin
```

```javascript
// webpack.config.js
module.exports = {
  // ... 
  devServer: {
    historyApiFallback: {
      index: 'index.html',
    },
  },
};
```

### **file loader 설치하기**

```
npm i -D file-loader
```

```javascript
//webpack.config.js
module.exports = {
  entry: path.resolve(__dirname, 'src/index.jsx'),
  module: {
    rules: [
      // ...
      {
        test: /\.(png|jpg|gif)$/i,
        use: 'file-loader',
      },
    ],
  },

};
```