---
layout: post
title: "Sails.js를 사용하여 Node.js 웹 API 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/buildinganodejsapiwithhsailsjs.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/buildinganodejsapiwithhsailsjs.png?fit=730%2C412&ssl=1)

서버뿐만 아니라 클라이언트에서도 JavaScript를 작성할 수 있다는 것은 소규모 팀과 대규모 팀 모두에게 매우 좋은 활용 방안이 될 수 있습니다. 그러나 이미 아름답게 생성된 애플리케이션, 프런트 엔드, 웹 또는 모바일 애플리케이션을 지원하기 위해 프로덕션 가능한 Node.js 웹 API를 구축하는 것은 일반 개발자에게 쉬운 작업이 아닙니다. 다음과 같은 오버헤드가 발생할 수 있습니다.

- 선택할 수 있는 수많은 프레임워크(고속, 하피, 코아 등)
- 명확한 지침 없이 프로젝트에 필요한 패키지를 조사하고 집계해야 함
- 프로젝트 코드베이스를 구성하는 모범 사례를 결정해야 함
- 어떤 ORM을 사용해야 합니까, 아니면 원시 쿼리를 써야 합니까? 🤔

이 기사에서는 Node.js에 대한 MVC 프레임워크인 Sails.js 프레임워크를 사용하여 프로덕션 가능한 Node.js 웹 API를 구축하는 방법에 대해 배울 것이다. 웹 API 서비스를 위한 사용자 관리 엔드포인트를 구축할 예정입니다. 다음과 같은 기능을 구현할 수 있도록 엔드포인트를 구축합니다.

- 사용자 등록
- 사용자 로그인
- 이메일 확인
- 암호 잊기/재설정

우체부를 사용하여 엔드포인트를 테스트할 것입니다. 마지막으로, 우리는 헤로쿠에 배치할 것이다. 먼저 Sails.js를 소개합니다.

## 전제조건

이 튜토리얼은 독자가 Node.js 및 서버측 개발에 상당히 익숙하다고 가정한다.

## Sails.js란?

Sails.js 또는 Sails는 프로덕션 준비 엔터프라이즈 Node.js 애플리케이션을 구축하기 위한 실시간 MVC 프레임워크이다. 2015년 Mike McNeil과 Sails Company가 Ruby on Rails MVC 프레임워크에서 영감을 받아 제작하였다. 돛은 웹 소켓 지원 기능을 갖추고 있어 실시간 채팅 앱, 게임 등을 구축하는 데 적합합니다. 돛은 다음과 함께 배송됩니다.

- 애플리케이션의 데이터 계층을 산뜻하게 만드는 Waterline(Node.js용 어댑터 기반 ORM)이라는 ORM
- 돛이라는 CLI 툴은 새 돛 애플리케이션을 설계하고, 컨트롤러 작업을 생성하고, 개발 서버, 데이터베이스 마이그레이션 등을 시작할 수 있도록 지원합니다.

Sails는 Postman, Paystack, devmountain과 같은 회사들이 그들의 다양한 고객들에게 힘을 실어주기 위해 웹 API를 구축하는 데 야생에서 활발히 이용되고 있다.

> 참고: Sails는 널리 사용되는 ExpressNode.js 프레임워크 위에 구축되었으므로 Express를 사용하여 Node.js 애플리케이션을 구축한 경우 친숙함을 느낄 수 있습니다.

## 시작 중

Seats에서 웹 API 구축을 시작하려면 Sails CLI 도구를 설치해야 합니다. 우리는 터미널에서 아래 명령을 실행하여 그렇게 할 것입니다.

> Node.js(버전 13.12.0)가 설치되어 있다고 가정합니다.

```coffeescript
npm install -g sails
```

위의 명령은 시스템에 전체적으로 Sails CLI 도구를 설치합니다. 돛이 설치되어 있는지 확인하려면 다음을 실행하십시오.

```undefined
sails -v
```

모든 작업이 성공적으로 완료된 경우 위의 명령은 버전 번호를 반환해야 합니다.

## 새 돛 애플리케이션 생성

새 돛 응용프로그램을 만들려면 응용프로그램 이름으로 전달되는 `sails new` 명령을 실행합니다. 이 명령은 새 Sails 앱을 생성하고 `npm install`을 실행하여 모든 종속성을 설치합니다.

```coffeescript
sails new <appname>
```

새 명령은 옵션 플래그도 포함하며, 우리는 Sails에게 "자산", "보기" 또는 "작업" 폴더를 생성하지 말라고 지시하기 위해 "-frontend" 플래그를 사용할 것이다. 웹 API를 구축하기 때문에 원하는 것입니다. 명령을 다음과 같이 실행합니다.

```coffeescript
sails new logrocket-sails-api --no-frontend
```

> 팁: -fast 플래그를 사용하여 새 명령을 실행하여 sails 앱을 생성하고 종속성 설치를 건너뛸 수 있습니다.

이제 logrocket-sails-api 디렉토리에 cd를 넣고 편집기에서 앱을 열 수 있습니다. 우리는 VS Code를 사용하게 될 것이고, 그래서 그냥 `code`를 실행하여 VS Code에서 현재 디렉토리를 열 것입니다.

## 프로젝트코드베이스구조

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/sailscodenamestructure-nocdn.png)

위의 스크린샷은 우리에게 생성된 sails new 명령을 보여준다. 몇 가지 파일 및 폴더에 대해 살펴보겠습니다.

