---
layout: post
title: "반응의 순수 기능 성분"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/10/pure-components-react.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2018/10/pure-components-react.png?fit=730%2C487&ssl=1)

편집자 참고: 이 게시물은 2020년 12월 28일에 업데이트되었습니다.

React는 사용자 인터페이스 구성 요소를 구축하기 위한 오픈 소스, 프런트엔드 JavaScript 라이브러리이다. 2020년 가장 인기 있는 프런트 엔드 라이브러리 중 하나인 리액트는 거의 300만 명의 사용자와 대규모 개발자 커뮤니티를 보유하고 있으며 웹에서 모바일 장치에 이르기까지 다양한 플랫폼에서 실행된다.

이 튜토리얼에서는 React의 순수 기능 구성 요소에 초점을 맞추고 `React`를 사용하는 방법을 보여드리겠습니다.React에서 기능 구성요소를 메모하는 PureComponent와 React.memo() API입니다.

다음 사항에 대해 자세히 설명합니다.

- 반응 구성 요소란 무엇입니까?
- 기능 구성 요소 대 클래스 구성 요소
- 반응 순수 구성 요소란 무엇입니까?
- 반응 순수 구성 요소는 어떻게 작동합니까?
- 반응 기능 구성 요소는 순수합니까?
- Recompose의 `{pure}` HOC 사용
- 리액트.메모() : 간략한 역사
- 리액트.메모() 사용 방법

React RFCs 리포지토리의 React 프레임워크에 대한 변경 사항 및 제안을 계속 수행할 수 있습니다.

## 반응 구성 요소란 무엇입니까?

대부분의 최신 JavaScript 프레임워크와 마찬가지로 React는 구성 요소 기반입니다. 반응 용어로 구성 요소는 일반적으로 상태 및 소품 함수로 정의됩니다.

### 기능 구성 요소 대 클래스 구성 요소

React는 클래스 구성 요소와 기능 구성 요소의 두 가지 맛을 지원합니다. 함수 구성 요소는 JSX를 반환하는 일반 JavaScript 함수입니다. 클래스 구성 요소는 `Ract`를 확장하는 JavaScript 클래스입니다.Component`는 렌더 메서드 내에서 JSX를 반환합니다.

다음 코드 조각은 클래스 구성 요소와 기능 구성 요소로 모두 정의된 간단한 `ReactHeader` 구성 요소를 보여 줍니다.

```js
// CLASS COMPONENT
class ReactHeader extends React.Component {
  render() {
    return (
      <h1>
        React {this.props.version || 16} Documentation
      </h1>
    )
  }
}


// FUNCTIONAL COMPONENT
function ReactHeader(props) {
  return (
    <h1>
      React {props.version || 16} Documentation
    </h1>
  )
}
```

반응 기능 구성 요소에 대한 새로 고침을 보려면 다음 비디오 튜토리얼을 참조하십시오.

## 반응 순수 구성 요소란 무엇입니까?

기능적 프로그래밍 패러다임의 순도 개념에 기초하여, 다음과 같은 경우 함수는 순수하다고 한다.

- 반환 값은 입력 값에 의해서만 결정됩니다.
- 해당 반환 값은 동일한 입력 값에 대해 항상 동일합니다.

반응 구성 요소는 동일한 상태 및 소품에 대해 동일한 출력을 렌더링하는 경우 순수하게 간주됩니다. 이와 같은 클래스 구성 요소의 경우 React는 `PureComponent` 기본 클래스를 제공합니다. 반응을 확장하는 클래스 구성 요소입니다.PureComponent` 클래스는 순수 성분으로 처리됩니다.

리액션이 소품 및 상태에 대한 얕은 비교로 "should ComponentUpdate()" 방법을 구현하기 때문에 순수 구성요소는 성능이 향상되고 최적화됩니다.

## 반응 순수 구성 요소는 어떻게 작동합니까?

실제 반응 순수 성분은 다음과 같습니다.

```js
import React from 'react';

class PercentageStat extends React.PureComponent {

  render() {
    const { label, score = 0, total = Math.max(1, score) } = this.props;

    return (
      <div>
        <h6>{ label }</h6>
        <span>{ Math.round(score / total * 100) }%</span>
      </div>
    )
  }

}

export default PercentageStat;
```

## 반응 기능 구성 요소는 순수합니까?

기능 구성 요소는 특히 구성 요소에서 상태 관리를 분리하려는 경우 React에서 매우 유용합니다. 이러한 이유로 이러한 구성 요소를 상태 비저장 구성 요소라고 합니다.

그러나 기능 구성요소는 성능 향상을 활용할 수 없으며 `반응`과 함께 제공되는 최적화를 제공할 수 없다.PureComponent는 정의별 클래스가 아니므로 "PureComponent"입니다.

React가 기능 구성 요소를 순수 구성 요소로 처리하려면 기능 구성 요소를 `Ract`를 확장하는 클래스 구성 요소로 변환해야 합니다.PureComponent"입니다.

다음은 간단한 예입니다.

