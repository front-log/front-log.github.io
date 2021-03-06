---
layout: post
title: "리액션 v17의 새로운 기능 및 v18로 가는 길"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/road-to-react-v18.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/road-to-react-v18.png?fit=730%2C487&ssl=1)

몇 주 전, 리액트 팀은 라이브러리의 최신 버전인 리액트 v17.0 RC를 출시했습니다. 이 게시물에서는 새로운 변경 사항을 살펴보고 함께 제공된 이 새 릴리스를 업데이트하겠습니다.

React는 사용자 인터페이스를 구축하기 위한 선언적이고 효율적이며 유연한 JavaScript 라이브러리이며, 가장 널리 사용되는 JavaScript 라이브러리 중 하나입니다. 기트허브에는 156,000개 이상의 별이 있으며, 가장 활기찬 프런트엔드 커뮤니티 중 하나로 훌륭한 애플리케이션을 구축하고 있다.

## 버전 업그레이드 및 v18로의 전환을 위한 UX

이 새 버전은 눈에 보이는 새 기능과 함께 제공되지 않는다는 점에서 독특합니다. 대신 전체적으로 React의 업그레이드 경험에 초점을 맞추고 있다.

React가 시작된 이후 항상 새로운 버전 설정 프로세스가 있어 새 기능을 사용하기 위해 버전을 업그레이드할지 또는 이전 버전을 계속 사용할지 선택해야 합니다. 예를 들어, 컨텍스트 API는 더 이상 사용되지 않으며, 대부분의 React 앱에서 사용하지 않더라도 React는 여전히 이를 지원합니다.

지원을 계속하거나 이전 버전을 사용하는 앱을 남겨두는 것에 대한 논쟁이 계속되고 있습니다. 따라서, 이 업그레이드 전략은 이전 버전을 보다 포괄적으로 포함하도록 재상상되었습니다. 따라서 새로운 버전 17은 점진적인 React 업그레이드를 가능하게 합니다.

React 팀은 기본적으로 한 React 버전에서 다른 버전으로 쉽고 원활하게 업그레이드할 수 있도록 노력했습니다. 버전 17을 사용하면 관리하는 트리가 완전히 다른 버전으로 관리되는 다른 트리에 포함되도록 하는 디딤돌이 제공됩니다.

이것은 단순히 더 많은 업그레이드 옵션을 제공하기 위한 것입니다. 왜냐하면 이전에 하나의 대응 버전에서 최신 버전으로 업그레이드하면 전체 앱이 업그레이드되기 때문입니다. 동일한 프로젝트에서 두 버전의 반응을 가질 수 있는 경우에 따라 이벤트와 관련된 많은 문제가 발생합니다.

예를 들어, React 18이 출시되면 17과 같은 버전에서 업그레이드하는 경우 전체 앱 업그레이드 또는 점진적 업그레이드를 위한 옵션이 제공됩니다. 점진적인 프로그램을 선택하면 앱 대부분이 대응 18로 업그레이드되고 대화 상자 구성 요소를 버전 17에 유지하는 등 앱이 하나씩 업그레이드됩니다.

이 방법이 이상적인 방법은 아니지만 대규모 프로젝트를 유지하는 개발자에게 유용할 것이며, 이 새로운 버전은 이러한 앱이 더 이상 뒤처지지 않도록 보장합니다.

## 이벤트 위임 변경

다음과 같은 단추에 인라인 방식으로 이벤트 핸들러를 처리합니다.

```xml
<button onClick={handleClick}>
```

컴파일 시 바닐라 JS는 다음과 같이 보입니다.

```bash
myButton.addEventListener('click', handleClick);
```

그런 다음 이벤트 유형당 하나의 처리기를 선언된 DOM 노드에 연결하는 대신, 문서 노드에 직접 연결합니다. 이를 이벤트 위임이라고 하며, 이벤트 재생과 같은 대형 앱과 프로세스에 매우 유용합니다.

