---
layout: post
title: "스벨트를 생산에 사용해야 합니까?"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/svelte-logo.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/svelte-logo.png?fit=730%2C487&ssl=1)

## 도입

"Svelte는 프레임워크가 없는 프레임워크이다" – Svelte에 의해 정의된 대로 Svelte.

잠깐, 그게 무슨 뜻이야?

SPA(Single Page Applications)의 등장으로 인해 웹 앱의 프런트 엔드로 많은 로직과 기능을 이동하게 되었습니다. 일반적으로 서버측에서 수행되던 대부분의 작업은 이제 클라이언트측에서 편리하게 수행되고 있습니다.

우리가 바닐라 자바스크립트로 그 모든 복잡성을 처리할 수 없는 것은 항상 시간문제였다. 이러한 복잡성을 처리하고 숨겨야 하는 필요성은 오늘날 우리가 보는 JavaScript 프레임워크의 증가로 이어졌습니다.

물론, 이것은 또한 자체적인 비용과 함께 왔다.

자바스크립트 언어 자체의 부족한 부분을 채우기 위해 이러한 프레임워크들은 우리에게 많은 빛나는 새로운 것들을 주었다. 데이터 바인딩, DOM 분산을 통한 더 쉬운 DOM 조작, 상태 관리 및 기존 아키텍처와 같은 몇 가지 사항은 언급할 수 있습니다.

하지만 또 무슨 대가를 치르나요?

당신이 틀을 악으로 그린다고 나를 공격하기 전에, 나는 나 자신이 무거운 프레임워크 사용자라는 것을 지적해야 한다. 특히 Vue.js. 하지만 때때로, 프레임워크가 우리가 필요로 하는 것보다 훨씬 더 많은 것을 하는 것처럼 느껴집니다. 그리고 솔직히, 이것은 또한 문제로 여겨질 수 있습니다.

다행히도, 얼마 전에 스벨트를 우연히 만나 그것을 제작 프로젝트에 실험해 보았습니다. 재미있었어요. 자, 여기 스벨테 복음을 전합니다.

### 그렇다면, 스벨트는 무엇일까요?

Angular, React 및 Vue와 같은 프레임워크는 브라우저에서 실행되므로 이러한 프레임워크 중 하나를 사용하여 만든 앱을 실행할 때마다 프레임워크가 먼저 부팅되어 자체 애플리케이션 코드가 실행됩니다.

이것은 두 가지 면에서 불리하다. 우선, 생산으로 수출되는 규모는 보통 그래야 할 만큼 무거울 것이다. 프레임워크 코드와 응용 프로그램 코드를 모두 내보내고 있기 때문입니다. 둘째, (프레임워크가 부팅되는 단계 동안) 실행의 초기 지연이 있다. 그러나 이후 실행 시 상황은 더 빨라집니다.

스벨트는 위에 언급된 두 가지 문제를 해결하는데 도움을 줍니다.

하지만, 어떻게 그럴 수 있을까요?

스벨트는 프레임워크이다(실제로 컴파일러)이다. 빌드 프로세스 중에 HTML, CSS, JS 코드를 "작은" 자바스크립트 코드로 컴파일한다.

이렇게 하면 애플리케이션 사용자에게 제공되는 추가 프레임워크가 없습니다. 단지 비즈니스 논리일 뿐입니다.

### 다른 프레임워크와의 비교(성능)

Svelte를 프로덕션에서 사용해 달라고 요청하는 것은 많은 것을 알고 있습니다. 하지만 왜 당신이 이 결정을 후회하지 않는지 이유를 설명하겠습니다. Svelte를 사용함으로써 얻을 수 있는 몇 가지 이점을 이해하기 위해, Svelte가 다른 확립된 프레임워크와 어떻게 비교하는지 보여드리겠습니다. 우리는 Vue.js, React 및 Angular에 대해 Svelte를 벤치마킹할 것이다.

위의 그림 1을 보면, 상호 작용성과 총 번들 크기에 관한 한 스벨트가 확실한 승자라는 것을 알 수 있다.

그림 2에서 메모리 사용량 측면에서 보면 Svelte가 상위권에 올라섰음을 알 수 있습니다.

이 벤치마크 테스트는 Krausest의 프레임워크 벤치마크 도구를 사용하여 수행되었습니다.

