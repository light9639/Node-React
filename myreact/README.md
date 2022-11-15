:zap: React 와 Node.js 연동하기

```bash
yarn create react-app myreact
```

- 프로젝트가 생성되었다면 myreact/src에 setupProxy.js 파일을 추가합니다.

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

- server.js 구성
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

- 서버 구동
```
node server.js
or
nodemon server.js
```

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

## 출처
- https://baegofda.tistory.com/210
- https://baegofda.tistory.com/211