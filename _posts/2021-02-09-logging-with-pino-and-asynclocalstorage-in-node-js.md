---
layout: post
title: "Node.js에서 Pino 및 AsyncLocalStorage로 로깅"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/02/logging-pino-asynclocalstorage.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/logging-pino-asynclocalstorage.png?fit=730%2C487&ssl=1)

몇 시간, 아니 며칠을 버그를 고치려고 하는 것은 좌절스럽고 비생산적이다. 결국, 여러분은 마법처럼 일어날 수 있는 유레카 순간을 기다리며 화면을 응시하게 될 것입니다.

하지만 마법처럼 해결책이 떠오르기를 기다리는 대신, 여러분이 다루고 있는 어떤 에지 케이스 버그를 체계적으로 추적할 수 있는 강력한 힘을 가지고 있다면 어떨까요?

로깅을 통해 가능합니다. 올바르게 사용할 경우 로깅을 통해 애플리케이션에 대한 필요한 통찰력을 얻을 수 있으므로 정확히 어떤 일이 발생했는지 파악할 수 있습니다. 적절한 로깅은 버그를 더 쉽게 찾고 더 빨리 해결할 수 있도록 도와주는 강력한 디버깅 도구와 디버그 문의 형편없는 덤프 간의 차이일 수 있습니다.

## 로깅 라이브러리란 무엇이며 사용해야 하는 이유는 무엇입니까?

로깅에서 인간이 읽기 쉽고 기계로 구문 분석할 수 있는 출력을 갖는 것이 중요합니다. 저희 개발자들에게는 로그를 살펴보고 검사할 때 우리가 이해할 수 있도록 하는 것이 중요합니다. 고급 쿼리를 실행하고 멋진 집계를 수행하기 위해서는 기계들이 로그를 구문 분석할 수 있어야 합니다.

JSON은 두 기준에 모두 적합한 형식이므로 로깅 라이브러리가 출력을 유효한 JSON으로 구문 분석하고 로그의 형식이 항상 올바른지 확인합니다.

당신은 취미 프로젝트에서 `console.log`를 사용할 수 있습니다. 그러나 프로덕션 등급의 Node.js 애플리케이션에서는 다른 로그 수준을 구별할 수 있는 것이 종종 유용합니다.

로깅 라이브러리를 입력하면 다른 수준에서 로깅을 켜거나 끌 수도 있습니다. 프로덕션 환경에서는 일반적으로 오류나 경고를 표시하려고 하지만, 준비 환경에서는 디버그/더 자세한 로그도 유용하며, 그렇지 않으면 프로덕션에서 너무 많은 노이즈를 추가할 수 있습니다.

## 피노란 무엇인가?

Pino는 Node.js 생태계에서 널리 사용되는 로깅 라이브러리이다. 속도가 빠르고 오버헤드가 최소화됩니다.

## Pino를 사용하기 위한 필수 조건

- Node.js 버전 >=12.17 또는 >=13.10
- NPM(일반적으로 Node.js에 포함됨)
- 로깅을 추가할 Express 웹 서버

## Node.js 응용 프로그램에서 Pino 사용

Node.js 프로젝트에 Pino를 설치하려면 다음을 실행합니다.

```coffeescript
npm install pino
```

`logger.js`라는 파일을 만듭니다. 이 파일에서는 Pino를 가져와 구성한 다음 프로젝트 전체에서 사용할 로깅 인스턴스를 내보냅니다.

```java
// logger.js
const pino = require('pino');

// Create a logging instance
const logger = pino({
  level: process.env.NODE_ENV === 'production' ? 'info' : 'debug',
});

module.exports.logger = logger;
```

피노를 사용하는 것은 비교적 간단하다. 유사한 이름의 메소드를 사용하여 여러 로그 수준(디버그, 정보, 경고, 오류 등)에서 메시지를 기록할 수 있습니다. 개체 및/또는 오류를 전달하여 다음 컨텍스트를 추가할 수도 있습니다.

