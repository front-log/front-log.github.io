---
layout: post
title: "반응 후크가 반응 라우터를 대체하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2019/07/reactrouter.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/reactrouter.png?fit=730%2C412&ssl=1)

편집자 참고: 이 React Hooks and React Router 튜토리얼은 지난 정보를 업데이트하고 React Router에 대한 새로운 정보를 포함하기 위해 2021년 2월 10일에 마지막으로 업데이트되었습니다.

리액트 훅스의 등장 이후, 많은 것들이 바뀌었다. 이전에는 문제가 없었던 몇 가지 사항들이 문제를 일으키기 시작했습니다. Hooks와 함께 제공되는 기능과 가능성은 React에서 특정 개념에 접근하는 방법을 재정의했고 라우팅도 그 중 하나입니다.

계속하기 전에 이 게시물은 React Router에서 촬영하거나 중요도를 저하시킬 목적으로 작성된 것이 아님을 말씀드리고자 합니다. 대신, 다른 가능성을 살펴보고 Hooks를 사용하여 React 앱의 라우팅 환경을 개선할 수 있는 방법을 알아보겠습니다.

이를 위해, 우리는 React 라우터에 대한 참조를 만들고 시연 목적으로 라우터를 후크합니다. 먼저 React Router에 대해 자세히 살펴보겠습니다.

## 대응 라우터란?

React Router는 React 응용프로그램에서 경로를 관리하는 널리 사용되는 선언적 방법입니다. 리액션 응용 프로그램의 모든 페이지와 화면에 대한 경로를 수동으로 설정할 때 발생하는 모든 스트레스를 해소합니다. Retact Router는 라우트, 링크 및 브라우저 라우터 등 라우팅을 가능하게 하는 세 가지 주요 구성 요소를 내보냅니다.

## React Router가 필요한가?

반응 라우터는 기본 탐색 및 라우팅 기능만 필요한 특정 프로젝트의 경우 오버킬이 될 수 있습니다. 이러한 맥락에서 대응 라우터는 전혀 필요하지 않습니다. 즉, React Router에는 응용 프로그램과 선언적으로 구성되는 탐색 구성 요소가 풍부하므로 React 응용 프로그램의 더 크고 복잡한 탐색 요구 사항에 매우 유용할 수 있습니다. React Native 애플리케이션에도 적합합니다.

## React 라우터가 React에 내장되어 있습니까?

아니오, 대응 라우터는 대응에 내장되어 있지 않습니다. Relect 응용 프로그램에서 라우팅 및 탐색 기능을 제공하기 위해 Relect 위에 특별히 구축된 별도의 라우팅 라이브러리입니다. 대응 애플리케이션에 대응 라우터를 추가할 때 다음과 같이 자체 모듈에서 이 라우터를 가져옵니다.

```coffeescript
import { Router, Route, Switch } from "react-router";
```

그러나 이러한 방식으로 프로젝트에 직접 설치하면 안 됩니다. 브라우저에서 실행할 응용프로그램을 작성하는 경우, 대신 Return Router DOM을 설치해야 합니다.

## React Router 및 React Router DOM이 필요합니까?

React Router와 React Router DOM은 함께 사용할 필요가 없습니다. React Router DOM을 사용하면 기본적으로 React Router에 액세스할 수 있습니다. 두 가지 모두를 사용하는 경우 이미 React Router DOM에 종속성으로 설치되어 있으므로 React Router를 제거해도 됩니다. 그러나 Retact Router DOM은 브라우저에서만 사용할 수 있으므로 웹 응용 프로그램에만 사용할 수 있습니다.

## 반응 라우터의 라우팅

React 앱을 만들고 있는데 세 페이지가 있는 경우 React Router를 사용한 라우팅은 다음과 같습니다.

```xml
import Users from "./components/Users";
import Contact from "./components/Contact";
import About from "./components/About";
function App() {
  return (
    <div>
      <Router>
        <div>
          <Route path="/about" component={About} />
          <Route path="/users" component={Users} />
          <Route path="/contact" component={Contact} />
        </div>
      </Router>
    </div>
  );
}
```

Retact Router 패키지에서 가져온 <Route/> 구성 요소는 지정된 경로로 사용자를 안내하는 `path`와 해당 경로의 내용을 정의하는 `component`라는 두 가지 소품을 사용합니다.

## 라우팅에 대한 후크 대안

