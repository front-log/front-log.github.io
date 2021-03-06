---
layout: post
title: "Redux 연결 대응: 사용 시기 및 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/react-redux-connect.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/react-redux-connect.png?fit=730%2C487&ssl=1)

편집자 참고: 본 React Redux Connect 튜토리얼은 React 16.8의 후크 지원을 반영하고 React 구성 요소를 Redux 저장소에 연결하는 방법에 따라 분리하기 위한 진화하는 모범 사례를 해결하기 위해 업데이트되었습니다.

React는 소품 및 상태라는 두 가지 주요 데이터 제공 메커니즘을 제공합니다. 소품은 읽기 전용이며 상위 구성요소가 하위 구성요소에 특성을 전달할 수 있습니다. 상태는 로컬이고 구성 요소 내에 캡슐화되어 있으며 구성 요소의 수명 주기 중 언제든지 변경될 수 있습니다.

상태는 동적 React 앱을 구축하는 매우 강력한 메커니즘이기 때문에 적절한 상태 관리가 가장 중요합니다. Flux, Redux 및 MobX와 같이 애플리케이션 상태를 관리하기 위한 잘 구조화된 아키텍처를 제공하는 여러 라이브러리가 이미 존재합니다.

이 가이드에서는 React-redux를 사용하여 React 애플리케이션에서 상태를 관리하는 방법에 대해 설명합니다. React와 Redux 아키텍처 및 API에 대한 기본적인 이해는 이미 알고 계실 것입니다.

## Redux란 무엇인가?

Redux는 바닐라 앱에서 프레임워크 앱에 이르는 자바스크립트 앱의 예측 가능한 상태 컨테이너이다. 설치 공간은 매우 작지만 어떤 환경에서나 실행할 수 있는 일관된 애플리케이션을 작성할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/redux-store-1.png?resize=720%2C671&ssl=1)

Redux 커뮤니티를 처음 접하고 Redux를 사용하는 동안 수많은 디자인 패턴과 비판에 압도당했다고 느낀다면 효율적인 Redux 개발을 위해 공식적이고 의견이 있는 배터리 포함 툴 세트인 Redux Toolkit을 사용해 보십시오.

Redux Toolkit은 Redux에 대해 제기된 많은 주장을 제거했다. 또한 모범 사례 및 패턴에 대한 지식 격차를 해소할 수 있습니다.

## Redux Connect란?

react-redux 패키지는 React bindings를 Redux 상태 컨테이너에 제공하므로 React 애플리케이션을 Redux 스토어에 쉽게 연결할 수 있습니다. 이렇게 하면 React 애플리케이션 구성 요소를 다음과 같이 Redux 저장소에 대한 연결에 따라 분리할 수 있습니다.

- 프레젠테이션 구성 요소는 상황이 어떻게 보이는지에만 관심이 있고 Redux 상태를 인식하지 못합니다. 소품에서 데이터를 가져오고 소품을 통해 콜백을 트리거할 수 있습니다.
- 컨테이너 구성 요소는 상황이 작동하는 방식을 책임지고 Redux 상태를 완전히 인식합니다. React Redux를 사용하여 생성되는 경우가 많으며 Redux 작업을 배포할 수 있습니다. 또한 Redux 상태의 변경 사항도 구독합니다.

2019년, Redux의 저자 Dan Abramov는 프레젠테이션 및 컨테이너 구성 요소에 대한 2015 블로그 게시물 수정에서 이러한 접근 방식에 대해 다음과 같이 조언했습니다.