- `api/` – 대부분의 개발 시간을 사용할 디렉토리입니다. 기본적으로 `컨트롤러/helpers/helpers` 모델/hales 및 `policy/` 하위 디렉터리가 포함되어 있습니다.
- `컨트롤러/` – 이 디렉토리에는 Sails 앱의 모든 컨트롤러가 들어 있습니다.
- 도움말/ - 돛에서 도움말은 노드 머신 사양을 따르는 재사용 가능한 코드 조각입니다. `helpers/` 디렉토리에는 프로그램에서 정의하는 모든 도우미가 포함됩니다.
- `models/` – 이 디렉터리에는 응용 프로그램 Waterline 모델이 포함됩니다. 워터라인 모델은 일반적으로 데이터베이스 테이블에 대한 데이터베이스 스키마를 포함하는 `.js` 파일입니다.
- `정책/` – 정책은 승인 및 액세스 제어를 위한 돛 장치입니다. 이 폴더에서 해당 항목을 정의합니다.
- `config/` – 이 디렉터리에는 돛 앱에 대한 모든 구성 파일이 포함되어 있습니다. 주목할 만한 한 가지 파일은 `routs`이다.Seats 응용 프로그램이 처리할 경로를 선언하는 데 사용되는 js` 구성 파일
- `config/datastore.js` – 이 구성 파일에서 Waterline에서 사용할 데이터베이스 어댑터를 정의합니다(나중에 추가 사항).
- `config/policies.js` – 여기서 정책 및 정책이 보호할 작업 또는 컨트롤러를 정의합니다.

> 돛 문서에서 다른 파일과 폴더에 대한 자세한 내용을 볼 수 있습니다.

## 첫 번째 끝점

지금 만약 당신이 `sails lift`에 기대어가 1337``localhost 방문하는 돛 개발 서버 시작하기 때문에 우리는 아직 응용 프로그램에 대한 경로 정의되지 못하면 `404` 것이다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/sailslift-nocdn.png)

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/notfound-nocdn.png)

> 팁: '세일즈 리프트 - 포트 <'를 실행하여 청취할 포트 돛을 변경할 수 있습니다.

첫 번째 끝점을 만들어 보겠습니다. 일반적으로 `routes.js`를 누르고 엔드포인트를 추가해야 합니다. `routes.js` 파일은 사전/개체를 사용하며 여기서 키는 `/`와 같은 경로 끝점이고 값은 해당 경로 요청을 처리할 컨트롤러 작업입니다. 경로 키에는 Sails(GET|POST|PATCH|DELETE)가 지원하는 HTTP 동사가 있어야 하며, 그 다음에 경로가 나오는 공백이 있어야 합니다.

응용 프로그램의 `/` 인덱스에 대한 경로를 추가하겠습니다. `config/routes.js`를 열고 다음 코드 조각을 추가하십시오.

```bash
"GET /": "home/index"
```

위의 경로가 의미하는 것은 GET 요청의 경우 홈 컨트롤러의 인덱스 작업(방법)이 해당 요청을 처리한다는 것입니다.

> 이제 Sails에서는 컨트롤러 작업(방법)을 단일 컨트롤러 파일로 정의하는 대신 별도의 파일에 유연하게 지정할 수 있다는 점을 언급하기에 좋습니다. 이 접근 방식을 사용하면 각 작업 파일에 초점을 맞출 수 있도록 작업을 보다 쉽게 읽을 수 있습니다. 우리는 컨트롤러와 내부의 '.js' 파일을 나타내는 디렉토리를 동작으로 갖는 이 방식을 취할 것이다.

`routes.js` 파일은 다음과 같아야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/routesjs.png?resize=1922%2C1057&ssl=1)

첫 번째 끝점을 만드는 작업이 절반 정도 완료되었습니다. 다음 단계는 인덱스 작업을 생성하는 것입니다. 우리는 Sails CLI `generate` 명령을 사용하여 `controllers/home` 디렉토리에 작업을 생성할 것입니다. 다음 명령을 실행합니다.

```undefined
sails generate action home/index
```

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/sailsuccessfullygenerated-nocdn.png)

Sails는 그것을 포함하기에 충분히 영리하기 때문에 우리는 파일 확장자를 통과하지 않을 것이다. 따라서 일반적으로 경로 속성의 경우처럼 인수를 전달합니다.js

위의 명령을 실행하면 `controllers/` 디렉토리에 새 폴더 `home`이 추가되고 그 안에 `index.js` 액션이 있어야 한다. 다음은 `index.js` 작업의 기본 내용입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/inputsundefined.png?resize=1922%2C1057&ssl=1)

돛의 동작 및 도우미 형식은 노드 머신 사양을 따릅니다. 이 사양을 이해하는 데 도움이 되는 문서를 여기에서 찾을 수 있습니다.

우리는 `/` 끝점에 요청이 있을 때 JSON 응답을 반환하려고 합니다. 다음과 같이 보이도록 `home/index.js`를 수정합니다.

```js
module.exports = {
  friendlyName: 'Index',
  description: 'Index home.',
  inputs: {
  },
  exits: {
  },
  fn: async function (_, exits) {
    // All done.
    exits.success({message: 'LogRocket Sails API'});
  }
};
```

기본적으로 돛 동작은 `출구`를 사용합니다.모든 요청에 대한 성공 응답을 제공합니다. 따라서 사용자가 "/" 또는 "웹 API"를 방문할 때 "property" 메시지가 포함된 JSON 페이로드가 응답으로 다시 전송됩니다.

이제 본격적으로 사용자 관리 및 인증 기능을 구현해 보겠습니다. 등록 끝점부터 시작하겠습니다.

## 새 사용자 등록 끝점

시작하려면 이 엔드포인트에서 수행할 작업에 대해 설명하겠습니다.

- 데이터베이스 연결 설정
- 데이터베이스에 새 사용자 레코드 저장
- 새 사용자에게 이메일 확인 보내기
- 등록에 성공한 후 사용자에게 응답 보내기
- 등록 프로세스 중에 오류가 발생한 경우 선택적으로 다시 전송

## 데이터베이스 연결 설정

우리는 Postgre를 사용할 것이다.SQL. 이 튜토리얼에 따라 Postgre를 설치할 수 있습니다.시스템의 SQL입니다.

Sails는 공식 Postgre를 가지고 있다.Waterline용 SQL 어댑터는 다음을 실행하여 npm 패키지를 설치하는 것과 동일한 방식으로 설치됩니다.

```undefined
npm install sails-postgresql --save
```

어댑터를 설치한 후 `config/datastore.js`에 어댑터 이름과 로컬 데이터베이스 연결 URL을 제공해야 합니다.

이전에 Postgres 데이터베이스를 생성했어야 합니다. 도움이 필요하면 여기를 참조하십시오.

다음 행의 압축을 풀어 `config/datastore.js`를 수정합니다.

```bash
// adapter: 'sails-mysql',
// url: 'mysql://user:password@host:port/database',
```

다음으로 교체:

```undefined
adapter: 'sails-postgresql',
url: 'postgres://logrocket_sails_api:logrocketsailsapi@localhost:5432/logrocket_sails_api', // Replace with your own connection URL
```

Postgre의 연결 URLSQL은 일반적으로 다음과 같은 형식을 사용합니다.

```undefined
"postgres://{user}:{password}@{hostname}:{port}/{database-name}"
```

Sails 개발 서버가 아직 실행 중이면 이를 처치하고 sails lift를 실행하여 sails를 다시 들어올립니다.

> 팁: VS 코드를 사용 중인 경우, Sailboat – VS 코드용 Sails 도구를 설치하고 명령 팔레트에서 Sails 개발 서버를 시작할 수 있습니다.

돛이 오류 없이 실행되면 데이터베이스가 성공적으로 연결되었습니다.

다음 단계에서 사용자 스키마를 만드는 것으로 넘어갑시다.

## 스키마 선언 및 사용자 모델

포스트그레 이후SQL은 명시적으로 정의된 스키마가 없으면 해당 테이블에 테이블이나 새 레코드를 만들 수 없는 스키마 기반 데이터베이스입니다. 워터라인은 해당 스키마를 정의하고 자바스크립트 객체를 정의하기 위한 API를 제공한다.

사용자 테이블의 경우 `full_name`, `eemail_status`, `eemail_proof_token`, `eemail_proof_token_expires_at`, `password_reset_token` 및 `password_reset_token_atemeres_at`라는 열이 필요합니다.

다음을 실행하여 사용자 모델을 생성하겠습니다.

```undefined
sails generate model user
```

Sails는 `models/User.js`에서 `User.js`라는 이름의 모델 파일을 생성한다. 이 파일을 열고 `속성` 속성 내에 다음 코드를 추가하십시오.

```css
fullName: {
      type: 'string',
      required: true,
      columnName: 'full_name'
    },
    email: {
      type: 'string',
      required: true,
      unique: true,
    },
    emailStatus: {
      type: 'string',
      isIn: ['unconfirmed', 'confirmed'],
      defaultsTo: 'unconfirmed',
      columnName: 'email_status'
    },
    emailProofToken: {
      type: 'string',
      description: 'This will be used in the account verification email',
      columnName: 'email_proof_token'
    },
    emailProofTokenExpiresAt: {
      type: 'number',
      description: 'time in milliseconds representing when the emailProofToken will expire',
      columnName: 'email_proof_token_expires_at'
    },
    password: {
      type: 'string',
      required: true
    }
