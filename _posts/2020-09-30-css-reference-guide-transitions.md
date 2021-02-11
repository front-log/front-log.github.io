---
layout: post
title: "CSS 참조 가이드: 전환"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-transitions-nocdn.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-transitions-nocdn.png?fit=730%2C487&ssl=1)

CSS `transition` 속성은 CSS에서 효과를 애니메이션할 수 있는 속기 속성이다. `transition`은 일정 기간 동안 CSS 속성을 원래 값에서 새 값으로 점차 변경한다.

#### 앞으로 이동:

- 구문
- 다중 전환
- 구성 요소 속성
`전환재산
과도현상
`전환기-기능`
과도현상
- `전환재산
- 과도현상
- `전환기-기능`
- 과도현상

CSS의 속성 변화는 갑작스러운 깜빡임과 같은 순간적인 것이다; 인간의 눈에는, 그것들은 기본적으로 알아차릴 수 없을 것이다. `전환`은 변화를 늦춰서 인간의 눈에 잘 띄는 방식으로 일정 기간에 걸쳐 점진적으로 일어날 수 있도록 한다.

예:

```undefined
div.banner {
    width: 100px;
    padding: 10px;
    background-color: seagreen;
    border: 1px solid deepskyblue;

    transition: width 2s;
}

div.banner:hover {
    width: 150px;
}
```

다음 예제를 사용할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/QWNxezz?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=philipsz-davido&amp;slug-hash=QWNxezz&amp;pen-title=css%20transitions&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css transitions" loading="lazy" id="cp_embed_QWNxezz"></iframe></div>

위에서, 우리는 그 요소의 폭을 150px로 늘렸다. div.배너에 설정된 전환 속성은 길이 증가를 2초 동안 느리게 렌더링합니다.

요소의 너비가 150px로 확대되는 것을 보게 될 것이고, 텍스트는 폭 증가를 보상하기 위해 천천히 다시 정렬된다. 마우스를 `div.배너` 요소에서 벗어나면 다시 느린 유체 동작으로 폭이 100px로 바뀌고, 텍스트 랩핑이 원래 상태로 되돌아간다.

다음 예제를 살펴보겠습니다.

```undefined
div.banner {
    width: 100px;
    padding: 10px;
    background-color: seagreen;
    border: 1px solid;

    transition: background-color 4s ease-in 1s;
}

div.banner:hover {
    background-color: limegreen;
}
```

여기서 위의 예제를 사용할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="350" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/philipsz-davido/embed/ExKGmQJ?height=350&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=philipsz-davido&amp;slug-hash=ExKGmQJ&amp;pen-title=css_transition_background_color&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="css_transition_background_color" loading="lazy" id="cp_embed_ExKGmQJ"></iframe></div>

여기서 사용자가 div.배너 요소를 맴돌면 전환이 1초 지연되고, 이어 seagreen의 배경색이 4초 동안 laimgreen으로 바뀐다. 우리는 점점 희미해지는 효과와 점차 희미해지는 `해녹색`과 점차 그 자리를 차지하게 되는 `라임그린`을 볼 수 있다.

우리는 전환을 트리거하기 위해 `:hover` 의사 클래스를 사용했습니다. 다른 수단을 사용하여 전환을 트리거할 수도 있습니다. 예를 들어, 요소의 상태를 유효한 상태에서 유효하지 않은 상태로 변경하거나, `:disabled` 상태를 비활성화하거나 활성화하거나, `:active` 상태를 활성화하는 등의 작업을 수행할 수 있습니다.

## 구문

`전환` 속성은 공백으로 구분된 값 목록을 사용합니다.

```xml
transition: <property> <duration> <timing-function> <delay>
```