> 나는 오래 전에 이 기사를 썼고 그 이후로 나의 견해는 진화되었다. 특히, 저는 더 이상 여러분의 구성요소를 이렇게 분할하지 않을 것을 제안합니다. 코드베이스에서 자연스럽다면 이 패턴이 편리할 수 있습니다. 하지만 나는 그것이 아무런 필요없이 그리고 거의 독단적인 열정으로 너무 많이 강요되는 것을 보았다. 제가 유용하다고 생각한 주된 이유는 복잡한 상태 저장 논리와 구성 요소의 다른 측면을 구분할 수 있기 때문입니다. [후크](https://reactjs.org/docs/hooks-custom.html)에서는 임의 분할 없이 동일한 작업을 수행할 수 있도록 합니다. 이 글은 역사적 이유로 그대로 남아 있지만 너무 심각하게 받아들이지는 마세요.

2019년 2월 리액트 16.8 출시로 수업을 작성하지 않고도 상태를 사용할 수 있는 훅스에 대한 공식 지원이 처음으로 도입됐다. 이후 많은 개발자들이 기존의 Redux 컨테이너를 Hooks로 변환하는 방식을 택했다. 다른 이들은 Redux가 앱에 추가하는 많은 양의 코드를 줄이기 위해 Redux Toolkit으로 눈을 돌렸다.

버전 7.1에서 Redux는 React Hooks를 지원합니다. 즉, 연결 대신 기능 구성 요소에서 Redux와 후크를 함께 사용할 수 있습니다.

그렇긴 하지만, 비즈니스 논리와 프레젠테이션 구성 요소를 분리하는 핵심 개념을 이해하는 것은 많은 복잡한 문제를 훨씬 쉽게 해결할 수 있기 때문에 유용합니다. 이 가이드에서는 반응 중복을 사용하여 Redux 상태에 연결된 컨테이너 구성 요소에 초점을 맞춥니다.

## Redux 저장소에 앱을 연결하는 방법

react-redux 패키지는 매우 단순한 인터페이스를 드러낸다. 다음 사항에만 신경 쓰면 됩니다.

- <Provider store>는 React 애플리케이션을 감싸고 애플리케이션 계층의 모든 컨테이너 구성요소에서 Redux 상태를 사용할 수 있도록 합니다.
- connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])는 baseRact 구성 요소로 컨테이너 구성 요소를 만들기 위한 고차 구성 요소를 생성합니다.

다음과 같이 프로젝트에 react-redux를 설치할 수 있습니다.

```undefined
npm install react-redux --save
```

React 애플리케이션에 대한 Redux 스토어 설정이 이미 되어 있는 경우, 이 앱을 Redux 스토어에 연결하는 방법은 다음과 같습니다.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import createStore from './createReduxStore';

const store = createStore();
const rootElement = document.getElementById('root');

ReactDOM.render((
 <Provider store={store}>
  <AppRootComponent />
 </Provider>
), rootElement);
```

이 설정을 사용하면 이제 `connect() API를 사용하여 AppRootComponent의 계층 내에서 Redux 저장소에 연결된 컨테이너 구성 요소를 생성할 수 있습니다.

## Redux에서 'connect()'를 사용하는 경우

`connect()`를 사용하는 것이 적절한 몇 가지 시나리오를 살펴보겠습니다.

### 1. 컨테이너 구성 요소 생성

React Redux `connect() API는 Redux 저장소에 연결된 컨테이너 요소를 생성하는 데 사용됩니다. Redux 저장소는 반응 컨텍스트 메커니즘을 사용하여 구성 요소의 최상위 상위 저장소에서 파생됩니다. 프레젠테이션 구성 요소만 생성하는 경우에는 "connect()"가 필요하지 않습니다.

Redux 스토어에서 데이터를 가져오거나 Redux 스토어에서 작업을 디스패치하거나 React 구성 요소에서 모두 수행하려는 경우 `connect()에서 반환되는 고차 구성 요소에 래핑하여 구성 요소를 컨테이너 구성 요소로 만들 수 있습니다.

```js
import React from 'react';
import { connect } from 'react-redux';
import Profile from './components/Profile';

function ProfileContainer(props) {
  return (
    props.loggedIn
      ? <Profile profile={props.profile} />
      : <div>Please login to view profile.</div>
  )
}

const mapStateToProps = function(state) {
  return {
    profile: state.user.profile,
    loggedIn: state.auth.loggedIn
  }
}

export default connect(mapStateToProps)(ProfileContainer);
```

