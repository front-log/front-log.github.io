---
layout: post
title: "Tippy를 사용하여 반응에서 도구 설명 위치 지정"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/Untitled-design-1.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/Untitled-design-1.png?fit=730%2C487&ssl=1)

도구 추가 정보는 커서가 응용프로그램의 요소 위에 있는 동안 나타나는 텍스트 상자로, 사용자에게 필요한 추가 정보를 표시하는 데 유용합니다.

Tippy.js는 다른 팝업 스타일 GUI 툴뿐만 아니라 툴팁 솔루션을 제공하는 가볍고 사용하기 쉬운 라이브러리이다. Tippyjs-react는 개발자들이 도구 팁, 팝업, 드롭다운 메뉴 및 리액트 프로젝트의 다른 요소들을 통합할 수 있게 해주는 Tippy 라이브러리의 구성 요소이다.

이 튜토리얼에서는 Tippy를 사용하여 React 프로젝트에서 툴팁을 생성하고 배치하는 방법을 보여줍니다.

## 전제조건

먼저 시작하기 전에 React 16.8+, Node.js 및 npm 또는 Ran을 설치해야 합니다. 이 가이드를 최대한 활용하려면 다음 사항에 익숙해지는 것이 좋습니다.

- 자바스크립트
- HTML
- 반응하다
- 함수, 개체, 배열 및 클래스와 같은 프로그래밍 개념

## 시작 중

명령줄에서 다음 명령 중 하나를 실행하여 tippyjs-react를 설치합니다.

```coffeescript
npm i @tippyjs/react
//OR
yarn add @tippyjs/react
```

클래스 및 Tippy 도구 팁을 추가할 요소를 사용하여 React 프로젝트를 열거나 만듭니다.

이 튜토리얼에서는 뉴스레터 요청 양식을 예로 들어 보겠습니다. 특히, 양식의 제출 단추에 도구 설명을 추가하는 데 중점을 둡니다.

## 대응 프로젝트에 Tippy 도구 설명 추가

다음 명령을 사용하여 Tippy 구성 요소 및 코어 CSS를 가져옵니다.

```coffeescript
import Tippy from '@tippyjs/react';
import 'tippy.js/dist/tippy.css';
```

tippy.css 가져오기 문은 선택 사항입니다. 그러면 추가 작업 없이 툴팁이 연마된 것처럼 보일 수 있습니다. 이를 "기본 티피"라고 합니다. 처음부터 자신만의 Tippy 요소 또는 "Tippi"를 만들고 스타일링하려면 "tippy.css" 대신 Headless Tippy를 가져와 사용할 수 있습니다.

```coffeescript
import Tippy from '@tippyjs/react/headless';
```

여기서는 `tippy.css`와 함께 기본 Tippy를 사용하여 Tippy React 툴팁을 만들고 배치합니다.

Tippy 구성 요소를 가져온 후 도구 설명을 추가할 요소 주위에 Tippy 래퍼(Tippy Wrapper)를 삽입합니다.

```xml
   <Tippy content="We'll never share your email with anyone else.">
   <Button variant="outline-success" >Submit</Button>
   </Tippy>
```

이 명령은 Submit 버튼을 표시합니다. 커서가 버튼 위로 이동할 때마다 "다른 사람과 전자 메일을 공유하지 않습니다."라는 텍스트와 함께 도구 설명이 나타납니다.

## 티피 반응 도구 설명 위치 지정

지금까지 제출 단추와 관련하여 Tippy Retact 도구 설명을 어디에 배치할 것인지 지정하지 않았습니다. 도구 팁 배치를 지정하지 않으면 Tippy는 자동으로 요소 위에 위치를 지정합니다. 요소가 페이지 맨 위에 있고 위에 공간이 없는 경우 Tippy는 자동으로 요소 아래에 도구 설명을 배치합니다.

현재 코드는 도구 설명이 그 위에 나타나는 페이지 요소를 표시합니다.