passwordResetToken: {
      type: 'string',
      description:
        'A unique token used to verify the user\'s identity when recovering a password.',
      columnName: 'password_reset_token',
    },
    passwordResetTokenExpiresAt: {
      type: 'number',
      description:
        'A timestamp representing the moment when this user\'s `passwordResetToken` will expire (or 0 if the user currently has no such token).',
      example: 1508944074211,
      columnName: 'password_reset_token_expires_at',
    },
```

워터라인 속성 정의 규격을 사용하여 사용자 테이블의 열을 나타내는 모델 속성을 정의했습니다.

> 'columnName' 속성을 사용하여 속성 값을 지정하지 않으면 돛이 속성 키를 열 이름으로 사용합니다.

Sails에서 모델 속성 설정에 대한 자세한 내용은 Waterline 속성 설명서를 참조하십시오.

이 문제를 마무리하기 전에 모델의 테이블 이름을 지정해야 합니다. 그러면 워터라인에게 테이블이 어떤 이름이 될 것인지 알려줄 것입니다. 따라서 User.js를 수정하고 다음 속성을 추가합니다.

```undefined
tableName: "users"
```

이제 `User.js`는 다음과 같이 표시됩니다.

```bash
/**
 * User.js
 *
 * @description :: A model definition represents a database table/collection.
 * @docs        :: https://sailsjs.com/docs/concepts/models-and-orm/models
 */
module.exports = {
  tableName: "users",
  attributes: {
    fullName: {
      type: 'string',
      required: true,
      columnName: 'full_name'
    },
    email: {
      type: 'string',
      required: true,
      unique: true,
    },
    emailStatus: {
      type: 'string',
      isIn: ['unconfirmed', 'confirmed'],
      defaultsTo: 'unconfirmed',
      columnName: 'email_status'
    },
    emailProofToken: {
      type: 'string',
      description: 'This will be used in the account verification email',
      columnName: 'email_proof_token'
    },
    emailProofTokenExpiresAt: {
      type: 'number',
      description: 'time in milliseconds representing when the emailProofToken will expire',
      columnName: 'email_proof_token_expires_at'
    },
    password: {
      type: 'string',
      required: true
    }
  },
};
```

돛을 사용하여 레코드를 응답으로 반환할 때 반환할 필드를 사용자 정의할 수 있습니다. 사용자 레코드를 반환할 때 이 기능을 사용하여 암호 필드를 제거합니다. 추가해 봅시다. 속성 후:{}` 다음을 추가합니다.

```js
customToJSON: function () {
    return _.omit(this, ["password"]);
  },
```

우리는 User.js 모델의 인스턴스를 JSON으로 변환할 때 항상 `password` 속성을 생략해야 한다고 Sails에 말하고 있다.

마지막으로 User.js에서 할 일은 비밀번호를 저장하기 전에 암호화하는 것입니다. 컨트롤러 작업에서 이 작업을 수행할 수 있지만 Waterline 라이프사이클 후크를 사용할 예정입니다. Waterline은 모델에 `BeforeCreate` 라이프사이클 후크를 제공하여 레코드가 생성되기 전에 일부 작업을 수행할 수 있도록 합니다.

암호를 암호화하기 위해 돛-후크-organics라는 돛 후크를 설치할 것이다(후크는 돛 능력을 확장하기 위한 돛 메커니즘이다). 이러한 후크는 암호를 해시하고 암호를 비교하며 토큰에 필요한 임의의 문자열을 만드는 데 도움이 됩니다. 설치 방법:

```undefined
npm install sails-hook-organics --save
```

설치가 완료되면 `사용자 정의` 다음에 이 코드를 추가합니다.JSON의 속성으로:

```js
// LIFE CYCLE
beforeCreate: async function (values, proceed) {
  // Hash password
  const hashedPassword = await sails.helpers.passwords.hashPassword(
    values.password
  );
  values.password = hashedPassword;
  return proceed();
},
```

데이터베이스를 마이그레이션하겠습니다! 돛을 다시 시작하여 돛을 다시 올리면 된다. 그러나 Sails는 다음 중 하나여야 하는 마이그레이션 전략을 입력하라는 메시지를 표시합니다.

- 변경(삭제/삭제 후 모든 데이터 재삽입 시도(권장)
- 돛을 올릴 때마다 모든 데이터를 삭제/삭제), 안전(데이터를 자동으로 마이그레이션하지 않음, 직접 실행)

메시지가 나타나면 개발에 권장되는 "변경"을 입력합니다.

> 팁: 돛을 올릴 때마다 이 프롬프트가 나타나지 않도록 하려면 다음 줄을 주석해 'config/model.js'에서 마이그레이션 전략을 명시적으로 설정해야 합니다.

```bash
// migrate: 'alter',
```

또한 `config/model.js`의 주석:

```bash
// schema: true,
```