### 2. Redux 스토어에 수동으로 가입하지 않음

컨테이너 구성 요소를 직접 생성하고 `store.subscribe()를 사용하여 해당 구성 요소를 Redux 스토어에 수동으로 구독할 수 있습니다. 그러나 `connect()`를 사용하면 일부 성능 향상 및 최적화 기능이 제공되므로 응용 프로그램에서 구현할 수 없습니다.

다음 코드 조각에서, 우리는 이전 코드 조각에서와 유사한 기능을 얻기 위해 저장소에 가입함으로써 컨테이너 구성 요소를 수동으로 생성하고 그것을 Redux 스토어에 연결하려고 시도한다.

```js
import React, { Component } from 'react';
import store from './reduxStore';
import Profile from './components/Profile';

class ProfileContainer extends Component {

  state = this.getCurrentStateFromStore()


  getCurrentStateFromStore() {
    return {
      profile: store.getState().user.profile,
      loggedIn: store.getState().auth.loggedIn
    }
  }


  updateStateFromStore = () => {
    const currentState = this.getCurrentStateFromStore();


    if (this.state !== currentState) {
      this.setState(currentState);
    }
  }


  componentDidMount() {
    this.unsubscribeStore = store.subscribe(this.updateStateFromStore);
  }


  componentWillUnmount() {
    this.unsubscribeStore();
  }


  render() {
    const { loggedIn, profile } = this.state;


    return (
      loggedIn
        ? <Profile profile={profile} />
        : <div>Please login to view profile.</div>
    )
  }


}

export default ProfileContainer;
```

커넥트(connect)는 또한 추가적인 유연성을 제공하여 컨테이너 구성요소가 처음에 전달한 소품을 기반으로 동적 소품을 받도록 구성할 수 있습니다. 이 기능은 소품을 기반으로 Redux 상태의 슬라이스를 선택하거나 소품에서 특정 변수에 작업 생성자를 바인딩할 때 유용합니다.

앱당 하나의 Redux 스토어만 사용하는 것이 가장 좋으나 프로젝트에 여러 Redux 스토어가 필요한 경우 `connect()`를 사용하면 컨테이너 구성 요소를 연결할 스토어를 쉽게 지정할 수 있습니다.

## 반응 중복 대 반응 대응 컨텍스트

많은 개발자들은 React Context가 Redux의 대체품이라고 생각한다. 제 생각에는 Context가 글로벌 상태 관리를 위해 설계된 것이 아니기 때문에 그렇지 않은 것 같습니다. 오히려 구성 요소 트리에 구멍을 뚫는 소품을 피하기 위한 것입니다.

중견 및 대규모 애플리케이션의 경우 글로벌 상태 관리가 유지 보수와 확장성을 위한 핵심입니다. 실제로 Connect and Redux는 일반적으로 후드 아래에서 컨텍스트를 반응하여 연결된 구성 요소에 데이터를 제공합니다. 또한 애플리케이션의 성능 향상을 위해 몇 가지 계층 간의 불변성과 메모화를 추가로 제공합니다.

## Redux 'connect()' 작동 방식

react-redux가 제공하는 connect() 함수는 최대 4개의 인수를 사용할 수 있으며, 모두 선택 사항이다. `connect() 함수`를 호출하면 반응 구성 요소를 래핑하는 데 사용할 수 있는 고차 구성 요소가 반환됩니다.

`connect()에 의해 고차 구성 요소가 반환되므로, 컨테이너 구성 요소로 변환하려면 baseRetact 구성 요소로 다시 호출해야 합니다.

```js
const ContainerComponent = connect()(BaseComponent);
Here is the signature of the connect() function:
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
```

### 반응 중복에서 'mapStateToProps'