이 모든 것은 Chris Engel이 이러한 데모를 집결시키기 위해 집중할 것입니다. 후크 라우터 모듈은 미리 정의된 경로 객체를 평가하고 결과를 반환하는 `useRoutes()` 후크를 내보냅니다. 경로 개체에서 경로가 일치할 때 호출되는 함수로 값을 사용하여 경로를 키로 정의합니다. 다음은 실제 시연입니다.

```coffeescript
import React from "react";
import Users from "./components/Users";
import Contact from "./components/Contact";
import About from "./components/About";
const routes = {
  "/": () => <Users />,
  "/about": () => <About />,
  "/contact": () => <Contact />
};
export default routes;
```

개인적으로 저는 이 방법이 마음에 들어요. 왜요? 우리가 그렇게 많은 일을 할 필요가 없었기 때문이죠. React Router를 사용하여 앱의 모든 개별 경로에 대해 `<Route/>` 구성 요소를 렌더링해야 했습니다. 우리가 전달한 모든 소품은 말할 것도 없고요. 다시 후크로 돌아가면, 우리는 정의된 이 "경로"를 "사용경로"() 후크에 전달하기만 하면 우리의 앱에서 우리는 이것을 사용할 수 있다.

```js
import {useRoutes} from 'hookrouter';
import Routes from './router'

function App() {
  const routeResult = useRoutes(Routes)
  return routeResult
}
```

이는 Retact Router 라우팅 데모와 동일한 결과를 제공하지만 보다 깔끔하고 가벼운 구현으로 얻을 수 있습니다.

코드 샌드박스에서 이 편집 가능한 코드 예제를 확인하십시오.

## 대응 라우터 탐색

React Router는 또한 우리에게 `<Link/>` 컴포넌트에 대한 액세스 권한을 준다. Return 앱에서 경로 탐색을 사용자 정의하고 대화형 라우팅을 관리하는 데 도움이 됩니다. 세 개의 경로가 있는 반응 앱이 있습니다. 화면을 통해 경로를 렌더링하고 클릭하면 해당 경로를 탐색해 보겠습니다.

```xml
import { Route, Link, BrowserRouter as Router } from "react-router-dom";
import Users from "./components/Users";
import Contact from "./components/Contact";
import About from "./components/About";

function App() {
  return (
    <div className="App">
      <Router>
        <div>
          <ul>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/users">Users</Link>
            </li>
            <li>
              <Link to="/contact">Contact</Link>
            </li>
          </ul>
          <Route path="/about" component={About} />
          <Route path="/users" component={Users} />
          <Route path="/contact" component={Contact} />
        </div>
      </Router>
    </div>
  );
}
```

이렇게 하면 앱 내에서 한 페이지에서 다른 페이지로 이동하는 데 필요한 탐색이 만들어집니다. 여기 우리가 여기서 하고 있는 일을 시각적으로 표현한 것이 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/reactrouterdemohook.gif?resize=730%2C509&ssl=1)

다음은 편집 가능한 코드 예제입니다.

## 반응 탐색에 대한 후크 대안

후크라우터 모듈은 HTML 앵커 태그 `a/`를 `A/`로 감싸는 래퍼를 제공합니다. React 구성 요소로 액세스할 수 있으며 기본 `a/` 태그와 호환되는 100% 기능으로 액세스할 수 있습니다. 유일한 차이점은 새 페이지를 실제로 로드하는 대신 기록 스택으로 탐색을 푸시한다는 것입니다.

```js
const routes = {
  "/user": () => <Users />,
  "/about": () => <About />,
  "/contact": () => <Contact />
};

function App() {
  const routeResult = useRoutes(routes);
  return (
    <div className="App">
      <A href="/user">Users Page</A>
      <A href="/about">About Page</A>
      <A href="/contact">Contacts Page</A>
      {routeResult}
    </div>
  );
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/reactrouterdemohook.gif?resize=730%2C509&ssl=1)

여기 인터랙티브 데모가 있습니다.

## 프로그램 탐색

후크라우터 모듈은 URL을 전달할 수 있는 `navigate() 후크` 기능에 액세스할 수 있게 해 주며, 사용자가 해당 URL로 이동할 수 있도록 사용자를 탐색합니다. 따라서 `navigate()` 기능에 대한 모든 호출은 앞으로 탐색되므로 사용자는 브라우저의 뒤로 버튼을 클릭하여 이전 URL로 돌아갈 수 있습니다.

```bash
navigate('/user/');
```

이 작업은 기본적으로 수행됩니다. 그러나 다른 동작이 필요한 경우 탐색을 대체할 수 있습니다. 어떻게요? `navigation() 후크에는 주로 `navigation(url, [replace], [queryParams]라는 세 가지 매개 변수가 포함됩니다.`두 번째 매개 변수는 교체 동작에 영향을 미치는 데 사용됩니다. 현재 기록 항목을 지우고 새 기록으로 대체합니다. 그 효과를 얻기 위해서는 단순히 그것의 주장을 `참`으로 설정하기만 하면 된다.

