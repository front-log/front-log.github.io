---
layout: post
title: "응답 클릭 이벤트 핸들러: 전체 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/07/react-onclick-event-handlers.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2018/07/react-onclick-event-handlers.png?fit=730%2C487&ssl=1)

이 튜토리얼에서는 이벤트 수신, 클래스 구성 요소의 바인딩 방법, 사용자 지정 이벤트 처리 등 React의 `onClick` 이벤트 핸들러의 기본 사항에 대해 살펴봅니다.

## 반응의 이벤트 핸들러란?

이벤트 핸들러는 이벤트가 발생할 때마다 수행할 작업을 결정합니다. 이것은 버튼 클릭 또는 텍스트 입력의 변경일 수 있습니다.

기본적으로 이벤트 핸들러는 사용자가 반응 앱과 상호 작용할 수 있도록 합니다.

반응 요소를 사용하여 이벤트를 처리하는 것은 몇 가지 사소한 예외가 있는 DOM 요소의 이벤트 처리와 유사합니다. 표준 HTML 및 JavaScript에서 이벤트가 작동하는 방식을 잘 알고 있다면 React에서 이벤트를 처리하는 방법을 쉽게 배울 수 있습니다.

리액터를 처음 접하거나 이벤트 처리에 대한 간단한 리프레셔가 필요한 경우, 이 비디오 튜토리얼은 개념을 상당히 세분화합니다.

## React의 'onClick' 처리기는 무엇입니까?

클릭 시 응답 이벤트 처리기를 사용하면 사용자가 앱에서 단추와 같은 요소를 클릭할 때 함수를 호출하고 작업을 트리거할 수 있습니다.

이벤트 이름은 camelCase로 작성되므로 Retact 앱에서는 onClick(온클릭) 이벤트가 onClick(클릭)으로 작성된다. 또한 반응 이벤트 핸들러는 컬리 브레이스 내부에 나타납니다.

HTML로 작성된 다음과 같은 간단한 예를 들어보십시오.

```xml
<button onclick="sayHello()">
  Say Hello
<button>
```

반응 앱에서는 다음과 같이 작성됩니다.

```xml
<button onClick={sayHello}>
  Say Hello
<button>
```

또 다른 주요 차이점은 HTML의 기본 동작을 피하기 위해 `false`를 반환하는 반면, React에서 `preventDefault`를 명시적으로 호출해야 한다는 점입니다.

다음 예에서는 기본적으로 링크가 새 페이지를 열지 못하게 하는 방법을 보여 줍니다.

```xml
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

대응에 다음과 같이 적습니다.

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }
  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

## 반응의 합성 이벤트

React는 React 앱 및 인터페이스에 일관성과 고성능을 제공하는 합성 이벤트 시스템을 구현합니다. 다른 브라우저와 플랫폼에 걸쳐 동일한 속성을 가지도록 이벤트를 정규화함으로써 일관성을 달성한다.

합성 이벤트는 브라우저의 기본 이벤트 주변의 크로스 브라우저 래퍼입니다. 모든 브라우저에서 동일하게 동작하는 이벤트를 제외하고 `스톱 전파()`와 `preventDefault()를 포함한 브라우저의 기본 이벤트와 동일한 인터페이스를 가지고 있다.

자동으로 이벤트 위임을 사용하여 높은 성능을 달성합니다. 실제로 React는 이벤트 핸들러를 노드 자체에 연결하지 않습니다. 대신, 단일 이벤트 수신기가 문서의 루트에 첨부됩니다. 이벤트가 발생하면 대응은 이벤트를 적절한 구성 요소 요소에 매핑합니다.

## 반응에서 이벤트 듣기

