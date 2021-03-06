---
layout: post
title: "Koa.js의 첫 번째 단계"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/firststepswithkoa.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/firststepswithkoa.png?fit=730%2C412&ssl=1)

웹 개발의 경우 백엔드 API를 실행하고 실행할 수 있는 프레임워크를 선택할 수 있습니다. 반면, 웹 서버 구축을 위한 좋은 내장 메커니즘을 가지고 있는 노드를 제외하고, 프레임워크는 미들웨어, 라우팅 메커니즘 등과 같은 멋진 기능을 제공함으로써 이 작업을 용이하게 한다.

Express는 Node.js 우주에서의 웹 개발 면에서 단연 선두이다. 2009년에 만들어진 것 중 가장 오래된 것이기도 합니다. 이것이 아마도 커뮤니티에서 그렇게 사랑받을 만큼 충분히 성숙하게 만든 이유 중 하나일 것입니다.

Koa.js는 점점 더 많은 공간을 확보하고 있는 또 다른 훌륭한 옵션입니다. 하지만 그들은 실제로 경쟁자가 아니다. 코아는 프런트엔드 커뮤니티의 공통 트렌드인 익스프레스 뒤에 있는 같은 팀이 디자인한 것으로, 예를 들어 노드 뒤에 있는 크리에이터가 같은 마음이었다.

코아는 코딩 면에서 더 작고, 빠르고, 더 표현력 있게 만들어졌다. 웹 API의 개발에 관한 한, Express는 훌륭하지만, 표현력이 약간 부족하고, 몇 분 후에 실제로 보게 될 것처럼 피할 수 있는 몇 가지 상용 코드들을 만들도록 유도합니다.

비동기 기능에 중점을 두고 설계되었기 때문에 콜백을 조작하고 코드 흐름의 오류를 보다 쉽게 처리할 수 있습니다.

이 게시물의 마지막에, 우리는 프레임워크의 몇 가지 멋진 기능을 사용하여 코아 기반 API에서 정보를 검색하는 앱을 가질 것이다. 모의 데이터는 가짜 데이터 생성 웹사이트인 https://randomuser.me/에서 검색될 것이다. 자, 이제 본격적으로 시작해보자!

## 설정 및 구성

노드 우주의 다른 모든 것과 마찬가지로, 설정도 빠르고 쉽습니다. 노드가 설치되어 있는지 확인하고 이 예제를 개발할 기본 설정 폴더를 선택한 후 다음 명령을 실행합니다.

```coffeescript
npm init
npm i koa koa-router koa-ejs axios
```

명령줄에서 노드 프로젝트에 대해 물어볼 정보를 입력하십시오. 모두 비워 두었지만, 원하는 것은 무엇이든 언제든지 알려주십시오.

처음 세 가지 의존성은 코아 관련으로, 프레임워크의 핵심이 되는 첫 번째 종속성입니다.

koa-router는 Express 라우팅 시스템 등가물이다. 예, 패키지에 추가해야 하는 두 번째 패키지에 포함되어 있습니다.제이슨을 따로따로 떼어놓다

Koa-ejs는 보너스다. ejs 프로젝트의 모든 기능에 지원을 추가하는 Koa 미들웨어입니다. 이렇게 하면 클라이언트 프로젝트 없이도 백엔드 웹 앱에 JavaScript 템플릿을 포함할 수 있습니다. 즉, 웹 페이지의 HTML 출력을 렌더링합니다.

마지막으로, 유명한 Axios 라이브러리는 외부 API인 randomuser.me에 전화해야 하기 때문에 여기에 있습니다.

이제 프로젝트의 루트에 새 index.js 파일을 생성하십시오. 다음 코드를 입력합니다.

```js
const koa = require("koa");
const path = require("path");
const render = require("koa-ejs");
const koaRouter = require("koa-router");
const axios = require("axios");

const app = new koa();
const router = new koaRouter();

render(app, {
    root: path.join(__dirname, "views"),
    layout: "index",
    viewExt: "html",
});

router.get("hello", "/", (ctx) => {
    ctx.body = "<h1>Hello World, Koa folks!</h1>";
});

router.get("users", "/users", async (ctx) => {
    const result = await axios.get("https://randomuser.me/api?results=5");

    return ctx.render("index", {
        users: result.data.results,
    });
});

app.use(router.routes()).use(router.allowedMethods());

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`running on port ${PORT}`));
```

이 목록의 처음 몇 줄은 방금 설치한 종속성의 가져오기와 관련이 있습니다. Koa의 주요 객체와 라우터 객체 모두 새로운 연산자를 통해 인스턴스화할 필요가 있다.

