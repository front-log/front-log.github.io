---
layout: post
title: "재료 UI 프로젝트에 사용자 지정 글꼴을 추가하는 3가지 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/3waystoaddcustomfontstoyourMaterialUIproject.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/3waystoaddcustomfontstoyourMaterialUIproject.png?fit=730%2C412&ssl=1)

## 재료 UI란?

재료 UI는 웹 인터페이스를 더 빠르고 쉽게 구축할 수 있는 React 구성 요소 라이브러리입니다. 개발자가 UI 구현에 많은 시간을 들이지 않고 애플리케이션에 기능을 추가하는 데 집중할 수 있도록 일반적으로 사용되는 UI 구성 요소 세트가 크게 구성되어 있습니다. 구글이 만든 재료설계 안내서의 원리를 활용했으며, 59,000개 이상의 스타와 1,800개 이상의 기고가 GitHub를 통해 리액트 개발자들이 가장 사랑하는 UI 라이브러리 중 하나이다.

## 재료 UI를 시작하는 방법

이 자료에서는 사용자가 생성-반응-앱 또는 다른 대응 도구 체인을 사용한다고 가정합니다. 자체 도구 체인 및 빌드 파이프라인을 설정하려면 글꼴을 로드하기 위한 플러그인 또는 로더를 포함해야 합니다.

시작하려면 create-react-app을 설치하십시오.

```undefined
/ with npm
npx create-react-app font-app
//with yarn
yarn create-react-app font-app
```

응용 프로그램에서 재료 UI를 사용하려면 npm 또는 yarn을 통해 설치합니다.

```coffeescript
// with npm
npm install @material-ui/core

// with yarn
yarn add @material-ui/core
```

그런 다음 App.js 내에서 작동할 일부 UI 구성 요소를 추가합니다.

```js
import React from 'react';
import Button from '@material-ui/core/Button';
import Typography from '@material-ui/core/Typography';
import './App.css';


function App() {
  return (
    <div className="App">
      <div>
      <Typography variant="h2" gutterBottom>
          Welcome to React
      </Typography>
        <Button variant="contained" color="secondary">Ready To Go</Button>
      </div>
    </div>
  );
}

export default App;
```

브라우저의 검사기를 사용하여 버튼과 헤더를 검사하면 로보토의 기본 글꼴 계열을 사용하여 렌더링되는 것을 볼 수 있습니다. 자, 어떻게 바꿀 수 있을까요?

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/welcome-to-react-in-roboto.png?resize=512%2C283&ssl=1)

## 재료 UI 프로젝트에 사용자 지정 글꼴을 추가하는 방법

아래에서는 재료 UI 프로젝트에 원하는 글꼴을 추가하는 세 가지 방법을 살펴보겠습니다.

## 방법 1: Google 글꼴 사용 CDN

Google Fonts로 이동하여 원하는 글꼴 제품군을 선택합니다. 칠랑카 필기체 쓰겠습니다. CDN 링크를 복사하여 `public/index.html` 파일의 `<head>`에 추가합니다.

```xml
<link href="https://fonts.googleapis.com/css2?family=Chilanka&display=swap" rel="stylesheet">
```

글꼴을 사용하려면 수신된 옵션을 기반으로 사용자 지정 테마를 생성하는 재료 UI에서 제공하는 API인 `CreateMuiTheme`와 응용 프로그램에 사용자 지정 테마를 주입하는 데 사용되는 구성 요소인 `TemeProvider`를 사용하여 글꼴을 초기화해야 합니다.

이것을 App.js 파일에 추가합니다.

```undefined
import { ThemeProvider, createMuiTheme } from '@material-ui/core/styles';

const theme = createMuiTheme({
  typography: {
    fontFamily: [
      'Chilanka',
      'cursive',
    ].join(','),
  },});
```

그런 다음 기본 재료 UI `테마 프로바이더` 구성 요소로 구성 요소를 포장하여 테마 소품으로 전달합니다. 테마 소품의 값은 정의된 테마의 이름이어야 합니다.

```xml
<ThemeProvider theme={theme}>
 <div className="App">
   <div>
     <Typography variant="h2" gutterBottom>
       Welcome to React
     </Typography>
     <Button variant="contained" color="secondary">Ready To Go</Button>
   </div>
 </div>
</ThemeProvider>
```

브라우저 검사 도구를 사용하여 구성 요소를 검사하면 글꼴 패밀리가 칠랑카로 변경되었음을 확인할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/material-chilanka.png?resize=1082%2C577&ssl=1)

## 방법 2: Google-Web 글꼴-도우미를 사용한 자체 호스트 글꼴

글꼴을 자체 호스팅하면 몇 가지 이점이 있습니다. 훨씬 빠르고 글꼴이 오프라인으로 로드됩니다.

Google-webfonts-helper는 자체 호스팅 글꼴을 번거롭지 않게 만드는 놀라운 도구입니다. 선택한 글꼴, 문자 집합, 스타일 및 브라우저 지원을 기반으로 글꼴 파일 및 글꼴 얼굴 선언을 제공합니다.

