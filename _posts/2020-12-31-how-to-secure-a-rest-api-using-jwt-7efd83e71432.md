---
layout: post
title: "JWT를 사용하여 REST API를 보호하는 방법"
author: 'Code Tower'
thumbnail: undefined
tags: undefined
---


REST API를 보호하는 것이 항상 쉬운 것은 아닙니다. 이 기사에서는 JWT(JSON 웹 토큰)를 사용하여 이 작업을 수행하는 방법에 대해 설명합니다.

## 소개: 안전한 Rest API

REST API는 논리적으로 단순하고, 복잡한 상태를 메모리에 유지하지 않으며, 리소스를 처리하므로 전체 비즈니스 로직을 통합합니다. 나는 정말 좋아하는데, 네가 이 글을 읽고 있으니까 너도 좋아할 것 같아. 그렇지 않다면, 연애에 참여하기 위해 이 튜토리얼을 확인해 보세요.

그렇다고 해서 REST API의 특성과 메커니즘 때문에 보안 확보가 항상 간단한 것은 아니다. 사용자가 자격 증명을 제출하면 어떻게 됩니까? 후속 요청에 올바르게 로그인했는지 어떻게 알 수 있습니까? 이 신호를 보내기 위해 서버 측에서 상태를 유지할 수 없습니다. 그래서 어떻게 해?

이 기사에서는 JSON Web Token을 사용하여 매우 강력하면서도 간단한 방법을 알려드리고자 합니다.

## JWT란?

JSON Web Token은 쌍방향 상호 작용 중에 사용자의 ID를 안전하게 나타낼 수 있는 개방형 표준(RFC 7519) 방식입니다. 즉, 두 시스템이 데이터를 교환할 때 모든 요청에 대해 개인 자격 증명을 보내지 않고도 JSON 웹 토큰을 사용하여 사용자를 식별할 수 있습니다.

이를 REST API 컨텍스트에 적용하면 이러한 메커니즘에서 클라이언트-서버 상호 작용이 어떻게 이점을 얻을 수 있는지 확인할 수 있습니다.

간단히 말해 JWT는 다음과 같이 작동합니다.

- 사용자/클라이언트 앱이 요청 서명을 보냅니다. 다시 말해, 사용자 이름/암호(또는 제공해야 하는 다른 유형의 로그인 자격 증명)가 이동할 위치입니다.
- 확인되면 API가 JSON 웹 토큰(조금 더 자세한 내용)을 생성하고 비밀 키를 사용하여 서명합니다.
- 그런 다음 API가 클라이언트 응용 프로그램에 해당 토큰을 반환합니다.
- 마지막으로 클라이언트 앱이 토큰을 수신하여 자체 확인 후 인증 정보를 더 이상 보내지 않고도 이후 모든 요청에 따라 토큰을 사용하여 사용자를 인증합니다.

사실이라고 하기엔 너무 단순하게 들리는 거 알아, 안 그래? 그게 어떻게 안전할 수 있죠? 좀 더 자세히 설명해 드리겠습니다.

### 토큰 구조

API에서 반환되는 토큰 자체는 인코딩된 문자열입니다. 이 섹션은 점 문자로 구분된 세 개의 섹션으로 구성되어 있습니다.

```css
header.payload.signature
```

각 섹션에는 퍼즐의 중요한 조각이 포함되어 있습니다. 디코딩이 완료되면 처음 두 개는 관련 정보를 포함하는 JSON 데이터 표시가 되며 마지막 두 개는 토큰의 신뢰성을 확인하는 데 사용됩니다.

- 헤더에는 처리 중인 토큰 유형과 해당 생성에 사용되는 알고리즘에 관련된 데이터가 포함됩니다. 여기에 몇 가지 호환 가능한 알고리즘이 있지만, 가장 일반적인 알고리즘은 HS256과 RS256이다. 어떤 보안 표준과 조치를 찾느냐에 따라 달라질 수 있습니다. 이 두 가지 예에서 하나는 서버와 클라이언트가 모두 알고 있는 비밀 키를 사용하고 다른 하나는 클라이언트가 알고 있는 공용 키와 함께 서버가 사용하는 개인 키를 사용합니다.
- 페이로드에는 요청 및 요청을 작성하는 사용자와 관련된 데이터가 포함됩니다. 구현 시 사용할 수 있는 JWT의 일부로 정의된 표준 키/값 쌍 집합은 다음과 같습니다.
- Sub(제목) - 즉, 요청을 하거나 인증 중인 사용자를 식별하는 방법
- 이슈(발행자) - 토큰을 발급한 서버입니다. 우리의 경우, 사용된 URI를 포함하는 것이 타당할 것이다.
- 청중(청중) - 이 토큰의 수신인에 대한 어떤 형태의 식별을 제공하려고 시도했습니다.
- 만료(만료 날짜) - 토큰은 일반적으로 영구적이지 않습니다. 토큰을 사용하는 사용자가 실제로 최근에 생성된 토큰을 제공하는지 확인하기 위한 것입니다.