```bash
navigate('/user', true);
```

## 반응 라우터 스위치

일반적으로 Retact Router는 정의된 탐색 경로가 일치하지 않을 때 `<Switch/>` 구성 요소를 사용하여 기본 페이지를 렌더링합니다. 일반적으로 404 페이지를 렌더링하여 선택한 경로가 응용프로그램에 정의되어 있지 않음을 사용자에게 알립니다. 이를 위해, 우리는 `<스위치/>` 구성 요소 안에 있는 모든 렌더링된 경로를 포장하고 그것에 대한 `경로` 받침대를 정의하지 않고 404 페이지를 렌더링합니다.

```xml
import { Route, Link, BrowserRouter as Router, Switch } from "react-router-dom";
import Users from "./components/Users";
import Contact from "./components/Contact";
import Home from "./components/About";
import NoPageFound from "./components/NoPageFound.js";

function App() {
  return (
    <div className="App">
      <Router>
        <div>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/users">Users</Link>
            </li>
            <li>
              <Link to="/contact">Contact</Link>
            </li>
          </ul>
          <Switch>
            <Route exact path="/" component={Home} />
            <Route path="/users" component={Users} />
            <Route path="/contact" component={Contact} />
            <Route component={NoPageFound} />
          </Switch>
        </div>
      </Router>
    </div>
  );
}
```

이렇게 하면 정의되지 않은 경로에 도달할 때마다 React Router는 `No 페이지`를 렌더링합니다.`구성 요소`를 찾았습니다. 이것은 사용자가 반응 사이트를 탐색하는 동안 항상 현재 위치와 상황을 알려주는 매우 중요한 방법입니다.

다음은 편집 가능한 코드 예제입니다.

## 스위치 대신 후크

모든 경로 경로를 포함하는 `경로` 개체를 정의하고 해당 개체를 `경로() 후크`에 전달하면 경로를 조건부로 렌더링하는 것이 매우 간단해진다. 선택한 경로가 정의되지 않은 경우 기본적으로 렌더링할 `NoPageFound` 파일을 정의하는 경우 다음과 같은 결과 기능과 함께 렌더링하기 위해 해당 파일만 전달하면 됩니다.

```js
import { useRoutes, A } from "hookrouter";
import routes from "./router";
import NoPageFound from "./components/NoPageFound";
function App() {
  const routeResult = useRoutes(routes);
  return (
    <div className="App">
      <A href="/user">Users Page</A> <br />
      <A href="/about">About Page</A>
      <br />
      <A href="/contact">Contacts Page</A> <br />
      {routeResult || <NoPageFound />}
    </div>
  );
}
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/alternativetoswitch.gif?resize=730%2C515&ssl=1)

리액트 라우터의 <스위치> 구성 요소를 사용하여 기본 페이지를 렌더링하는 것에 비해, 이 부분이 좀 더 깔끔하고 읽기 쉬운 것 같습니다.

이 예제를 확인하십시오.

## 라우터 리디렉션 대응

리디렉션은 사용자를 한 경로에서 다른 경로로 동적으로 리디렉션하려는 경우에 발생합니다. 예를 들어 로그인 중에 사용자가 로그인하면 `(`/로그인`) 경로에서 `(`/대시보드` 경로로 리디렉션됩니다.

React Router를 사용하면 history 객체나 `Redirect/` 구성 요소를 사용하여 몇 가지 방법으로 이 작업을 수행할 수 있습니다. 예를 들어 로그인 양식이 있는 경우 브라우저의 기록 개체를 활용하여 로그인 시 사용자를 `/대시보드` 경로로 밀어넣을 수 있습니다.

```coffeescript
import React from 'react'
class Login extends React.Component {
  loginUser = () => {
  // if (user is logged in successfully)
    this.props.history.push('/dashboard')
  }
  render() {
    return (
      <form>
        <input type="name" />
        <input type="email" />
        <button onClick={this.loginUser}>Login</button>
      </form>
    )
  }
}
export default Login
```

따라서 React Router에서 사용할 수 있는 `<Redirect/>` 구성 요소를 사용하여 사용자를 동적으로 리디렉션할 수도 있다.

## 리디렉션에 대한 후크 대체

후크 라우터 모듈은 소스 경로와 대상 경로를 매개 변수로 사용할 수 있는 `usseRedirect() 후크를 내보냅니다.