반응에서 이벤트를 청취하려면 이벤트 핸들러인 `온클릭` 특성을 대상 요소에 추가하십시오. 요소를 클릭할 때 실행할 기능을 지정합니다.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class ShowAlert extends Component {
  showAlert() {
    alert("I'm an alert");
  }

  render() {
    return <button onClick={this.showAlert}>show alert</button>;
  }
}
export default ShowAlert;
```

위의 예에서 `onClick` 속성은 메시지를 경고하는 `showAlert` 함수로 설정됩니다. 이는 버튼을 클릭할 때마다 `경고 표시` 기능이 호출되고, 차례로 경고 상자가 표시된다는 것을 의미한다.

## 클래스 구성 요소의 이벤트 처리

JavaScript에서 클래스 메서드는 기본적으로 바인딩되어 있지 않습니다. 따라서 클래스 인스턴스에 함수를 바인딩할 필요가 있습니다.

### '렌더()' 메서드의 바인딩

바인딩 문제를 해결하는 한 가지 방법은 `렌더` 함수에서 바인드를 호출하는 것입니다.

위의 예에서, 우리는 텍스트 입력에 이벤트를 입력하기 위해 `onChange` 이벤트 핸들러를 사용하고 있다. 이 작업은 렌더링() 함수에 바인딩하여 수행됩니다. 이 메서드는 렌더링() 함수에서 .bind(this)를 호출해야 합니다.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class ChangeInput extends Component {
    constructor(props) {
      super(props);
      this.state = {
        name: ""
      };
    }

    changeText(event) {
      this.setState({
      name: event.target.value
    });
  }

  render() {
     return (
       <div>
         <label htmlFor="name">Enter Text here </label>
         <input type="text" id="name" onChange={this.changeText.bind(this)} />
         <h3>{this.state.name}</h3>
       </div>
     );
  }
}

export default ChangeInput;
```

이는 모든 ES6 클래스 메서드가 일반 JavaScript 함수이기 때문에 Function 메서드에서 bind()를 상속하기 때문입니다. 그래서 이제 JSX에서 on Change()를 부를 때 이것은 우리 반의 예를 가리킬 것이다. 쉬워요.

그러나 이 방법을 사용하면 모든 렌더에서 함수를 재할당하기 때문에 성능에 영향을 미칠 수 있습니다. 이러한 성능 비용은 소규모 Retact 애플리케이션에서는 전혀 보이지 않을 수 있지만 대규모 Retact 애플리케이션에서는 눈에 띄게 나타날 수 있습니다.

### 생성자() 메서드의 바인딩

렌더에서 바인딩이 작동하지 않는 경우 생성자를 바인딩할 수 있습니다. 아래 예제를 참조하십시오.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class ChangeInput extends Component {
  constructor(props) {
    super(props);
    this.state = {
      name: ""
    };    
    this.changeText = this.changeText.bind(this);
  }

  changeText(event) {
    this.setState({
    name: event.target.value
  });
  }

  render() {
    return (
      <div>
        <label htmlFor="name">Enter Text here </label>
        <input type="text" id="name" onChange={this.changeText} />
        <h3>{this.state.name}</h3>
      </div>
    );
  }
}

export default ChangeInput;
```

위에서 볼 수 있듯이 텍스트 변경 함수는 생성자 메소드에서 바인딩되어 있습니다.

```coffeescript
this.changeText = this.changeText.bind(this)
```

위의 코드 라인이 어떻게 작동하는지 살펴봅시다.

첫 번째 `this.changeText`는 `changeText` 방식을 가리킨다. 이 작업은 생성자에서 수행되므로 this는 ChangeInput 클래스 구성 요소를 나타냅니다.

두 번째 `this.changeText`도 같은 `changeText() 방식`을 가리키고 있지만, 지금은 `.bind()`라고 부르고 있다.

마지막 "this"는 ".bind()"로 전달하는 컨텍스트이며 "ChangeInput" 클래스 구성 요소를 나타냅니다.

또한 `ChangeText`가 클래스 인스턴스에 바인딩되어 있지 않으면 `이`에 액세스할 수 없습니다.state를 설정하면 this는 정의되지 않음으로 설정됩니다. 이것이 이벤트 처리 기능을 바인딩하는 또 다른 중요한 이유입니다.

### 화살표 기능으로 바인딩

클래스 구성 요소의 이벤트를 지방 화살표 함수와 바인딩하여 처리할 수 있습니다.

ES7 클래스 속성은 아래 예제와 같이 메서드 정의에서 바인딩을 활성화합니다. 정의상 화살표 함수 식은 함수 식보다 구문이 짧고 자체적인 this, 인수, 수퍼, new.target이 없다.

```js
import React, { Component } from "react";
import ReactDOM from "react-dom";

