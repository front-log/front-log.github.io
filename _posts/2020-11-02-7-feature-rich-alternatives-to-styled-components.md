---
layout: post
title: "스타일 구성 요소에 대한 7가지 기능이 풍부한 대안"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/styled-components-alternatives.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/styled-components-alternatives.png?fit=730%2C487&ssl=1)

우리는 지난 몇 년 동안 CSS-in-JS가 현대의 프런트엔드 개발에 필수적인 부분이 되는 것을 보아왔다. 스타일 구성 요소 제작자 Max Stoiber에 따르면, React 설치의 약 60%가 CSS-in-JS 라이브러리를 설치한다고 한다. 2014년 11월에 JSS로 시작된 모험은 현재 비교적 안정적이며, 스타일 구성 요소들이 CSS-in-JS 시장의 큰 부분을 차지하고 있다.

여기서 우리는 큰 가치를 제공하고 다음 앱의 CSS-in-JS 라이브러리가 될 수 있는 스타일 구성 요소에 대한 몇 가지 멋진 대안을 공유할 것이다.

## Linaria: 제로 런타임 CSS-in-JS 라이브러리

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/linaria-logo.png?resize=730%2C236&ssl=1)

CSS-in-JS 라이브러리의 단점 중 하나는 DOM 요소가 변경될 때 대부분 `<style> 태그에서 스타일을 추가 및 제거하기 때문에 런타임 비용이다.

Linaria는 빌드하는 동안 파일의 모든 CSS를 추출하여 이 문제를 해결합니다. 또 다른 멋진 기능은 모든 동적 스타일이 CSS 변수를 사용하여 적용된다는 것이며, 이는 런타임으로부터 완전한 독립성을 이끌어낸다.

그러나 이는 비용이 수반됩니다. CSS 변수를 지원하지 않는 브라우저에서는 동적 스타일을 사용할 수 없습니다. Linaria는 중첩된 스타일에 대해 Sass와 유사한 구문도 지원합니다.

개발자 경험에 대해서는 스타일린트를 지원하고 원활한 디버깅 경험을 위한 CSS 소스 맵을 제공한다. 또한 웹 팩 가이드, 롤업 플러그인과 개츠비, 스벨트, 프리액트 가이드를 갖춘 바벨 로더가 있다.

스타일 구성 요소에서 이동할 계획이라면 리나리아에는 스타일 도우미가 있어 쉽게 전환할 수 있습니다. 또한 linaria/contract는 스타일링된 요소 같은 구문과 함께 동적 스타일을 지원합니다.

## 연결별 CSS 블록인

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/css-blocks-logo.png?resize=730%2C151&ssl=1)

오늘날 라이브러리를 스타일링하는 데 있어 핵심 과제 중 하나는 성능과 유지보수성 사이에서 최상의 균형을 찾는 것입니다. CSS 블록은 둘 다의 장점을 제공할 계획이다. CSS 블록은 CSS 모듈, BEM, Atomic CSS에서 영감을 받았다.

무엇보다도 CSS 블록은 정적 분석이 가능하다. 코드베이스를 보고 CSS의 어떤 부분이 사용되는지, 사용되지 않는 부분 또는 조건부로 사용되는지 분석할 수 있습니다. CSS의 모든 규칙을 반복 없이 고유한 그룹으로 나눕니다. CSS를 자신과 다른 개발자를 위해 더 쉽게 유지 관리하고 최종 사용자를 위해 더 잘 최적화할 수 있습니다.

CSS 블록은 스타일 구성 요소 또는 유사한 스타일링 라이브러리와 비교하여 새로운 모델을 제공한다. 일부 팀은 학습하고 적응하는 데 시간이 걸릴 수 있지만, 성능과 유지 보수성이 향상되어 가치가 있습니다.

## 실밥: 거의 제로(0)에 가까운 런타임과 동급 최고의 개발자 경험

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/stitches-logo.png?resize=730%2C365&ssl=1)

유지 관리자들은 Silbs를 거의 제로(0)에 가까운 런타임, 서버측 렌더링, 다양한 지원, 동급 최강의 개발자 경험을 갖춘 스타일링 라이브러리라고 설명한다. 리나리아 블록이나 CSS 블록과 비교했을 때, Silves는 아키텍처에서 스타일 구성 요소에 더 가깝다. 스타일 구성 요소보다 작은 크기로 유사한 API를 통해 동일한 기능의 대부분을 제공합니다.

Silbs의 가장 좋은 부분은 변형으로, 더 나은 구성요소 API를 개발하는 데 도움이 된다. 각 변형에 대한 스타일을 정의하고 결합할 수도 있습니다. 또한 Ming에 CSS 변수를 사용하므로 런타임 소품 보간을 피할 수 있어 사용 가능한 다른 스타일 라이브러리에 비해 성능이 상당히 향상된다.

또 다른 아름다운 기능은 토큰으로, 변수를 선언하고 이를 CSS 값으로 사용할 수 있습니다(예, 심지어 단축키에서도). 또한, 스타일 구성 요소의 스위치는 API가 상당히 유사하기 때문에 상대적으로 원활하다.

## 스타일트론: 구성요소 지향 스타일링을 위한 범용 툴킷

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/styletron-logo.png?resize=730%2C212&ssl=1)

모든 UI 라이브러리 또는 프레임워크에서 잘 작동하는 라이브러리와 구성 요소를 개발하는 것은 오늘날 모든 프런트 엔드 개발자들이 찾고 있는 것입니다. Styletron은 라이브러리에 구애받지 않으므로, 모든 UI 라이브러리에서 잘 작동하는 구성 요소를 리액티브 또는 다른 어떤 구성 요소와도 쓸 수 있습니다.

Styletron은 Atomic CSS와 Critical 렌더링 경로에 모두 적합합니다. 태그 스타일링에 필요한 CSS만 추가하고, 브라우저가 처리해야 하는 CSS의 크기를 줄이는 선언 수준의 중복 제거를 수행한다. 이 모든 것을 작은 8KB의 집기 라이브러리에서 얻을 수 있습니다. 개발자 경험에 대해서는 번들러 구성이나 툴링 설정이 필요하지 않습니다.

## 감정: 차세대 CSS-in-JS

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/emotion-logo.png?resize=450%2C450&ssl=1)

감정은 한동안 존재해왔고 다른 스타일링 도서관이 채택한 많은 아이디어를 개척했다. 프레임워크에 구애받지 않으며 스타일이 부착된 React 구성 요소를 생성할 수 있는 스타일 API도 있습니다.

감정은 소스 맵 지원으로 인해 훌륭한 개발자 경험을 제공합니다. 기본 제공 테마 메커니즘, ESLint 플러그인 및 CSS 소품 지원이 함께 제공됩니다.

요컨대, Emotion은 스타일 구성 요소가 제공하는 모든 기능을 갖추고 있으며, 번들 크기가 약간 작아 부팅이 가능하므로 최소한의 노력으로 스타일 구성 요소를 Emotion과 교체할 수 있습니다.

## 펠라: 상태 함수로서의 스타일

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/fela-logo.png?resize=406%2C176&ssl=1)

Fela는 고유한 원리에 기초합니다: 뷰가 주의 함수라면 CSS도 마찬가지입니다. 왜냐하면 뷰의 일부이기 때문입니다. React 및 Redux와 마찬가지로 Fela는 사용자에게 스타일을 작성하는 방법을 명시적으로 알려주지 않습니다. Fela는 스타일링 환경을 구축하는 데 도움이 되는 강력한 API를 제공합니다.

펠라는 동적 스타일을 핵심으로 생각하며 프레임워크에 구애받지 않도록 제작되었다. 또한 모든 규칙에는 고유 클래스가 부여되므로 Atomic CSS 원칙을 채택하여 CSS의 크기를 줄이고 성능을 향상시킨다.

그것의 API와 스타일 구성 요소로부터의 전환에 대해서, 펠라는 다른 정신 모델과 매우 다른 API를 가지고 있다. 속도를 따라잡는 데는 다소 시간이 걸릴 수 있지만, 이는 독특하고 역동적인 스타일이 많은 앱에 큰 이점이 있습니다.

## Goober: 1KB 미만의 CSS-in-JS 솔루션

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/goober-logo.png?resize=730%2C365&ssl=1)

구버는 스타일 구성 요소(~12KB)와 감성(~11KB)의 번들 크기 영향을 피하기 위한 동기 부여로 만들어진 덜 알려진 라이브러리이다. 구버는 모든 종류의 구성품이 제공된다고 주장한다.

성능의 관점에서 구버는 스타일의 구성 요소를 능가할 수 있다. SSR에 대한 벤치마킹에서는 Emotion에 약간 밀렸습니다. 그것의 특징에 대해서는, 가장 널리 사용되는 스타일 구성 요소들의 거의 모든 특징들이 구버와 함께 이용 가능하다.

API는 스타일링 컴포넌트와 상당히 유사하며, 멘탈 모델도 그대로인 만큼 스타일링 컴포넌트에서 구버로 이동하는 데 큰 번거로움이 없을 것이다.

## 다 싸고.

CSS-in-JS는 개발자들이 앱 스타일을 거의 번거롭지 않게 유지할 수 있는 방법을 제공했고 베어 CSS와 관련된 많은 문제들을 해결했다. 앞으로 나아가면서, 다양한 거대 기술 기업들과 함께 놀라운 프런트 엔드 커뮤니티는 이제 우리가 스타일 구성 요소에서 얻은 학습을 기반으로 확장 가능하고 더 성능이 뛰어난 솔루션을 구축하고 있습니다.

나는 이 멋진 것들을 유지하기 위해 노력하는 사람들에게 감사한다. 그들 모두는 그들만의 장단점을 가지고 있고, 각각은 특정한 시나리오에서 유용할 수 있다. 더 많은 것을 찾으신다면, Michele Bertoli와 비교해서 다른 CSS-in-JS 라이브러리를 살펴보십시오.