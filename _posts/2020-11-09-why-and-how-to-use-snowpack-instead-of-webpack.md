---
layout: post
title: "웹 팩 대신 Snowpack을 사용하는 이유 및 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/webpack-snowpack-comparison-bundle.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/webpack-snowpack-comparison-bundle.png?fit=730%2C487&ssl=1)

웹팩은 그것의 효과적인 모듈 번들링 능력 때문에 수년간 개발자들이 즐겨 사용해 왔다. 다만 2019년에는 속도와 사용 편의성을 높인 웹팩의 가벼운 대안인 스노우팩이 도입됐다. 이 자료에서는 이 두 가지 도구를 간략하게 비교하고 Snowpack을 시작하는 데 대한 단계별 가이드를 제공합니다.

## 웹 팩이란 무엇입니까?

2012년 처음 출시된 이후 웹팩은 웹 개발에 필수적인 도구로 널리 인정받고 있다. Webpack은 많은 수의 JavaScript 파일을 가져와서 하나의 출력 파일이나 앱을 실행하는 데 사용할 수 있는 몇 개의 작은 파일로 변환하는 모듈 번들러입니다.

웹 팩은 모듈 간의 종속 관계를 관리하는 종속성 그래프를 만듭니다. 로더를 추가하여, 툴은 또한 CSS, PNG, JPG 등과 같은 자바스크립트 이외의 다른 파일 형식을 번들로 제공할 수 있다. 이렇게 하면 모든 정적 자산을 JavaScript와 함께 종속성 그래프에 효과적으로 추가할 수 있으므로 각 종속성을 수동으로 관리하는 데 필요한 추가 작업이 줄어듭니다.

웹 팩의 다른 이점은 다음과 같습니다.

- 사용하지 않는 자산의 효율화 및 제거
- 자산 처리에 대한 유연한 제어
- 페이지 로드 속도를 높이기 위해 여러 JavaScript 출력 파일로 코드 분할

그러나 웹 팩의 강력한 복잡성에는 다음과 같은 몇 가지 단점도 있습니다.

- 어려운 구성 프로세스
- 가파른 학습 곡선
- 느린 빌드 시간

## Snowpack이란?

2019년에 도입된 스노팩은 자바스크립트 애플리케이션을 위한 차세대 프런트 엔드 빌드 툴이다. 스노팩은 웹팩에 비해 빠르고 가벼우며 초보자용으로 훨씬 쉽게 구성할 수 있다.

## 스노우팩과 웹팩은 어떻게 비교되나요?

웹 팩과 달리 Snowpack은 자산 파일을 수정하고 저장할 때마다 전체 재구축 및 재결합 프로세스를 수행할 필요가 없습니다. 웹 팩의 시간 집약적인 빌드 프로세스는 파일을 저장하고 브라우저에서 변경 내용이 로드되는 것을 보는 것 사이의 지연을 설명합니다. 일부 개발자들은 대형 앱의 대기 시간을 무려 12분으로 보고하기도 했다.

일부 사람들은 UgliifJsPlugin 업그레이드, 이미지 로더 분사, 캐싱 전략 재고, 노드-ass에서 sass-loader로 전환 등 웹 팩의 속도를 높이기 위한 현명한 해결책을 고안했다.

프로젝트 일정에서 며칠을 빼서 빌드 성능을 분석하고 최적화 계획을 짜낼 여유가 없는 고객에게 Snowpack은 더 나은 옵션입니다.

## Snowpack은 어떻게 작동합니까?

그렇다면 스노우팩의 차이점은 무엇일까요? JavaScript의 ESM(ES 모듈 시스템)을 사용하여 대기 시간 없이 브라우저에 직접 그리고 즉시 파일 변경 사항을 기록합니다. 웹 팩과 같은 번들러가 아닌 Snowpack은 모든 파일을 한 번 빌드한 다음 캐시합니다. 파일을 변경할 때만 파일을 재구성하면 되며 재구성은 해당 단일 파일에 대해서만 수행됩니다.

Snowpack을 사용하면 번개처럼 빠른 번들링되지 않은 개발 환경에서 작업할 수 있으며 최종 빌드를 번들링되지 않고 실행할 수 없게 유지할 수도 있습니다. 또한 앱을 프로덕션으로 원격 설치할 때 번들 빌드를 지원하는 옵션도 있습니다. 즐겨찾는 번들러에 적합한 플러그인을 추가하거나 Snowpack 플러그인 API를 사용하여 직접 작성하기만 하면 됩니다.

간단히 말해, Snowpack은 웹 팩과 같은 기존 번들러 툴에 비해 다음과 같은 이점을 제공합니다.

