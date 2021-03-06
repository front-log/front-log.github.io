---
layout: post
title: "Nux.js 2.13에서 완전 정적 모드를 완전히 활용하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/how-to-take-full-advantage-of-full-static-mode-in-nuxt-2-13-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/how-to-take-full-advantage-of-full-static-mode-in-nuxt-2-13-nocdn.png?fit=730%2C487&ssl=1)

Nux.js는 빠르고 강력한 웹 애플리케이션을 구축하기 위한 오픈 소스 프레임워크이다. 6월 18일, Nux 버전 2.13은 정적 앱에 대한 향상된 지원을 도입하여 유니버설 모드(`{ mode:`universal`})`의 앱 처리 방법을 지정할 수 있게 되었다. 이제 일반 서버측 렌더링 앱과 정적 앱의 처리 방식 사이에는 분명한 차이가 있습니다.

이 새로운 완전 정적 모드를 확대하여 Nux.js로 다음 정적 앱을 구축할 때 전력을 활용하는 몇 가지 방법을 살펴보겠습니다.

## Nux.js의 알려진 문제

Nux 2.13에 대해 자세히 살펴보기 전에 앞서 언급한 변화를 주도했던 몇 가지 문제에 대해 간략히 살펴보겠습니다.

- next generate(next generate)를 실행하면 필요 없는 경우에도 전체 앱이 재구성되어 배포 시간이 느려집니다.
- `next generate`를 실행할 때만 페이지가 제외되므로 SPA fallback을 테스트할 수 없습니다.
- next generate를 실행할 때만 `process.static`이 `true`이기 때문에 정적 앱을 위한 모듈 또는 플러그인을 구축하는 것은 지루합니다.

## 솔루션: '대상' 옵션

Nux 2.13에서 사용할 수 있는 `target` 옵션을 사용하면 Nux에서 앱을 처리하는 방법을 명시적으로 지정할 수 있습니다.

`nux.config.js` 파일에 설정하려면:

```css
export default {
  mode: 'universal',
  target: 'static' /* or 'server' */
}
```

이제 `nuxt dev`로 프로젝트를 개발 모드로 실행합니다.

- 404의 클라이언트 측 렌더링으로 돌아가기, 오류 및 리디렉션
- `process.static`을 `true`로 설정

참고: `모드`가 `spa`로 설정되어 있으면 전체 정적 모드가 작동하지 않습니다. 이 모드를 사용하려면 모드를 유니버설 모드로, 대상을 정적 모드로 변경해야 합니다. target의 기본값은 server이므로 target 옵션을 생략하면 항상 server가 됩니다.

Nux 2.13이 위에 나열된 문제를 해결하는 데 도움이 되는 다른 몇 가지 방법을 살펴보겠습니다.

### 더 이상 빌드 지연 없음

전체 정적 앱의 경우 코드에 변경 사항이 없는 경우 전체 프로젝트를 재구성할 필요가 없습니다. next generate가 실행되면(>= v2.14에서 사용 가능), Nux는 배포 중에 이전 빌드 캐시를 사용할 수 있을 만큼 충분히 스마트합니다. 이러한 개선으로 배포 속도가 약 3.6배 빨라집니다.

### 더 빠른 로드 시간

이제 모든 페이지는 HTML에 `mode:`universal`과 `target:`static`으로 미리 렌더링되며, `syncData`와 `fetch`가 포함된 HTTP 요청은 클라이언트 측 탐색에 사용될 JS 파일에 만들어 저장되므로 이 페이지로 이동하면 HTTP 요청이 실제로 수행되지 않습니다.

### 운영 사이트 로컬 실행

next generate를 실행하여 /dist 폴더에서 정적 페이지를 생성할 때 next start를 사용하여 정적 앱의 프로덕션 서버를 스핀업할 수 있습니다. 이는 정적 호스트에 배포하기 전에 로컬에서 테스트하는 데 적합합니다.

### 세대 구성

이제 `nux.config.js`에서 `generate` 속성을 사용하여 정적 앱이 생성되는 방식을 구성할 수 있습니다.

#### 캐시에서 파일 제외

변경 시 빌드가 트리거되지 않도록 프로젝트의 특정 디렉터리를 무시하도록 선택할 수 있습니다.

```js
export default {
  generate: {
    cache: {
      ignore: ['guides'] // changes in the guides folder will not cause a re-build
    }
  }
}
```

기본적으로 캐시는 다음 파일 및 디렉터리를 무시합니다.

- `멀리/`
- .nxxt/
- 정적인/정적인
- .env, .npmrc 및 기타 도트 파일
- `node_contract`
- README.md

#### 크롤러

이전 버전(= v2.12)에서는 `nux.config.js` 파일의 `generate.disc` 옵션에 동적 링크를 수동으로 추가해야 했습니다. Nux 2.13에는 상대 링크를 자동으로 감지하고 해당 링크를 위한 페이지를 생성하는 크롤러가 내장되어 있습니다.

크롤러가 일부 경로에 대해 생성을 건너뛰도록 하려면 `generate.exclude`를 사용하여 경로의 정규식 또는 문자열을 지정할 수 있습니다. 크롤러를 사용하지 않으려면 `generate.crawler`를 `false`로 설정하십시오.

```coffeescript
// in nuxt.config.js file
export default {
  generate: {
    crawler: false
  }
}
```

어떤 이유로 크롤러가 일부 페이지를 생성할 수 없는 경우 `generate.routs` 옵션을 사용하여 직접 페이지를 추가할 수 있습니다.

## 다음 단계

Nux 2.13에 소개된 새로운 기능의 정적 부분을 다루었으므로 이제 이러한 기능을 사용하여 애플리케이션을 개선할 때입니다.

다음은 다음 Nux 프로젝트에서 완전히 정지하기 위해 취할 수 있는 몇 가지 단계입니다.

- Nux v2.14로 업그레이드
- `nux.config.js` 파일에서 `target`이 `static`로 설정되어 있는지 확인하십시오.
- 패키지의 `scripts` 옵션에서 명령을 전환합니다.json의 파일은 다음과 같습니다.
"flict": {
"dev": "nux",
"generate": "next generate",
"start": "next start"
}

- `next dev`가 개발 서버를 시작하고 정적 모드에서 실행 중인지 여부를 인식합니다.
next generate는 정적 페이지를 `/dist`라는 폴더로 생성합니다.
next start(next start)가 프로덕션 서버를 스핀업하여 정적 앱을 서비스합니다.
- `next dev`가 개발 서버를 시작하고 정적 모드에서 실행 중인지 여부를 인식합니다.
- next generate는 정적 페이지를 `/dist`라는 폴더로 생성합니다.
- next start(next start)가 프로덕션 서버를 스핀업하여 정적 앱을 서비스합니다.

최신 Nux 릴리스에 도입된 기능 및 버그 수정에 대한 자세한 내용은 GitHub로 이동하십시오.