방금 지난 섹션에서 언급한 점진적인 업그레이드를 고려하면, 서로 다른 React 버전으로 구축된 중첩 앱도 다시 상상해야 했습니다. 이 새 버전의 대응에서는 이벤트 핸들러가 더 이상 문서 수준에서 연결되지 않고 트리가 렌더링된 DOM 컨테이너에 연결됩니다.

```js
const rootNode = document.getElementById('root');
ReactDOM.render(<App />, rootNode);
```

이러한 변경으로 다른 버전의 React로 빌드된 앱을 중첩하는 것이 매우 안전하지만 17 버전 이상부터 시작됩니다.

## 변경 중단

이 새 버전과 함께 제공된 몇 가지 최신 변경 사항이 있습니다. 그 팀은 그들이 가능한 한 최소임을 보장했다; 그들 중 일부는 단지 행동적일 뿐이지만 여전히 중요한 것이다. 따라서 이전 릴리스에서 사용되지 않는 방법과 같은 방법은 계속 사용할 수 있습니다.

### 이벤트 위임

이벤트 위임을 둘러싼 새로운 변경 사항으로 인해 몇 가지 문제가 발생할 수 있습니다.

예를 들어 `document.addEventListener()`를 사용하여 수동 수신기를 추가하면 일반적으로 모든 Return 이벤트가 수집될 것으로 예상할 수 있습니다. 이전 대응 버전에서는 이벤트 핸들러에서 전파 중지 기능을 호출하더라도 기본 이벤트가 이미 문서 수준에 있기 때문에 사용자 정의 문서 수신기가 계속 수신됩니다.

이 새 버전에서 `e.stopPropagation()`은 실제로 문서 처리기에서 다음을 해제하지 못하게 합니다.

```js
document.addEventListener('click', function() {
  // This custom handler will no longer receive clicks
  // from React components that called e.stopPropagation()
});
```

이 문제를 해결하려면 다음과 같은 세 번째 인수로 "{capture:true}" 옵션을 전달하여 이벤트 수신기를 `capture` 구문을 사용하도록 변환하십시오.

```js
document.addEventListener('click', function() {
  // Now this event handler uses the capture phase,
  // so it receives *all* click events below!
}, { capture: true });
```

이를 통해 이제 이벤트 위임이 그 어느 때보다 일반 DOM에 가까워졌다는 것을 알 수 있습니다.

### 브라우저 정렬

React에서 이벤트 시스템이 몇 가지 변경되었으며, 그 중 일부는 다음과 같습니다.

- 하위 요소를 스크롤할 때 발생과 같은 일반적인 혼동을 방지하기 위해 `onScroll` 이벤트는 더 이상 버블이 발생하지 않습니다.
- 리액트 `온블러`와 `온포커스` 이벤트는 이제 내부적으로 네이티브 `포커스인`과 `포커스아웃` 이벤트를 사용하는 것으로 바뀌어 리액트의 기존 행동에 더 잘 어울리고 더 많은 정보를 제공한다.
- 이제 `onClickCapture`와 같은 캡처 구문 이벤트가 실제 브라우저 캡처 구문 수신기를 사용합니다.

이러한 몇 가지 변경 사항은 브라우저의 작동 방식과 더 밀접하게 대응하고 상호 운용성을 개선합니다.

### 이벤트 풀링 없음

이 새로운 버전부터는 지속적인 혼란과 현대적인 브라우저 성능을 향상시키지 못하는 단순한 사실로 인해 이벤트 풀링 최적화는 React에서 제거되었다.

```js
function handleChange(e) {
  setData(data => ({
    ...data,
    // This crashes in React 16 and earlier:
    text: e.target.value
  }));
}
```

리액트 팀은 이것을 행동 변화라고 부르며, 비록 페이스북에서 어떤 것도 깨지는 것을 본 적이 없지만, 그것을 깨는 것으로 분류했다. 그래서 가능성은 매우 낮다. 또한 e.persist()는 이벤트 개체에서 사용할 수 있지만 아무 작업도 수행하지 않습니다.

### 효과 정리 타이밍

이 새로운 버전은 또한 사용 효과 후크 정리 기능의 타이밍을 보다 일관성 있게 한다.

