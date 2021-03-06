---
layout: post
title: "프록시란 무엇이며 Node.js에서는 어떻게 작동합니까?"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/node-proxy.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/node-proxy.png?fit=730%2C487&ssl=1)

이 기사에서는 프록시 서버의 종류, 이점, 사용 가능한 유형 및 잠재적인 단점을 포함하여 프록시 서버에 대해 자세히 설명합니다. 그런 다음, Node.js의 프록시 서버를 사용하여 후드 아래에서 무슨 일이 일어나는지 파악하는 방법을 탐색할 것이다.

## 프록시 서버란?

당신이 레스토랑에 있다고 상상해 보세요. 당신은 카베르네 소비뇽 한 병을 주문하고 싶지만, 바에 올라가기 싫습니다. 대신, 웨이터를 불러서 주문을 하면, 그들은 당신을 위해 와인 한 병을 주문하러 바에 갑니다. 이 시나리오에서 사용자는 컴퓨터이고, 웨이터는 프록시 서버이며, 바 카운터는 인터넷입니다.

프록시 서버는 사용자의 정보를 인터넷에 제공하지 않고 사용자의 요구와 요청을 전달하는 중간 역할을 하므로 강력합니다. 프록시를 사용하면 특정 보안 블록을 바이패스하고 그렇지 않으면 차단될 수 있는 정보에 액세스할 수도 있습니다. 이것은 개인 IP 주소가 표시되지 않는 프록시 서버를 사용하여 가능합니다.

인터넷에 연결하는 각 컴퓨터에는 이미 IP 주소가 있습니다. 개별 사용자의 경우 IP 주소는 컴퓨터의 집 주소와 같습니다. 웨이터가 와인을 어느 테이블로 가져가야 할지 알았듯이, 인터넷도 당신의 IP 주소로 당신이 요청한 데이터를 다시 보내야 한다는 것을 알고 있다.

반면 프록시 서버는 요청을 보낼 때 IP 주소를 자체 주소와 "위치"로 변경할 수 있으므로 개인 주소와 데이터를 보호할 수 있습니다.

그런데 왜 프록시 서버를 사용하는 걸까요? IP를 보호하기 위해 항상 IP를 사용해야 합니까? 데이터를 검색하는 가장 안전한 방법입니까? 올바른 서버를 사용하고 있는지 확인하는 방법은 무엇입니까? 이 질문들은 모두 중요한 질문이며, 다음 섹션에서 답변해 드리겠습니다.

## 프록시 서버를 사용하는 이유는 무엇입니까?

개인 또는 조직이 프록시를 사용하는 이유는 여러 가지가 있으며, 가장 일반적인 이유는 해당 지역에서 제한된 사이트를 차단 해제하거나 탐색할 때 익명으로 남아 있기 때문입니다. 다음은 프록시 서버를 사용할 수 있는 몇 가지 경우입니다.

- 제한된 웹 컨텐츠에 대한 액세스: 대부분의 조직은 지금까지 프록시 서버를 사용했지만 오랫동안 직원들은 프록시를 사용하여 음악, 비디오 또는 게임 사이트와 같은 회사에서 금지된 웹 컨텐츠에 액세스할 수 있었습니다. 웹 사용자는 프록시 서버를 사용하여 특정 영역에서 차단된 웹 내용을 무시할 수도 있습니다. 예를 들어, 해외 여행 중이라 스트리밍 서비스를 통해 영화를 볼 수 없는 경우 프록시 서버를 사용하면 스트리밍 사이트를 아직 미국에 있다고 믿도록 `속일` 수 있다.
- 익명성 제공: 프록시를 사용하면 웹 서퍼가 검색 중에 자신의 신원을 노출하지 않도록 보호합니다. 프록시는 또한 사용자의 웹 활동을 추적할 수 없도록 하고 IP 주소와 개인 정보를 안전하게 유지합니다.
- 보안 강화: 익명성을 넘어 프록시 서버는 암호화된 웹 요청과 웹 필터 또는 방화벽으로 설정할 수 있는 기능을 포함한 추가적인 보안 이점을 제공합니다.
- 더 빠른 성능: 프록시 서버는 개인 정보를 인터넷과 공유하지 않지만 자주 방문하는 웹 콘텐츠를 캐시할 수 있습니다. 저희 와인 예에서 웨이터가 바텐더에게 당신이 누군지 말하지 않았을 수도 있지만, 다음에 식당에 오면 당신이 택시를 더 좋아한다는 걸 알 거예요. 프록시 서버는 자주 액세스하는 리소스의 복사본을 유지함으로써 대역폭 사용을 줄이고 검색 속도를 높일 수 있습니다.

## 전달 및 역방향 프록시 유형

프록시 서버에는 전달 및 역방향 프록시라는 두 가지 주요 유형이 있습니다. 프록시는 클라이언트와 서버 간에 이동하는 반면 다른 프록시는 서버와 클라이언트 간에 이동하는 등 작동 방향에 따라 다릅니다. 우리는 아래에서 더 자세히 토론할 것입니다.

전달 프록시
종종 터널 또는 게이트웨이로 알려진 포워드 프록시는 인터넷에서 가장 일반적이고 액세스 가능한 프록시 유형입니다.

전달 프록시는 이전 섹션에서 다루었던 사용 사례와 같이 조직 또는 특정 개인에게 서비스를 제공하는 개방형 프록시입니다. 전달 프록시의 가장 일반적인 용도는 웹 페이지의 저장 및 전달이며, 대역폭 사용을 줄임으로써 성능을 향상시킨다.