마지막으로, 우리는 열 이름에 snake_case를 사용하기 때문에 두 열 모두에 `columnName`을 추가하여 `config/models.js`에서 자동 생성된 속성, `createdAt` 및 `updateAt`를 수정할 것이다. 다음과 같은 방식을 사용해야 합니다.

```css
createdAt: { type: 'number', autoCreatedAt: true, columnName: 'created_at'},
updatedAt: { type: 'number', autoUpdatedAt: true, columnName: 'updated_at'},
```

마지막으로 `세일즈 리프트`를 실행하여 변경 내용을 데이터베이스에 적용합니다.

## 레지스터 끝점 및 컨트롤러 설정

이제 사용자 모델을 모두 설정하고 쿼리 준비를 마쳤습니다. 새 사용자를 생성하기 위한 경로 및 컨트롤러(작업)를 생성하겠습니다. 먼저 경로를 `경로`로 선언하겠습니다.js:

```bash
'POST /user/register': 'user/register'
```

아래 명령을 실행하여 레지스터 작업을 생성합니다.

```cpp
sails generate action user/register
```

user/register.js 작업 파일을 엽니다. 돛에서 POST 요청과 같은 데이터가 필요한 경우 작업의 입력 필드에 해당 데이터를 정의합니다. 정의의 구문은 앞에서 User.js 모델에서 했던 속성 정의와 비슷하다. `입력`에 다음 코드를 추가합니다.{}은(는) 다음과 같습니다.

```css
inputs: {
    fullName: {
      type: 'string',
      required: true,
    },
    email: {
      type: 'string',
      required: true,
      unique: true,
      isEmail: true,
    },
    password: {
      type: 'string',
      required: true,
      minLength: 6,
    },
  },
```

돛을 사용하면 일부 유효성 검사 속성을 설정하여 각 입력의 유효성을 확인할 수 있습니다. 다음으로, 엔드포인트에 대해 가능한 결과인 `출구`를 설정하겠습니다. 성공 종료 호출은 요청이 성공했음을 의미하며, 웹 API는 200으로 응답합니다. 그러나 상태 코드는 201을 반환하려는 새 레코드를 만들기 때문에 사용자 정의할 수 있습니다. 또한 전자 메일이 고유 필드이므로 사용자가 전자 메일에 이미 있는 경우에 대한 응답을 설정하려고 합니다. 마지막으로, 예상하지 못한 다른 오류에 대한 캐치 올(catch-all) 역할을 하는 일반적인 오류 종료가 있습니다. `exit`에 다음 코드를 추가합니다.{}은(는) 다음과 같습니다.

```css
success: {
     statusCode: 201,
     description: 'New muna user created',
   },
   emailAlreadyInUse: {
     statusCode: 400,
     description: 'Email address already in use',
   },
   error: {
     description: 'Something went wrong',
   },
```

fn 비동기 함수의 exit를 사용하려면 먼저 exit를 항상 인수로 전달해야 합니다. 그렇지 않으면 작동하지 않습니다.

이제 `fn` 비동기 기능으로 넘어갑시다. 먼저 트라이어치 블록 선언부터 하겠습니다. 이 블록에서는 e-메일이 모두 소문자인지 확인하는 것으로 시작합니다.

```cpp
const newEmailAddress = inputs.email.toLowerCase();
```

그런 다음 전자 메일 확인에 사용할 이 사용자에 대한 토큰을 만듭니다.

```undefined
const token = await sails.helpers.strings.random('url-friendly');
```

다음에 모델 방법 `create()를 사용합니다. 사용자 모델에서는 새로운 레코드를 만들지만, 그 전에 토큰의 만료 시간을 `config/custom`에 추가해야 합니다.js` – 이 파일을 사용하여 개발 중에 프로그램에서 액세스할 수 있는 값을 정의합니다. `config/custom.js`로 이동하여 다음을 추가하십시오.

```cpp
emailProofTokenTTL: 24 * 60 * 60 * 1000, // 24 hours
```

그런 다음 `사용자/등록`으로 돌아갑니다.이제 새 사용자 레코드를 생성할 수 있습니다.

```js
let newUser = await User.create({
        fullName: inputs.fullName,
        email: newEmailAddress,
        password: inputs.password,
        emailProofToken: token,
        emailProofTokenExpiresAt:
          Date.now() + sails.config.custom.emailProofTokenTTL,
      }).fetch();
```

다음으로 사용자에게 전송되는 확인 링크를 구성해야 합니다.

```js
const confirmLink = `${sails.config.custom.baseUrl}/user/confirm?token=${token}`;
```

`config/custom`에서 `baseUrl`을 정의하지 않은 것을 볼 수 있습니다.js` 코드가 끊어집니다:

```bash
baseUrl: 'http://localhost:1337'
```

> 참고: 엔드포인트를 확인하는 작업이 아직 없습니다.

다음으로 해야 할 일은 이메일을 설정하고 보내는 것입니다.

```undefined
const email = {
        to: newUser.email,
        subject: 'Confirm Your account',
        template: 'confirm',
        context: {
          name: newUser.fullName,
          confirmLink: confirmLink,
        },
      };
await sails.helpers.sendMail(email);
```

마지막으로, 사용자에게 응답을 보내 새 사용자를 모두 생성했음을 표시합니다.

```css
return exits.success({
      message: `An account has been created for ${newUser.email} successfully. Check your email to verify`,
    });