class ChangeInput extends Component {
  handleEvent = event => {
    alert("I was clicked");
  };

  render() {
    return (
      <button onClick={this.handleEvent}>Click on me</button>
    );
  }
}

export default ChangeInput;
```

위의 예에서 구성 요소가 생성되면 `this.handleEvent`는 다시 변경되지 않습니다. 이는 다시 `버튼`이 다시 렌더링되지 않는다는 것을 의미한다. 이 접근 방식은 매우 간단하고 읽기 쉽습니다.

## 기능 구성 요소의 이벤트 처리

기능 반응 구성 요소의 이벤트를 처리하는 방법에는 여러 가지가 있습니다. 여기서 다섯 개를 살펴보도록 하죠.

### 1. '온클릭' 이벤트 핸들러에서 인라인 함수 호출

인라인 기능을 사용하면 JSX에서 직접 이벤트 처리를 위한 코드를 작성할 수 있습니다. 아래 예제를 참조하십시오.

```js
import React from "react";

const App = () => {
  return (
    <>
      <button onClick={() => alert("Hello!")}>Say Hello</button>
    </>
  );
};

export default App;
```

이것은 일반적으로 JSX 외부의 추가 함수 선언을 피하기 위해 사용되지만, 인라인 함수의 내용이 너무 많으면 가독성이 떨어지고 유지하기 어려울 수 있다.

### 2. '온클릭' 이벤트 핸들러 내에서 상태 업데이트

응답 응용 프로그램에서 `onClick` 이벤트 처리기에서 로컬 상태를 업데이트해야 한다고 가정해 보겠습니다. 방법은 다음과 같습니다.

```js
import React, { useState } from "react";

const App = () => {
  const [count, setCount] = useState(0);
  return (
    <>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
    </>
  );
};

export default App;
```

위의 예에서 로컬 상태의 값은 `onClick` 이벤트 핸들러 내부에 업데이트 프로그램 기능인 `setCount`가 있는 `증가` 및 `감소` 버튼에 의해 수정된다.

### 3. '온클릭' 이벤트 핸들러에서 다중 기능 호출

또한 `onClick` 이벤트 핸들러를 사용하여 여러 기능을 호출할 수 있습니다.

```js
import React, { useState } from "react";