```undefined
mapStateToProps(state, [ownProps]) => stateProps
```

이 인수는 일반 개체 또는 다른 함수를 반환하는 함수입니다. 이 인수를 전달하면 컨테이너 구성 요소가 Redux 저장소 업데이트에 가입하게 되는데, 이는 저장소가 업데이트될 때마다 `mapStateToProps` 기능이 호출된다는 것을 의미합니다. 저장소 업데이트에 관심이 없는 경우 정의되지 않았거나 null인 상태로 두십시오.

mapStateToProps는 두 개의 매개 변수로 선언되며 두 번째 매개 변수는 선택 사항입니다. 첫 번째 매개 변수는 Redux 저장소의 현재 상태입니다. 두 번째 파라미터(통과된 경우)는 구성 요소에 전달된 소품의 개체입니다.

```js
const mapStateToProps = function(state) {
  return {
    profile: state.user.profile,
    loggedIn: state.auth.loggedIn
  }
}

export default connect(mapStateToProps)(ProfileComponent);
```

mapStateToProps에서 일반 객체가 반환되면 반환된 StateProps 객체가 구성 요소의 소품으로 병합됩니다. 다음과 같이 구성 요소에서 이러한 소품에 액세스할 수 있습니다.

```js
function ProfileComponent(props) {
  return (
    props.loggedIn
      ? <Profile profile={props.profile} />
      : <div>Please login to view profile.</div>
  )
}
```

그러나 함수가 반환되면 해당 함수는 구성 요소의 각 인스턴스에 대해 `mapStateToProps`로 사용됩니다. 이 기능은 렌더링 성능 향상 및 메모화에 유용합니다.

앱의 성능을 향상시키려면 Redux에서 다시 선택을 사용하는 것이 좋습니다. 글로벌 저장소에서 필요한 데이터 청크만 선택하는 데 도움이 됩니다. 셀렉터는 파생 데이터를 계산하여 저장 공간을 최소화하고, 애플리케이션의 성능을 향상시키기 위해 선택 항목에 메모를 추가할 수도 있습니다.

```php
mapDispatchToProps(dispatch, [ownProps]) => dispatchProps
```

이 인수는 객체이거나 일반 객체 또는 다른 함수를 반환하는 함수일 수 있습니다. mapDispatchToProps의 작동 방식을 더 잘 설명하기 위해서는 몇 가지 작업 생성자가 있어야 합니다.

예를 들어 다음과 같은 작업 생성자가 있다고 가정해 보겠습니다.

```coffeescript
export const writeComment = (comment) => ({
  comment,
  type: 'WRITE_COMMENT'
});

export const updateComment = (id, comment) => ({
  id,
  comment,
  type: 'UPDATE_COMMENT'
});

export const deleteComment = (id) => ({
  id,
  type: 'DELETE_COMMENT'
});
```

#### 1. 기본 구현

자체 mapDispatchToProps 개체나 함수를 제공하지 않으면 기본 구현이 사용되며, 이는 단순히 구성 요소에 대한 prop로 스토어의 디스패치 메서드를 주입하는 것입니다.

```js
You can use the dispatch prop in your component as follows:
import React from 'react';
import { connect } from 'react-redux';
import { updateComment, deleteComment } from './actions';

function Comment(props) {
  const { id, content } = props.comment;


  // Invoking the actions via props.dispatch()
  const editComment = () => props.dispatch(updateComment(id, content));
  const removeComment = () => props.dispatch(deleteComment(id));


  return (
    <div>
      <p>{ content }</p>
      <button type="button" onClick={editComment}>Edit Comment</button>
      <button type="button" onClick={removeComment}>Remove Comment</button>
    </div>
  )
}

export default connect()(Comment);
```

#### 2. 물체 전달

이 인수에 대해 객체가 전달되면 객체의 각 함수는 Redux 작업 생성자로 간주되어 직접 호출할 수 있도록 스토어의 디스패치 메서드에 대한 호출로 포장됩니다. 결과적인 액션 크리에이터의 디스패치 프롭스 객체가 컴포넌트의 소품으로 통합된다.