```

오류 처리 방법을 살펴보겠습니다. 캐치 블록에서 다음 코드를 추가하여 기존 전자 메일에 등록하려고 시도한 결과 오류가 발생했는지 먼저 확인하십시오.

```bash
if (error.code === 'E_UNIQUE') {
        return exits.emailAlreadyInUse({
          message: 'Oops :) an error occurred',
          error: 'This email address already exits',
        });
}
```

마지막으로, 다른 모든 오류에 `exit.error`로 응답합니다.

```css
return exits.error({
      message: 'Oops :) an error occurred',
      error: error.message,
});
```

## 전자 메일 보내기

앱에 대한 이메일을 보내지 않았기 때문에 아직 위의 등록을 테스트할 수 없습니다. 그렇게 합시다.

no e-mailer를 사용하여 e-메일 전송 흐름에 전원을 공급합니다. 우리는 또한 "노드메일" 전송으로 SendGrid를 사용할 것이다. e-메일을 작성하기 위해 nodemailer용 핸들바 플러그인을 사용하여 .hbs 템플릿 파일을 e-메일로 컴파일합니다. 먼저 다음 패키지를 설치하십시오.

```undefined
npm install nodemailer nodemailer-express-handlebars nodemailer-sendgrid --save
```

설치 후 우리는 이러한 이메일을 보내는 데 필수적인 SendGrid API 키를 생성할 것입니다. 이 API 키는 `config/local.js`에 추가하겠습니다.

> local.js는 개발에서만 사용할 수 있으므로 존재하지 않는 경우 'local.js'를 생성하고 이 앱을 로컬로 테스트하려면 SendGrid API 키를 제공해야 합니다.

### 메일 도우미

연락드립니다.

```css
await sails.helpers.sendMail(email);
```

이 도우미를 아직 만들지 않았습니다. 이를 위해 다음을 실행합니다.

```undefined
sails generate helper send-mail
```

이렇게 하면 `helpers/` 디렉토리에 `send-mail.js`가 생성됩니다. 내용을 다음과 같이 바꿉니다.

```js
const nodemailer = require("nodemailer");
var nodemailerSendgrid = require("nodemailer-sendgrid");
const hbs = require("nodemailer-express-handlebars");
module.exports = {
  friendlyName: "Send mail",
  description: "",
  inputs: {
    options: {
      type: "ref",
      required: true,
    },
  },
  exits: {
    success: {
      description: "All done.",
    },
  },
  fn: async function (inputs) {
    const transporter = nodemailer.createTransport(
      nodemailerSendgrid({
        apiKey: sails.config.sendGridAPIkey || process.env.SENDGRID_API_KEY,
      })
    );
    transporter.use(
      "compile",
      hbs({
        viewEngine: {
          extName: ".hbs",
          partialsDir: "./views",
          layoutsDir: "./views",
          defaultLayout: "",
        },
        viewPath: "./views/",
        extName: ".hbs",
      })
    );
    try {
      let emailOptions = {
        from: "LogrocketSailsAPI <alert@logrocketsailsapi.com>",
        ...inputs.options,
      };
      await transporter.sendMail(emailOptions);
    } catch (error) {
      sails.log(error);
    }
  },
};
```

이 도우미는 제목, 템플릿 파일 등과 같은 전자 메일 전송과 관련된 정보를 포함하는 개체이므로 `옵션`을 사용합니다. `transporter.use` 방법에서는 이 디렉토리가 생성되었는지 확인하기 위해 nodemailer에게 보기 디렉토리에서 .hbs 파일을 찾도록 지시하고 있다.

마지막으로 확인 이메일 템플릿 `.hbs` 파일을 생성하겠습니다. 그런 다음 다음 템플릿을 추가하십시오.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
        body {
            padding: 10px 20px;
            font-family: Arial, Helvetica, sans-serif;
        }
        .cta {
            text-decoration: none;
            background-color: rgb(76, 119, 175);
            color: #fff !important;
            font-weight: bold;
            padding: 10px 20px;
            border-radius: 5px;
            margin: 0 auto;
        }
        footer {
            margin-top: 2rem;
        }
        p {
            margin: 30px 0;
        }
    </style>
</head>
<body>
    <section class="hero">
        <h1 class="title">Welcome, { name }</h1>
    </section>
    <main>
        <p>Please confirm your account by clicking the button below:</p>
        <a class="cta" href="{confirmLink}" target="_blank">Confirm email</a>
        <p>Once confirmed, you'll be able to log in.</p>
    </main>
    <footer>
        <p>🖤 Love,</p>
    </footer>
</body>
</html>
```

메일 발송 시 이메일 옵션의 일부로 전달된 사용자에게 이름 및 링크 확인 기능을 제공하기 위해 핸들바의 보간법을 사용하고 있습니다.

다 끝냈어! 그래서 지금 우리는 등록 절차를 완료했습니다. 이 끝점을 테스트하려면 `세일즈 리프트`를 실행하고 포스트맨을 엽니다. 활성 전자 메일을 입력하면 사용자가 등록되고 전자 메일도 사용자에게 전송됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/sailsliftinpostman.png?resize=730%2C411&ssl=1)

## 이메일 확인

`routes.js`를 열고 다음 항목을 경로 사전에 추가합니다.

`GET/user/confirm`: `user/confirm`

그런 다음 `sails generate action user/confirm`을 실행하여 확인 작업을 비계합니다.

우리는 이 경로가 이메일의 확인 토큰인 토큰이라는 단일 쿼리 매개변수를 채택하기를 원합니다. 따라서 입력에 단일 입력을 추가합니다.{}. 다음을 추가합니다.

```css
token: {
    type: 'string',
    description: "The confirmation token from the email.",
    example: "4-32fad81jdaf$329",
  },
```

또한 두 개의 출구를 추가합니다.

```css
success: {
      description: "Email address confirmed and requesting user logged in.",
    },
  invalidOrExpiredToken: {
    statusCode: 400,
    description:
      "The provided token is expired, invalid, or already used up.",
  },
```

`fn` 기능에서는 요청에 토큰 매개 변수가 포함되어 있지 않은지 확인하는 것으로 시작합니다.

```bash
if (!inputs.token) {
      return exits.invalidOrExpiredToken({
        error: "The provided token is expired, invalid, or already used up.",
      });
    }
```

그런 다음 데이터베이스에서 토큰을 받은 사용자를 가져옵니다.

```undefined
var user = await User.findOne({ emailProofToken: inputs.token });
```

그런 다음 해당 사용자가 없거나 토큰이 이미 만료되었는지 확인합니다.

```js
if (!user || user.emailProofTokenExpiresAt <= Date.now()) {
     return exits.invalidOrExpiredToken({
       error: "The provided token is expired, invalid, or already used up.",
     });
   }
```

마지막으로, 사용자가 `확인되지 않음`의 `e-mailStatus`를 가지고 있는지 확인하고 이를 `confirmed`로 업데이트한 후 발신자에게 회신합니다.

```undefined
if (user.emailStatus === "unconfirmed") {
      await User.updateOne({ id: user.id }).set({
      emailStatus: "confirmed",
      emailProofToken: "",
      emailProofTokenExpiresAt: 0,
  });
  return exits.success({
    message: "Your account has been confirmed",
  });
}
```

이를 테스트하려면 `ails lift`를 실행한 다음 액세스 권한이 있는 유효한 전자 메일 주소로 Postman에 새 사용자를 만드십시오. 그런 다음 확인 이메일을 확인하고 확인 이메일 버튼을 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/emailconfirmationmessage.png?resize=1313%2C413&ssl=1)

모든 것이 성공적이었다면 당신의 계정이 확인되었다는 메시지를 보여줄 것이다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/youraccounthasbeenconfirmed.png?resize=730%2C169&ssl=1)

