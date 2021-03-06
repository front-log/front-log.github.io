---
layout: post
title: "널리 사용되는 노드 프로젝트에 대한 대안 없음"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/deno-node.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/deno-node.png?fit=730%2C487&ssl=1)

Deno는 Rust에 내장된 V8 엔진을 사용하는 JavaScript와 TypeScript를 사용하여 애플리케이션을 작성하는 간단한 현대 보안 런타임이다.

이것은 데노가 무엇인지 설명하는 기사가 아니다. 하지만 데노에 대해 더 알고 싶다면 도와줄게. 다음 문서를 확인하십시오. Deno는 무엇이며 Node.js와 어떻게 다른가?

향후 개발에 관심이 있으시다면 Deno 저장소도 확인하실 수 있습니다.

이제 노드는 지난 몇 년 동안 강력하고 대중적으로 성장했습니다. 또한 종속성이라고 하는 몇 가지 인기 있는 노드 프로젝트도 있습니다. 데노가 태어난 이후, 데노에 있는 노드에서 우리가 하는 것과 같은 일을 어떻게 할 것인가에 대한 많은 의구심이 있었다.

노드에서 가장 인기 있는 몇 가지를 살펴보고 데노에서 그 대안을 보여드리겠습니다.

## next.js/Alph.js

알레프.js는 데노의 리액트 프레임워크이다. 그래, 바로 그거야. Next.js가 노드에 있는 것처럼. Next.js for Node는 React를 사용하여 프로덕션에 필요한 모든 기능을 제공합니다. 서버 측 렌더링, 자동 코드 분할, 정적 내보내기 옵션, 간편한 프로덕션 빌드 및 정적 웹 응용 프로그램을 빌드할 수 있습니다.

Next.js와 달리 Aleph.js는 ESM(ECMascript Module) 가져오기 구문을 사용하기 때문에 번들을 사용하지 않거나 번들러를 필요로 하지 않습니다. 가져온 각 모듈은 한 번만 컴파일되고 캐시됩니다.

```js
import React from "https://esm.sh/react@17.0.1"
import Logo from "../components/logo.tsx"

export default function Home() {
    return (
      <div>
        <Logo />
        <h1>Hello World!</h1>
      </div>
    )
}
```

Aleph.js가 제공하는 다른 고급 기능으로는 비동기 가져오기, 사용자 지정 `404` 페이지, 사용자 지정 로딩 페이지, `use Deno` Hook 등이 있습니다.

## Express.js/Opine

Express.js는 노드를 위한 개방형, 자유 또는 자유분방한 미니멀리스트 웹 프레임워크로 매우 인기가 있다. 그것의 철학은 작고 빠른 HTTP 서버용 도구를 제공하여 SPA, 웹 사이트, 하이브리드 또는 공용 HTTP API를 위한 훌륭한 옵션으로 만드는 것이다. 어쨌든 Express.js를 사용하여 자신만의 프레임워크를 만들 수 있습니다. 또 다른 좋은 점은 클라이언트 요청에 효율적으로 응답하는 강력한 라우팅 시스템과 HTTP 도우미를 거의 선택하지 않는다는 것이다.

Express.js와 달리 Opine은 고성능, 컨텐츠 협상(요청 유형에 따라 응답을 반환할 때 멋진), 강력한 라우팅, HTTP 프록시 미들웨어 지원(인증, 로드 밸런싱, 로깅 등에 유용)에 중점을 둔다. 오핀의 미래는 밝다!

```js
import opine from "../../mod.ts";

const app = opine();

app.get("/", function (req, res) {
  res.send("Hello Deno!");
});

const server = app.listen();
const address = server.listener.addr as Deno.NetAddr;
console.log(`Server started on ${address.hostname}:${address.port}`);
```

Opine에서 컨텐츠 협상을 처리하여 각 유형의 서식을 한눈에 처리할 수 있도록 하는 것이 매우 마음에 듭니다.

```coffeescript
import { opine } from "../../mod.ts";
import { users } from "./db.ts";
import type { Request, Response } from "../../src/types.ts";

const app = opine();

app.get("/", (req, res) => {
  res.format({
    html() {
      res.send(
        `<ul>${users.map((user) => `<li>${user.name}</li>`).join("")}</ul>`,
      );
    },

    text() {
      res.send(users.map((user) => ` - ${user.name}\n`).join(""));
    },

    json() {
      res.json(users);
    },
  });
});
```

## Passport.js/Onyx

Passport.js는 단순히 노드 서버의 인증을 처리하는 데 도움이 됩니다. 이것은 미들웨어의 역할을 하며, 매우 유연하며 모듈화됩니다. Express.js 기반 웹 응용 프로그램에서 널리 사용됩니다. 일련의 인증 전략(사용자 이름 및 암호, Facebook, Twitter 등)에 대한 지원을 제공합니다.