다음 코드 조각은 작업 생성자의 개체를 제공하여 `mapDispatchToProps`를 정의하는 방법과 작업 생성자를 Retact 구성 요소에 대한 소품으로 사용하는 방법을 보여 줍니다.

```js
import React from 'react';
import { connect } from 'react-redux';
import { updateComment, deleteComment } from './actions';

function Comment(props) {
  const { id, content } = props.comment;


  // Invoking the actions directly as component props
  const editComment = () => props.updatePostComment(id, content);
  const removeComment = () => props.deletePostComment(id);


  return (
    <div>
      <p>{ content }</p>
      <button type="button" onClick={editComment}>Edit Comment</button>
      <button type="button" onClick={removeComment}>Remove Comment</button>
    </div>
  )
}

// Object of action creators
const mapDispatchToProps = {
  updatePostComment: updateComment,
  deletePostComment: deleteComment
}

export default connect(null, mapDispatchToProps)(Comment);
```

#### 3. 기능 전달

함수가 통과되면 상점의 디스패치 방식으로 액션 크리에이터를 묶는 `디스패치Props` 개체를 반환해야 합니다. 함수는 저장소의 디스패치를 첫 번째 매개 변수로 사용합니다. mapStateToProps와 마찬가지로 구성 요소에 전달된 원래 소품에 매핑되는 선택적 자체 Props second 매개 변수를 사용할 수도 있다.

이 함수가 다른 함수를 반환하면 반환된 함수가 대신 `mapDispatchToProps`로 사용되므로 렌더링 성능 및 메모라이제이션 향상에 유용합니다.

이 기능에서는 Redux에서 제공하는 bindAction Creators() 도우미를 사용하여 작업 생성자를 스토어의 디스패치 방법에 바인딩할 수 있습니다.

다음 코드 조각은 함수를 제공하여 `mapDispatchToProps`를 정의하는 방법과, `bindAction Creators()` 도우미를 사용하여 주석 작업 작성자를 React 구성 요소의 `prop.actions`에 바인딩하는 방법을 보여 줍니다.

```js
import React from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import * as commentActions from './actions';

function Comment(props) {
  const { id, content } = props.comment;
  const { updateComment, deleteComment } = props.actions;


  // Invoking the actions from props.actions
  const editComment = () => updateComment(id, content);
  const removeComment = () => deleteComment(id);


  return (
    <div>
      <p>{ content }</p>
      <button type="button" onClick={editComment}>Edit Comment</button>
      <button type="button" onClick={removeComment}>Remove Comment</button>
    </div>
  )
}

const mapDispatchToProps = (dispatch) => {
  return {
    actions: bindActionCreators(commentActions, dispatch)
  }
}

export default connect(null, mapDispatchToProps)(Comment);


mergeProps(stateProps, dispatchProps, ownProps) => props
```

이 인수는 다음 세 가지 매개 변수를 사용하는 함수입니다.

- `StateProps`: mapStateToProps() 호출에서 반환된 소품 개체
- `DispatchProps`: 작업 생성자가 mapDispatchToProps()의 소품 개체
- 자체 소품: 부품에서 받은 오리지널 소품

이 함수는 래핑된 구성 요소로 전달되는 소품의 일반 개체를 반환합니다. 이것은 Redux 스토어의 상태 또는 소품을 기반으로 한 액션 크리에이터의 일부를 조건부로 매핑하는 데 유용합니다.

이 기능이 제공되지 않을 경우 기본 구현은 다음과 같습니다.

```js
const mergeProps = (stateProps, dispatchProps, ownProps) => {
  return Object.assign({}, ownProps, stateProps, dispatchProps)
}
```

### 옵션