koa-ejs의 렌더 함수는 ejs 템플릿 시스템의 매개 변수를 설명하기 위해 여기에 있다. 첫 번째 매개 변수는 최근에 생성된 Koa 앱 개체이며, 두 번째 매개 변수는 ejs 템플릿이 배치된 루트 폴더, 레이아웃 메인 파일, 이러한 파일의 확장자(HTML)와 같은 정보를 포함하는 또 다른 개체이다. 가장 기본적인 구성이지만 다른 사용 가능한 매개 변수를 찾을 수 있습니다.

그런 다음 두 개의 끝점을 만듭니다. 첫 번째는 단순히 루트 끝점에 매핑된 간단한 Hello World인 반면, 두 번째는 좀 더 복잡하며 템플릿 입력에 연결된 임의의 사용자 목록을 반환합니다.

Koa와 Express의 엔드포인트 경로와 얼마나 유사한지 주목하십시오. 첫 번째 매개 변수는 엔드포인트의 정식 이름을 요구하고 두 번째 매개 변수는 이 엔드포인트에 매핑되는 경로 문자열을 요구합니다. 주요 차이점은 ctx 객체인 Koa 컨텍스트에 의해 조작되는 응답 처리를 다루는 콜백 함수(세 번째 매개 변수) 내에 있다.

코아 컨텍스트는 노드의 요청 객체와 응답 객체를 모두 단일 객체로 수용하여 익스프레스가 하는 것과 다르게 접근 방식을 단순화한다. 코아 크리에이터들은 그것들이 너무 자주 사용되기 때문에, 함께 붙는 것이 더 낫다는 것을 발견했습니다.

```php
app.use(async ctx => {
  ctx; // This is the context
  ctx.request; // This is a Koa request
  ctx.response; // This is a Koa response
});
```

그러나 항상 요청 또는 응답 개체를 개별적으로 호출하여 속성에 액세스할 필요는 없습니다. 예를 들어, 요청 본문에 액세스하려면 다음을 직접 수행하십시오.

```cpp
ctx.body; // It delegates to Koa’s ctx.request
```

Ko는 단지 편의를 위해 모든 재산에 대한 접근을 마법처럼 위임한다.

코아는 또한 당신이 비동기식 기능을 사용할 것인지 아닌지를 결정할 수 있게 해준다. 두 번째 끝점을 예로 들어 보겠습니다. 콜백 기능 내에서 외부 URL을 호출하여 랜덤 사용자 어레이를 가져와 koa-ejs 템플릿으로 직접 전송합니다(몇 분 후에 구축).

이것은 약속을 기반으로 한 Express 1에 비해 훨씬 깔끔한 접근 방식입니다. Express를 사용하면 다음과 같은 이점이 있습니다.

```coffeescript
app.get('/users', (req, res, next) => {
  axios.get("https://randomuser.me/api?results=5").then((result) => {
    res.status(200).json(result.data.results);
  }).catch((err) => next(err));
});
```

이 예는 간단하기 때문에 첫눈에 봐도 괜찮아 보일 수 있습니다. 그러나 데이터베이스에서 데이터 검색을 추가하는 등 개발이 복잡해지면 다음과 같은 문제가 발생할 수 있습니다.

```coffeescript
app.get('/users', (req, res, next) => {
  MyDatabaseObj.findByPk(myPk).then(data => 
    if (!data) {
      return res.status(404).send({});
    }

    axios.get("https://randomuser.me/api?results=5&id=" + data.id).then((result) => {
      res.status(200).json(result.data.results);
    }).catch((err) => next(err));    
  );
});
```

이러한 스타일은 개발자가 유지 관리하기 어려운 많은 코드 블록을 중첩하도록 유도할 수 있습니다.

index.js 코드로 돌아가면 나머지 코드는 Express에서 수행하는 것과 매우 유사합니다. 코아 미들웨어는 사용() 방식으로 명시해야 한다. 서버를 설정하고 실행하려면 `수신()` 방법을 통해 서버를 시작하십시오. 아주 비슷하죠, 그렇죠?

## 템플릿

API 생성을 마쳤으니 이제 ejs 템플릿으로 넘어가겠습니다. 먼저 프로젝트 루트에 보기라는 폴더를 새로 만들어야 합니다. 그리고 그 안에 index.html이라는 메인 파일을 만듭니다.

템플릿 시스템을 최소로 분해하는 것은 기사의 초점이 아니기 때문에, 우리는 하나의 파일을 템플릿으로 고수할 것입니다. 이 코드를 추가해야 합니다.

