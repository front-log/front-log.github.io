---
layout: post
title: "반응: 9가지 방법의 조건 렌더링과 예"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2018/02/conditional-rendering-in-react.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2018/02/conditional-rendering-in-react.png?fit=730%2C487&ssl=1)

편집자 참고: 본 튜토리얼은 React 16.8에 도입된 변경사항, 즉 React Hooks 및 Spragments를 조건부 렌더링에 사용하는 방법을 반영하기 위해 2020년 10월에 마지막으로 업데이트되었습니다.

JSX는 UI 구성 요소를 정의할 수 있는 JavaScript의 강력한 확장 기능입니다. 그러나 루프나 조건부 표현은 직접 지원하지 않습니다(이전에도 조건부 표현식의 추가에 대해 논의한 적이 있지만).

둘 이상의 구성 요소를 렌더링하거나 일부 조건부 논리를 구현하기 위해 목록에 반복하려면 순수 JavaScript를 사용해야 합니다. 루프에 대한 옵션도 많지 않습니다. 대부분의 경우 지도는 여러분의 요구를 충족시켜 줄 것입니다.

하지만 조건부 렌더링? 그건 별개의 이야기다.

## React에서 조건부 렌더링이란 무엇입니까?

React에서 조건부 렌더링이란 특정 조건을 기반으로 요소 및 구성 요소를 전달하는 프로세스를 말합니다.

React에서 조건부 렌더링을 사용하는 방법은 두 가지가 아닙니다. 프로그래밍의 대부분의 경우와 마찬가지로, 어떤 것은 해결하려는 문제에 따라 다른 것보다 더 적합합니다.

이 튜토리얼에서는 React에서 조건부 렌더링을 구현하는 가장 일반적인 방법에 대해 설명합니다.

- 반응에서 if/else를 쓰는 방법
- `null`로 렌더링 방지
- 반응 요소 변수
- 반응의 터미널 연산자
- 단락 AND 연산자(`
- 즉시 호출된 함수 식(IIFE)
- 하위 구성 요소 대응
- 열거형 객체
- 반응의 고차 성분(HOC)

또한 React에서 조건부 렌더링을 구현하기 위한 다음과 같은 몇 가지 팁과 모범 사례를 검토할 것입니다.

- 성능 고려 사항
- 조각이 있는 조건부 렌더링
- 반응 후크를 사용한 조건부 렌더링
- React에서 조건부 렌더링을 구현하는 방법 결정

이러한 모든 방법의 작동 방식을 입증하기 위해 보기/편집 기능이 있는 구성 요소를 구현합니다.

![image](https://cdn-images-1.medium.com/max/1600/0*vS8AU_xnc4VHcHrK.)

JSFiddle의 모든 예제를 포크로 만들 수 있습니다.

먼저 if/else 블록을 사용하는 가장 단순한 구현부터 시작하여 거기서 구축하겠습니다.

## 1. 반응일 경우/다른 경우 쓰는 방법

다음 상태의 구성 요소를 생성하십시오.

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {text: '', inputText: '', mode:'view'};
  }
}
```

저장된 텍스트에 대해 하나의 속성을 사용하고 편집 중인 텍스트에 대해 다른 속성을 사용합니다. 세 번째 속성은 사용자가 `편집` 또는 `보기` 모드인지 나타냅니다.

그런 다음 입력 텍스트를 처리하는 몇 가지 방법을 추가한 다음 이벤트를 저장하고 편집합니다.

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {text: '', inputText: '', mode:'view'};
    
    this.handleChange = this.handleChange.bind(this);
    this.handleSave = this.handleSave.bind(this);
    this.handleEdit = this.handleEdit.bind(this);
  }
  
  handleChange(e) {
    this.setState({ inputText: e.target.value });
  }
  
  handleSave() {
    this.setState({text: this.state.inputText, mode: 'view'});
  }

  handleEdit() {
    this.setState({mode: 'edit'});
  }
}
```

이제 `렌더` 방법의 경우, `모드` 상태 속성을 확인하여 저장된 텍스트 외에 편집 버튼이나 텍스트 입력 및 저장 버튼을 렌더링합니다.

```js
class App extends React.Component {
  // …
  render () {
    if(this.state.mode === 'view') {
      return (
        <div>
          <p>Text: {this.state.text}</p>
          <button onClick={this.handleEdit}>
            Edit
          </button>
        </div>
      );
    } else {
      return (
        <div>
          <p>Text: {this.state.text}</p>
            <input
              onChange={this.handleChange}
              value={this.state.inputText}
            />
          <button onClick={this.handleSave}>
            Save
          </button>
        </div>
      );
    }
}
```

테스트해 볼 수 있는 완벽한 피드는 다음과 같습니다.

만약/다른 블록이 문제를 해결하는 가장 쉬운 방법이지만, 여러분은 이것이 좋은 구현이 아니라는 것을 알고 있을 것입니다.

간단한 사용 사례에 유용하며, 모든 프로그래머가 어떻게 작동하는지 알고 있습니다. 하지만 반복이 많고, `렌더` 방식은 복잡해 보인다.

모든 조건부 로직을 두 가지 렌더 방식으로 추출하여 단순화합니다. 하나는 입력 상자를 렌더링하고 다른 하나는 버튼을 렌더링합니다.

```js
class App extends React.Component {
  // …
  