### 스벨트를 사용하는 인기 사이트

프로덕션에서 Svelte를 사용하기로 결정했다면 혼자가 아니라는 것을 확신할 수 있습니다. 이미 사용하고 있는 기성 기업들도 많다.

다음은 이 제품을 사용하는 몇 가지 인기 있는 회사입니다.

이름 코치, 라쿠텐, 1비밀번호, 뉴욕타임즈, 크리에이티브 팀스, 메일.뤼

svlte.dev의 프로덕션에서 Svelte를 이미 활용하고 있는 사이트를 더 많이 찾을 수 있습니다.

## 스벨트를 생산에 사용해야 합니까?

스벨트는 좋은 개발자 경험을 약속합니다. 전환 시 누릴 수 있는 이점은 다음과 같습니다.

- 최소 학습 곡선: 스벨트는 자신이 엄청나게 배우기 쉽다고 자부합니다. 일반적인 HTML, CSS, Javascript로 Svelte 구성요소를 작성하기 때문에 약 5분 안에 Svelte 앱 작성을 시작할 수 있습니다.
- 실행 속도: 앞에서 언급했듯이, Svelte는 컴파일러이며, 빌드 프로세스 동안 Svelte 구성 요소는 바닐라 JavaScript 코드로 변환됩니다. 이렇게 하면 브라우저에서 코드가 실행되기 전에 프레임워크 부팅 또는 부트스트래핑의 오버헤드를 방지할 수 있습니다.
- 구성 요소 기반 앱 개발: 다른 프레임워크를 사용한 적이 있는 경우 앱에서 재사용 가능한 구성 요소를 만드는 것이 얼마나 유용한지 알 수 있을 것입니다. 스벨트는 또한 이러한 접근 방식을 핵심으로 하여 구축된다.
- 전체 앱을 빌드하는 데 사용할 수 있으며 증분 사용: Vue.js와 마찬가지로 Svelte를 사용하여 앱을 완전히 빌드하거나 프로그램의 일부에 추가할 수 있습니다.
- 스코프 스타일링 즉시 사용 가능: 스코프 스타일링을 사용하면 CSS가 다른 구성 요소로 유출될 염려 없이 구성 요소를 스타일링할 수 있습니다.
- 배터리 포함: 상태 관리, 템플릿, 서버 측 렌더링, 플러그인 시스템 및 애니메이션은 Svelte와 함께 제공되는 많은 도구 중 일부입니다.
- 성장하는 커뮤니티: 스벨트는 여전히 비교적 새로운 프레임워크이지만, 그 공동체는 이미 빠르게 성장하고 있다. Svelte on Discord에 대한 토론에 참여할 수 있으며 Stack Overflow에 대한 질문도 1,000개가 넘습니다.

### 왜 못 지나가니?

이 시점에서 "내가 왜 스벨트를 생산에 사용해야 하는가?"라고 여전히 묻는다면, 더 나은 질문을 드리겠습니다. 왜 안 돼?

주요 지원 없음(아직)

Vue.js와 Angular는 구글이 강력하게 지원하는 반면, React는 페이스북이 지원한다. 스벨트는 현재 주요 기업이 없어 기업과 개발자들 사이에서 여전히 인기가 낮다.

작은 공동체

스벨트는 상당히 새롭기 때문에 다른 프레임워크가 즐기는 대형 커뮤니티와 개발자 팬은 아직 없습니다.

툴링 및 패키지 지원

개발자 도구와 패키지의 경우, 현재 Svelte 개발자가 선택할 수 있는 옵션이 제한되어 있습니다. 하지만 공동체가 성장하고 더 많은 개발자들이 스벨트가 놀랍다는 것을 발견하기 시작하면서, 이 문제는 사라질 것입니다.

## 결론.

이 게시물의 과정 내내, 우리는 스벨트 프레임워크의 장단점을 모두 살펴보았습니다. 의심할 여지 없이, 찬성이 반대보다 더 많다.

Svelte가 개발자로서 가질 수 있는 모든 문제에 대한 완벽한 솔루션이 아닐 수도 있는 한(아무것도 그렇지 않은 경우) Svelte는 제공할 수 있는 것이 없습니다. 그리고 이것은 시작에 불과합니다.