```xml
<html>
    <head>
        <title>First Steps with Koa</title>
        <link
            rel="stylesheet"
            href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
        />
    </head>
    <body>
        <div class="container">
            <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
                <h1 class="display-4">Koa.js</h1>
                <p class="lead">
                    A simple list of users retrieved from
                    <a href="https://randomuser.me/">randomuser.me</a>
                </p>
            </div>

            <div class="list-group">
                <% users.forEach( function(user) { %>
                <div class="list-group-item">
                    <div class="d-flex w-100 justify-content-between">
                        <h5 class="mb-1">
                            <%= user.name.title %> <%= user.name.first %> <%= user.name.last
                            %>
                        </h5>
                        <small
                            >Registered at: <%= new
                            Date(user.registered.date).toISOString().split('T')[0] %></small
                        >
                    </div>
                    <p class="mb-1">
                        <strong>Address:</strong>
                        <%= user.location.street.number %> <%= user.location.street.name
                        %>, <%= user.location.city %> <%= user.location.postcode %> - <%=
                        user.location.country %>
                    </p>
                    <small>Phone: <%= user.phone %></small>
                </div>
                <% }); %>
            </div>
        </div>
    </body>
</html>
```

일반 HTML이므로 여기에 새로울 것이 없습니다. 맨 처음에, 헤드 태그 안에, 우리는 부트스트랩의 CSS 스타일시트를 추가하고 있습니다. 예제 페이지에 스타일링을 입력하는 데 도움이 됩니다.

koa-ejs 라이브러리는 표현식을 사용하여 HTML과 자바스크립트 코드를 구분한다.

새 JavaScript 코드 블록을 열고 싶을 때마다 `<% ... %> 연산자로 코드를 감습니다. 그렇지 않은 경우 일부 값을 HTML로 직접 인쇄하려면 `=%= … %` 연산자를 사용하십시오. 다른 JavaScript 코드 블록과 마찬가지로, 일반적으로 JavaScript 코드 파일에서와 같이 블록이 제대로 열리고 닫힙니다.

계속하여 우리가 하고 있는 반복 작업을 살펴보십시오. 자바스크립트 코드는 간단합니다.

자, 이제 시험해 봅시다. 콘솔에서 `node index.js` 명령을 실행하여 애플리케이션을 실행하고 브라우저에서 http://localhost:3000/ 주소를 입력합니다. 그러면 다음과 같은 출력이 생성됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/helloworldkoa.png?resize=860%2C370&ssl=1)

그런 다음 주소를 http://localhost:3000/users로 변경합니다. 브라우저를 새로 고칠 때마다 사용자 목록이 변경될 수 있습니다.

## 실수는 어떻게 하죠?

오류는 웹 응용 프로그램에서 매우 중요한 부분입니다. Express와 마찬가지로 각 엔드포인트에서 항상 개별적으로 처리할 수 있지만, Koa는 앱 오류를 처리할 중앙 집중식 미들웨어를 만들 수도 있습니다.

다음 코드 조각을 보십시오.

```js
app.use(async (ctx, next) => {
    try {
        await next();
    } catch (err) {
        console.error(err);
        ctx.body = "Ops, something wrong happened:<br>" + err.message;
    }
});
```

이 미들웨어는 다른 미들웨어보다 먼저 배치되어야 하므로, 다른 미들웨어와 관련된 오류를 포함하여 앱에서 발생할 수 있는 모든 종류의 오류를 잡고 기록할 수 있습니다.

엔드포인트가 매핑된 두 개 이상의 JavaScript 파일이 있는 경우 이 미들웨어를 한 곳에 중앙 집중화하여 모두로 내보낼 수 있습니다.

## 통나무는요?

네, Koa는 개발 및 디버깅을 위한 보다 세분화된 로깅 메커니즘을 다루는 또 다른 패키지도 가지고 있습니다.

이를 위해서는 먼저 적절한 종속성을 설치해야 합니다.

```coffeescript
npm i koa-logger
```

그리고 다음 내용을 index.js에 추가하십시오.

```php
// At the beginning of the file
const Logger = require("koa-logger");

...

app.use(Logger())
    .use(router.routes())
    .use(router.allowedMethods());
```

또한 `로거` 미들웨어가 가장 먼저 추가되었는지 확인하십시오. 그렇지 않으면 코아는 당신의 경로와 HTTP 호출의 로그를 듣지 않습니다.

앱을 다시 시작하고 엔드포인트에 다시 액세스하면 다음과 유사한 로그가 표시될 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/runnningport.png?resize=1560%2C328&ssl=1)

## 결론

이것은 Koa.js의 능력에 대한 간단한 소개와 코드 구성에 관한 한 어떻게 더 명확하게 할 수 있는지에 대한 것이었습니다. 이러한 장점을 더 많이 보려면 Sysuppleize for 데이터베이스 처리, Jasmine 테스트 등과 같은 일반적인 프레임워크를 사용하는 것이 필수적입니다. 또는 이러한 프레임워크가 현재 프로젝트에 포함되어 있는지 테스트해 볼 수도 있습니다. 그냥 포크를 만들고 도전해 보세요!

Koa는 CSRF와 JWT(JSON Web Token), 액세스 로그, charset 변환, 캐싱, 미니어처, 압축기 등과 같은 추가 기능들을 지원한다. 이에 대한 자세한 내용은 GitHub 공식 프로젝트를 참조하시기 바랍니다. 행운을 빌어요!