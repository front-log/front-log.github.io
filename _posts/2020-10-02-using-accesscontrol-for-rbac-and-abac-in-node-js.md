---
layout: post
title: "Node.js에서 RBAC 및 ABAC에 대한 Access Control을 사용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/Untitled-design-5.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/Untitled-design-5.png?fit=730%2C487&ssl=1)

시스템 보안은 소프트웨어를 구축할 때 고려해야 할 주요 사항 중 하나이며, 소프트웨어 시스템의 보안을 보장하기 위해 사용되는 다양한 메커니즘이 있습니다. 일반적으로 역할 기반 액세스 제어(RBAC)와 속성 기반 액세스 제어(ABAC)가 있습니다.

이러한 두 가지 액세스 제어 메커니즘을 구현하는 데 Node.js 모듈인 AccessControl을 사용할 수 있습니다. 액세스 제어의 작동 방식에 대해 자세히 알아보기 전에 이 두 가지 메커니즘과 메커니즘의 작동 방식을 간략히 설명하겠습니다.

## 역할 기반 액세스 제어(RBAC)

역할 기반 보안이라고도 하는 역할 기반 액세스 제어는 역할, 권한 및 권한을 사용하는 사용자에 대한 시스템 액세스를 제한하는 메커니즘입니다.

응용프로그램 내에서 다양한 사용자 유형(예: 작성자 또는 독서자)에 대한 역할이 작성됩니다. 특정 작업을 수행하거나 응용 프로그램 리소스에 액세스할 수 있는 권한이 특정 역할에 할당됩니다. 예를 들어, 쓰기 응용 프로그램에서, 작성자는 게시물을 만들고, 업데이트하고, 읽고, 삭제할 수 있는 권한을 부여할 수 있는 반면, 독서자는 게시물을 읽을 수만 있는 것으로 제한될 수 있다.

RBAC를 사용할 때 다음과 같은 세 가지 안내 규칙이 있습니다.

- 역할 할당: 제목(예: 사용자)은 환자가 역할을 선택했거나 할당된 경우에만 사용 권한을 행사할 수 있습니다.
- 역할 권한 부여: 피사체의 활성 역할은 피사체에 대해 승인되어야 합니다. 위의 규칙 1을 사용하면 사용자가 권한이 부여된 역할만 수행할 수 있습니다.
- 권한 인증: 피사체의 활성 역할에 대한 권한이 부여된 경우에만 피사체가 권한을 행사할 수 있습니다. 규칙 1과 규칙 2를 사용하면 사용자가 권한이 부여된 권한만 행사할 수 있습니다.

사용자는 여러 역할을 가질 수 있고 역할은 여러 권한을 가질 수 있습니다. 또한 RBAC는 상위 역할이 하위 역할의 권한을 상속하는 역할 계층을 지원합니다. 다른 역할에서 사용 권한을 상속할 수 있는 제한 규칙을 배치하기 위해 추가 제약 조건을 적용할 수 있습니다. 제약 조건의 몇 가지 예는 다음과 같습니다.

- 역할은 자체 상속할 수 없습니다.
- 교차 상속은 허용되지 않습니다(작성자가 독서자로부터 상속하는 경우, 독서자는 작성자로부터 상속할 수 없음).
- 역할이 존재하지 않는 역할을 미리 확장할 수 없습니다.

## ABAC(Attribute-Based Access Control)

IAM의 정책 기반 액세스 제어라고도 하는 속성 기반 액세스 제어는 속성을 함께 결합하는 정책의 사용을 통해 사용자에게 액세스 권한이 부여되는 액세스 제어 패러다임을 정의합니다. 정책은 사용자 속성, 리소스 속성, 개체 및 환경 속성과 같은 모든 유형의 속성을 사용할 수 있습니다.

ABAC는 역할 및 권한 외에도 허용되거나 허용되지 않는 속성을 정의하는 데 정책을 사용할 수 있다는 점에서 RBAC를 보완하는 데 사용할 수 있습니다.

## Access Control을 사용한 Node.js RBAC-ABAC

Node.js 모듈인 `AccessControl`은 RBAC와 ABAC의 최고 기능을 병합한다. RBAC 기본 사항을 구현하고 리소스 및 작업 속성에도 초점을 맞춥니다. 모듈의 기능에 대한 전체 목록을 보려면 설명서를 참조하십시오.

### 설치

npm: `npmi access control --save`를 사용합니다.
Yarn add access control(액세스 제어 추가) 포함

### 역할

역할은 사용 권한의 컨테이너 역할을 합니다. 사용자의 책임에 따라 사용자에게 할당됩니다. 액세스 제어 인스턴스에서 .grant(<role>) 또는 .deny(<role>) 메서드를 호출하여 역할을 생성하고 정의할 수 있습니다.

```js
import { AccessControl } from 'accesscontrol';

const ac = new AccessControl();
ac.grant('reader');
```

역할은 다른 역할을 확장할 수 있습니다. 기존 역할에서 `.extend`를 호출하여 역할을 확장할 수 있습니다.