  renderInputField() {
    if(this.state.mode === 'view') {
      return <div></div>;
    } else {
      return (
          <p>
            <input
              onChange={this.handleChange}
              value={this.state.inputText}
            />
          </p>
      );
    }
  }
  
  renderButton() {
    if(this.state.mode === 'view') {
      return (
          <button onClick={this.handleEdit}>
            Edit
          </button>
      );
    } else {
      return (
          <button onClick={this.handleSave}>
            Save
          </button>
      );
    }
  }

  render () {
    return (
      <div>
        <p>Text: {this.state.text}</p>
        {this.renderInputField()}
        {this.renderButton()}
      </div>
    );
  }
}
```

테스트해 볼 수 있는 완벽한 피드는 다음과 같습니다.

큰 `if/else` 블록 대신 동일한 변수에 의존하여 조건을 평가하는 분기가 두 개 이상 있는 경우:

```cpp
if(this.state.mode === 'a') {
  // ...   
} else if(this.state.mode === 'b') {
  // ...
} else if(this.state.mode === 'c') {
  // ...
} else {
  // ...
}
```

"스위치" 문을 사용할 수 있습니다.

```cpp
switch(this.state.mode) {
  case 'a':
    // ...
  case 'b':
    // ...
  case 'c':
    // ...
  default:
    // equivalent to the last else clause ...
}
```

좀 더 명확해 질 수 있지만,

- 아직 너무 장황하다.
- 여러/다른 조건에서는 작동하지 않습니다.
- `if/else` 문과 마찬가지로 JSX와 함께 `return` 문 안에 사용할 수 없습니다(즉시 호출된 기능을 사용할 때는 예외이며, 나중에 다루겠습니다).

이 코드를 개선하기 위한 몇 가지 추가 기술을 살펴보겠습니다.

app가 보기 모드에 있을 때 `enderInputField` 메서드 `div`가 비어 있는 `<` 요소를 반환합니다. 그러나 이것은 필요하지 않다.

## 2. 'null'로 렌더링 방지

구성 요소를 숨기려면 해당 렌더 메서드가 `null`을 반환하도록 할 수 있으므로 빈 요소(및 다른 요소)를 자리 표시자로 렌더링할 필요가 없습니다. 그러나 Null을 반환할 때 명심해야 할 한 가지 중요한 점은 부품이 나타나지 않더라도 해당 부품의 수명 주기 방법은 여전히 실행된다는 점이다.

두 개의 구성 요소로 카운터를 구현하는 다음 피드를 예로 들어 보겠습니다.

Number(숫자) 구성 요소는 짝수 값의 카운터만 렌더링하며, 그렇지 않으면 Null이 반환됩니다. 그러나 콘솔을 보면 `렌더`에서 반환되는 값에 관계없이 `componentDidUpdate`가 항상 호출되는 것을 볼 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/02/componentDidUpdate.png?resize=664%2C327&ssl=1)

이 예제로 돌아가 `렌더 입력 필드` 방법을 다음과 같이 변경하십시오.

```js
  renderInputField() {
    if(this.state.mode === 'view') {
      return null;
    } else {
      return (
          <p>
            <input
              onChange={this.handleChange}
              value={this.state.inputText}
            />
          </p>
      );
    }
  }
```

완벽한 피드는 다음과 같습니다.

빈 요소 대신 `null`을 반환하는 한 가지 이점은 React가 구성 요소를 교체하기 위해 마운트 해제할 필요가 없기 때문에 앱의 성능을 약간 향상시킬 수 있다는 것입니다.

예를 들어 빈 `<div> 요소를 렌더링하는 피드를 시도할 때 Inspector 탭을 열면 루트 아래의 `<div> 요소가 항상 업데이트되는 방법을 볼 수 있습니다.

![image](https://cdn-images-1.medium.com/max/1600/0*1f--Ics8DXB3UFp_.)

구성 요소를 숨기기 위해 `null`이 반환되는 경우와 달리 `편집` 버튼을 클릭해도 해당 `div` 요소가 업데이트되지 않습니다.

![image](https://cdn-images-1.medium.com/max/1600/0*7SzdmNMiVje-msFz.)

React에서 조정에 대해 자세히 알아보자. 기본적으로 React가 DOM 요소를 업데이트하는 방법과 분산 알고리즘의 작동 방식을 참조한다.

이 간단한 예에서는 성능 개선이 미미한 수준이지만 큰 구성 요소를 사용할 때는 차이가 있을 수 있습니다. 나중에 조건부 렌더링의 성능에 대해 더 자세히 설명하겠습니다. 지금은 이 예제를 계속 개선해 보겠습니다.

## 3. 반응 요소 변수

한 가지 마음에 들지 않는 것은 방법에 둘 이상의 `반환` 문이 있는 것이므로 변수를 사용하여 JSX 요소를 저장하고 조건이 `참`일 때만 초기화하려고 합니다.

```undefined
renderInputField() {
    let input;
    
    if(this.state.mode !== 'view') {
      input = 
        <p>
          <input
            onChange={this.handleChange}
            value={this.state.inputText} />
        </p>;
    }
      
      return input;
  }
  