이제 계정을 만들고 암호를 확인할 수 있지만 아직 로그인할 수 없습니다. 고칩시다!

## 로그인 기능

로그인 기능을 구현하기 위해 아래 경로를 `routes`로 선언한다.js:

```undefined
POST /user/login': 'user/login
```

그런 다음 터미널에서 `sails generate action user/login`을 실행하여 `login` 동작을 만들 것입니다.

사용자가 로그인할 수 있으려면 다음이 참이어야 합니다.

- 이메일 주소가 등록되어 있어야 합니다.
- 이메일 주소가 확인되었음이 틀림없습니다.

이를 시행하기 위해, 우리는 `policies/` 디렉토리에 `can-login.js`라는 정책을 작성할 것이다. 다음은 이 정책의 내용입니다.

```js
module.exports = async function (req, res, proceed) {
  const { email } = req.allParams();
  try {
    const user = await User.findOne({ email: email });
    if (!user) {
      res.status(404).json({
        error: `${email} does not belong to a user`,
      });
    } else if (user.emailStatus === 'unconfirmed') {
      res.status(401).json({
        error: 'This account has not been confirmed. Click on the link in the email sent to you to confirm.',
      });
    } else {
      return proceed();
    }
  } catch (error) {
    res.status(401).json({ error: error.message });
  }
};
```

여기서는 사용자 모델에 워터라인에서 제공하는 `하나 찾기` 방법을 사용하여 로그인 요청 시 이메일을 수신하고 있습니다. 기록이 발견되지 않으면 404호 메시지를 발신자에게 반송합니다. 다음으로 e-메일 상태가 `확인되지 않음`인지 확인하고, 계정 확인이 안 된 경우 401(승인되지 않음)을 반환합니다. 마지막으로, 이전 조건이 모두 참이 아닌 경우 요청을 `반품 진행()` 콜백으로 처리할 수 있도록 허용한다.

## 정책 설정

정책 작성은 작업에 가드를 추가하는 프로세스의 일부에 불과합니다. 다른 부분은 `config/policies.js` 파일의 작업이 될 작업에 정책을 매핑하는 것입니다. 이렇게 하려면 다음 코드 조각을 파일의 개체에 항목으로 추가하십시오.

```bash
"user/login": 'can-login'
```

이제 `user/login.js` 작업을 구체화합니다. 먼저 입력을 `입력: {}` 개체에 추가합니다.

```css
email: {
    type: "string",
    required: true,
  },
password: {
  type: "string",
  required: true,
},
```

또한 다음 출구를 추가합니다.

```css
success: {
      description: "Login successful",
    },
  notAUser: {
    statusCode: 404,
    description: "User not found",
  },
  passwordMismatch: {
    statusCode: 401,
    description: "Password do not match",
  },
  operationalError: {
    statusCode: 400,
    description: 'The request was formed properly'
}
```

그런 다음 fn 함수에서 try/catch 블록으로 시작할 것입니다. try(시도) 블록에서 제공된 e-메일로 사용자를 찾습니다.

```undefined
const user = await User.findOne({ email: inputs.email });
```

사용자가 발견되지 않았는지 확인한 후 not로 종료됩니다.사용자 종료:

```js
if (!user) {
   return exits.notAUser({
     error: `An account belonging to ${inputs.email} was not found`,
   });
}
```

이어서, 우리는 1부에 설치한 "세일즈 후크-organics" 후크가 제공하는 "check Password" 방법을 사용하여 비밀번호를 확인할 것이다.

```coffeescript
await sails.helpers.passwords
    .checkPassword(inputs.password, user.password)
    .intercept('incorrect', (error) => {
      exits.passwordMismatch({ error: error.message });
});
```

그런 다음 새 JWT 토큰을 생성합니다(아직 구현하지 않았습니다).

```undefined
const token = await sails.helpers.generateNewJwtToken(user.email);
```

그런 다음 "req" 스트림/개체에 사용자 레코드가 있는 "me" 개체를 설정합니다.

```coffeescript
this.req.me = user;
```

마지막으로 메시지, 사용자 속성 및 JWT 토큰을 제공하는 성공 종료를 반환합니다.

```css
return exits.success({
      message: `${user.email} has been logged in`,
      data: user,
      token,
});
```

Catch(캐치) 블록에서는 먼저 Sails 내장 로거를 사용하여 오류를 기록합니다.

```css
sails.log.error(error);
```

그러면 오류가 작업 오류인지 확인하겠습니다. 그렇다면 사용자에게 `원시` 메시지가 표시된 JSON을 반환합니다.

```coffeescript
if (error.isOperational) {
  return exits.operationalError({
    message: `Error logging in user ${inputs.email}`,
    error: error.raw,
  });
}
```

다른 모든 오류의 경우 오류 출구를 사용하여 500을 반환할 것인지 알 수 없습니다.

```css
return exits.error({
       message: `Error logging in user ${inputs.email}`,
       error: error.message,
 });
```

`Generate NewJwt`를 생성하지 않았으므로 로그인 기능을 테스트할 수 없습니다.토큰 도우미입니다.

## 인증용 JWT

json 웹토큰 패키지는 다음과 같이 실행해서 종속적으로 설치하겠습니다.

```undefined
npm install jsonwebtoken --save
```

그런 다음 generate NewJwt를 만들 것입니다.토큰 도우미:

```coffeescript
sails generate helper generate-new-jwt-token
```

파일을 열고 맨 위에 있는 `json 웹 토큰` 패키지가 필요합니다.

```js
const jwt = require("jsonwebtoken");
```

이 도우미는 사용자의 전자 메일인 단일 인수를 사용합니다(이 전자 메일을 고유 식별자로 사용하여 JWT 토큰을 만듭니다). 입력 필드에 다음을 추가합니다.

```css
subject: {
    type: "string",
    required: true
}
```

이제 도우미의 몸을 살펴보도록 하겠습니다. 먼저 JWT에서 토큰을 발행하는 데 필요한 페이로드 개체를 생성하는 것으로 시작합니다.

```undefined
const payload = {
     sub: inputs.subject, // subject
     iss: "LogRocket Sails API" // issuer
};
```

그런 다음 config/local.js에서 문자열이어야 하는 jwtSecret 항목을 추가할 것이다.

> 보안상 좋지 않기 때문에 비밀은 우리 코드에 직접 저장하지 않고 있습니다.