const App = () => {
  const [count, setCount] = useState(0);
  const sayHello = () => {
    alert("Hello!");
  };

  return (
    <>
      <p>{count}</p>
      <button
        onClick={() => {
          sayHello();
          setCount(count + 1);
        }
      >
        Say Hello and Increment
      </button>
    </>
  );
};

export default App;
```

위의 코드 블록에서 이 버튼을 클릭하면 로컬 상태가 증가하며 메시지를 경고합니다. 두 동작 모두 `onClick` 이벤트 핸들러에서 별도의 함수에 의해 실행된다.

### 4. 'onClick' 이벤트 핸들러에 매개 변수 전달

이벤트 핸들러의 또 다른 일반적인 사용 사례는 매개 변수를 함수에 전달하여 나중에 사용할 수 있도록 하는 것이다. 예를 들어:

```js
import React from "react";

const App = () => {
  const sayHello = (name) => {
    alert(`Hello, ${name}!`);
  };

  return (
    <button
      onClick={() => {
        sayHello("Yomi");
      }
    >
      Say Hello
    </button>
  );
};

export default App;
```

여기서 `sayHello` 함수는 매개 변수로 이름을 수락한 다음 메시지를 알리는 데 사용됩니다.

### 5. '온클릭' 이벤트 핸들러 내에서 직접 합성 이벤트 사용

또한 `onClick` 이벤트 핸들러 내부에서 직접 합성 이벤트를 사용할 수도 있습니다. 아래 예에서 버튼 값은 `e.target.value`를 통해 얻은 다음 메시지를 알리는 데 사용됩니다.

```js
import React from "react";

const App = () => {
  return (
    <button value="Hello!" onClick={(e) => alert(e.target.value)}>
      Say Hello
    </button>
  );
};

export default App;
```

## 반응의 사용자 정의 구성 요소 및 이벤트

대응의 이벤트에 대해서는 DOM 요소만 이벤트 핸들러를 가질 수 있습니다. `OnClick` 이벤트가 있는 `CustomButton` 구성 요소의 예를 들어보십시오. 위의 이유로 인해 클릭에는 응답하지 않습니다.

그러면 사용자 정의 구성 요소에 대한 이벤트 처리를 어떻게 처리해야 할까요?

CustomButton 구성 요소 내부에 DOM 요소를 렌더링하고 onClick(온클릭) 프로펠러를 전달한다. 우리의 CustomButton은 클릭 이벤트의 패스스루이다.

```js
import React from "react";

const CustomButton = ({ onPress }) => {
  return (
    <button type="button" onClick={onPress}>
      Click on me
    </button>
  );
};

const App = () => {
  const handleEvent = () => {
    alert("I was clicked");
  };
  return <CustomButton onPress={handleEvent} />;
};

export default App;
```

위의 예에서 CustomButton 구성 요소는 on Press의 받침대를 전달한 다음 버튼의 onClick으로 전달됩니다.

## 결론

이벤트 핸들러는 이벤트가 발생할 때 수행할 작업을 결정합니다. onClick(온클릭) 이벤트는 DOM 요소의 클릭 이벤트를 수신하는 데 사용됩니다.

클래스 구성 요소에서 이벤트 처리를 할 때는 바인딩이 중요하며, 이에 대한 몇 가지 방법이 있습니다. 이 튜토리얼에서 다루었습니다.

또한 상태 업데이트, 다중 함수 호출, 합성 이벤트 사용 등과 같은 기능 구성 요소에서 `onClick` 이벤트 처리기의 몇 가지 일반적인 사용 사례를 검토했다.

마지막으로, 우리는 사용자 정의 구성 요소에서 `onClick` 이벤트 처리기가 작동하는 방식을 다루었다.

## 프로덕션에서 모든 "onClick" 이벤트 모니터링

특히 복잡한 상태가 있는 경우 디버깅 반응 응용 프로그램이 어려울 수 있습니다. 프로덕션 중인 모든 사용자에 대해 Redux 상태를 모니터링하고 추적하는 데 관심이 있는 경우 LogRocket을 사용해 보십시오.

![image](https://i2.wp.com/files.readme.io/27c94e7-Image_2017-06-05_at_9.46.04_PM.png?ssl=1)

LogRocket은 웹 애플리케이션용 DVR과 유사하며, 사이트에서 발생하는 모든 것을 문자 그대로 기록합니다. 문제가 발생하는 이유를 추측하는 대신 문제가 발생했을 때 응용 프로그램이 어떤 상태에 있었는지 집계하여 보고할 수 있습니다.

LogRocket Redux 미들웨어 패키지는 사용자 세션에 추가적인 가시성을 제공합니다. LogRocket은 Redux 스토어의 모든 작업 및 상태를 기록합니다.

응답 앱 디버그 방법 현대화 - 무료로 모니터링 시작