옵션 개체(지정된 경우)에 `connect()의 동작을 수정하는 옵션이 포함되어 있습니다. connect()는 일부 추가 옵션과 함께 "connectAdvanced"()에 사용할 수 있는 대부분의 옵션을 수락하는 경우 "connectAdvanced"()의 특수 구현입니다.

`옵션` 객체 설명서를 참조하여 `연결()`에 사용할 수 있는 모든 옵션과 해당 동작의 수정 방법을 확인할 수 있습니다. 이 API를 활용하면 Redux가 어떻게 운영되며 다른 상태 관리 패턴에 비해 어떤 이점을 얻을 수 있는지 파악할 수 있습니다.

다음은 `옵션` 개체에 대한 API입니다.

- `스파이브?: Object` — 앞에서 설명한 것처럼 Redux는 컨텍스트를 후드 아래에 사용합니다. 또한 전역 저장소 옆의 사용자 정의 컨텍스트를 컨테이너에 전달할 수 있습니다.
- `순수: 부울` — 이렇게 하면 Redux가 용기가 순수한지 여부를 이해할 수 있습니다. 즉, 소품 및 상태 외에 다른 것에 의존하지 않습니다. 기본적으로 `true`이지만 다음 API를 활용하여 기본 검사를 재정의할 수 있습니다.
StatesEqual?: (다음: Object, 이전: Object) => 부울;
AreOwnPropsEqual?: (다음: Object, 이전: Object) => 부울;
StatePropsEqual?: (다음: Object, 이전: Object) => 부울;
병합된 PropsEqual?: (다음: Object, 이전: Object) => 부울;
기본적으로 이러한 모든 방법은 얕은 비교를 사용하여 변경 내용을 확인합니다. 성능 집약적인 컨테이너에 대해서는 재정의할 수 있지만, 이는 어느 지점에서든 컨테이너가 파손될 수 있으므로 주의해야 합니다.
- `forwardRef: boolean` — 외부에서 컨테이너의 인스턴스(instance) 메서드에 액세스하려는 경우에 유용합니다.

## connect() 사용 방법

### 저장소 설정

`connect()`를 사용하여 일반 React 구성 요소를 컨테이너 구성 요소로 변환하기 전에 구성 요소가 연결될 Redux 저장소를 지정해야 합니다.

게시물에 새 주석을 추가하고 주석을 제출할 단추도 표시하기 위한 컨테이너 구성 요소인 `New Comment`가 있다고 가정합니다. 구성 요소는 다음과 같은 코드 조각처럼 보일 수 있습니다.

```js
import React from 'react';
import { connect } from 'react-redux';

class NewComment extends React.Component {

  input = null
  
  writeComment = evt => {
    evt.preventDefault();
    const comment = this.input.value;
    
    comment && this.props.dispatch({ type: 'WRITE_COMMENT', comment });
  }
  
  render() {
    const { id, content } = this.props.comment;
    
    return (
      <div>
        <input type="text" ref={e => this.input = e} placeholder="Write a comment" />
        <button type="button" onClick={this.writeComment}>Submit Comment</button>
      </div>
    )
  }
  
}

export default connect()(NewComment);
```

응용 프로그램에서 이 구성 요소를 실제로 사용하려면 구성 요소가 연결되어 있어야 하는 Redux 저장소를 지정해야 합니다. 그렇지 않으면 오류가 발생합니다.

이 작업은 두 가지 방법으로 수행할 수 있습니다.

#### 1. 용기부품의 저장 받침대를 설정합니다.

첫 번째 방법은 Redux 스토어를 구성 요소의 `store` prop 값으로 참조 전달하여 구성 요소에 Redux 스토어를 지정하는 것입니다.

```js
import React from 'react';
import store from './reduxStore';
import NewComment from './components/NewComment';