표준의 일부로 정의된 페이로드 개체에 추가할 수 있는 다른 속성이 있지만 위의 속성이 가장 일반적인 속성입니다. 물론 클라이언트와 서버가 모두 구현에 대해 동의하는 한 이러한 기능을 사용하거나 직접 정의할 수 있습니다.

- 서명은 서버와 클라이언트가 페이로드의 신뢰성을 확인하는 데 사용하는 인코딩된 문자열입니다.

이제 지금까지 다룬 모든 것을 하나의 예로 묶어 보겠습니다.

## JWT를 사용하여 Rest API 보안

우리 회사의 급여 API를 위한 고객을 개발하는 것으로 가정합시다. 이 API는 회사 직원에게 급여를 지급하고, 그에 대한 과거 정보를 검색한 후, 마지막으로 직원의 정보를 편집하기 위한 것입니다.

또한 (휴먼 오류를 방지하기 위해) API 개발자는 이러한 작업 중 일부는 관리자 권한이 필요하다고 결정했습니다. 따라서 정상 액세스 권한을 가진 사용자와 정보를 검토할 수 있는 슈퍼 액세스 권한(관리자) 사용자만 결제 및 데이터 편집이 가능합니다.

이것은 매우 기본적인 예이지만, 우리가 왜 JWT로 하는가에 대한 명확한 생각을 제공하기에 충분할 것이다.

위에서 언급한 바와 같이 당사의 보안 API와의 모든 상호 작용은 로그인 요청으로 시작됩니다. 이렇게 보일 수도 있습니다.

`POST/api/users-session`

페이로드:

```undefined
{
“Username”: “fernando”
“Password”: “fernando123”
}
```

또한 자격 증명이 유효하다고 가정하면 시스템에서 새 JSON 웹 토큰을 반환합니다. 하지만 이 토큰에 대한 자세한 내용을 살펴보겠습니다.

특히, 우리의 페이로드 안에 있는 정보에 대해 생각해 봅시다. 몇 가지 흥미로운 옵션은 다음과 같습니다.

- 문제 - 로그인한 사용자의 사용자 이름을 포함합니다. UI에서 이 기능을 표시할 수 있으므로 특히 유용합니다.
- Exp – 향후 8시간 동안만 이 새 토큰을 사용할 수 있도록 허용하기 때문입니다(일반적으로 사용자가 매일 사용해야 하는 기간).
- 관리 – 사용자의 역할을 설명하는 부울입니다. 일부 UI 요소를 표시하거나 숨길지 알아야 하므로 UI에 유용합니다.

그리고 간단한 작업을 위해 HS256 알고리즘을 사용하여 데이터를 인코딩합니다. 즉, 클라이언트와 API 모두에서 동일한 암호를 사용합니다. 이 예제의 목적상, 우리의 비밀은 다음과 같습니다.

보안 API 예제

이제 토큰의 다양한 섹션이 어떻게 보여질지 살펴보겠습니다.

헤더:

```undefined
{
“alg”: “HS256”,
“typ”: “JWT”
}
```

페이로드:

```undefined
{
“Iss”: “fernando”
“Exp”: 1550946689,
“Admin”: false
}
```

이제, 실제 토큰을 만들기 위해서, 우리는 위의 항목들을 인코딩하고 그 결과 값에 서명해서 토큰에 최종 조각을 추가해야 합니다.

다음과 같은 이점이 있습니다.

```undefined
Base64(header) = ewoiYWxnIjogIkhTMjU2IiwKInR5cCI6ICJKV1QiCn0K
```

```undefined
Base64(payload) = ewoiSXNzIjogImZlcm5hbmRvIiwKIkV4cCI6IDE1NTA5NDY2ODksCiJBZG1pbiI6IGZhbHNlCn0K
```

```undefined
HS256(Base64(header) + “.” + Base64(payload), “A secret API example”) = TseARzVBAtDbU8f3TEiRgsUoKYaW2SbhCWB0QlKpdp8
```

API에서 반환되는 최종 토큰은 다음과 같습니다.

