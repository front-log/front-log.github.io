---
layout: post
title: "Guess.js 및 개츠비 사이트 최적화"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/guess-js.png
tags: undefined
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/guess-js.png?fit=730%2C487&ssl=1)

Guess.js는 예측 분석을 사용하여 사용자 경험을 향상시키는 라이브러리와 도구의 모음이다.

개츠비.js는 즉시 훌륭한 성능 최적화를 제공하지만, 이 글은 사이트 효율성을 높이고 UX를 개선하기 위해 게츠비 사이트로 Guess.js를 통합하는 방법을 설명할 것이다.

## 도입

개츠비.js는 현대의 반응 기반 정적 사이트 생성기(SSG)이다. 정적 사이트 생성기는 정적 사이트를 만들기 위한 도구를 게시하는 중입니다. 이러한 사이트는 주로 HTML 파일과 서버에 배포된 자산이 들어 있는 폴더로 구성됩니다.

기존 웹 개발 스택도 데이터, 컨텐츠 및 템플릿에서 페이지를 생성하지만 이 프로세스는 사용자가 요청 시 페이지를 요청할 때만 시작됩니다. 페이지 구성은 클라이언트가 페이지를 요청할 때마다 반복되므로, 사이트의 인기가 높아지고 페이지 보기가 증가하면 페이지 작성에 사용되는 시간도 증가합니다. 이는 사이트 성능에 부정적인 영향을 미칩니다.

반면에 SSG를 사용하면 페이지 작성 프로세스가 빌드 시간에 수행됩니다.

SSG 아키텍처는 빌드 시 페이지 구성을 시작하여 사이트의 페이지 뷰가 증가함에 따라 요청 시 페이지 구성이 지속적으로 증가하는 것을 방지합니다. 모든 페이지는 빌드 시간에 구성되므로, 사용자의 페이지 요청에 대한 응답으로 서버가 해야 하는 모든 작업은 파일 전송입니다. 이 방법을 사용하면 페이지 뷰가 증가해도 사이트 성능에 영향을 미치지 않으므로 사용자 환경이 향상됩니다.

다양한 SSG가 존재하며 각각 다르게 구현됩니다. 그러나 위에서 논의한 기본 원칙은 각각 유효합니다.

이 기사에서는 속도, 플러그인 및 직설성으로 유명한 리액트 기반 SSG인 개츠비.js에 초점을 맞출 것이다.

## 개츠비.js의 작동 방식

### '개발' vs '구축'

Gatsby.js는 사이트를 컴파일하기 위한 두 가지 명령을 가지고 있는데, 각각은 별개의 사용 사례를 가지고 있다. 다음 명령은 다음과 같습니다.

- `비교 발전`
- `비틀린 체격`

gatsby develop 명령어는 라이브 재로드와 개츠비 그래피QL 탐색기와 같은 기능을 지원하는 개발 서버를 회전시키는 데 사용된다. 개츠비 탐색기는 개발자들이 런타임에 개츠비 사이트 및 데이터 탐색기와 상호 작용할 수 있게 한다.

반면에 "gats by by build" 명령은 사이트 개발이 완료되면 실행해야 합니다. 운영 준비 최적화 기능을 갖춘 사이트 버전을 생성합니다. gats by build 명령을 실행하면 사이트의 구성, 데이터 및 코드가 패키징됩니다. 또한 사이트의 모든 정적 HTML 페이지를 만든 다음 응답 응용프로그램으로 재하이드레이션됩니다.

gats by build 명령을 실행하면 Node.js 서버가 후드 아래에서 전체 빌드 프로세스를 실행합니다.

빌드 시 개츠비는 서버측 API를 호출하여 최적화된 정적 컨텐츠(HTML, CSS, JavaScript, 이미지 등)를 생산한다.

### 서버측 렌더링(SSR)

개츠비가 Node.js 서버를 사용하여 자바스크립트 모듈에서 정적 HTML 페이지를 생성하는 과정을 서버측 렌더링 또는 SSR라고 한다. 개츠비는 우리 사이트의 모든 정적 컨텐츠(HTML 페이지, 이미지 등)를 생산하고 그 컨텐츠를 서버에 배포하여 브라우저에서 요청하고 렌더링할 수 있다.

SSR가 빌드 시 수행되므로 Gatsby는 최적화된 데이터 및 페이지로 전체 사이트를 미리 구축할 수 있습니다. 사용자가 페이지를 요청할 때마다 재구성을 반복할 필요 없이 서버가 사용자 요청에 즉시 응답할 수 있기 때문에 사이트 성능에 상당한 영향을 미칩니다.

아래 이미지에서는 이에 대해 자세히 설명합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/gatsby-build-1.png?resize=730%2C487&ssl=1)