- `<property>`는 속성이 변경될 때 원래 값에서 새 값으로 애니메이션하고 싶은 애니메이션 가능한 CSS 속성입니다.
- <<br>>는 애니메이션이 지속될 기간이다. 지속 시간을 초(`s) 또는 밀리초(`ms`)로 설정할 수 있습니다.
- <기능>은 애니메이션의 속도를 조절하는 방법이다.
- 전환 전에 미룰 때가 바로 <<br>>이다.

이전 예에서는 다음과 같습니다.

```undefined
div.banner {
    ...
    transition: background-color 4s ease-in 1s;
}
...
```

- background-color는 속성이 변경될 때 애니메이션으로 만들려는 div.banner의 CSS 속성입니다.
- 4s는 자산 변경 애니메이션의 기간이다.
- `시계 기능`은 `시계 기능`이다. 이렇게 하면 효과가 느리게 시작되며 속도가 빨라집니다.
- 전환 시작 전 1s 연기

## 다중 전환

여러 속성을 `전환`으로 설정할 수 있습니다. 속성은 쉼표로 구분되어야 합니다.

```undefined
transition: background-color 4s ease-in 1s, width 2s;
```

여기서는 전환 애니메이션에 대해 배경색 및 폭의 두 가지 속성을 설정합니다. 이제 전환은 속성(배경색과 너비) 중 하나 또는 전체를 변경할 때 수신 대기합니다.

## 구성 요소 속성

전환 속성은 네 가지 장기 전환 속성의 줄임말입니다.

- `전환재산
- 과도현상
- `전환기-기능`
- 과도현상

### '전환재산

전환할 애니메이션 속성입니다.

```undefined
transition-property: border-radius
```

이것은 `경계 반지름`을 변경 시 전환 효과를 실행하려는 요소의 CSS 속성으로 설정한다.

```undefined
transition-property: margin-right
```

원소의 오른쪽 여백(오른쪽 여백)이 바뀌면 전환(transition(전환)

`transition-property`는 지정된 속성으로만 전환을 제한하고 나머지는 즉시 변경할 수 있도록 합니다. 여러 속성을 쉼표로 구분된 목록에서 `전환 속성`에 할당할 수 있습니다.

아래의 예를 참조하십시오.

```undefined
transition-property: margin-right, color, border-radius;
```

이 예에서는 값이 변경될 때마다 전환을 수행할 오른쪽 여백, 텍스트 색 및 테두리 반지름 속성만 선택합니다.

`transition-property`는 다음 값을 취할 수 있습니다.

- 없음: 전환에 사용할 속성이 선택되지 않았습니다.
- `all`: 요소의 애니메이션 적용 가능한 모든 속성이 전환에 선택됩니다.

### 과도현상

애니메이션의 기간 또는 완료하는 데 걸리는 시간입니다. 단위는 초(`s`) 또는 밀리초(`ms`)로 설정할 수 있습니다.

```undefined
transition-duration: 1s
```

`transition-property`와 마찬가지로 `transition-duration`에 대한 여러 값을 쉼표로 구분된 목록으로 제공할 수 있습니다.

```undefined
transition-duration: 1s, 200ms;
```

목록의 각 값은 각각 `전환 속성`에 설정된 각 값에 적용됩니다.

```undefined
transition-property: width, color;
transition-duration: 1s, 200ms;
```

1s는 너비, 200ms는 색상 속성에 적용된다. 즉, 요소의 너비 속성 값 변경 전환 기간은 1초이고, 요소의 색상 속성 값 변경 전환 시간은 200밀리초입니다.

### '전환기-기능'

전환 효과 또는 전환 실행 방법. 그것은 궁극적으로 전환의 속도를 통제한다.

전환 타이밍 함수는 다음과 같은 값을 취할 수 있습니다.

- ➡➡➡➡➡➡➡➡➡➡ ➡
- 선형
- `인입`
- `외출`
- `불참`
- 스텝 스타트
- 스텝 엔드
- step(n, start) 여기서 n은 스텝의 수이다.
- "x1, y1, x2, y2)"

위의 주요 값을 살펴보겠습니다.

- 쉬움=이렇게 되면 전환은 서서히 시작하다가 점차 빨라지고, 느려지고, 매우 느리게 끝나게 된다.
- `선형`: 시작에서 끝까지 전이 속도가 동일하며 선형입니다.
- `인`: 전환이 느리게 시작되어 속도가 빨라집니다.
- `불참`: 전환이 빠르게 시작된 다음 느려집니다.
- `내보내기`: 전환은 느리게 시작되고, 중간에 더 빨리 움직이며, 그 다음 너무 느리게 끝나지 않습니다.

예는 다음과 같습니다.

```undefined
transition-property: width;
transition-duration: 1s;
transition-timing-function: ease-in;
```

위에서는 `width` 속성 변경 시 전환이 트리거됩니다. 이즈인(eas-in)으로 설정된 전환 타이밍 기능은 폭 변화를 천천히 시작하고 1초 안에 점진적으로 속도를 높일 수 있게 한다.

### 과도현상

속성 상태의 변경과 새 상태로의 전환 시작 사이의 시간. 값은 초(`s`) 또는 밀리초(ms)로 표시됩니다.

```undefined
transition-property: width;
transition-duration: 1s;
transition-timing-function: ease-in;

transition-delay: 1s;
```

우선 전환이 시작되기 전 1s 지연을 설정했다.

다른 예를 생각해 보십시오.

```undefined
transition-property: border-radius;
transition-duration: 2s;
transition-timing-function: ease-in;

transition-delay: 10ms;
```

여기서 전환 애니메이션은 원소의 국경 반지름 상태가 바뀌면 10ms 동안 지연된다.

이 속성의 값이 `0s`인 경우 대상 속성의 상태가 변경되면 전환이 즉시 시작됩니다.