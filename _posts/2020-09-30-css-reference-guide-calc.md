---
layout: post
title: "CSS 참조 가이드: 'calc()'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-calc-nocdn.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-calc-nocdn.png?fit=730%2C487&ssl=1)

CSS `calc()` 함수는 CSS에서 간단한 산술 표현식을 평가하는 데 사용된다.

#### 앞으로 이동:

- 값 유형
- 규칙.
- CSS 변수 및 `calc`
- `calc` 사용 예

산술 식은 덧셈, 뺄셈, 나눗셈, 곱셈입니다. 다음 식은 우선 순위 순서를 따릅니다.

- P – 괄호
- D – 디비전
- M – 곱셈
- A – 추가
- S – 감산

식의 괄호는 왼쪽에서 오른쪽으로 먼저 평가됩니다. 그런 다음 분할 연산을 수행한 다음 곱하기, 추가 및 마지막으로 감산합니다.

예를 들어 `calc`에는 다음과 같은 식 인수가 있습니다.

```undefined
calc(4px - (9px + 1px) / 5px * 9px)
```

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="450" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/XWdqmpy?height=450&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=philipsz-davido&amp;slug-hash=XWdqmpy&amp;pen-title=css%20calc%20example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css calc example" loading="lazy" id="cp_embed_XWdqmpy"></iframe></div>

괄호 `(9px + 1px)`가 먼저 평가되므로, 이제 다음과 같은 사항이 있습니다.

```undefined
4px - 10px / 5px * 9px
```

다음은 디비전 오퍼입니다. `10px / 5px`는 다음과 같이 평가된다.

```undefined
4px - 2px * 9px
```

그런 다음 곱셈 op는 `2px * 9px`로 평가됩니다.

```undefined
4px - 18px
```

마지막으로, 감산 연산자는 `4px - 18px`로 평가된다.

```undefined
-14px
```

## 값 유형

CSS의 일부 값 유형은 `calc` 함수에서 허용되지 않습니다. 허용되는 값은 다음과 같습니다.

- <<길이>>
- <주파수>
- `<각도>
- <<time>>
- <백분율>
- <<숫자>
- <<<<<>>

## 규칙.

calc 함수를 사용할 때 엄격하게 준수해야 하는 특정 규칙이 있습니다.

1.) 덧셈과 뺄셈 연산자의 RHS와 LHS 값은 동일한 단위 유형이어야 하며, 숫자 및 정수일 수 있다.

- 값이 단위 유형이 다르기 때문에 `5px + 2`는 잘못되었습니다. 첫 번째는 <길이>형이고 두 번째는 <숫자>형이다.
- 2px + 5vh는 모두 `<길이> 단위 유형이기 때문에 유효하며, 식의 피연산자는 `<길이> 구문 값일 수 있습니다.
- `3.4 - 4`가 유효합니다.

2.) 덧셈 및 뺄셈 연산자 값은 흰색으로 되어야 합니다. 그렇지 않으면 식이 잘못 구문 분석 및 평가될 수 있습니다.

- 4px-5vh는 - 기호 앞뒤에 공백이 없으므로 유효하지 않으며, 4px와 음의 5vh로 해석됩니다. 이 문제를 해결하려면 `4px - 5vh` 사이에 공백을 넣으십시오.

3.) 곱셈 연산의 값 중 하나는 <숫자>(또는 <정수>)여야 한다.

- 모든 값이 `길이` 단위 유형이기 때문에 `4px * 5em`은 잘못되었습니다.
- RHS 값이 `<숫자>이므로 `4px * 5`는 유효합니다.

> N.B.는 덧셈과 뺄셈 연산자(예: '4*5px')와 달리 곱셈과 나눗셈 연산자는 공백을 필요로 하지 않는다. 그러나 일관성을 위해 권장됩니다.

4.) 분할 작업에서 RHS 값은 `<숫자>여야 한다.

- RHS 값(`5em`)이 길이 단위이므로 4px / 5em이 잘못되었습니다. 4px / 5는 RHS 값(`5`)이 <숫자>이므로 유효합니다.
- 평가 후 반환 값의 단위 유형은 LHS 단위 유형에 따라 결정됩니다. LHS 장치 유형이 `<정수>인 경우 반환 장치 유형은 `<숫자>입니다.

5.) 0을 산출하는 분할은 무효로 간주한다.

- `4rem/0`은 0을 산출하므로 유효하지 않습니다.

6.) `calc` 함수는 최대 20개의 항을 지원합니다.

- "<길이>"""""""""""시간"""""""""""""""""""""""""""""""수"""""""정수"""""""""""""""""""""" 이 용어 번호를 초과하면 `calc`의 식 인수가 유효하지 않습니다.

## CSS 변수 및 'calc'

CSS 변수는 `calc` 식 arg에 사용할 수 있습니다. CSS 변수는 선택기에 변수를 저장하고 `var` 함수를 사용하여 스타일링의 어느 곳에서나 사용할 수 있습니다.

```undefined
::root {
    --width: 500px;
    --height: 900px;
}

div.burger {
    width: calc(10% - var(--width));
    height:  calc(12% + var(--height));
}
```

우리는 루트 스코프에서 선언된 CSS 변수 `--width`와 `--height`를 가지고 있으며, 그런 다음 `calc` 함수 내에서 `var(...)` 함수를 사용하여 검색된다.

div.burger 폭 길이는 "10% - 500px"가 됩니다. calc(10% - var(--width) 문장의 var(--width)는 500px로 대체된다. div.burger의 높이는 calc(12% + 900px)가 된다.

## 'calc' 사용 예

개발 중에 `➡`은 꽤 자주 쓰입니다. 여백 대신 calc를 사용해 버튼 좌우에 간격을 둘 수 있는 곳이 어디인지 알아보자.

```undefined
.btn {
    padding: 6px 12px;
    margin-bottom: 0;
    font-weight: 400;
    cursor: pointer;
    text-align: center;
    white-space: nowrap;
    display: inline-block;
    background-image: none;

    border-radius: 3px;
    box-shadow: none;
    border: 1px solid transparent;
}

.btn-block {
    width: calc(100% - 20px);
}
```

.btn.block 선택기는 왼쪽과 오른쪽 사이에 20px의 간격을 두고 상위 컨테이너에 걸쳐 버튼을 길게 늘린다.