Passport.js와 달리 Onyx의 주요 초점은 코드를 깨끗하고 체계적으로 유지하는 것이다. Passport.js에서와 같이 가져오기 없이 인증 전략을 간소화할 수 있습니다.

Passport.js를 미들웨어로 적용하는 것처럼 Onyx도 마찬가지입니다.

```js
import { Router } from '../deps.ts';
import { onyx } from '../deps.ts';
import User from './models/userModels.ts';

const router = new Router();

// invoke onyx.authenticate with the name of the strategy, invoke the result with context
router.post('/login', async (ctx) => {
  await (await onyx.authenticate('local'))(ctx);

  if (await ctx.state.isAuthenticated()) {
    const { username } = await ctx.state.getUser();
    ctx.response.body = {
      success: true,
      username,
      isAuth: true,
    };
  } else {
    const message = ctx.state.onyx.errorMessage || 'login unsuccessful';
    ctx.response.body = {
      success: false,
      message,
      isAuth: false,
    };
  }
});
```

서버 파일에 쉽게 설치할 수 있습니다. 이 프로세스는 노드에서 수행하는 프로세스와 동일합니다.

```js
import { Application, send, join, log } from '../deps.ts';
import { Session } from '../deps.ts';

// Import in onyx and setup
import { onyx } from '../deps.ts';
import './onyx-setup.ts';

// Server Middlewares
import router from './routes.ts';

// SSR
import { html, browserBundlePath, js } from './ssrConstants.tsx';

const port: number = Number(Deno.env.get('PORT')) || 4000;
const app: Application = new Application();

// session with Server Memory
// const session = new Session({ framework: 'oak' });

// session with Redis Database
const session = new Session({
  framework: 'oak',
  store: 'redis',
  hostname: '127.0.0.1',
  port: 6379,
});

// Initialize Session
await session.init();
app.use(session.use()(session));

// Initialize onyx after session
app.use(onyx.initialize());
```

## 노드 Redis/DenoRedis

레디스(Redis)는 데이터베이스 및 메시지 브로커링뿐만 아니라 캐싱에 자주 사용되는 인기 있는 메모리 내 데이터 구조 서버이다. 그것은 놀라울 정도로 높은 처리량으로 매우 빠릅니다.

노드(Node)에서 노드-redis는 완전히 비동기적인 고성능 Redis 클라이언트였다.

Deno-redis는 Deno를 위한 Redis 클라이언트의 구현입니다.

```undefined
import { connect } from "https://deno.land/x/redis/mod.ts";
const redis = await connect({
  hostname: "127.0.0.1",
  port: 6379
});
const ok = await redis.set("hoge", "fuga");
const fuga = await redis.get("hoge");
```

## 노데몬/데논

Nodeemon은 파일 시스템에서 노드 프로젝트의 변경 사항을 감시하고 프로세스/서버를 자동으로 재시작하는 CLI(명령줄 인터페이스)입니다. 이렇게 하면 이미 변경한 내용을 확인하기 위해 서버를 수동으로 재시작하지 않아도 됩니다.

데노에는 데논이 있습니다. 에몬과 다를 바 없다. 데노를 데논으로 바꾸기만 하면 된다.

```css
denon run app.ts
```

## 웹소켓

웹 소켓 API는 클라이언트와 서버 간의 양방향 대화형 통신을 엽니다. 기본적으로 클라이언트와 서버 사이에는 지속적인 연결이 있어 두 서버 모두 언제든지 데이터를 전송할 수 있습니다. ws 라이브러리는 노드 프로젝트의 클라이언트 및 서버 구현으로 구축되었습니다.

다행히도, 데노를 위한 ws와 같은 라이브러리인 웹 소켓이 없습니다.

서버측 사용량은 노드 프로젝트에서 발생할 수 있는 것과 매우 유사합니다.

```js
import { WebSocket, WebSocketServer } from "https://deno.land/x/websocket@v0.0.5/mod.ts";

const wss = new WebSocketServer(8080);
wss.on("connection", function (ws: WebSocket) {
  ws.on("message", function (message: string) {
    console.log(message);
    ws.send(message)
  });
});
```

클라이언트 측도 마찬가지입니다.

```js
import { WebSocket } from "https://deno.land/x/websocket@v0.0.5/mod.ts";
const endpoint = "ws://127.0.0.1:8080";
const ws: WebSocket = new WebSocket(endpoint);
ws.on("open", function() {
  console.log("ws connected!");
});
ws.on("message", function (message: string) {
  console.log(message);
});
ws.send("something");
```

## 코르스

다음으로는 다양한 옵션과 함께 CORS를 활성화하는 데 사용할 수 있는 미들웨어 지원을 제공하는 노드 패키지인 Cors가 있습니다.

Express.js 응용 프로그램에서 모든 Cors 요청을 사용하기 위한 간단한 사용법은 다음과 같습니다.

```js
var express = require('express')
var cors = require('cors')
var app = express()

app.use(cors())

app.get('/products/:id', function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for all origins!'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
```

