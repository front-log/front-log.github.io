---
layout: post
title: "Rakeit을 통해 액세스 가능한 구성 요소"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/accessiblecomponentswithreakit.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/accessiblecomponentswithreakit.png?fit=730%2C412&ssl=1)

반응 애플리케이션에서 가장 일반적인 것은 구성 요소의 재사용 가능성입니다. 우리는 애플리케이션의 여러 부분에서 동일한 구성 요소를 사용하고 재사용해 왔습니다. 이는 React 애플리케이션에서 우리가 가지고 있는 가장 환상적인 기능 중 하나입니다.

재사용 가능성을 염두에 두고, 우리는 놀라운 것들을 만들 수 있고, 우리는 같은 기준과 원칙을 따르는 다른 프로젝트에서 서로 다른 팀들이 사용할 수 있는 전체적인 디자인 시스템을 만들 수 있습니다. 그 결과, 디자인 시스템에 특정 구성 요소가 있다면 처음부터 특정 구성 요소를 만들 필요가 없기 때문에 생산성이 증가할 것이라는 것을 알 수 있습니다.

React는 커뮤니티에서 웹 애플리케이션을 만드는 데 가장 많이 사용되는 자바스크립트 라이브러리로 출시되고 채택된 이후 디자인 시스템, 컴포넌트 라이브러리, UI 라이브러리의 수가 증가했음을 알 수 있다. React 애플리케이션을 구축할 수 있는 다양한 옵션이 있으며 이를 위해 서로 다른 설계 시스템이나 구성 요소 라이브러리를 선택할 수 있습니다.

Reakit은 이러한 라이브러리 중 하나이지만, 접근성이라는 중요한 개념을 염두에 두고 있습니다. 이제 그 어느 때보다 접근성이 중요한 주제이며 모든 개발자가 자신의 애플리케이션을 사용할 수 있기를 원하는 모든 개발자가 우선적으로 처리해야 합니다.

Reakit 구성 요소 라이브러리와 이를 특별하게 만드는 이유에 대해 알아보기 전에 먼저 액세스 용이성과 최신 웹에서 중요한 이유에 대해 알아보겠습니다.

## WCAG(Web Content Accessibility Guideline)

접근성은 많은 회사와 개발자들에 의해 최우선 과제로 다루어지지는 않았지만, 모든 사람들이 더 쉽게 접근할 수 있는 애플리케이션을 만들기 위해 현대 애플리케이션에서는 중요한 역할을 한다.

W3C는 WCAG(Web Content Accessibility Guideline)라는 표준 가이드라인을 만들 정도로 웹에 대한 접근성은 매우 중요합니다. 이는 웹 콘텐츠 접근성을 위한 표준과 원칙의 집합으로, 서로 다른 사람들에게 더 접근하기 쉬운 애플리케이션을 구축하고 제공합니다.

세계은행에 따르면 접근성의 중요성을 인식하기 위해:

> 세계 인구의 15%에 해당하는 10억 명이 어떤 형태의 장애를 경험하고 있으며, 개발도상국의 경우 장애 유병률이 더 높다. 1억 1천만 명에서 1억 9천만 명으로 추산되는 전 세계 인구의 5분의 1이 심각한 장애를 겪고 있습니다. 장애인은 교육을 덜 받고, 건강이 나빠지고, 고용 수준이 낮아지고, 빈곤율이 높아지는 등 장애가 없는 사람보다 불리한 사회경제적 결과를 경험할 가능성이 높다.

모든 사람이 문제 없이 액세스할 수 있는 응용프로그램을 제공하는 것은 매우 중요합니다. Reakit은 React에서 보다 접근성, 구성성 및 빠른 애플리케이션을 만들 수 있도록 지원합니다.

## 레이킷

Reakit은 보다 쉽게 액세스할 수 있는 React 구성 요소, 라이브러리, 설계 시스템 및 응용 프로그램을 만드는 데 도움이 되는 낮은 수준의 구성 요소 라이브러리입니다. Diego Haz에 의해 만들어진 Reakit은 MIT 라이선스로 출시되었고 보다 접근하기 쉬운 React 애플리케이션을 구축하고자 하는 더 많은 채택자를 얻고 있습니다.