```css
ewoiYWxnIjogIkhTMjU2IiwKInR5cCI6ICJKV1QiCn0K.ewoiSXNzIjogImZlcm5hbmRvIiwKIkV4cCI6IDE1NTA5NDY2ODksCiJBZG1pbiI6IGZhbHNlCn0K.TseARzVBAtDbU8f3TEiRgsUoKYaW2SbhCWB0QlKpdp8
```

여기 흥미로운 부분이 있습니다. 이것이 왜 그렇게 강력한지 보여드리겠습니다.

이 토큰을 받은 클라이언트 애플리케이션은 헤더와 페이로드 부분을 잡고 스스로 서명함으로써 토큰을 해독하고 검증할 수 있다(물론 클라이언트와 서버 모두 비밀 문구를 알고 있기 때문에 가능). 이렇게 하면 아무도 메시지 내용을 변경하지 않고 사용해도 안전한지 확인할 수 있습니다.

동시에 클라이언트 앱에서 보낸 추가 요청에는 동일한 토큰이 포함되며, 이 토큰은 매번 재서명하고 토큰의 서명 부분과 결과를 비교하여 서버에서 유효성을 검사합니다.

예를 들어, 누군가가 메시지의 페이로드에 개입하여 "admin" 속성을 "true"로 변경하는 것을 방지하여 가짜(또는 유효한 비관리 사용자)가 (일부 특정 직원에게 지급을 발행하는 것과 같은) 권한 있는 작업을 실행하도록 허용한다.

이러한 작업을 수행하면 다음과 같은 페이로드 콘텐츠가 수정됩니다.

```undefined
ewoiSXNzIjogImZlcm5hbmRvIiwKIkV4cCI6IDE1NTA5NDY2ODksCiJBZG1pbiI6IHRydWUKfQo=
```

클라이언트 앱에서 보낸 최종 토큰은 다음과 같습니다.

```undefined
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.ewoiSXNzIjogImZlcm5hbmRvIiwKIkV4cCI6IDE1NTA5NDY2ODksCiJBZG1pbiI6IHRydWUKfQo=.TseARzVBAtDbU8f3TEiRgsUoKYaW2SbhCWB0QlKpdp8
```

이 토큰의 서명은 다음과 같아야 합니다.

```undefined
doRnK7CoVjFOiFmvrQ2wvxcGeQuCYjzUchayNAYx1jw
```

메시지의 일부로 보낸 내용과 일치하지 않으므로 요청이 변조되었음을 증명합니다.

## 결론: JWT를 사용한 안전한 REST API

지금쯤이면 JWT 보안에 수반되는 기본 사항을 파악하여 REST API를 보호하는 것이 실제로 어렵지 않다는 것을 깨달았기를 바랍니다. 물론 제가 언급하고 이 기사에서 보여준 것에는 변형이 있지만, 여러분은 jwt.io을 방문하시면 그것을 직접 보실 수 있습니다. 해당 사이트에서는 JSON 웹 토큰을 생성하고 검증할 수 있을 뿐만 아니라 가장 일반적인 프로그래밍 언어에 대한 기본 JWT 라이브러리에 대한 링크도 제공합니다.

기본적으로, JWT 보안을 API에 추가하는 작업을 시작하는 데 필요한 모든 작업은 이미 해당 웹 사이트를 통해 쉽게 액세스할 수 있습니다.

마지막으로 여기서 다룬 메커니즘은 매우 간단하고 모든 사람이 액세스할 수 있지만, API에 JWT 보안만 추가하는 것은 충분하지 않다는 점을 이해해야 합니다. 위 내용만 사용한다면 방탄이 될 수 없습니다. 많은 스마트 해커들이 이 문제를 해결할 방법을 찾을 것입니다. 보안은 단순히 하나의 일반적인 보안 체계를 구현하는 것이 아니라 모든 전선을 포괄하는 것입니다. JWT와 항상 함께 사용되는 추가적인 보호 계층은 HTTPS 연결을 통해 모든 네트워크 트래픽을 보호하는 것입니다. 즉, 사용자가 보내고 받는 모든 항목이 오래된 보안되지 않은 포트 80이 아닌 포트 443(또는 사용 중인 사용자 지정 번호)을 통과하는지 확인합니다.

바로 그거야! 제가 이 주제에서 중요한 부분을 언급하는 것을 잊은 것 같거나, 흥미롭고 도움이 된다고 생각하셨다면 언제든지 연락해서 의견을 남겨주세요.

다음 번까지!