```coffeescript
const { logger } = require('./logger.js');

// Log a simple message at the "info" level
logger.info('Application started!');

// Outputs:
// {"level":30,"time":1608568334356,"pid":67017,"hostname":"Maxims-MacBook-Pro.local","msg":"Application started!"}

// Log an object in addition to a message to supply context
const user = { firstName: 'Maxim', lastName: 'Orlov' };
logger.info(user, 'User successfully authenticated');

// Outputs:
// {"level":30,"time":1608568334356,"pid":67017,"hostname":"Maxims-MacBook-Pro.local","firstName":"Maxim","lastName":"Orlov","msg":"User successfully authenticated"}

// Log an error at the "error" level
const error = new Error('Database crashed!');
logger.error(error, 'Failed to fetch user');

// Outputs:
// {"level":50,"time":1608568334356,"pid":67017,"hostname":"Maxims-MacBook-Pro.local","stack":"Error: Database crashed!\n    at Object.<anonymous> (/Users/maxim/Code/playground/test.js:19:15)\n    at Module._compile (node:internal/modules/cjs/loader:1102:14)\n    at Object.Module._extensions..js (node:internal/modules/cjs/loader:1131:10)\n    at Module.load (node:internal/modules/cjs/loader:967:32)\n    at Function.Module._load (node:internal/modules/cjs/loader:807:14)\n    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:76:12)\n    at node:internal/main/run_main_module:17:47","type":"Error","msg":"Failed to fetch user"}
```

기존 Node.js 응용 프로그램과 Pino를 통합하려면 로거 인스턴스를 가져와 프로젝트 전체에서 사용하면 됩니다. 만약 당신의 프로젝트가 `discount.log`를 사용한다면, 당신은 프로젝트 와이드 찾기를 할 수 있고, logger.info으로 대체 할 수 있다. 파일 맨 위에 있는 로거 인스턴스를 가져와야 합니다.

또한 로그를 다른 로그 수준으로 분류하는 데 시간을 할애할 수 있습니다. 이렇게 하면 여러 심각도 수준에서 로그 메시지를 구분할 수 있으며 프로덕션의 디버그 로그와 같은 특정 환경의 특정 로그 수준 아래의 로그를 음소거할 수 있습니다.

최소한 프로젝트의 "정보" 및 "오류" 로그 수준을 사용하여 오류 로그를 일반 작업 로그와 쉽게 구분할 수 있습니다.

다음은 사용자를 가져오기 위해 단일 끝점으로 Express 웹 서버를 실행하는 Node.js 응용 프로그램의 예입니다.

