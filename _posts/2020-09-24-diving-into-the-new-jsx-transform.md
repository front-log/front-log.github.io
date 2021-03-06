---
layout: post
title: "새로운 JSX 혁신에 뛰어들기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/divingintothenewjsxtransform.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/divingintothenewjsxtransform.png?fit=730%2C412&ssl=1)

React 17을 사용하면 React를 사용하기 위해 파일에서 React를 더 이상 가져올 필요가 없습니다. 혼란스러웠나요? 이 자료에는 코드와 지식을 새로운 방식으로 마이그레이션하기 위해 알아야 할 사항이 나와 있습니다.

## JSX 변환이란 무엇입니까?

반응을 작성한 적이 있다면 일반 JavaScript 파일에서 이상한 HTML 모양의 구문을 발견했을 것입니다. 이 구문은 JSX라고 하며 유효한 JavaScript가 아닙니다. 다음과 같습니다.

```js
const Welcome = () => {
  return <h1 className="hero">Welcome!</h1>;
};
```

브라우저에서 이 코드를 실행하려면 컴파일러(일반적으로 Babel 또는 TypeScript)를 통해 코드를 실행해야 하며, 이 경우 코드가 유효한 JavaScript로 변경됩니다. 왜냐하면 JSX는 이 글을 쓰는 또 다른 방법이기 때문입니다.

```coffeescript
const Welcome = () => {
  return React.createElement("h1", { className: "hero", children: "Welcome" });
};
```

따라서 Babel 또는 TypeScript(또는 어떤 도구를 선택하든)는 `React.createElement` 함수에 대한 호출로 코드를 변경하며, 이에 대한 과제가 있습니다. 왜냐하면 갑자기 코드에서 "반응"을 참조하게 되었기 때문입니다! 파일 맨 위에 React를 가져오지 않은 경우 JavaScript는 이 컴파일된 코드를 어떻게 처리해야 하는지 알지 못합니다. 그렇기 때문에 모든 JSX 파일의 맨 위에 `react에서 반응 가져오기`를 추가해야 합니다.

모든 파일의 맨 위에 이것을 추가하는 것은 초보자에게는 큰 걸림돌이고 전문가들에게는 골칫거리이다. 반응 17을 사용하면 더 이상 지정할 필요가 없습니다!

React 팀은 커뮤니티와 협력하여 새로운 변환을 생성했으며, 이 변환은 새로운 사용자 지정 진입점에서 새로운 jsx 함수를 자동으로 가져옵니다. 컴파일된 코드는 다음과 같습니다.

```coffeescript
import { jsx as _jsx } from "react/jsx-runtime";

const Welcome = () => {
  return _jsx("h1", { className: "hero", children: "Welcome" });
};
```

이것은 또한 잠재적으로 더 작은 묶음 크기의 사랑스러운 부작용을 낳을 것입니다!

## 어떻게 해야 되죠?

첫째, 이 기능은 현재 React 17의 릴리스 후보에서만 사용할 수 있음을 다시 한번 강조하고자 합니다. 이전 버전으로도 백포팅이 되겠지만, 오늘 테스트하려면 버전 17을 고수해야 합니다.

응답 앱을 작성하기 위해 사용하는 작업에 따라 필요한 작업이 달라집니다.

- create-react-app을 사용하는 경우 버전 4.0.0(현재 베타 버전)으로 업데이트해야 합니다.
- Next JS를 사용하는 경우 버전 9.5.3 이상으로 업데이트해야 합니다.
- Gatsby JS를 사용하는 경우 버전 2.24.5 이상으로 업데이트해야 합니다.

Babel을 직접 구성한 경우 몇 가지 수동 단계를 더 수행해야 합니다. `@babel/preset-react`를 사용하는 경우 최신 버전으로 업데이트하십시오. `@babel/plugin-transform-react-jsx`를 사용하는 경우 대신 업데이트하십시오. 그리고 `@babel/core`도 업데이트하세요.

```coffeescript
npm update @babel/core @babel/preset-react

# or

npm update @babel/core @babel/plugin-transform-react-jsx
```

그런 다음 다음과 같은 조각으로 Babel 구성을 업데이트합니다.

```undefined
{
  "presets": [
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ]
  ]
}
```

또는

```undefined
{
  "plugins": [
    [
      "@babel/plugin-transform-react-jsx",
      {
        "runtime": "automatic"
      }
    ]
  ]
}
```

그리고 넌 끝이야!

## 기존 가져오기 제거

뭐, 거의 다 됐어. 왜냐하면 이제 더 이상 필요 없는 수입품이 엄청나게 많기 때문입니다! 그리고 우리는 그들이 돌아다니는 것을 내버려둘 수 없다, 그렇죠?

음, 사실, 할 수 있어요. 모든 것이 정상적으로 작동하고 가까운 미래에도 계속 그렇게 할 것이다. 하지만 악성코드를 가지고 있는 것은 성가신 일일 뿐이고, 만약 그것들을 제거할 자원이 있다면, 왜 안 될까요?

리액트 팀의 훌륭한 직원들은 수백 개의 구성 요소를 일일이 일일이 일일이 검토하지 않고 자동 스크립트(코드 모드라고 함)를 작성하여 모든 작업을 수행할 수 있도록 했습니다. 🙌 다음 작업만 하면 됩니다.

```undefined
npx react-codemod update-react-imports
```

이렇게 하면 기본 React 내보내기(`react`에서 `import React`)에 대한 모든 가져오기가 제거되고 대신 후크 및 기타 기능에 대한 참조가 명명된 가져오기로 다시 작성됩니다. 즉, `React.use Effect`가 대신 `use Effect`로 변경되고 `react`에서 `import {useEffect` 문이 파일 상단에 추가됩니다.

이 후반부는 나를 놀라게 했지만(읽고 쓰기가 더 쉽기 때문에 항상 접두사로 썼다). 이것은 미래에 반응의 ES 모듈 빌드를 만드는 길을 열어준다. 시간이 좀 지나면 분명 익숙해질 거야 😅

## 보수는

이 짜증나는 리액션 가져오기가 중단되면 리액션은 배우기가 훨씬 쉬워질 것입니다. 기억해야 할 개념이 하나 줄었고, 모든 것이 틀에서 벗어나 더 잘 작동합니다. 또한 새 JSX 파일을 부팅할 때마다 한 줄씩 줄여서 쓸 수 있습니다.