function CommentsApp(props) {
  return <NewComment store={store} />
}
```

#### 2. '<제공자>' 구성요소에 저장소 받침대 설정

애플리케이션에 대해 Redux 스토어를 한 번 설정하려는 경우 이 방법을 사용하십시오. 일반적으로 Redux 스토어를 하나만 사용하는 앱의 경우에 해당됩니다.

react-redux는 루트 애플리케이션 구성 요소를 래핑하는 데 사용할 수 있는 `<Provider> 구성 요소를 제공합니다. 이 프로그램은 응용 프로그램에 사용할 Redux 스토어에 대한 참조를 기대하는 `스토어` 프로포트를 수락합니다. store는 React의 컨텍스트 메커니즘을 사용하여 앱 계층 아래의 컨테이너 구성요소로 전달됩니다.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import store from './reduxStore';
import { Provider } from 'react-redux';
import NewComment from './components/NewComment';

function CommentsApp(props) {
  return <NewComment />
}

ReactDOM.render((
  <Provider store={store}>
    <CommentsApp />
  </Provider>
), document.getElementById('root'))
```

### '자체 프로프'

앞서 설명한 것처럼 connect()에 전달된 mapStateToProps와 mapDispatchToProps 함수는 구성 요소의 자체 Props를 두 번째 매개 변수로 선언할 수 있다.

하지만, 주의할 점이 있다. 선언된 함수의 필수 매개 변수 수가 2개 미만일 경우 `소유 제안`은 절대 통과되지 않습니다. 그러나 함수가 필수 매개 변수나 두 개 이상의 매개 변수 없이 선언되면 `nownProps`가 통과됩니다.

다음은 몇 가지 시나리오입니다.

#### 1. 매개 변수 없이 선언됨

```js
const mapStateToProps = function() {
  console.log(arguments[0]); // state
  console.log(arguments[1]); // ownProps
};
```

여기서는 함수가 필수 매개 변수 없이 선언되기 때문에 `자유의 제안`이 통과됩니다. 따라서 새로운 ES6 rest parameters 구문을 사용하여 다음과 유사한 방식으로도 작동합니다.

```js
const mapStateToProps = function(...args) {
  console.log(args[0]); // state
  console.log(args[1]); // ownProps
};
```

#### 2. 한 개의 파라미터로 선언됨

```js
const mapStateToProps = function(state) {
  console.log(state); // state
  console.log(arguments[1]); // undefined
};
```

여기에는 하나의 매개 변수인 `state`만 있습니다. 따라서 "자신의 제안"이 통과되지 않기 때문에 "주장[1]"은 정의되지 않은" 것이다.

#### 3. 기본 매개 변수로 선언됨

```js
const mapStateToProps = function(state, ownProps = {}) {
  console.log(state); // state
  console.log(ownProps); // {}
};
```

여기서 필수 매개 변수인 `state`는 기본값이 지정되었기 때문에 두 번째 `ownProps` 매개 변수는 선택 사항이기 때문에 한 가지 필수 매개 변수인 `state`만 있습니다. 따라서 필수 매개 변수가 하나만 있으므로 `ownProps`는 전달되지 않고 결과적으로 할당된 기본값인 `{}`에 매핑됩니다.

#### 4. 두 개의 파라미터로 선언됨

```js
const mapStateToProps = function(state, ownProps) {
  console.log(state); // state
  console.log(ownProps); // ownProps
};
```

이것은 꽤 간단하다. 함수가 두 개의 필수 매개변수로 선언되기 때문에 "자체 프로프"는 여기서 통과된다.

## 결론

본 가이드에서는 react-redux 패키지에서 제공하는 `connect() API를 사용하여 Redux 상태에 연결된 컨테이너 구성 요소를 생성하는 시기와 방법을 살펴보았습니다. 새로 축소하는 경우 언급한 바와 같이 빠르게 진행할 수 있도록 Redux Toolkit을 사용하는 것이 좋습니다.

이 가이드에서는 connect() API의 구조와 사용법에 대해 자세히 설명하지만, 사용 사례는 광범위하게 보여주지 않았다. 자세한 내용은 이 설명서를 참조하십시오.