생산 시에는 `config/local.js`를 사용할 수 없으므로 `J`라는 환경 변수를 추가합니다.서버 환경에 WT_SECRET`이 있습니다. 따라서 도우미에 다음 줄을 추가하십시오.

```cpp
const secret = sails.config.jwtSecret || process.env.JWT_SECRET;
```

위의 줄에서는 개발 시 config/local.js에서 암호를 가져오려 하고 환경 변수 J에서 값을 가져오려고 할 뿐입니다.개발 중이 아닐 경우(즉, 생산) WT_SECRET

마지막으로 토큰을 생성하고 24시간 후에 만료되도록 설정합니다.

```cpp
const token = jwt.sign(payload, secret, { expiresIn: "1d" });
```

마지막으로, 토큰을 발신자에게 반환합니다.

```bash
return token;
```

## 웹 API 테스트

이미 생성한 사용자 계정을 사용하여 Postman에서 로그인 기능을 테스트해 보겠습니다. 끝점에 이메일 및 암호를 제공하면 아래 스크린샷과 유사한 응답을 API에서 받아야 합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/postman-nocdn.png)

## 암호를 잊어버림

여기 시나리오가 있습니다. 사용자가 API를 사용하는 클라이언트 중 하나로 돌아와 계정 암호를 호출할 수 없습니다. 우리가 다시 들어갈 수 있도록 확실히 도와주자. 잊어버린 암호 기능을 구현하기 위해 경로 선언을 `routs`로 시작할 것이다.js:

```bash
'POST /user/forgot-password': 'user/forgot-password'
```

작업 파일도 생성하겠습니다.

```undefined
sails generate action user/forgot-password
```

이 끝점을 로그인할 수 있는 사용자만 보호하려고 합니다. 우리는 이미 재사용할 수 있도록 `can-login`이라는 정책을 시행했습니다. `config/policies.js`에서 다음 항목을 추가하십시오.

```bash
'user/forgot-password': 'can-login'
```

우리가 `암호를 잊어버렸다`고 주장하는 사용자의 이메일이 포함된 POST 요청이 엔드포인트에 전송되고 사용자의 받은 편지함으로 복구 링크를 보내는 것이 `잊혀진 비밀번호`의 작동 방식이다. 시작하려면 이메일 입력을 선언합니다.

```css
email: {
        description:
          "The email address of the user who wants to recover their password.",
        example: "albus@dumbledore.com",
        type: "string",
        required: true,
    },
```

`성공`의 출구는 다음과 같습니다.

```css
success: {
     description:
       "Email matched a user and a recovery email might have been sent",
 },
```

`fn`의 경우 먼저 사용자가 해당 이메일 주소와 일치하도록 시도하지만 일치하는 항목이 없으면 계속 진행하지 않습니다.

```undefined
var user = await User.findOne({ email: inputs.email });
   if (!user) {
     return;
   }
```

그런 다음 사용자에게 전송되는 복구 토큰을 생성합니다.

```undefined
const token = await sails.helpers.strings.random("url-friendly");
```

그런 다음 토큰을 만료 시간과 함께 데이터베이스에 저장합니다.

```css
await User.update({ id: user.id }).set({
      passwordResetToken: token,
      passwordResetTokenExpiresAt:
        Date.now() + sails.config.custom.passwordResetTokenTTL,
});
```

현재 `passwordReset`이 있습니다.`config/custom.js`의 토큰TTL 속성에는 다음과 같은 값이 추가됩니다.

```cpp
passwordResetTokenTTL: 24 * 60 * 60 * 1000, // 24 hours
```

`user/forgot-password.js` 동작으로 돌아가서 일단은 비밀번호 재설정을 담당하는 끝점을 직접 가리키도록 하겠습니다.

> 이 링크는 클라이언트에서 재설정 끝점을 사용하는 방법에 따라 달라집니다. 우리는 프런트 엔드에 집중하지 않기 때문에, 우리는 단순히 재설정 토큰을 복사해서 포스트맨에서 사용할 것이다.

다음은 조각입니다.

```js
const recoveryLink = `${sails.config.custom.baseUrl}/user/reset-password?token=${token}`;
```

그런 다음 아직 생성하지 않은 forgot-password.hbs라는 이메일 템플릿을 사용하여 이메일을 보낼 것입니다. 따라서 `views/` 디렉토리에 `forgot-password.hbs`를 작성합니다. 다음 템플릿을 추가합니다.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
        body {
            padding: 10px 20px;
            font-family: Arial, Helvetica, sans-serif;
        }
        .cta {
            text-decoration: none;
            background-color: rgb(76, 119, 175);
            color: #fff !important;
            font-weight: bold;
            padding: 10px 20px !important;
            border-radius: 5px;
            margin: 0 auto;
        }
        footer {
            margin-top: 2rem;
        }
        p {
            margin: 30px 0;
        }
    </style>
</head>
<body>
    <section class="hero">
        <h1 class="title">Forgot your password { name }?</h1>
    </section>
    <main>
        <p>
            Someone requested a password reset for your account. If this was not
            you, please disregard this email. Otherwise, simply click the button
            below:
        </p>
        <a class="cta" href="{recoverLink}" target="_blank">Reset password</a>
    </main>
    <footer>
        <p>🖤 Love,</p>
    </footer>
</body>
</html>
```

암호를 잊어버린 액션으로 돌아가면 전자 메일 옵션을 구성하고 전자 메일을 보냅니다.

```cpp
const email = {
      to: user.emailAddress,
      subject: "Reset Password",
      template: "forgot-password",
      context: {
        name: user.fullName,
        recoverLink: recoveryLink,
      },
    };
    try {
      await sails.helpers.sendMail(email);
    } catch (error) {
      sails.log(error);
    }
```

마지막으로, 성공적인 종료와 함께 발신자에게 회신을 보냅니다.

```css
return exits.success({
      message: `A reset password email has been sent to ${user.email}.`,
    });
```

암호를 잊어버린 기능을 테스트하기 전에 암호 재설정 기능을 구현하여 마무리하겠습니다.

## 암호 재설정

다음 항목을 `routes.js` 파일에 추가하십시오.

```undefined
"POST /user/reset-password": "user/reset-password",
```

다음을 실행하여 작업 생성:

```undefined
sails generate action user/reset-password
```

요청 시 재설정 토큰과 새 토큰이 전송될 것으로 예상되므로 reset-password.js를 열고 다음 두 개의 입력을 추가합니다.

```css
password: {
     description: "The new, unencrypted password.",
     example: "myfancypassword",
     required: true,
   },
   token: {
     description:
       "The password token that was in the forgot-password endpoint",
     example: "gwa8gs8hgw9h2g9hg29",
     required: true,
   },
```

또한 다음 출구를 선언하십시오.