```js
// server.js
const express = require('express');
const { logger } = require('./logger.js');
const db = require('./db.js');

const PORT = process.env.PORT || 3000;

const app = express();

app.get('/users/:id', async (req, res) => {
  let userId = req.params.id;

  if (isNaN(userId)) {
    logger.warn({ userId }, 'Invalid user ID');
    return res.status(400).send('Invalid user ID');
  } else {
    userId = Number(userId);
  }

  try {
    logger.info({ userId }, 'Fetching user from DB');
    const user = await db.getUser({ userId });

    if (!user) {
      logger.warn({ userId }, 'User not found');
      return res.status(404).send('User not found');
    }

    logger.debug({ user }, 'User found, sending to client');
    return res.status(200).json(user);
  } catch (error) {
    logger.error(error, 'Failed to fetch user from DB');
    return res.status(500).send('An error occurred while fetching user');
  }
});

app.listen(PORT, () => {
  logger.info(`Server listening at http://localhost:${PORT}`);
});
```

이 예에서는 다양한 로그 수준(디버그, 정보, 경고 및 오류)을 사용합니다. 저는 보통 작업도 오류도 아닌 심각한 문제가 발생했을 때 "경고" 심각도로 로그인을 하는 경향이 있습니다.

디버그 로그는 Pino에서 기본적으로 비활성화되어 있으므로 프로덕션에서 노이즈가 너무 많이 추가되지만 디버깅 중에 스테이징 또는 로컬 개발에 유용할 수 있는 로그에 사용합니다.

## AsyncLocalStorage란 무엇이며 작동 방식은 무엇입니까?

Node.js는 단일 스레드 언어이므로 이벤트 루프를 사용하여 동시 비동기 작업을 처리한다. 이로 인해 Node.js는 웹 요청을 매우 빠르게 제공하지만, 단점은 함수 스택과 컨텍스트가 프로세스에서 손실된다는 것이다.

AsyncLocalStorage 클래스는 `sync_hooks` 모듈의 일부입니다. 콜백 기능과 비동기 작업 내에 데이터를 저장할 수 있는 비교적 새로운 Node.js API입니다.

이를 사용하려면 새 클래스 인스턴스를 만들고 저장소와 콜백 함수라는 두 개의 인수를 전달하여 `실행` 메서드를 호출합니다.

저장소는 단순한 정수 또는 문자열에서 복잡한 개체 또는 지도에 이르는 모든 항목일 수 있습니다. 두 번째 인수로 전달된 콜백 함수는 저장소의 컨텍스트에서 실행됩니다. 저장소에 액세스하려면 인스턴스의 getStore 메서드를 호출합니다.

어떻게 작동하는지 살펴보겠습니다.

```js
const { AsyncLocalStorage } = require('async_hooks');

// Create a new context
const context = new AsyncLocalStorage();

function doSomethingAsync() {
  // Use setImmediate to mimic async execution
  setImmediate(() => {
    const store = context.getStore();
    store.get('name'); // Maxim
  });
}

function main() {
  const store = new Map();
  store.set('name', 'Maxim');

  // Run the callback function (second argument) inside a context with `store` as data
  context.run(store, () => {
    doSomethingAsync();
  });

  // This is outside of the context
    context.getStore(); // undefined
}

main();
```

스토어는 콜백 기능과 해당 하위 기능 내에서만 액세스할 수 있습니다. getStore 방식은 외부의 어느 곳에서도 정의되지 않은 상태로 반환됩니다.

이 예제에서는 하나의 중첩 수준만 사용하고 두 함수가 동일한 파일에 있으므로 이점이 명확하지 않을 수 있습니다. 그러나 함수 스택이 여러 층 깊이에 있고 서로 다른 파일에 걸쳐 퍼져 있는 더 큰 프로젝트에서는 어디에서나 스레드 로컬 저장소에 액세스할 수 있는 유용성을 상상할 수 있습니다.

## 특정 요청에 로그 연결

스택 수준에서 데이터를 저장하고 검색하는 데는 여러 가지 사용 사례가 있습니다. 한 가지 사용 사례는 로그를 웹 요청과 연결하는 것입니다.

프로덕션 응용 프로그램에서 오류 로그가 발생하면 동일한 요청/응답 주기의 일부였던 다른 모든 로그를 볼 수 있는 것이 매우 유용합니다. 이렇게 하면 응용 프로그램을 통과하는 요청을 추적할 수 있으므로 특정 버그로 이어진 조건과 변수를 수집할 수 있습니다.

그러기 위해서는 각 요청에 고유 식별자를 할당해야 합니다. UUID(Universally Unique ID)는 우리가 찾고 있는 것이며, UUID 라이브러리는 바로 그것을 우리에게 제공한다. 보다 구체적으로 각 요청에 대해 버전 4 UUID를 생성합니다.

위의 예제 Node.js 응용 프로그램을 확장해 보겠습니다. 먼저 AsyncLocalStorage 인스턴스를 내보내는 모듈을 생성합니다.

```java
// async-context.js
const { AsyncLocalStorage } = require('async_hooks');

const context = new AsyncLocalStorage();

