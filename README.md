# :zap: React 와 Node.js 연동하기

## :tada: React 프로젝트 생성.
```bash
yarn create react-app myreact
```

- 프로젝트가 생성되었다면 myreact/src에 `yarn add http-proxy-middleware`로 http-proxy-middleware 설치 후 setupProxy.js 파일을 추가합니다.

```bash
// setupProxy.js
const {createProxyMiddleware} = require('http-proxy-middleware');

module.exports = function(app){
  app.use(
    createProxyMiddleware("/api",{
      target:"http://localhost:8080",
      changeOrigin:true,
    })
  )
}
```

## 💾 Node.js 서버 구성
- nodemon, concurrently(react 서버와 node 서버를 동시에 실행시키기 위한 모듈) 설치
```bash
yarn add nodemon
yarn add concurrently
```

- server.js 생성
```bash
//server.js
const express = require('express');
const path = require('path');
const cors = require('cors');
const app = express();

const server = require('http').createServer(app);

app.use(cors()); // cors 미들웨어를 삽입합니다.

app.get('/', (req,res) => { // 요청패스에 대한 콜백함수를 넣어줍니다.
  res.send({message:'hello'});
});

server.listen(8080, ()=>{
  console.log('server is running on 8080')
})
```

- 작성 완료 후 서버 구동하는 방법
```
node server.js
or
nodemon server.js
```

## 📋 리액트 프로젝트 추가 수정사항
- axios 설치
```bash
yarn add axios
```

- App.js 작성
```bash
//App.js

import './App.css';
import axios from 'axios';
import React,{useState,useEffect} from 'react';

function App() {

  const sendRequest = async() => {
    const response = await axios.get('http://localhost:8080');
    console.log(response);
    console.log(response.data);
  };

  useEffect(()=>{
    sendRequest();
  });

  return (
    <div className="App">
    </div>
  );
}

export default App;
```

## 📦 package.json 수정사항.
- 가장 상위 `package.json`에 스크립트 추가
```bash
  "scripts": {
    "server": "cd server && nodemon server",
    "client": "cd client && npm start",
    "start": "concurrently --kill-others-on-fail \"npm run server\" \"npm run client\""
  },
```

- 이후 상위 폴더에서 `yarn start`시 React, Node.js이 동시에 작동함.


## 📎 출처
- 출처1: https://baegofda.tistory.com/210
- 출처2: https://baegofda.tistory.com/211