```css
success: {
     description:
       "Password successfully updated, and requesting user agent automatically logged in",
   },
   invalidToken: {
     statusCode: 401,
     description:
       "The provided password token is invalid, expired, or has already been used.",
   },
```

fn에서는 요청서에 다른 토큰이 포함되어 있는지 확인하고 잘못된 토큰을 통해 신속하게 구제합니다.토큰 종료:

```bash
if (!inputs.token) {
     return exits.invalidToken({
       error: "Your reset token is either invalid or expired",
     });
}
```

토큰이 유효한 경우 해당 토큰을 가진 사용자 레코드를 가져옵니다.

```undefined
var user = await User.findOne({ passwordResetToken: inputs.token });
```

또한 사용자 레코드가 검색되었는지, 토큰이 아직 만료되지 않았는지도 확인합니다.

```js
if (!user || user.passwordResetTokenExpiresAt <= Date.now()) {
    return exits.invalidToken({
      error: "Your reset token is either invalid or expired",
    });
  }
```

그런 다음 호출자가 제공한 일반 암호를 해시합니다.

```undefined
const hashedPassword = await sails.helpers.passwords.hashPassword(
      inputs.password
    );
```

그런 다음 사용자 레코드를 `hashedPassword`로 업데이트하고 `passwordReset`을 모두 재설정합니다.토큰 및 `암호 재설정`토큰 TTL` 필드:

```css
await User.updateOne({ id: user.id }).set({
          password: hashedPassword,
          passwordResetToken: "",
          passwordResetTokenExpiresAt: 0,
    });
```

사용자를 인증하기 위해 JWT 토큰을 생성합니다.

```undefined
const token = await sails.helpers.generateNewJwtToken(user.email);
```

마지막으로 사용자 레코드를 `req` 개체에 추가하고 메시지, 사용자 정보 및 JWT 토큰을 포함하는 페이로드로 성공 종료를 통해 200개의 응답을 보냅니다.

```coffeescript
this.req.user = user;
return exits.success({
  message: `Password reset successful. ${user.email} has been logged in`,
  data: user,
  token,
});
```

이 두 개의 엔드포인트를 테스트해 보겠습니다. Postman으로 돌아가 이전에 만든 계정 전자 메일로 암호를 잊어버린 끝점을 치십시오. 받은 편지함으로 이동한 다음 링크를 클릭하고 토큰을 복사합니다. 그런 다음 Postman으로 돌아가 토큰과 새 암호로 재설정 암호 끝점을 누릅니다. 두 엔드포인트에서 다음 응답을 받아야 합니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/forgotpasswordendpoint-nocdn.png)

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/forgotpasswordemail.png?resize=1648%2C335&ssl=1)

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/postmanpassword-nocdn.png)

새 암호로 로그인해 보십시오. 이 암호도 사용할 수 있습니다.

![image](https://blog.logrocket.com/wp-content/uploads/2020/10/newpasswordlogin-nocdn.png)

이제 모든 엔드포인트를 성공적으로 구현했습니다. 포스트맨에서 한 바퀴 돌면 돼요. 마무리를 위해 우리의 웹 서비스를 전 세계에 알리자.

## 전개

우리는 우리의 API를 Heroku PaaS에 배치할 것이다. 배치하기 전에 집 청소를 좀 해야 할 것 같아요. API에 대한 모든 프로덕션 설정이 포함된 `config/production.js`를 열어 보겠습니다.

http 속성인//trustProxy: true의 줄을 찾아 헤로쿠에서 Sails가 서비스되도록 승인하지 않는다.

그런 다음 데이터스토어 개체의 기본 속성에 있는 다음 두 항목을 대체하여 개발 데이터베이스를 운영 데이터베이스로 재정의합니다.

```css
adapter: "sails-postgresql",
url: process.env.DATABASE_URL,
```

> Heroku는 Postgre를 추가할 때 DATABLE_URL Env 변수를 설정합니다.앱에 SQL 데이터베이스 제공

이메일도 사용할 수 있도록 `config/production.js`의 baseUrl을 Heroku 앱 URL로 바꾸어야 합니다.

그런 다음 Heroku 계정을 생성하고 Heroku에 Node.js 앱을 배포할 때 API를 배포할 수 있습니다.

Heroku Postgre를 추가해야 합니다.데이터베이스에 의존하므로 SQL 추가 기능. 또한 환경변수 `JWT_SECRET`와 `SENDGRID_`를 설정합니다.API_KEY`입니다.

데이터베이스 스키마를 Heroku로 가져오려면 데이터베이스 URL을 복사하고 테이블을 Heroku 프로비저닝된 데이터베이스로 마이그레이션하는 `세일즈 리프트`를 실행할 수 있습니다. Heroku 데이터베이스는 데이터베이스에 연결하려면 SSL이 필요하므로 어댑터 속성에 다음과 같은 `ssl:true`를 추가합니다.

```cpp
// config/datastore.js
  adapter: "sails-postgresql",
  url:"heroku-database-url",
  ssl: true
```

마지막으로 보안을 위해 소켓을 통해 앱에 연결할 수 있는 URL 배열을 전달해야 합니다. 이는 기본적으로 Sails에 웹 소켓 지원이 내장되어 있기 때문입니다. 소켓은 사용하지 않지만. config/production.js의 소켓에서 줄을 찾아 아래 줄의 압축을 풉니다.

```cpp
// onlyAllowOrigins: [
    //   'https://example.com',
    //   'https://staging.example.com',
    // ],
```

Heroku 앱 URL로 회선을 바꿉니다.

```undefined
onlyAllowOrigins: ['https://logrocketsailsapi.herokuapp.com'], // change to yours!
```

그런 다음 Sails로 구동되는 Node.js 웹 API를 Heroku에 배포할 수 있습니다!

## 쌈이야!

이 기사에서는 Sails MVC 프레임워크를 사용하여 Node.js에서 웹 API를 만드는 힘과 용이성에 대해 살펴봤다. 도움이 되는 몇 가지 추가 리소스는 다음과 같습니다.

- https://sails.js.com
- 돛단배 연장선
- Herku에 돛 앱 배포
- https://elements.heroku.com/addons
- https://documenter.getpostman.com/view/4151223/T1LV9jQx?version=latest(우리의 API에 대한 설명)
- https://elements.heroku.com/addons/papertrail(로그를 위한 Heroku 추가 기능, Heroku에 배포하는 경우 로그에 액세스하는 데 매우 유용함)

이 기사의 소스 코드는 GitHub에 있다. 또한 엔드포인트를 테스트할 포스트맨 컬렉션도 여기에 있습니다.