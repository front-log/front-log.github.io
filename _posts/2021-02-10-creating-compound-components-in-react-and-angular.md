---
layout: post
title: "반응 및 각도에서 복합 성분 생성"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/02/compound-components-react-angular.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/compound-components-react-angular.png?fit=730%2C487&ssl=1)

구성 요소가 프런트 엔드 사용자 인터페이스(그리고 알고 보니 텍스트 기반 UI, 애플리케이션 라우터 및 기타 많은 유형의 인터페이스)를 작성하기 위한 강력한 추상화라는 것에는 의심의 여지가 없습니다.

그러나 저작 구성요소에 대한 다양한 접근 방식이 있습니다. 이 게시물에서는 컴포넌트 패턴과 컴포넌트 작성자에게 적합한 선택 시기에 대해 설명합니다.

## 복합 구성 요소는 무엇입니까?

복합 구성 요소는 응집력 있고 복잡한 동작을 만들기 위해 함께 모이는 개별 구성 요소의 그룹입니다. 이들은 함께 작동하며 배후 상태를 공유하여 구성요소를 사용하는 프로그래머에게 직관적이고 표현적인 구문을 제공합니다. 복합 구성 요소의 가장 친숙한 예 중 하나는 바닐라 HTML입니다.

```xml
<select>
  <option value="apples">Apples</option>
  <option value="oranges">Oranges</option>
  <option value="pears">Pears</option>
</select>
```

이 예에서, `select`와 `option` 요소는 많은 사용자에게 친숙한 복잡한 입력 제어를 형성하기 위해 결합된다. `<select>`는 사용자가 현재 선택한 `option`의 자체 내부 상태를 유지하고 키보드 입력과 같은 고급 동작을 제공하여 선택한 `option`을 변경합니다.

## 복합 구성요소 구현 방법

먼저 복합 구성요소가 적절한 API 설계인지 확인합니다. 다른 프로그래밍 패러다임과 마찬가지로, 복합 요소들은 그들의 공정한 트레이드오프 몫을 가지고 온다. 일반적으로 암묵적 공유 상태 또는 "마법적" 또는 쉽게 발견할 수 없는 행동이 있는 모든 패턴은 최대한 주의를 기울여 설계하지 않을 경우 향후 골칫거리를 초래할 수 있습니다.

복합 구성 요소를 설계하기 전에 자문해야 할 몇 가지 질문은 다음과 같습니다.

- 구성 요소를 사용하는 프로그래머가 원하는 결과를 얻기 위해 두 개 이상의 구성 요소를 함께 구성해야 합니까? 또는 보다 단순한 단일 구성요소 설계와 적절한 입력 또는 소품으로 동일한 결과를 얻을 수 있는가?
- 구성 요소 간의 상호 작용과 역할이 사용 중인 프로그래머에게 명확하고 직관적인가?

새로운 구성 요소에 대한 다양한 API를 신중하게 고려하고 어떤 API를 사용하길 원하는지 동료와 상의하십시오.

다음은 복합 구성요소 패턴에 적합한 공통 UI 패러다임의 몇 가지 예입니다.

- 구성 요소 사용자가 테이블 데이터뿐만 아니라 사용자 정의 정렬 또는 필터링 동작도 제공할 수 있는 고급 테이블 구성 요소
- 구성 요소의 사용자가 옵션을 제공하는 검색 가능한 드롭다운 구성 요소

## React의 복합 구성요소 아키텍처

React에서 예제 복합 구성 요소를 구현하기 위해 여러 React API를 활용할 예정이므로 각 구성 요소에 대한 기초 지식이 도움이 될 것입니다.

- 후크 및 기능 구성 요소
- 구성 요소 간 상태 공유를 위한 컨텍스트 API

### 복합 구성요소 작성

사용자가 원하는 경우 텍스트 필드를 사용하여 목록에서 여러 옵션을 선택할 수 있는 복합 구성요소의 대략적인 초안을 작성합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/selecting-items-from-list.gif?resize=472%2C476&ssl=1)

### 하위 구성 요소 및 'App' 구성 요소 구현

복합 구성 요소를 생성하기 위해 다음 두 개의 하위 구성 요소를 구현합니다.

- `Enhanced MultiSelect`: 이 구성 요소는 외측 래핑 구성 요소로, 다음 역할을 수행합니다.
하위 옵션의 상태 캡슐화
필터링 옵션에 대한 텍스트 필드 렌더링
- 하위 옵션의 상태 캡슐화
- 필터링 옵션에 대한 텍스트 필드 렌더링
- `향상된 다중 선택 옵션`: 이 구성 요소는 선택할 수 있는 개별 옵션을 표현하며 다음과 같은 역할을 수행합니다.
사용자 상호 작용에 따라 선택 상태에서 읽기 및 쓰기
필터 값을 읽고 해당하는 경우 렌더링에서 제외
- 사용자 상호 작용에 따라 선택 상태에서 읽기 및 쓰기
- 필터 값을 읽고 해당하는 경우 렌더링에서 제외

마지막으로, 복합 컴포넌트를 사용하는 `앱` 컴포넌트도 구현하여 API를 개발하고 테스트할 것이다.