Tippy의 기본 툴팁 배치가 항상 이상적인 것은 아닙니다. 대부분의 경우 도구 설명이 중요한 페이지 요소를 덮지 못하도록 배치를 지정해야 합니다.

Tippy를 사용하면 반응 프로젝트에서 툴팁의 위치를 쉽게 변경할 수 있습니다. 배치 옵션은 다음과 같습니다.

- 맨 위
- 하의
- 왼쪽
- 맞다

이 예에서는 전자 메일 필드의 방해받지 않는 보기를 원하므로 도구 설명을 `버튼` 요소의 오른쪽으로 이동하십시오.

```xml
  <Tippy placement='right' content="We'll never share your email with anyone else.">
  <Button variant="outline-success" >Submit</Button>
  </Tippy>
```

Tippy 명령을 중첩하여 단일 요소에 여러 개의 팁을 배치할 수 있습니다.

```xml
  <Tippy placement='top' content="Top Tooltip">
  <Tippy placement='bottom' content="Bottom Tooltip">
  <Button variant="outline-success" >Submit</Button>
  </Tippy>
  </Tippy>
```

이 명령은 관련 요소 위에 "Top Tooltip"이라고 표시된 도구 설명과 동일한 요소 아래에 "Bottom Tooltip"이라고 표시된 다른 도구 설명을 삽입합니다.

단일 요소에 여러 개의 Tippy 래퍼를 삽입할 때 다른 도구 설명 배치를 지정해야 합니다. 그렇지 않으면 한 도구 설명에서 해당 요소의 다른 모든 부분을 다룹니다.

## 리액션의 Tippy 툴팁에 대한 기타 팁 및 트릭

Tippy 명령 내에 HTML 블록을 추가하여 툴팁을 변경하고 스타일링할 수 있습니다. 예를 들어 `span` 태그를 추가하여 툴팁의 텍스트 색상을 주황색으로 변경할 수 있습니다.

```undefined
  <Tippy placement='right' content={<span style={color: 'orange'}>Orange</span>}>
  <Button variant="outline-success" >Submit</Button>
  </Tippy>
```

요소를 가리키는 도구 설명 가장자리의 작은 화살표를 제거하려면 `화살표` 특성을 false로 설정합니다.

```xml
  <Tippy placement='right' arrow={false} content="We'll never share your email with anyone else.">
  <Button variant="outline-success" >Submit</Button>
  </Tippy>
```

또한 Tippy 래퍼에 지연 속성을 추가하여 커서가 요소 위로 맴돌 때와 도구 설명이 나타나는 시간 사이의 지연을 만들 수 있습니다. 지연은 밀리초 단위로 측정되므로 지연 속성을 1000으로 설정하면 1초 지연이 발생합니다.

```xml
  <Tippy placement='right' delay={1000} content="We'll never share your email with anyone else.">
  <Button variant="outline-success" >Submit</Button>
  </Tippy>
```

## 결론

이제 Tippy의 배치 속성을 사용하여 요소 주위에 툴팁을 배치하는 방법과 같은 Tippy 툴팁을 생성하고 포지셔닝하는 방법에 대한 기본 사항에 대해 알아보았습니다. 도구 팁을 사용자 정의하기 위해 HTML을 추가하거나 도구 설명의 화살표를 제거하거나 시간 지연을 추가할 수도 있습니다.

이 튜토리얼에 사용된 프로젝트와 작성된 코드는 GitHub에서 찾을 수 있습니다. 전체 프로젝트는 리액트 부트스트랩 프레임워크와 리액트 돔 라우터 모듈을 사용했지만, 이 문서에 엄격하게 포함된 정보에 대해서는 필요하지 않다.

## 추가 리소스

React와 함께 Tippy.js를 사용하는 방법에 대해 자세히 알아보려면 다음과 같은 우수한 리소스를 확인하십시오.

- 실제 반응: 툴팁
- Tippy.js를 사용한 멋진 툴팁
- 반응을 위한 Tippy.js 시작