- 재구성에 대한 대기 시간 없이 브라우저에 즉시 기록된 변경 사항
- 중복 다운로드가 없는 효율적인 캐시
- Snowpack의 ESM 기반 덕분에 CDN 서버에서 모듈을 가져올 수 있습니다.
- 간단하고 쉬운 구성 프로세스
- 쉬운 학습 곡선

Snowpack이 웹 팩에 어떻게 대비하는지 자세히 알아보려면 당사의 블로그 게시물을 참조하십시오.

## Snowpack 시작하기

당신은 조사를 했고, 도구를 비교했고, 스노우팩이 당신을 위한 것이라고 결정했어요. 그럼 어떻게 시작할까요? 다행스럽게도, 당신은 적당한 장소에 왔군요.

이 기사의 나머지 부분에서는 개발 환경을 마이그레이션하고 Snowpack을 사용하여 앱을 실행하는 단계를 안내합니다.

## 전제조건

시작하기 전에 설치를 완료하는 데 필요한 항목이 있는지 확인하십시오. 다행히 스노우팩의 요구사항은 미미하다. 툴을 개발하려면 다음이 필요합니다.

- ESM-ready 코드로 작성된 JavaScript 앱
- 최신 버전의 Chrome, Firefox 또는 Edge와 같은 ES 모듈 구문을 지원하는 최신 브라우저

> 팁: 최신 브라우저 요구 사항은 개발 환경에만 적용됩니다. Snowpack으로 최종 빌드를 만들고 프로덕션으로 전환하면 사용자는 최신 또는 레거시 브라우저로 사이트를 볼 수 있습니다.

### 1. 스노우팩 설치

다음 설치 명령을 사용할 수 있습니다.

npm 사용:

```coffeescript
id="globalinstallation">global installation
npm install -g snowpack
local installation per project
npm install --save-dev snowpack
```

Snowpack의 작성자는 가능한 경우 전역이 아닌 로컬로 설치하는 것이 좋습니다.

실 포함:

```bash
local installation per project 
yarn add --dev snowpack
```

### 2. Snowpack 명령줄 인터페이스(CLI) 시작

Snowpack CLI를 로컬로 실행할 수 있는 세 가지 방법은 다음과 같습니다.

- `root 패키지`를 사용합니다.json의 파일
- npm 명령 사용: `npx snowpack`
- yarn snowpack 명령 사용:

### 3. Snowpack App 생성 템플릿을 사용하여 앱을 초기화합니다.

새 앱 프로젝트를 생성하려면 CSA(Snowpack App)를 사용하십시오. CSA를 사용하면 Snowpack의 앱 템플릿 중 하나를 사용하여 구성된 새로운 초기화된 앱을 얻을 수 있습니다. Snowpack은 React, Vue, Svelte 및 기타 널리 사용되는 프레임워크에 대한 템플릿을 제공합니다.

다음은 대응에 Snowpack의 템플릿을 사용하는 샘플 초기화 명령입니다.

```cpp
npx create-snowpack-app new-dir --template [@snowpack/app-template-react] [--use-yarn]
```

공식 CSA 템플릿 목록은 Snowpack 설명서 페이지에서 확인할 수 있습니다.

### 4. 기존 앱을 Snowpack으로 마이그레이션

CSA 템플릿을 사용하여 만든 앱을 기존 앱을 마이그레이션하는 시작점으로 사용합니다.

스노우팩은 Babel, PostCSS, Create React App 등 오늘날 웹 개발에 사용되는 대부분의 기술을 지원한다. 앱에서 이미 지원되는 기술 중 하나를 사용하고 있으므로 Snowpack으로의 마이그레이션은 매우 간단합니다.

실제로 기존 Create React App 프로젝트가 있으면 변경 없이 CSA를 통해 직접 실행할 수 있습니다.

앱을 마이그레이션하려면 먼저 선택한 CSA 템플릿을 사용하여 새 앱 프로젝트를 초기화합니다(지침은 이전 섹션 참조). 그런 다음 기존 앱 사이트의 src 및 public 디렉터리에서 모든 파일을 복사합니다. 이제 개발 서버를 시작하고 `snowpack dev` 명령을 사용하여 사이트를 로컬로 로드합니다.

이제 문제가 발생할 수 있는 모든 문제를 해결해야 합니다. 걸리면 스노팩에서 도움을 요청하십시오.

마지막으로, 준비가 되었으면 `스노우팩 빌드`를 위한 사이트를 구축하십시오.

## 결론

기술적으로는 웹 팩만큼 강력하지는 않지만 Snowpack은 대부분의 개발자가 필요로 하는 모든 기능을 제공하는 더 빠르고 사용자 친화적인 대안을 제공합니다. 워크플로우를 간소화하거나 속도를 높이려는 경우 Snowpack에서 작업을 수행할 수 있습니다.