SSR에서 사전 빌드된 페이지를 사용하도록 설정한 사용자 요청에 서버가 즉시 응답하면 개츠비 사이트가 즉시 로드될 수 있습니다. 다음 섹션에서는 Guess.js를 사용하여 개츠비 사이트가 추가 성능 최적화를 달성하는 방법에 대해 알아보겠습니다.

## Guess.js를 사용한 성능 최적화

### Guess.js란?

Guess.js는 프레임워크에 구애받지 않는 도구와 라이브러리의 집합으로 지능형 프리페치를 사용하여 사용자 경험을 향상시킨다.

Guess.js는 지능적인 프리페치(pretective prefetching)를 위해 예측 프리페치(pretective prefetching Google Analytics(GA)와 같은 소스의 분석 데이터를 사용하고, 이 데이터를 사용하여 사용자가 요청할 가능성이 가장 높은 리소스를 지능적으로 사전 추출합니다.

사이트에 다음 페이지가 있다고 가정해 보겠습니다. 홈, 정보, 연락처 및 뉴스입니다. Google Analytics를 사용하여 사용자가 홈 페이지에서 해당 페이지에 액세스할 가능성을 확인할 수 있습니다. 예를 들어 GA는 사용자가 홈 페이지에서 뉴스 페이지를 방문할 확률은 96%이지만 연락처 페이지를 방문할 확률은 4%에 불과하다고 알려줄 수 있습니다.

이 경우 앱은 사용자가 홈 페이지에 있을 때 뉴스 페이지 데이터를 미리 가져오게 됩니다. 사용자가 다음 페이지로 이동할 가능성이 높기 때문입니다. 반면에 사용자 중 4%만이 해당 경로를 따르므로 연락처 페이지를 미리 가져올 지능적인 이유는 거의 없습니다.

Guess.js를 개츠비 애플리케이션에 통합하면 개츠비 앱이 Google Analytics 데이터를 다운로드할 수 있으며, 이 데이터는 미리 추출할 리소스를 위한 모델을 형성하는 데 사용된다.

다음 섹션에서는 Guess.js를 개츠비 사이트로 통합하는 방법에 대해 알아보겠습니다.

## 개츠비와 함께 Guess.js를 사용하라.js

지적했듯이, 개츠비는 솔직함과 플러그인으로 유명하다. 실제로 Guess.js를 Gatsby 앱으로 통합하는 작업은 매우 간단하며 복잡한 코드 논리나 웹 팩 구성 없이 다른 개츠비 플러그인-guess-js를 설치하고 구성하기만 하면 된다.

개츠비 사이트에 Guess.js를 추가하려면 다음 단계를 따르십시오.

먼저 다음을 실행하여 gats-plugin-guess-js 플러그인을 설치합니다.

```undefined
npm install gatsby-plugin-guess-js
```

또는:

```undefined
yarn add gatsby-plugin-guess-js
```

그런 다음 `gatsby-config.js`에서 아래와 같이 플러그인 구성을 추가하십시오.

```java
    module.exports = { 
      plugins: [
        ...
        { 
          resolve: "gatsby-plugin-guess-js", 
          options: { 
              // Find the view id in the Goggle Analytics > Admin > view settings
              GAViewID: `VIEW_ID`,
          }, 
        }
        ...
      ], 
    }
```

(선택한 경우 여기서 고급 구성을 얻을 수 있습니다.)

다음으로, 서버를 재시작하여 응용프로그램을 다시 작성합니다.

```undefined
gatsby develop
```

또는 애플리케이션을 구축하여 다음을 수행합니다.

```undefined
gatsby build
```

마지막으로, 재구축 중에 구성이 성공적이면 예측 사전 추출에 필요한 모델을 구축하는 데 필요한 Google Analytics 데이터에 액세스하기 위해 Guise.js를 Google 계정으로 인증하라는 메시지가 표시됩니다.

Guise.js가 개츠비 사이트에 성공적으로 통합되면 개츠비-플러그인-게스-js 플러그인은 구글 분석 데이터를 다운로드하여 예측 프리페치 모델을 만든다. 이 데이터를 사용하면 사이트의 HTML 페이지가 생성될 때 사용자가 방문할 가능성이 가장 높은 페이지에 pages 링크 프리페치(prefetch)를 추가합니다.

사용자가 사이트나 응용 프로그램에 참여할 때 플러그인은 사용자가 가장 방문할 가능성이 높은 페이지의 리소스를 계속 예측하고 미리 추출하므로 사이트 성능과 사용자 환경이 향상됩니다.

## 결론

Guess.js는 프레임워크에 구애받지 않는 혁신적인 성능 최적화 도구이다. 기계 학습 및 사용자 행동 분석을 통해 개츠비 사이트에 예측 가능한 프리페치 기능을 제공한다. 사용자 여정을 더 잘 이해함으로써 Guise.js는 사이트의 속도와 사용자의 전반적인 환경을 개선하는 데 도움이 될 수 있습니다.