Reakit은 WAI-ARIA 표준을 따르는 접근성, 구성성 및 사용자 정의가 가능한 다양한 구성요소를 제공합니다. 버튼, 체크박스, 입력 등 많은 응용 프로그램에서 많이 사용되는 접근 가능한 구성 요소를 많이 가질 수 있다는 뜻입니다.

Reakit의 가장 좋은 점 중 하나는 이미 즉시 키보드 통합에 집중할 수 있기 때문에 구성 요소에 통합할 필요가 없다는 것입니다. 또한 기본 CSS 스타일이 없으므로 CSS를 직접 가져와 원하는 CSS 솔루션을 사용할 수 있습니다.

## 접근성

우리는 접근성이 매우 중요하다는 것을 알고 있으며, 완전히 접근 가능한 구성요소를 가진 구성요소 라이브러리와 함께 작업하면 응용프로그램에서 큰 변화를 일으킬 수 있다는 것을 알고 있습니다.

Reakit은 WAI-ARIA 표준을 엄격히 준수하며, 이는 모든 구성요소가 접근성을 염두에 두고 설계 및 개발되어 실제 접근 가능한 구성요소를 제공하고 사용자 경험을 향상시킨다는 것을 의미한다.

Reakit은 포커스 및 키보드 통합 기능도 제공합니다(예:

- Enter 키를 누를 때 버튼은 응답해야 합니다.
- 키보드의 화살표 키만으로 `탭` 구성 요소를 쉽게 탐색할 수 있습니다.

## 합성 가능

React는 애플리케이션의 여러 부분에 있는 구성요소를 쉽게 재사용할 수 있도록 해 주기 때문에 다양한 구성요소와 함께 작업할 수 있는 매우 좋은 솔루션입니다.

Reakit은 다양한 구성 요소를 쉽게 구축할 수 있도록 하기 위한 구성을 염두에 두고 있습니다. 우리는 `as` 받침대를 사용하여 구성요소를 구성하고 Reakit 구성요소의 기본 요소를 변경할 수 있다.

우리가 `라디오` 구성 요소를 가지고 있고 이 구성 요소를 `버튼`으로 구성하고자 한다고 가정해 보자면, `버튼`처럼 `as` 소품만 건네도 쉽게 구성할 수 있다.

```js
import { useRadioState, Radio, RadioGroup, Button } from "reakit";

const App = () => {
  const radio = useRadioState();
  return (
    <div>
      <h1>App</h1>
      <RadioGroup {...radio} aria-label="cars" as={Button}>
        <label>
          <Radio {...radio} value="tesla" /> Tesla
        </label>
      </RadioGroup>
    </div>
  );
}

export default App;
```

## 사용자 지정 가능

Reakit은 기본 CSS와 함께 제공되지 않으므로 구성 요소를 매우 사용자 지정할 수 있고 스타일링하기 쉽습니다.

Reakit에서 간단한 `Button`을 가져오면 기본 CSS가 없습니다.

```js
import { Button } from "reakit";

const MyButton = () => (
  <Button>Reakit Button</Button>
);

export default MyButton;
```

우리는 우리가 원하는 CSS 솔루션(예: CSS-in-JS 라이브러리)과 매우 쉽게 통합할 수 있다.

```js
import styled from 'styled-components';
import { Button } from "reakit";

const StyledButton = styled(Button)`
  width: 100px;
  height: 30px;
  background: turquoise;
  border-radius: 5px;
  color: white;
`;

const MyButton = () => (
  <StyledButton>Reakit Button</StyledButton>
);

export default MyButton;
```

## 크기

번들 크기에 관한 한 Reakit은 매우 멋진 번들 크기를 가지고 있으며 현재 사용 가능한 다른 React 구성 요소 라이브러리에 비해 용량이 큰 라이브러리는 아닙니다.

레이킷은 31kB, 부품별 평균 크기는 1kB다. 재료 UI와 Ant와 같은 다른 구성요소 라이브러리에 비해 Reakit은 매우 가벼운 솔루션입니다.

## 시작 중

이제 레이킷이 가지고 있는 특징에 대해 알았으니, 처음부터 다시 만들어 봅시다. 우리는 단지 몇 개의 접근 가능한 부품으로 작업관리 앱을 만들 것입니다.

시작하기 전에 react와 react-dom이 설치되어 있는지 확인해야 합니다.

```undefined
yarn add react react-dom
```

이제 리킷을 설치할 수 있습니다.

```undefined
yarn add reakit
```

우리는 리킷에서 입력과 버튼이라는 두 가지 부품을 수입할 예정입니다. 이 예에서는 이 두 가지 구성 요소만 있으면 되지만 실제 애플리케이션에서는 결과를 얻을 수 있도록 다양한 구성 요소를 제공합니다.

새 `react-app` 응용 프로그램을 만들어 보겠습니다.

```undefined
npx create-react-app reakit-example --template typescript
```

앱 내에서는 레이킷을 이용해 할 수 있는 앱을 만들 예정이다. 입력과 버튼을 모두 가져와 상태 논리를 만들어 보겠습니다.

```js
import React, { useState } from 'react';
import { Input, Button } from "reakit";

const App = () => {
  const [tasks, setTasks] = useState([]);
  const [text, setText] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!text) return;
    addTask(text);
    setText("");
  };

  const addTask = (text) => {
    const newTasks = [...tasks, { text }];
    setTasks(newTasks);
  };

  const deleteTask = (index) => {
    const newTasks = [...tasks];
    newTasks.splice(index, 1);
    setTasks(newTasks);
  };
};

export default App;
```

Reakit에는 Form, FormLabel, FormInput과 같은 실험 모드의 구성 요소가 있다. 이 예에서는 이러한 구성 요소를 사용하지 않을 것입니다. 이러한 구성 요소는 변경 사항을 발생시키거나 향후 버전에서 제거될 수도 있기 때문입니다.

이제 우리는 리킷의 입력과 버튼을 모두 사용할 것이다. 우리의 작업관리 앱은 다음과 같습니다.

```js
import React, { useState } from 'react';
import { Input, Button } from "reakit";

const App = () => {
  const [tasks, setTasks] = useState([]);
  const [text, setText] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!text) return;
    addTask(text);
    setText("");
  };

  const addTask = (text) => {
    const newTasks = [...tasks, { text }];
    setTasks(newTasks);
  };

  const deleteTask = (index) => {
    const newTasks = [...tasks];
    newTasks.splice(index, 1);
    setTasks(newTasks);
  };

  return (
    <form onSubmit={handleSubmit}>
    <Input
      placeholder="Add a task"
      value={text}
      onChange={e => setText(e.target.value)}
    />

    <Button onClick={handleSubmit}>Add</Button>
    {tasks.map((task: any, index: number) => (
      <div key={index} onClick={() => deleteTask(index)}>
        <h1>{task.text}</h1>
      </div>
    ))}
   </form>
  )
};
export default App;
```

Reakit의 좋은 특징은 예를 들어 버튼 구성 요소를 사용할 때 이를 사용하지 않는 것으로 전달하고자 할 때 `아리아 비활성화`가 이미 `true`로 설정된다는 것입니다.

특히 새로운 디자인 시스템을 만들고 싶어하고 접근 가능한 구성 요소를 만들고 싶어하는 사람들에게 Reakit은 매우 좋은 옵션입니다. 일부 구성 요소의 경우 Rakeit을 사용하여 매우 멋지고 견고한 설계 시스템을 만들 수 있으며, 특히 접근성과 함께 우수한 결과를 얻을 수 있습니다.

## 결론

액세스 가능한 애플리케이션을 구축하는 것은 쉬운 일이 아니며 힘든 작업이 요구됩니다. 오늘날 우리는 좋은 결과를 얻고 모두에게 액세스 가능한 애플리케이션을 제공할 수 있는 몇 가지 구성 요소 라이브러리를 보유하고 있습니다. Reakit은 기본 CSS 없이 접근성을 염두에 둔 다양한 구성 요소를 제공함으로써 이를 지원할 수 있는 구성 요소 라이브러리이다.