"향상된 다중 선택"

먼저 상위 구성 요소와 하위 구성 요소 간의 상태를 공유하는 데 사용할 컨텍스트를 만듭니다.

```cpp
export const EnhancedMultiSelectContext = createContext();
```

다음으로, 구성 요소에 대한 서명을 구현합니다. 여기에는 세 가지 소품이 필요합니다.

- `children`: UI에서 요구하는 선택 가능한 옵션과 기타 마크업을 포함하는 렌더 트리의 하위 항목입니다. 옵션이 얼마나 깊이 중첩되어 나타나는지 또는 트리에 다른 구성 요소가 있는지 여부에 상관 없이 구성 요소를 사용하는 엔지니어가 유연하게 사용할 수 있습니다.
- `value`: 선택한 옵션을 나타내는 문자열의 집합
- On Change(변경 시) : 선택이 바뀔 때마다 새로운 Set(설정)으로 호출하는 기능

```js
export default function EnhancedMultiSelect({ children, value, onChange }) {}
```

이제 구성 요소의 본문을 구현하겠습니다. 먼저 사용자가 필터 텍스트 입력에 입력한 쿼리를 추적하기 위해 `useState` 후크를 사용합니다.

```undefined
const [filter, setFilter] = useState('');
```

그리고 Return에서 렌더링할 구성 요소를 반환하겠습니다. 먼저 앞에서 설정한 컨텍스트에 대한 공급자를 설정하고 나중에 옵션에서 사용할 몇 가지 값을 제공하는 데 사용합니다.

- 문자열 키를 가져와서 선택한 키에 지정된 키가 표시되는지 여부를 반환하는 `isSelected` 함수
- 그림과 같이 키를 가져와서 선택 항목에서 키를 추가하거나 제거하는 `Set Selected` 함수
- 필터 텍스트 입력의 현재 값

또한 필터 텍스트 입력과 컨텍스트 공급자 내부의 구성 요소도 렌더링합니다. 다음은 `Enhanced MultiSelect`의 전체 소스 코드입니다.

```undefined
import { createContext, useState } from 'react';

export const EnhancedMultiSelectContext = createContext();

export default function EnhancedMultiSelect({ children, value, onChange }) {
  const [filter, setFilter] = useState('');
  return (
    <EnhancedMultiSelectContext.Provider
      value={
        isSelected: key => value.has(key),
        setSelected: (key, selected) => {
          const newValue = new Set([...value]);
          if (selected) {
            newValue.add(key);
          } else {
            newValue.delete(key);
          }
          onChange(newValue);
        },
        filter,
      }
    >
      <input
        type="text"
        placeholder="Filter options..."
        value={filter}
        onChange={evt => setFilter(evt.target.value)}
      />
      {children}
    </EnhancedMultiSelectContext.Provider>
  );
}
```

"향상된 다중 선택 옵션"

이제 나머지 절반의 컴파운드 구성 요소를 구현합니다. 두 가지 요소가 필요합니다.

- `children`: 컴포넌트 사용자가 선택 가능한 옵션 내에서 렌더링하려는 항목을 표시합니다.
- `value`: 이 옵션을 나타내는 문자열. 이 옵션을 선택하면 상위 `Enhanced MultiSelect` 구성 요소에 의해 표시되는 `Set`에 이 값이 포함됩니다.

```js
export default function EnhancedMultiSelectOption({ children, value }) {}
```

구성 요소의 본문에서 가장 먼저 수행할 일은 상위 `Enhanced MultiSelect` 구성 요소가 제공하는 컨텍스트를 사용하는 것이며, 보다 쉬운 사용을 위해 컨텍스트를 분리하기 위해 구조화 할당을 사용합니다.

```cpp
const { isSelected, setSelected, filter } = useContext(
  EnhancedMultiSelectContext,
);
```

이제 컨텍스트에서 사용자의 필터 쿼리가 제공되었으므로 옵션 값과 일치하지 않으면 null을 반환하여 아무 것도 렌더링하지 않습니다.

```undefined
if (!value.includes(filter)) {
  return null;
}
```

마지막으로, 체크박스를 렌더링하고 컴포넌트의 선택 상태와 컴포넌트의 소비자가 렌더링하려는 모든 어린이를 연결합니다. 다음은 `향상된 다중 선택 옵션`의 전체 소스 코드입니다.

```js
import { useContext } from 'react';
import { EnhancedMultiSelectContext } from './EnhancedMultiSelect';

export default function EnhancedMultiSelectOption({ children, value }) {
  const { isSelected, setSelected, filter } = useContext(
    EnhancedMultiSelectContext,
  );
  if (!value.includes(filter)) {
    return null;
  }
  return (
    <label style={ display: 'block' }>
      <input
        type="checkbox"
        checked={isSelected(value)}
        onChange={evt => setSelected(value, evt.target.checked)}
      />
      {children}
    </label>
  );
}
```

앱

이 모든 것이 어떻게 작동하는지 알아보기 위해 복합 구성 요소를 사용하고 진입점 `App` 구성 요소로 렌더링합니다.