  renderButton() {
    let button;
    
    if(this.state.mode === 'view') {
      button =
          <button onClick={this.handleEdit}>
            Edit
          </button>;
    } else {
      button =
          <button onClick={this.handleSave}>
            Save
          </button>;
    }
    
    return button;
  }
```

이는 그 방법에서 null을 반환하는 것과 같은 결과를 가져온다. 시험해 볼 수 있는 만지작거리는 다음과 같습니다.

주 `렌더` 방법은 이런 방식으로 더 읽기 쉽지만, 만약 다른 방법(또는 `스위치` 문 같은 것)과 보조 렌더 방법을 사용할 필요가 없을 수도 있다. 좀 더 간단한 방법으로 접근해 봅시다.

## 4. React의 Terminary 연산자

if/else 블록을 사용하는 대신 ternary 조건부 연산자를 사용할 수 있습니다.

```undefined
condition ? expr_if_true : expr_if_false
```

연산자는 곱슬곱슬한 가새로 포장되어 있으며, 표현식은 가독성을 향상시키기 위해 선택적으로 괄호 안에 포장된 JSX를 포함할 수 있다. 구성 요소의 다른 부분에도 적용할 수 있습니다.

이것을 실제로 볼 수 있도록 이 예에 적용하겠습니다. 렌더링 입력 필드와 렌더버튼을 제거하고 렌더링 방법에서 해당 구성 요소가 `보기` 모드인지 `편집` 모드인지 확인할 변수를 추가합니다.

```js
render () {
  const view = this.state.mode === 'view';

  return (
      <div>
      </div>
  );
}
```

이제 터미널 연산자를 사용하여 `보기` 모드가 설정된 경우 `null`을 반환하거나 입력 필드를 반환할 수 있습니다.

```js
  // ...

  return (
      <div>
        <p>Text: {this.state.text}</p>
        
        {
          view
          ? null
          : (
            <p>
              <input
                onChange={this.handleChange}
                value={this.state.inputText} />
            </p>
          )
        }

      </div>
  );
```

터미널 연산자를 사용하여 한 구성 요소의 핸들러 및 레이블을 적절하게 변경하여 저장 또는 편집 버튼을 렌더링할 구성 요소를 선언할 수 있습니다.

```js
  // ...

