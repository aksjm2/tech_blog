---
description: 설치된 node에서 ES6 코드가 syntax error 발생하여 해결하는 방법을 정리함.
---

# Nodejs :: ES6 적용하기

#### 01. Work log.

01.01. Project Setup

```bash
express PROJECT_NAME --no-view // express-generator를 통한 프로젝트 기본 골격 setting.
cd PROJECT_NAME                
npm install                    // npm을 이용한 패키지 installation
```

![](https://wiki.simplexi.com/download/attachments/1462182403/image2019-7-12_15-47-47.png?version=1&modificationDate=1562915073000&api=v2) ![](https://wiki.simplexi.com/download/attachments/1462182403/image2019-7-12_15-48-25.png?version=1&modificationDate=1562915073000&api=v2)

```text
- 프로젝트 폴더 하위에 src 폴더를 만든다.
- bin/, app.js, routes/ 를 src 폴더에 이동시킨다.
- bin/www 를 bin/www.js로 이름을 변경한다.
- public 폴더는 project의 root에 놔둔다.
- routes/user.js를 삭제한다. -- 이건 샘플이라 지워도 됨.
```

![](https://wiki.simplexi.com/download/attachments/1462182403/image2019-7-12_15-50-22.png?version=1&modificationDate=1562915073000&api=v2)

{% code-tabs %}
{% code-tabs-item title="bin/www.js" %}
```javascript
import app from '../app';
import debugLib from 'debug';
import http from 'http';

const debug = debugLib('es6-node:server');
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="router/index.js" %}
```javascript
import express from 'express';
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

export default router;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="app.js" %}
```javascript
import express from 'express';
import path from 'path';
import cookieParser from 'cookie-parser';
import logger from 'morgan';

import indexRouter from './routes/index';

const app = express();

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, '../public')));

app.use('/', indexRouter);

export default app;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="npm-run-all installation" %}
```bash
npm install --save npm-run-all // windows cmd에서 몇몇 터미널 커멘드가 동작하지 않아, npm-run-all 패키지를 설치한다.
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```bash
// ES6 를 사용하게 해 줄 babel 및 다른 패키지 설치.
npm install -D @babel/core @babel/cli @babel/preset-env @babel/node (-D 옵션 : devDependency에 추가/ --save 옵션 : dependency에 추가.)
```

{% code-tabs %}
{% code-tabs-item title="PROJECT/.babelrc" %}
```text
{ "presets" : ["@babel/preset-env"] }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="package.json" %}
```javascript
{
  "name": "es6-node",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www",
    "server" : "babel-node ./src/bin/www",
    "dev": "set NODE_ENV=development&&npm-run-all server" // linux OS의 경우 : "NODE_ENV=development npm-run-all server"
  },
  "dependencies": {
    "cookie-parser": "~1.4.3",
    "debug": "~2.6.9",
    "express": "~4.16.0",
    "morgan": "~1.9.0",
    "npm-run-all": "^4.1.5"
  },
  "devDependencies": {
    "@babel/cli": "^7.5.0",
    "@babel/core": "^7.5.4",
    "@babel/node": "^7.5.0",
    "@babel/preset-env": "^7.5.4"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