```js
import { useState } from 'react';
import EnhancedMultiSelect from './EnhancedMultiSelect';
import EnhancedMultiSelectOption from './EnhancedMultiSelectOption';

export default function App() {
  const [selection, setSelection] = useState(new Set());
  return (
    <section>
      <EnhancedMultiSelect value={selection} onChange={v => setSelection(v)}>
        <EnhancedMultiSelectOption value="apples">
          Apples
        </EnhancedMultiSelectOption>
        <EnhancedMultiSelectOption value="oranges">
          Oranges
        </EnhancedMultiSelectOption>
        <EnhancedMultiSelectOption value="peaches">
          Peaches
        </EnhancedMultiSelectOption>
        <EnhancedMultiSelectOption value="grapes">
          Grapes
        </EnhancedMultiSelectOption>
        <EnhancedMultiSelectOption value="plums">
          Plums
        </EnhancedMultiSelectOption>
      </EnhancedMultiSelect>
      <pre>
        <code>{JSON.stringify([...selection], null, 2)}</code>
      </pre>
    </section>
  );
}
```

## Angular의 복합요소 아키텍처

Angular 프레임워크를 사용하여 동일한 단순 복합 구성요소를 구축하겠습니다.

`component-multi-select.component.ts`

외부 구성 요소의 경우 `필터` 속성에 대한 양방향 바인딩이 있는 텍스트 입력을 포함하는 단순 템플릿을 설정합니다. 반응 예제와 마찬가지로 선택 상태가 변경될 때 선택 상태와 출력에 대한 입력을 생성합니다. 전체 소스 코드는 다음과 같습니다.

```coffeescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-enhanced-multi-select',
  template: `
    <input type="text" [(ngModel)]="filter" />
    <ng-content></ng-content>
  `,
})
export class EnhancedMultiSelectComponent {
  @Input()
  value!: Set<string>;

  @Output()
  valueChange = new EventEmitter<Set<string>>();

  filter = '';
}
```

`multi-select-option.component.ts`

옵션 항목의 경우 반응 예제와 같이 확인란과 구성 요소의 내용을 감싸는 레이블을 렌더링합니다. Angular의 종속성 주입 시스템을 사용하여 `compilator`를 통해 전달된 상위 `Enhanced MultiSelect Component` 인스턴스를 참조할 수 있습니다.

그 참조를 통해 상태를 직접 평가하고 조작할 수 있으며 사용자가 제공한 필터 문자열 값에 따라 옵션이 표시되어야 하는지 확인할 수 있다. 소스 코드는 다음과 같습니다.

```undefined
import { Component, Input } from '@angular/core';
import { EnhancedMultiSelectComponent } from './enhanced-multi-select.component';

@Component({
  selector: 'app-enhanced-multi-select-option',
  template: `
    <label *ngIf="visible()" style="display: block">
      <input
        type="checkbox"
        [ngModel]="selected()"
        (ngModelChange)="setSelected($event)"
      />
      <ng-content></ng-content>
    </label>
  `,
})
export class EnhancedMultiSelectOptionComponent {
  constructor(private readonly select: EnhancedMultiSelectComponent) {}

  visible() {
    return this.value.includes(this.select.filter);
  }

  selected() {
    return this.select.value.has(this.value);
  }

  setSelected(selected: boolean) {
    if (selected) {
      this.select.value.add(this.value);
    } else {
      this.select.value.delete(this.value);
    }
  }

  @Input()
  value!: string;
}
```

`app.component.ts`

마지막으로, 복합 구성 요소를 활용하여 포맷된 JSON 선택 데이터를 시연용으로 표시합니다.

```undefined
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <app-enhanced-multi-select [(value)]="selection">
      <app-enhanced-multi-select-option value="apples">
        Apples
      </app-enhanced-multi-select-option>
      <app-enhanced-multi-select-option value="oranges">
        Oranges
      </app-enhanced-multi-select-option>
      <app-enhanced-multi-select-option value="peaches">
        Peaches
      </app-enhanced-multi-select-option>
      <app-enhanced-multi-select-option value="grapes">
        Grapes
      </app-enhanced-multi-select-option>
      <app-enhanced-multi-select-option value="plums">
        Plums
      </app-enhanced-multi-select-option>
    </app-enhanced-multi-select>
    <pre><code>{ selectionArray() | json }</code></pre>
  `,
})
export class AppComponent {
  selection = new Set<string>();

  selectionArray() {
    return [...this.selection];
  }
}
```

이 게시물에서는 React에서 Context API를 사용하여 필터링 가능한 다중 선택 복합 구성 요소를 구현했고 Angular에서는 종속성 주입을 사용하여 구현했습니다.

복합 구성요소는 단일 구성요소에 대해 너무 복잡한 동작을 구성하기 위해 간단한 API를 만드는 하나의 선택사항입니다. React에는 "렌더 소품"과 같은 대체 패턴이 많이 있으며, 특정 사용 사례에 대해서는 각 패턴의 트레이드오프를 신중하게 고려해야 한다.

개발 환경에서 위의 예를 실행하기 위한 전체 소스 코드는 GitHub에서 찾을 수 있다.