```js
// FUNCTIONAL COMPONENT
function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}


// CONVERTED TO PURE COMPONENT
class PercentageStat extends React.PureComponent {

  render() {
    const { label, score = 0, total = Math.max(1, score) } = this.props;

    return (
      <div>
        <h6>{ label }</h6>
        <span>{ Math.round(score / total * 100) }%</span>
      </div>
    )
  }

}
```

## Recompose의 '{pure}' HOC 사용

React가 이를 순수한 구성 요소로 간주할 수 있도록 기능 구성 요소를 최적화한다고 해서 구성 요소를 클래스 구성 요소로 변환할 필요는 없습니다.

Recompose 패키지에 이미 익숙하다면 이 패키지는 기능 구성 요소를 처리할 때 매우 유용한 광범위한 HOC(고차 구성 요소)를 제공합니다.

Recompose 패키지는 "Shallow Equal()을 사용하여 변경 사항을 테스트하는 경우, "Shallow Equal"()을 사용하여 변경 사항을 테스트하지 않는 한 구성 요소의 업데이트를 방지하여 Retact 구성 요소를 최적화하는 `{pure}` HOC를 내보냅니다.

순수 HOC를 사용하여 당사의 기능 구성 요소는 다음과 같이 포장할 수 있습니다.

```js
import React from 'react';
import { pure } from 'recompose';

function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

// Wrap component using the `pure` HOC from recompose
export default pure(PercentageStat);
```

## 리액트.메모() : 간략한 역사

2018년 10월, 핵심 리액트 팀의 댄 아브라모프는 리액트 다음 마이너 릴리스에 대해 검토 중인 몇 가지 추가 기능에 대해 트윗을 올렸다.

새로운 기능 중 하나는 React.pure() API로, React를 사용하여 클래스 구성요소를 최적화하는 것과 유사한 방식으로 기능 구성요소를 최적화하였다.PureComponent"입니다.

API 이름을 `순수()`로 지정하면 기능 프로그래밍에서 순수함수의 개념과 혼동하게 된다. 리액트 커뮤니티는 기본적으로 기능 구성요소를 메모하기 때문에 2018년 리액트 v16.6(작성 당시 리액트 최신 버전은 리액트 v17) 출시와 함께 리액트.메모()로 이름을 바꾸기로 했다.

## 리액트.메모() 사용 방법

React.memo()itrate를 사용하면 소품의 얕은 비교를 사용하여 불필요한 업데이트에 대한 렌더링에서 벗어나는 메모된 기능 구성요소를 만들 수 있습니다.

새로운 `Ract.memo() API를 사용하여 이전 기능 구성요소를 다음과 같이 포장할 수 있습니다.

```js
import React, { memo } from 'react';

function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

// Wrap component using `React.memo()`
export default memo(PercentageStat);
```

### 이행내역

React.memo() API 구현에 대해 알아야 할 몇 가지 사항이 있습니다.

- React.memo()는 고차 성분입니다. 반응 구성 요소를 첫 번째 인수로 사용하고 특수한 종류의 반응 구성 요소를 반환합니다.
- `Ract.memo()`는 특수 반응 구성 요소 유형을 반환합니다. 이렇게 하면 렌더러가 출력을 메모하는 동안 구성 요소를 렌더링할 수 있으므로 구성 요소의 소품이 얕게 동일할 경우 업데이트가 필요하지 않습니다.
- React.memo()는 모든 React 구성 요소와 함께 작동합니다. React.memo()에 전달된 첫 번째 인수는 모든 유형의 React 구성 요소일 수 있습니다. 그러나 클래스 구성 요소의 경우 `응답하라`를 사용해야 합니다.React.memo()를 사용하지 않고 PureComponent
- React.memo()는 React DOM Server를 사용하여 서버에서 렌더링된 구성 요소와도 작동합니다.

### 맞춤형 구제금융조건

React.memo() API는 두 번째 인수를 사용할 수 있으며, 이는 arePropsEqual() 함수입니다. React.memo()의 기본 동작은 구성 요소 소품을 얕게 비교하는 것입니다. 그러나 arePropsEqual() 기능을 사용하면 구성요소 업데이트에 대한 구제금융 조건을 사용자 정의할 수 있습니다. arePropsEqual() 함수는 prevProps와 nextProps의 두 가지 매개 변수로 정의된다. arePropsEqual() 함수는 소품을 비교할 때 true를 반환하고(따라서 구성 요소가 다시 렌더링되지 않도록 함) 소품이 같지 않을 때 false를 반환합니다. arePropsEqual() 함수는 클래스 구성 요소에서 "should ComponentUpdate()" 라이프사이클 방식과 유사하지만 그 반대입니다. 다음 코드 조각은 사용자 지정 구제금융 조건을 사용합니다.

```js
import React, { memo } from 'react';

function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

function arePropsEqual(prevProps, nextProps) {
  return prevProps.label === nextProps.label; 
}

// Wrap component using `React.memo()` and pass `arePropsEqual`
export default memo(PercentageStat, arePropsEqual);
```

## 결론

React.memo() API를 사용하면 기능적 컴포넌트를 사용하여 컴포넌트를 메모하는 최적화와 함께 제공되는 성능상의 이점을 누릴 수 있습니다.