정방향 프록시는 인터넷과 클라이언트 사이에 배치되며, 조직이 프록시를 소유하는 경우 내부 네트워크 내에 배치됩니다. 많은 조직이 익명성을 유지하기 위해 전달 프록시를 사용하여 웹 요청 및 응답을 모니터링하고, 일부 웹 콘텐츠에 대한 액세스를 제한하고, 트랜잭션을 암호화하고, IP 주소를 변경합니다.

역방향 프록시
정방향 프록시와는 달리 역방향 프록시는 열려 있지 않습니다. 대신 백엔드에서 작동하며 내부 네트워크 내의 조직에서 일반적으로 사용됩니다.

역방향 프록시는 클라이언트로부터 중간 서버 역할을 하는 모든 요청을 받은 다음 요청을 처리해야 하는 실제 서버로 전달합니다. 그런 다음 서버의 응답이 프록시를 통해 전송되어 웹 서버 자체에서 온 것처럼 클라이언트에 전달됩니다.

역방향 프록시 아키텍처에는 많은 이점이 있지만 가장 일반적인 것은 로드 밸런싱입니다. 역방향 프록시는 인터넷과 웹 서버 사이에 위치하기 때문에 요청과 트래픽을 서로 다른 서버에 분산시킬 수 있으며, 서버 과부하를 제한하기 위해 콘텐츠 캐싱도 관리할 수 있다.

내부 네트워크에 역방향 프록시를 설치하는 다른 이유는 다음과 같습니다.

- 압축: 웹 파일을 압축하여 로드 시간을 늘립니다.
- 보안: DDoS 공격을 방지합니다. 실제 서버에 도달하는 대신, 공격이 프록시를 공격합니다.
- 엑스트라넷 게시: 방화벽을 통해 콘텐츠에 대한 액세스를 관리합니다.
- 스푼 공급: 웹 컨텐츠를 캐시하고 초과 리소스 지출을 제한합니다.
- 암호화: 개인 데이터를 보호하고 SSL을 관리합니다.

## Node.js에서 프록시 사용

이제 프록시가 무엇인지, 프록시가 무엇을 위해 사용되는지를 이해했으므로, Node.js에서 프록시를 사용하는 방법을 살펴볼 준비가 되었습니다. 아래에서는 npm 패키지를 사용하여 간단한 프록시를 만들겠습니다. (완전한 코드는 이 Github 저장소를 확인하십시오.)

이 npm 패키지를 사용하여 클라이언트 GET 요청을 대상 호스트에 프록시하는 방법을 시연합니다.

먼저, 우리는 패키지를 생성하기 위해 프로젝트를 초기화해야 합니다.json 및 index.js 파일:

```coffeescript
npm init
```

다음으로 두 가지 종속성을 설치해야 합니다.

- `express`: 우리의 미니 노드 프레임워크
- `http-http-http-http-middleware`: 프록시 프레임워크 npm 설치

index.js 파일(또는 요청을 프록시할 파일)에서 다음과 같은 필수 종속성을 추가합니다.

```coffeescript
import { Router } from 'express';
import { createProxyMiddleware } from 'http-proxy-middleware'
```

마지막으로, 옵션을 추가하고 경로를 정의합니다.

```undefined
const router = Router();

const options = {
    target: 'https://jsonplaceholder.typicode.com/users', // target host
    changeOrigin: true, // needed for virtual hosted sites
    pathRewrite: {
       [`^/api/users/all`]: '',
    }, // rewrites our endpoints to '' when forwarded to our target
}
router.get('/all', createProxyMiddleware(options));
```

pathRewrite를 사용하면 끝점 `api/users/all`이 있는 애플리케이션을 `jsonplaceholder`라는 API에 프록시할 수 있다. 브라우저에서 api/users/all을 누르면 아래와 같이 https://jsonplaceholder.typicode.com/users에서 사용자 목록을 받게 됩니다.

## 프록시 사용의 잠재적 위험

프록시를 사용하면 이점이 있는 것처럼 다음과 같은 여러 위험과 단점도 있습니다.

- 사용 가능한 프록시는 안전하지 않습니다. 데이터를 훔치기 위해 해커가 만든 것이 분명합니다.
- 저렴한 프록시는 요청을 완전히 암호화하지 않아 일부 데이터는 보안이 유지되지 않을 수 있습니다.
- 프록시에 많이 의존하는 경우 웹 요청이 느려질 수 있습니다.
- 캐시된 웹 콘텐츠는 여전히 암호와 보안 데이터를 저장할 수 있으며, 이는 개인 정보를 프록시 서비스 공급자가 볼 수 있음을 의미합니다.

즉, 이러한 리스크에 대한 해결책은 간단합니다. 신뢰할 수 있고 신뢰할 수 있는 프록시 서버를 찾는 것입니다.

## 결론

이 기사에서는 프록시 서버가 웹 요청에 대해 중간 관리자처럼 행동하여 웹 상의 거래에 더 많은 보안을 부여한다는 것을 배웠습니다. Node.js 샘플에서는 JSON Placeholder에서 엔드포인트를 프록시할 수 있었고, API에서 ID가 마스크되도록 URL을 다시 썼다.

포워드 프록시와 리버스 프록시는 서로 다른 기능을 제공할 수 있지만, 전반적으로 프록시 서버는 보안 위험을 줄이고 익명성을 향상시키며 검색 속도까지 높일 수 있다.