데노에서, 우리는 오핀에서도 같은 일을 할 것이다. 예를 들어:

```coffeescript
import { opine, serveStatic } from "https://deno.land/x/opine/mod.ts";
import { opineCors } from "../../mod.ts";

const app = opine();

app.use(opineCors()); // Enable CORS for All Routes

app
  .use(serveStatic(`${Deno.cwd()}/examples/opine/static`))
  .get("/products/:id", (_req, res) => {
    res.json({msg: 'This is CORS-enabled for all origins!'});
  })
  .listen(
    { port: 8000 },
    () => console.info("CORS-enabled web server listening on port 8000"),
  );
```

## Bcrypt/BCrypt

둘 다 해시 암호에 사용되는 유사한 라이브러리입니다. 니엘 프로보스가 설계한 비크립트는 비밀번호에 주로 사용되는 암호해시 기능이다. 기본적으로 암호에서 해시를 생성하여 저장합니다.

노드용 Bcrypt를 사용했지만, 지금은 Deno용 Bcrypt가 있습니다. Deno에서의 사용량은 여전히 거의 Node에서와 같습니다.

```js
import * as bcrypt from "https://deno.land/x/bcrypt/mod.ts";

const hash = await bcrypt.hash("test");

// To check a password
const result = await bcrypt.compare("test", hash);
```

## 전자/웹 보기

노드에서는 Electron을 사용하여 크로스 플랫폼 데스크톱 애플리케이션을 생성할 수 있습니다. Deno에서는 이제 WebView가 있습니다. 또한 크로스 플랫폼 데스크톱 애플리케이션을 위한 GUI를 생성하는 데도 사용됩니다.

```coffeescript
import { WebView } from "https://deno.land/x/webview/mod.ts";

const html = `
  <html>
  <body>
    <h1>Hello from deno</h1>
  </body>
  </html>
`;

await new WebView({
  title: "Local webview_deno example",
  url: `data:text/html,${encodeURIComponent(html)}`,
  height: 600,
  resizable: true,
  debug: true,
  frameless: false,
}).run();
```

## 몽고DB

MongoDB는 대중적이고 확장 가능하며 유연합니다. 이것은 주로 자바스크립트 생태계에서 널리 사용되는 문서 기반 데이터베이스이다.

이제 Deno에서 MongoDB를 사용할 수 있습니다.

```undefined
import { MongoClient } from "https://deno.land/x/mongo@v1.0.0/mod.ts";

const client = new MongoClient();
client.connectWithUri("mongodb://localhost:27017");

// Defining schema interface
interface UserSchema {
  _id: { $oid: string };
  username: string;
  password: string;
}

const db = client.database("test");
const users = db.collection<UserSchema>("users");

// insert
const insertId = await users.insertOne({
  username: "user1",
  password: "pass1",
});

// insertMany
const insertIds = await users.insertMany([
  {
    username: "user1",
    password: "pass1",
  },
  {
    username: "user2",
    password: "pass2",
  },
]);

// findOne
const user1 = await users.findOne({ _id: insertId });

// find
const all_users = await users.find({ username: { $ne: null } });

// find by ObjectId
const user1_id = await users.findOne({ _id: "idxxx" });
```

## SMTP

SMTP는 노드 전자 메일러에서 메시지를 보내는 주 전송 역할을 합니다. 이제 Deno에서 Deno SMTP로 사용 가능:

```undefined
import { SmtpClient } from "https://deno.land/x/smtp/mod.ts";

const client = new SmtpClient();

await client.connect({
  hostname: "smtp.163.com",
  port: 25,
  username: "username",
  password: "password",
});

await client.send({
  from: "mailaddress@163.com",
  to: "to-address@xx.com",
  subject: "Mail Title",
  content: "Mail Content，maybe HTML",
});

await client.close();
```

## NPM/Trex

npm in Node는 오랫동안 존재해 온 인기 패키지 관리 툴로, 데노에서 Trex와 함께 대안이 있다는 것이 놀랍다. npm과 같은 많은 옵션들이 함께 제공됩니다. 패키지는 캐시되고 하나의 파일만 생성됩니다.

현재 개발 중인 Deno 대안들이 많이 있습니다. 아직 다음과 같은 기능이 있습니다.

- dinoenev – 환경 변수 관리
- denodb – MySQL, MongoDB, SQLite용 ORM
- csv – CSV 파서
- deno-mysql – MySQL 드라이버
- cac – 명령줄 응용 프로그램을 빌드하기 위한 프레임워크입니다.

## 결론

이 기사에서는 개발 중에 사용되는 인기 있는 노드 프로젝트에 대한 데노 대안을 살펴봤습니다. 우리는 Deno가 여전히 성장하고 있다고 믿으며, 만약 당신이 Deno 세계로 더 나아가고 있다면, 이미 이용 가능한 현재의 대안들을 아는 것이 좋을 것이다.

인기 있는 노드 대안을 건너뛰었다고 생각되면 응답으로 삭제해도 됩니다.