module.exports = context;
```

다음으로 고유한 요청 ID로 하위 로거를 생성하여 컨텍스트 저장소에 추가하는 Express 미들웨어 기능을 갖춘 `logger.js` 파일을 확장하겠습니다.

```js
// logger.js
const pino = require('pino');
const uuid = require('uuid');
const context = require('./async-context.js');

// Create a logging instance
const logger = pino({
  level: process.env.NODE_ENV === 'production' ? 'info' : 'debug',
});

// Proxify logger instance to use child logger from context if it exists
module.exports.logger = new Proxy(logger, {
  get(target, property, receiver) {
    target = context.getStore()?.get('logger') || target;
    return Reflect.get(target, property, receiver);
  },
});

// Generate a unique ID for each incoming request and store a child logger in context
// to always log the request ID
module.exports.contextMiddleware = (req, res, next) => {
  const child = logger.child({ requestId: uuid.v4() });
  const store = new Map();
  store.set('logger', child);

  return context.run(store, next);
};
```

> 하위 로거는 로거에 상태를 추가하는 방법입니다. 하위 로거를 사용하여 생성한 속성은 하위 로거를 사용할 때 모든 로그 라인에 출력됩니다. 하위 로거는 로그에 요청 ID를 추가하는 데 탁월한 사용 사례입니다.

또한 프록시를 사용하여 기록 인스턴스가 있는 경우 하위 로거 인스턴스를 사용하여 기록하도록 수정합니다.

마지막으로 `server.js`에서 미들웨어 기능을 사용해 보겠습니다.

```js
// server.js
const express = require('express');
const { logger, contextMiddleware } = require('./logger.js');
const db = require('./db.js');

const PORT = process.env.PORT || 3000;

const app = express();

// Attach a unique request ID to every log line
app.use(contextMiddleware);

app.get('/users/:id', async (req, res) => {
  let userId = req.params.id;

  if (isNaN(userId)) {
    logger.warn({ userId }, 'Invalid user ID');
    return res.status(400).send('Invalid user ID');
  } else {
    userId = Number(userId);
  }

  try {
    logger.info({ userId }, 'Fetching user from DB');
    const user = await db.getUser({ userId });

    if (!user) {
      logger.warn({ userId }, 'User not found');
      return res.status(404).send('User not found');
    }

    logger.debug({ user }, 'User found, sending to client');
    return res.status(200).json(user);
  } catch (error) {
    logger.error(error, 'Failed to fetch user from DB');
    return res.status(500).send('An error occurred while fetching user');
  }
});

app.listen(PORT, () => {
  logger.info(`Server listening at http://localhost:${PORT}`);
});
```

다 됐다! 더 이상 어쩔 수 없다! 이제 모든 로그 라인에 고유한 요청 ID가 첨부되었습니다.

```bash
{...,"requestId":"da672623-818b-4b18-89ca-7eb073accbfe","userId":1,"msg":"Fetching user from DB"}
{...,"requestId":"da672623-818b-4b18-89ca-7eb073accbfe","user":{...},"msg":"User found, sending to client"}
{...,"requestId":"01107c17-d3c8-4e20-b1ed-165e279a9f75","userId":2,"msg":"Fetching user from DB"}
{...,"requestId":"01107c17-d3c8-4e20-b1ed-165e279a9f75","user":{...},"msg":"User found, sending to client"}
```

## 결론

Github의 이 리포지토리에서 완전한 작업 예를 찾을 수 있습니다.

요청 ID 외에도 사용자 ID 및/또는 전자 메일과 같은 모든 로그에 다른 데이터를 포함하는 것이 유용할 수 있습니다. 그러면 요청이 인증되면 바로 인증 미들웨어에 해당됩니다.

이제 여러분은 여러분이 마주치게 될 모든 알려지지 않은 벌레들의 빵 부스러기를 따라갈 모든 것을 준비했습니다.