  return (
      <div>
        <p>Text: {this.state.text}</p>
        
        {
          ...
        }

        <button
          onClick={
            view 
              ? this.handleEdit 
              : this.handleSave
          } >
              {view ? 'Edit' : 'Save'}
        </button>

      </div>
  );
```

시험해 볼 수 있는 만지작거리는 다음과 같습니다.

앞에서 언급했듯이, 이 연산자는 반환문과 JSX 내부에서도 구성 요소의 다른 부분에 적용될 수 있으며, 한 줄의 `if/else` 문 역할을 한다. 하지만, 정확히 이러한 이유로, 모든 것이 빠르게 흐트러질 수 있습니다.

코드를 개선할 수 있는 다른 방법을 검토하겠습니다.

## 5. 단락 AND 연산자('

터미널 운영자는 단순화할 수 있는 특별한 경우를 가지고 있습니다. 무언가를 렌더링하거나 아무것도 렌더링하지 않을 때는 `만 사용할 수 있습니다.

예를 들어 첫 번째 식이 false(`false`)로 평가되는 경우

반응에서 다음과 같은 식을 사용할 수 있습니다.

```js
return (
    <div>
        { showHeader && <Header /> }
    </div>
);
```

ShowHeader가 true로 평가하면 <의 구성요소는 해당 표현으로 반환된다. 쇼헤더가 거짓으로 평가하면 【헤더/】부품은 무시되고 빈 【div】는 반환된다.

이렇게 하면 다음 식이 됩니다.

```js
{
  view
  ? null
  : (
    <p>
      <input
        onChange={this.handleChange}
        value={this.state.inputText} />
    </p>
  )
}
```

다음으로 전환 가능:

```js
!view && (
  <p>
    <input
      onChange={this.handleChange}
      value={this.state.inputText} />
  </p>
)
```

완벽한 피드는 다음과 같습니다.

좋아 보이죠?

하지만, 터미널 운영자가 항상 더 좋아 보이는 것은 아닙니다. 복잡한 중첩된 조건 집합을 고려하십시오.

```js
return (
  <div>
    { condition1
      ? <Component1 />
      : ( condition2
        ? <Component2 />
        : ( condition3
          ? <Component3 />
          : <Component 4 />
        )
      )
    }
  </div>
);
```

이것은 꽤 빨리 엉망진창이 될 수 있다. 이러한 이유로 즉시 호출되는 함수와 같은 다른 매개 변수를 사용할 수도 있습니다.

## 6. 즉시 호출된 함수 식(IIFE)

이름에서 알 수 있듯이 즉시 호출된 함수 식(IIFE)은 정의된 직후에 실행되는 함수입니다. 명시적으로 호출할 필요가 없습니다.

일반적으로 다음과 같이 함수를 정의하고 실행합니다(나중에 다음 지점에서).

```js
function myFunction() {

// ...

}

myFunction();
```

그러나 함수가 정의된 즉시 함수를 실행하려면 전체 선언을 괄호로 묶은 다음(표현식으로 변환하려면) 두 개의 괄호를 더 추가하여 실행해야 합니다(함수가 취할 수 있는 인수 전달).

어느 쪽이든:

```js
( function myFunction(/* arguments */) {
    // ...
}(/* arguments */) );
```

또는 이 방법:

```js
( function myFunction(/* arguments */) {
    // ...
} ) (/* arguments */);
```

다른 장소에서는 기능이 호출되지 않으므로 이름을 삭제할 수 있습니다.

```js
( function (/* arguments */) {
    // ...
} ) (/* arguments */);
```

또는 화살표 기능을 사용할 수도 있습니다.

```js
( (/* arguments */) => {
    // ...
} ) (/* arguments */);
```

반응에서 곱슬곱슬한 가새로 ILIFE를 감싸고 원하는 논리(만약/다른 논리, 전환, 터미널 연산자 등)를 모두 넣은 다음 렌더링할 항목을 반환합니다.

즉, ILIFE 안에서 우리는 어떤 종류의 조건부 논리도 사용할 수 있습니다. 이를 통해 우리는 `반환` 문 안에 있는 `if/else` 문과 `switch` 문을 사용할 수 있으며, 코드의 가독성을 향상시킨다고 생각되는 경우 JSX를 사용할 수 있다.

```php
return (
  <div>
    <p>...</p>
    {
      (()=> {
        switch (condition) {
          case 1: return <Component1 />;
          case 2: return <Component2 />;
          default: null;
        }
      })()
     }
  </div>
);
```

예를 들어, 저장/편집 버튼을 렌더링하는 논리에는 다음과 같이 표시됩니다.

```js
{
  (() => {
    const handler = view 
                ? this.handleEdit 
                : this.handleSave;
    const label = view ? 'Edit' : 'Save';
          
    return (
      <button onClick={handler}>
        {label}
      </button>
    );
  })()
}
```

완벽한 피드는 다음과 같습니다.

## 7. 하위 구성 요소 대응

때때로, IFFE는 진부한 해결책처럼 보일 수 있다. 결국, React를 사용하는 것입니다. 권장되는 접근 방식은 앱의 논리를 최대한 많은 구성 요소로 나누고 필수 프로그래밍 대신 기능 프로그래밍을 사용하는 것입니다.

따라서 조건 렌더링 로직을 소품에 따라 다른 것을 렌더링하는 하위 구성 요소로 이동하는 것이 좋습니다. 하지만 여기서는 조금 다른 것을 보여드리겠습니다. 여러분이 필수적인 해결책에서 보다 선언적이고 기능적인 해결책으로 갈 수 있는 방법을 보여드리겠습니다.

먼저 `Save Component`를 생성하겠습니다.

```js
const SaveComponent = (props) => {
  return (
    <div>
      <p>
        <input
          onChange={props.handleChange}
          value={props.text}
        />
      </p>
      <button onClick={props.handleSave}>
        Save
      </button>
    </div>
  );
};
```

속성으로, 그것은 작동하는데 필요한 모든 것을 받습니다. 같은 방법으로 `구성 요소 편집`이 있습니다.

```js
const EditComponent = (props) => {
  return (
    <button onClick={props.handleEdit}>
      Edit
    </button>
  );
};
```

이제 `렌더` 방법은 다음과 같습니다.

```xml
render () {
    const view = this.state.mode === 'view';
    
    return (
      <div>
        <p>Text: {this.state.text}</p>
        
        {
          view
            ? <EditComponent handleEdit={this.handleEdit}  />
            : (
              <SaveComponent 
               handleChange={this.handleChange}
               handleSave={this.handleSave}
               text={this.state.inputText}
             />
            )
        } 
      </div>
    );
}
```

완벽한 피드는 다음과 같습니다.

### 'if' 구성 요소

JSX를 확장하여 다음과 같은 조건문을 추가하는 jsx-control-statement와 같은 라이브러리가 있습니다.

```xml
<If condition={ a === 1 }>
  <span>Hi!</span>
</If>
```

이 라이브러리는 실제로 Babel 플러그인이므로 위의 코드는 다음과 같습니다.

```js
{
  a === 1 ? <span>Hi!</span> : null;
}
```

또는 보다 복잡한 조건문에 사용되는 `선택` 태그:

```xml
<Choose>
  <When condition={ a === 1 }>
    <span>One</span>
  </When>
  <When condition={ a === 2 }>
    <span>Two</span>
  </When>
  <Otherwise>
    <span>Default</span>
  </Otherwise>
</Choose>
```

위 내용은 다음과 같습니다.

```js
{
  a === 1 ? (
    <span>One</span>
  ) : a === 2 ? (
    <span>Two</span>
  ) : (
    <span>Default</span>
  );
}
```

이러한 라이브러리는 보다 고급 구성 요소를 제공하지만 간단한 if/other와 같은 것이 필요하다면 이 문제에 대한 의견의 Michael J. Ryan과 유사한 솔루션을 사용할 수 있습니다.

```undefined
const If = (props) => {
  const condition = props.condition || false;
  const positive = props.then || null;
  const negative = props.else || null;
  
  return condition ? positive : negative;
};

// …

render () {
    const view = this.state.mode === 'view';
    const editComponent = <EditComponent handleEdit={this.handleEdit}  />;
    const saveComponent = <SaveComponent 
               handleChange={this.handleChange}
               handleSave={this.handleSave}
               text={this.state.inputText}
             />;
    
    return (
      <div>
        <p>Text: {this.state.text}</p>
        <If
          condition={ view }
          then={ editComponent }
          else={ saveComponent }
        />
      </div>
    );
}
```

완벽한 피드는 다음과 같습니다.

## 8. 개체를 열거합니다.

저장/편집 기능이 두 가지 구성 요소로 캡슐화되었으므로 애플리케이션 상태에 따라 열거형 객체를 사용하여 그 중 하나를 렌더링할 수도 있다.

열거형은 상수 값을 그룹화하는 유형입니다. 예를 들어, TypeScript에서 하나를 정의하는 방법은 다음과 같습니다.

```undefined
enum State {
  Save = "Some value",
  Edit = "Another value"
}
```

JavaScript는 기본적으로 열거형을 지원하지 않지만 개체를 사용하여 열거형의 모든 속성을 그룹화하고 실수로 변경되는 것을 방지할 수 있습니다.

```js
const State = Object.freeze({
  Save: "Some value",
  Edit: "Another value"
});
```

그냥 상수만 쓰면 되잖아? 주된 이점은 동적으로 생성된 키를 사용하여 개체의 속성에 액세스할 수 있다는 것입니다.

```cpp
const key = condition ? "Save" : "Edit":
const state = State[key];
```

이를 예제에 적용하면 다음과 같은 두 가지 구성 요소를 사용하여 열거형 개체를 선언하여 저장 및 편집할 수 있습니다.

```xml
const Components = Object.freeze({
  view: <EditComponent handleEdit={this.handleEdit} />,
  edit: <SaveComponent 
          handleChange={this.handleChange}
          handleSave={this.handleSave}
          text={this.state.inputText}
        />
});
```

또한 `모드` 상태 변수를 사용하여 표시할 구성요소를 나타냅니다.

```js
const key = this.state.mode;
return (
  <div>
    <p>Text: {this.state.text}</p>
    {
      Components[key]
    } 
  </div>
);
```

다음 피드에서 전체 코드를 볼 수 있습니다.

https://jsfiddle.net/eh3rrera/7ey56xud/embedded

Enum 개체는 여러 조건을 기준으로 값을 사용하거나 반환하려는 경우에 매우 좋은 옵션이며, 많은 경우 `if/else` 및 `switch` 문을 대체하기에 적합합니다.

## 9. 반응하는 고차 성분(HOC)

고차 구성 요소(HOC)는 기존 구성 요소를 사용하고 몇 가지 기능이 추가된 새 구성 요소를 반환하는 함수입니다.

```cpp
const EnhancedComponent = higherOrderComponent(component);
```

조건부 렌더링에 적용되면 HOC는 다음 조건에 따라 전달된 구성 요소와 다른 구성 요소를 반환할 수 있습니다.

```php
function higherOrderComponent(Component) {
  return function EnhancedComponent(props) {
    if (condition) {
      return <AnotherComponent { ...props } />;
    }

    return <Component { ...props } />;
  };
}
```

로빈 위러치의 HOC에 대한 훌륭한 기사가 있는데, 고차 구성 요소를 사용한 조건부 렌더링에 대해 자세히 알아봅니다. 이 기사를 위해, 저는 `어느 한 가지 구성 요소`의 개념을 빌려보려고 합니다.

함수 프로그래밍에서 둘 중 하나는 일반적으로 두 가지 값을 반환하는 래퍼로 사용된다. 먼저 두 개의 인수를 사용하는 함수, 즉 부울 값(조건 평가 결과)을 반환하는 다른 함수 및 해당 값이 `true`인 경우 반환되는 구성 요소를 정의하는 것으로 시작하겠습니다.

```js
function withEither(conditionalRenderingFn, EitherComponent) {

}
```

그것은 "with"라는 단어로 HOC의 이름을 시작하는 협약이다. 이 기능은 원래 구성 요소가 새 구성 요소를 반환하는 데 필요한 다른 기능을 반환합니다.

```js
function withEither(conditionalRenderingFn, EitherComponent) {
    return function buildNewComponent(Component) {

    }
}
```

이 내부 기능에 의해 반환되는 구성 요소(기능)는 앱에서 사용할 구성 요소가 되므로 작업에 필요한 모든 속성을 가진 개체가 필요합니다.

```js
function withEither(conditionalRenderingFn, EitherComponent) {
    return function buildNewComponent(Component) {
        return function FinalComponent(props) {

        }
    }
}
```

내부 함수는 외부 함수의 매개변수에 액세스할 수 있으므로, 이제 `조건부 렌더링 Fn` 함수에 의해 반환되는 값을 기준으로 `구성 요소` 또는 원래 `구성 요소`를 반환합니다.

```js
function withEither(conditionalRenderingFn, EitherComponent) {
    return function buildNewComponent(Component) {
        return function FinalComponent(props) {
            return conditionalRenderingFn(props)
                ? <EitherComponent { ...props } />
                 : <Component { ...props } />;
        }
    }
}
```

또는 화살표 기능 사용:

```js
const withEither = (conditionalRenderingFn, EitherComponent) => (Component) => (props) =>
  conditionalRenderingFn(props)
    ? <EitherComponent { ...props } />
    : <Component { ...props } />;
```

이렇게 하면 이전에 정의한 `SaveComponent` 및 `EditConditionalRendering`을 사용하여 `EditConditionalRendering` HOC를 생성하고 `EditSaveWithConditionalRendering` 구성 요소를 생성할 수 있습니다.

```js
const isViewConditionFn = (props) => props.mode === 'view';

const withEditContionalRendering = withEither(isViewConditionFn, EditComponent);
const EditSaveWithConditionalRendering = withEditContionalRendering(SaveComponent);
```

이제 필요한 모든 속성을 전달하여 `렌더` 방법으로 사용할 수 있습니다.

```js
render () {    
    return (
      <div>
        <p>Text: {this.state.text}</p>
        <EditSaveWithConditionalRendering 
               mode={this.state.mode}
               handleEdit={this.handleEdit}
               handleChange={this.handleChange}
               handleSave={this.handleSave}
               text={this.state.inputText}
             />
      </div>
    );
}
```

완벽한 피드는 다음과 같습니다.

## 반응에서의 조건부 렌더링: 성능 고려 사항

조건부 렌더링이 까다로울 수 있습니다. 앞서 보여드린 것처럼 옵션별 성능이 다를 수 있습니다. 하지만, 대부분의 경우, 차이는 별로 중요하지 않습니다.

하지만 Retact가 가상 DOM과 어떻게 작동하는지 잘 이해하고 성능을 최적화하는 몇 가지 요령이 필요합니다. React의 조건부 렌더링 최적화에 대한 좋은 기사가 있습니다. 읽어보시기 바랍니다.

기본 아이디어는 조건부 렌더링으로 인해 구성 요소의 위치를 변경하면 앱의 구성 요소를 마운트 해제/마운트할 수 있는 리플로우가 발생할 수 있다는 것입니다. 기사의 예를 들어 JS Fiddle 두 개를 만들었습니다.

첫 번째 블록은 if/else 블록을 사용하여 `SubHeader` 구성 요소를 표시하거나 숨깁니다.

두 번째 것은 단락 연산자를 사용합니다(`).

Inspector를 열고 버튼을 몇 번 클릭합니다. 각 구현에 따라 `콘텐츠` 구성 요소가 다르게 처리되는 방식을 볼 수 있습니다.

if/else 블록이 구성 요소를 처리하는 방법은 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2018/02/if-else-block-content-component.gif?resize=720%2C342&ssl=1)

단락 작업자는 다음과 같은 작업을 수행합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/02/short-circuit-operator-content-component.gif?resize=720%2C342&ssl=1)

## 조각이 있는 조건부 렌더링

특정 조건에 따라 여러 자식 구성 요소를 렌더링하는 방법은 무엇입니까? 답은 조각을 사용하는 것입니다.

조각을 사용하면 DOM(문서 객체 모델)에 노드를 추가하지 않고도 여러 요소를 그룹화하여 반환할 수 있습니다.

기존 구문과 함께 조각을 사용할 수 있습니다.

```js
return (
  <React.Fragment>
    <Button />
    <Button />
    <Button />
  </React.Fragment>
);
```

또는 짧은 구문을 사용하여 다음을 수행합니다.

```js
return (
  <>
    <Button />
    <Button />
    <Button />
  </>
);
```

단, 조건에 따라 여러 요소를 조각으로 렌더링하는 경우에는 이 문서에 설명된 기술을 모두 사용할 수 있습니다.

예를 들어 다음과 같은 방법으로 터미널 연산자를 사용할 수 있습니다.

```js
{ 
  view
  ? null
  : (
    <React.Fragment>
      <Button />
      <Button />
      <Button />
    </React.Fragment>
  )
}
```

차라리 단락 회로라도 쓸 수 있을 거야

```js
{ 
  condition &&
  <React.Fragment>
    <Button />
    <Button />
    <Button />
  </React.Fragment>
}
```

하위 요소의 렌더링을 메소드에 캡슐화하고 `if` 또는 `switch` 문을 사용하여 반환할 항목을 결정할 수도 있습니다.

```js
render() {
  return <div>{ this.renderChildren() }</div>;
}

renderChildren() {
  if (this.state.children.length === 0) {
    return <p>Nothing to show</p>;
  } else {
    return (
      <React.Fragment>
        {this.state.children.map(child => (
          <p>{child}</p>
        ))}
      </React.Fragment>
    );
 }
}
```

## 반응 후크를 사용한 조건부 렌더링

오늘날, 대부분의 경험이 풍부한 반응 개발자들은 후크를 사용하여 구성 요소를 작성합니다. 그래서 이런 수업을 하는 대신에:

```js
import React, { Component } from 'react';

class Doubler extends Component {
  constructor(props) {
    super(props);

    this.state = {
      num: 1,
    };
  }

  render() {
    return (
      <div>
        <p>{this.state.num}</p>
        <button onClick={() =>
            this.setState({ num: this.state.num * 2 })
        }>
          Double
        </button>
      </div>
    );
  }
}
```

`useState` 후크를 사용하여 다음 함수로 구성 요소를 쓸 수 있습니다.

```js
import React from 'react';

function Doubler() {
  const [num, setNum] = React.useState(1);

  return (
    <div>
      <p>{num}</p>
      <button onClick={() => setNum(num * 2)}>
        Double
      </button>
    </div>
  );
}
```

조각과 마찬가지로 이 문서에서 설명하는 기술을 사용하여 후크를 사용하는 구성 요소를 조건부로 렌더링할 수 있습니다.

```js
function Doubler() {
  const [num, setNum] = React.useState(1);
  const showButton = num <= 8;
  const button = <button onClick={() => setNum(num * 2)}>Double</button>;

  return (
    <div>
      <p>{num}</p>
      {showButton && button}
    </div>
  );
}
```

유일한 주의사항은 조건부로 후크를 호출할 수 없기 때문에 항상 실행되지는 않는다는 것입니다. 후크 설명서에 따라:

> 루프, 조건 또는 중첩 함수 내부에서는 후크를 호출하지 마십시오. 대신 항상 반응 함수의 최상위 수준에 있는 후크를 사용하십시오. 이 규칙을 따르면 구성 요소가 렌더링될 때마다 동일한 순서로 후크가 호출됩니다. 그것이 리액션이 여러 개의 useState와 useEffect 통화 사이에 훅스 상태를 올바르게 보존할 수 있게 하는 것이다.

일반적으로 사용 효과 후크에서 발생합니다. 구성 요소가 렌더링될 때마다 다음과 같이 후크가 호출되지 않도록 할 수 있는 조건을 지정할 수 없습니다.

```coffeescript
if (shouldExecute) {
  useEffect(() => {
    // ...
  }
}
```

후크 안에 조건을 넣어야 합니다.

```coffeescript
useEffect(() => {
  if (shouldExecute) {
    // ...
  }
}, [shouldExecute])
```

## React에서 조건부 렌더링을 구현하는 가장 좋은 방법은 무엇입니까?

프로그래밍에서와 마찬가지로 React에서도 조건부 렌더링을 구현하는 방법은 여러 가지가 있습니다. 첫 번째 방법(반품이 많은 경우/다른 방법)을 제외하고 원하는 방법을 자유롭게 선택할 수 있습니다.

다음을 기준으로 상황에 가장 적합한 항목을 결정할 수 있습니다.

- 프로그램 스타일
- 조건부 논리가 얼마나 복잡한지
- JavaScript, JSX 및 고급 React 개념(예: HOC)에 얼마나 익숙하십니까?

그리고 모든 것이 평등하기 때문에, 항상 단순함과 가독성을 선호합니다.