```bash
useRedirect('/user', '/dashboard');
```

이렇게 하면 `/user` 경로가 일치할 때마다 사용자가 `/dashboard` 경로로 자동 리디렉션됩니다. 예를 들어, 사용자를 표시하지 않고 에서 `/대시보드`로 사용자를 자동으로 리디렉션하려는 경우 다음과 같이 앱을 정의합니다.

```coffeescript
import {useRoutes, useRedirect} from 'hookrouter';
import dashboard from "./components/Dashboard";
const routes = {
    '/home': () => <Users />,
    '/dashboard': () => <Dashboard />
};
const Users = () => {
    useRedirect('/user', '/dashboard');
    const routeResult = useRoutes(routes);
    return routeResult
}
```

이 프로세스의 시각적 출력은 다음과 같습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/alternativestoredirects.gif?resize=730%2C504&ssl=1)

use Redirect() 후크가 대체 탐색 의도를 트리거한다는 점에 주목할 필요가 있다. 따라서 탐색 기록에 한 항목만 남게 됩니다. 즉, 마지막 코드 조각에서 보듯이 `/user`에서 `/dashboard`로 리디렉션이 발생하면 `/user` 경로가 검색 기록에 나타나지 않습니다. 우리는 `/대시보드` 경로만 가질 것이다.

## 응답 라우터를 사용하여 URL 매개 변수 처리

URL 매개 변수는 동적 URL을 기반으로 구성 요소를 렌더링하는 데 도움이 됩니다. 중첩된 경로와 유사한 방식으로 작동하지만, 이 경우 경로가 정확하게 변경되지 않고 업데이트되고 있습니다.

예를 들어, 우리 앱에 서로 다른 사용자가 있다면 `user/user1/`, `user/user2/` 등과 같은 개별 경로로 구분해서 식별하는 것이 타당할 것이다. 그러려면 URL 매개 변수를 사용해야 합니다. React Router에서, 우리는 간단히 콜론으로 시작하는 자리 표시자(`id`와 같은)를 `<Route/>` 구성 요소의 `path` prop로 전달한다.

```xml
<Route path="users/:id" component={Users} />
```

이제 브라우저에서 `users/1`로 이동하면 이 특정 사용자를 `Users.js` propo에서 사용할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2019/07/alternativestorouter.png?resize=730%2C295&ssl=1)

## URL 매개 변수 처리에 대한 후크 대안

후크 라우터가 Retact Router와 비교하여 URL 매개 변수를 처리하는 방식에는 큰 차이가 없습니다. 구조는 동일합니다. 즉, 콜론과 매개 변수 이름을 사용하여 URL 매개 변수를 대상 경로에 전달할 수 있습니다.

하지만, 후크가 일하는 방식에는 여전히 차이가 있습니다. 모든 URL 매개 변수를 읽고 개체에 넣습니다. 경로 개체에 정의된 키를 사용하여 이 작업을 수행합니다. 그런 다음 명명된 모든 매개 변수가 결합된 개체로 경로 결과 함수로 전달됩니다.

```coffeescript
const routes = {
  '/user/:id': ({id}) => <User userId={id} />
}
```

객체 파괴를 사용하여 소품 객체에서 id 속성을 가져와서 우리의 구성요소에 적용하기만 하면 된다. 그렇게 하면, 우리는 Retact Router 대안과 정확히 같은 결과를 얻을 수 있다.

## 결론

이 게시물의 시작 부분에서 말씀드린 바와 같이, 리액트 프로젝트에서 라우팅할 수 있는 다른 방법을 제공하고자 합니다. Retact Router는 훌륭한 도구이지만, Hooks의 등장으로 인해 React에서 많은 것이 바뀌었으며 라우팅이 작동하는 방식도 포함되어 있다고 생각합니다. 후크를 기반으로 하는 이 모듈은 소규모 프로젝트에서 경로를 처리할 경우 보다 유연하고 깔끔한 방법을 제공합니다. 저처럼 새로운 도구를 사용해 보는 것을 좋아하신다면, 한번 시도해 보시길 권합니다.

이 게시물에는 두 도구가 중첩된 라우팅을 처리하는 방법 등과 같이 아직 다루지 않은 다른 많은 측면이 있습니다. 여기에서 후크 라우터 모듈에 대해 자세히 알아보십시오.