```js
useEffect(() => {
  // This is the effect itself.
  return () => {    // This is its cleanup.  };});
```

반응 16에서 효과 정리 기능은 대부분의 효과와 반대로 동기적으로 실행되며, 이는 화면 업데이트를 지연시키지 않으며 기본적으로 어떤 반응이 비동기식으로 실행된다. React 팀은 사용자가 탭을 전환할 때 대형 앱의 구성 요소인 `WillMount`가 아닌 것처럼 동기식 프로세스가 그렇게 이상적이지 않다는 것을 발견했다.

그래서 이 새로운 버전은 약간의 변화를 가져온다. 이제 효과 정리 기능이 다른 구성 요소와 마찬가지로 비동기식으로 실행되며 구성 요소가 마운트 해제되어 있으면 업데이트가 화면에 표시된 후 정리가 실행됩니다.

### 정의되지 않은 반환에 대한 일관된 오류

반응 개발자는 정의되지 않은 상태로 반환되는 함수가 다음과 같은 오류로 플래그가 지정된다는 것을 알고 있습니다.

```js
function Button() {
  return; // Error: Nothing was returned from render
}
```

대부분 의도치 않게 정의되지 않은 상태로 반환하는 것이 얼마나 쉬운지 여부 때문입니다.

```js
function Button() {
  // We forgot to write return, so this component returns undefined.
  // React surfaces this as an error instead of ignoring it.
  <button />;
}
```

처음에는 이 동작은 클래스 및 함수 구성 요소에만 국한되었지만, 새로운 버전에서는 포워드Ref와 memo 구성 요소가 추가되어 일반 클래스 및 함수 구성 요소와 일관된다.

```coffeescript
let Button = forwardRef(() => {
  // We forgot to write return, so this component returns undefined.
  // React 17 surfaces this as an error instead of ignoring it.
  <button />;
});
let Button = memo(() => {
  // We forgot to write return, so this component returns undefined.
  // React 17 surfaces this as an error instead of ignoring it.
  <button />;
});
```

의도적으로 아무 것도 렌더링하지 않는 경우에는 n`u`ll이 대신 반환됩니다.

### 개인 내보내기 제거

일부 리액트 내부는 관련 없는 다른 프로젝트, 특히 리액트 네이티브(React Native) 웹에 노출되어 있는데, 이 프로젝트는 매우 취약하고 쉽게 깨지는 이벤트 시스템의 내부에만 의존하곤 했다. 이제 이러한 민간 수출품은 이 새로운 React 버전에서 제거되었습니다. 이러한 수출품에 대한 내부 의존을 막는 다른 해결책이 마련되었습니다.

이러한 변경은 웹 버전용 이전 RN이 React v17과 호환되지 않지만 최신 버전이 된다는 것을 의미합니다.

## 업그레이드 방법

가까운 미래에 보다 안정적인 버전이 출시될 예정이므로 테스트 프로젝트에 이 버전을 사용하는 것이 좋습니다. GitHub에서 React v17 RC를 사용하거나 업그레이드하는 동안 발생할 수 있는 모든 문제를 제기할 수 있습니다.

npm과 함께 설치하려면 다음을 실행합니다.

```css
npm install react@17.0.0-rc.1 react-dom@17.0.0-rc.1
```

Yarn과 함께 설치하려면 다음을 실행합니다.

```css
yarn add react@17.0.0-rc.1 react-dom@17.0.0-rc.1
```

또한 CDN을 통한 반응의 UMD 빌드를 보유하고 있습니다.

```xml
<script crossorigin src="https://unpkg.com/react@17.0.0-rc.1/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@17.0.0-rc.1/umd/react-dom.production.min.js"></script>
```

자세한 설치 지침은 문서를 참조하십시오.

## 결론

이것으로 우리는 검토를 마칩니다. 리액트 17은 어떠한 전면적인 새로운 기능도 제공하지 않았지만, 업그레이드 경험을 직접 다루어 버전 18의 선례를 남기며 리액트의 동작을 현대의 브라우저와 더 밀접하게 일치시킨다. 해피 해킹!