```bash
ac.grant('reader').extend('writer');
```

### 작업 및 작업 속성

작업 및 작업 속성은 역할별로 리소스에 대해 수행할 수 있는 작업을 나타냅니다. 그것들은 고전적인 CRUD에 기초한 유한한 고정 리스트이다. 역할별로 리소스 소유를 정의하는 두 가지 작업 속성(소유 및 임의)이 있습니다.

예를 들어 편집자 역할은 포스트 리소스를 생성, 읽기, 업데이트 또는 삭제(CRUD)할 수 있습니다. 그러나 `작가` 역할은 자신의 `포스트` 자원을 읽거나 업데이트할 뿐이다.

`createOwn`, `readOwn`, `updateOwn`, `deleteOwn`, `create`를 사용하여 리소스에 대한 작업과 소유권을 정의할 수 있습니다.Any, readAny, update임의 및 `삭제`임의의 방법.

```undefined
const ac = new AccessControl();
ac.grant('reader')
  .readAny('post')
  .grant('writer')
    .createOwn('post')             
    .deleteOwn('post')
    .readAny('post')
  .grant('editor')                   
    .extend('writer')                 
    .updateAny('post')  
    .deleteAny('post');
```

### 리소스 및 리소스 속성

이것들은 포스트와 같이 우리가 보호하고자 하는 시스템 요소들을 나타낸다. 여러 역할이 특정 리소스에 액세스할 수 있지만 리소스의 모든 속성에 대해 동일한 액세스 권한을 가질 수는 없습니다. 글로벌 표기법을 사용하여 허용 또는 거부된 속성을 정의할 수 있습니다.

예를 들어, post 리소스에는 id, title 및 description이라는 속성이 있습니다. 모든 `포스트` 리소스의 모든 속성은 `편집자` 역할로 읽을 수 있습니다.

```bash
ac.grant('editor').readAny('post', ['*']);
```

그러나 id 속성을 leader 역할로 읽으면 안 된다.

```bash
ac.grant('reader').readAny('post', ['!id']);
```

### 사용 권한 및 필터링 속성 확인

부여된 권한은 역할, 작업 및 리소스의 조합을 사용하여 결정됩니다. `.can(<role>)을 추가할 수 있습니다.특정 리소스 및 작업에 대한 사용 권한이 있는지 확인하려면 `액세스 제어` 인스턴스의 <(<리소스>)를 선택합니다.

```undefined
const permission = ac.can('reader').readAny('post');
permission.granted;
permission.attributes;
permission.filter(data);
```

### 한 번에 모든 보조금 정의

보조금을 `액세스 제어` 생성자에게 직접 전달할 수 있습니다. 다음 중 하나를 `개체`로 수락합니다.

```undefined
let grantObjects = {
reader: {
        post: {
            'read:any': ['*', '!id]
        }
    },
    writer: {
        post: {
            'create:own': ['*'],
            'read:any': ['*'],
            'update:own': ['*'],
            'delete:own': ['*']
        }
    },
    editor: {
        post: {
            'create:any': ['*'],
            'read:any': ['*'],
            'update:any': ['*'],
            'delete:any': ['*']
        }
    }
}

const ac = new AccessControl(grantsObject);
```

또는 배열:

```undefined
let grantArray = [
  { role: 'reader', resource: 'post', action: 'read:any', attributes: '*, !id' },
  { role: 'writer', resource: 'post', action: 'read:any', attributes: '*' },
  { role: 'writer', resource: 'post', action: 'create:own', attributes: '*' },
  { role: 'writer', resource: 'post', action: 'update:own', attributes: '*' },
  { role: 'writer', resource: 'post', action: 'delete:own', attributes: '*' },
  { role: 'editor', resource: 'post', action: 'read:any', attributes: '*' },
  { role: 'editor', resource: 'post', action: 'create:any', attributes: '*' },
  { role: 'editor', resource: 'post', action: 'update:any', attributes: '*' },
  { role: 'editor', resource: 'post', action: 'delete:any', attributes: '*' },
]

const ac = new AccessControl(grantArray);
```

### Express.js의 예

```js
const ac = new AccessControl(grants);

router.get('/posts/:title', function (req, res, next) {
    const permission = ac.can(req.user.role).readAny('post');
    if (permission.granted) {
        Video.find(req.params.title, function (err, data) {
            if (err || !data) return res.status(404).end();
            res.json(permission.filter(data));
        });
    } else {
        res.status(403).end();
    }
});
```

## 결론

서버측 응용프로그램에서 `액세스 제어`를 인증에 사용하는 방법을 보여드렸습니다. 또한 이를 사용하여 클라이언트 측 응용 프로그램에서 경로 및 UI 요소를 인증할 수 있으며, 이는 서버와 클라이언트 모두에 대해 동일한 권한 부여 개체를 사용하여 수행할 수 있습니다. `AccessControl`은 Node.js에서 액세스 제어를 구현하기 위한 유일한 라이브러리입니다. Node-casbin과 CASL은 접근 제어를 구현하기 위한 Node.js 라이브러리이기도 하다.