구글 글꼴을 검색하면 원하는 글꼴 가중치를 선택할 수 있습니다. 호세핀산스를 이용하겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/googlefonts.png?resize=512%2C234&ssl=1)

결과 글꼴 선언을 `src/index.css` 파일에 복사합니다. 글꼴 파일 위치를 사용자 지정할 수 있습니다. 기본값은 `.../fonts/finc`로 가정합니다. 다운로드된 글꼴을 `src/fonts` 디렉토리에 배치하기 때문에 `/fonts`를 사용할 것입니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/font-sources.png?resize=730%2C335&ssl=1)

마지막으로 파일을 다운로드합니다. 압축을 풀고 프로젝트에 적절한 위치인 `src/fonts`에 배치

이전과 마찬가지로, `CreateMuiTeme`를 사용하여 글꼴 패밀리를 정의하고 `TemeProvider` 구성 요소로 구성요소를 싸야 합니다.

```undefined
const theme = createMuiTheme({
 typography: {
   fontFamily: [
     'Josefin Sans',
     'sans-serif',
   ].join(','),
},});
```

지금 확인해보니, 당신은 그 글꼴 패밀리가 호세핀 산스로 바뀌었음을 알 수 있을 것이다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/material-josefin.png?resize=1086%2C592&ssl=1)

## 방법 3: Typefaces NPM 패키지를 사용하는 자체 호스트 글꼴

타이프페이스(Typefaces)는 Kyle Matthews에 의해 만들어진 구글 글꼴과 다른 오픈 소스 글꼴을 위한 NPM 패키지의 모음이다. 글꼴은 다른 종속성과 마찬가지로 NPM과 함께 설치하여 프로젝트에 추가할 수 있습니다.

가능한 한 NPM을 통해 모든 웹(사이트|app) 의존성을 관리해야 하므로 이 방법이 가장 좋습니다.

리포트를 통해 선택한 서체를 검색하고 글꼴 폴더를 클릭하여 필요한 npm 설치 명령을 찾으십시오. 코모란트로 하겠습니다.

```coffeescript
npm install typeface-cormorant
```

그런 다음 프로젝트의 입력 파일인 `src/index.js`로 패키지를 가져오십시오.

```coffeescript
import "typeface-cormorant";
```

또한 이전과 마찬가지로 CreateMuiTheme을 사용하여 글꼴 패밀리를 정의하고 테마 공급자 구성 요소로 구성요소를 싸야 합니다.

```undefined
const theme = createMuiTheme({
  typography: {
    fontFamily: [
      'Cormorant',
      'serif',
    ].join(','),
},});
```

지금 확인해보시면, 글꼴 패밀리가 코모란트로 바뀌었습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/cormorant.png?resize=512%2C297&ssl=1)

## 보너스

헤더와 단추에 대해 다른 글꼴을 정의하려면 어떻게 해야 합니까? 기본 글꼴과 보조 글꼴을 말합니까?

두 개의 테마 상수를 정의하고 테마 공급자 구성 요소(각각 해당 글꼴의 테마 받침대)로 원하는 구성 요소를 줄이면 됩니다.

예를 들어, 머리글에 Cormorant 글꼴을 사용하고 버튼에 Josefin-sans를 사용하려면 먼저 Cormorant 글꼴과 Josefin-sans 글꼴에 대해 각각 두 가지 테마를 정의합니다.

```undefined
const headingFont = createMuiTheme({
  typography: {
    fontFamily: [
      'Cormorant',
      'serif',
    ].join(','),
},});

const buttonFont = createMuiTheme({
  typography: {
    fontFamily: [
      'Josefin Sans',
      'sans-serif',
    ].join(','),
},});
```

그런 다음 대상 구성요소를 다음과 같이 필요한 글꼴의 `테마 제공자` 구성요소로 묶습니다.

```js
function App() {
  return (

    <div className="App">
      <div>
        <ThemeProvider theme={headingFont}>
          <Typography variant="h2" gutterBottom>
            Welcome to React
          </Typography>
        </ThemeProvider>
        <ThemeProvider theme={buttonFont}>
          <Button variant="contained" color="secondary">
                         Ready To Go
          </Button>
        </ThemeProvider>

      </div>
    </div>
  );
}
```

그리고 비올라! 이제 머리글이 Cormorant 글꼴로 렌더링되고 버튼이 Josefin Sans 글꼴로 렌더링되는 것을 볼 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/cormorant-1.png?resize=512%2C303&ssl=1)

## 결론

이 기사에서는 재료 UI 프로젝트에 사용자 지정 글꼴을 추가하는 세 가지 방법에 대해 다루었습니다. 우리는 또한 다른 구성 요소에 대해 별도의 글꼴을 정의하는 방법에 대해서도 알아보았다. 이 글을 읽으면 주제에 관한 어떤 질문이든 대답할 수 있었기를 바랍니다.

자, 이제 멋진 것을 만들어 보세요!