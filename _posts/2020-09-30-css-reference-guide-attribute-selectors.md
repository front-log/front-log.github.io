---
layout: post
title: "CSS 참조 가이드: 특성 선택기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/css-reference-guide-attribute-selectors-nocdn.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/css-reference-guide-attribute-selectors-nocdn.png?fit=730%2C487&ssl=1)

HTML에서는 속성을 사용하여 DOM 요소에 정보나 추가 옵션을 전달합니다. 이러한 특성은 CSS에서 활용하여 특정 요소를 타겟으로 하여 빠른 스타일링을 할 수 있다.

#### 앞으로 이동:

- 【➡】
- `[➡="값"]
- `[➡^="값"]
- `[예금$="값"]
- `[➡*="값"]
- [➡~="값"]
- [➡➡➡]
- 실제 사례

다음 예를 고려하십시오.

```xml
<div class="flex-item" id="flexm" disabled>Hi</div>
```

div 요소는 클래스, id, disabled의 세 가지 속성을 가지며 각각 플렉스 아이템 플렉스 플럼(flexm) true(true) 값을 갖는다.

특성 선택기는 요소의 특성에 따라 HTML의 특정 선택기를 대상으로 합니다.

## 【➡】

[attr] 선택기는 `attr` 특성을 가진 요소를 선택합니다.

```undefined
div[test] {
    color: red;
}
```

이 예에서는 빨간색으로 `test` 속성을 가진 div 요소를 스타일링합니다.

```undefined
p[target] {
    border-radius: 3px;
    padding: 10px;
}
```

이 셀렉터는 모든 p 요소를 target 속성과 일치시키고 border-radius 3px와 padding 10px로 만든다.

CSS는 정의된 값이 있는지 여부에 관계없이 요소와 일치합니다.

## '[➡="값"]

이 선택기는 속성 및 속성 값으로 더 구체적입니다. 값이 값인 속성 attr과 모든 요소를 일치시킵니다.

아래의 예를 참조하십시오.

```undefined
div[test="testing"] {
    color: tomato;
}
```

testing 값이 "testing"인 `test` 특성을 가진 모든 `div` 요소를 선택합니다.

```xml
<div test>1</div>
<div test="testing">2</div>
<div test="testing">3</div>
```

여기서 div 2와 3만 일치하고 스타일링됩니다. div 2와 3은 값이 "testing"으로 설정된 `test` 속성을 가지기 때문입니다. Div 1은 "testing" 값이 없으므로 "test" 특성이 있어도 스타일링되지 않으므로 선택되지 않습니다.

## '[➡^="값"]

이 선택기는 일치하는 패턴 선택기입니다. 값이 "값"으로 시작하는 속성 `attr`의 요소를 선택합니다.

여기서 중요한 단어는 다음으로 시작합니다. 속성은 지정된 값으로 시작해야 합니다.

예는 다음과 같습니다.

```undefined
div[test^="test"] {
    color: tomato;
}
```

값이 "test"로 시작하는 `test` 특성을 가진 div 요소를 선택합니다.

```xml
<div test="lasttesting">1</div>
<div test="testing">2</div>
<div test="tester">3</div>
```

여기서는 값이 "test"로 시작하는 "test" 속성을 가지므로 div 1과 2만 선택됩니다. Div 1의 `test` 속성 값이 `test`로 시작되지 않으므로 Div 1이 선택되지 않았습니다.

## '[예금$="값"]

이것은 또한 일치하는 패턴 선택기입니다. 값이 `값`으로 끝나는 속성 `attr`의 요소를 선택한다.

예는 다음과 같습니다.

```undefined
div[test$="ing"] {
    color: green;
}
```

이 선택기는 값이 "ing"으로 끝나는 모든 div 요소를 `test` 속성과 일치시킵니다.

```xml
<div test="lasttesting">1</div>
<div test="testing">2</div>
<div test="tester">3</div>
```

div 1과 2만 selector와 일치합니다. divs 1과 2는 `test` 속성을 가지며 값이 `ing`으로 끝납니다. Div 3은 영향을 받지 않습니다.

## '[➡*="값"]

다른 일치하는 패턴 선택기로 포함된 속성 값을 확인합니다. 이 선택기는 값이 `값`인 `attr` 특성을 가진 요소를 선택합니다.

```undefined
div[test*="val"] {
    color: turquoise;
}
```

위의 예에서는 값이 "val" 문자열을 포함하는 `test` 속성을 가진 모든 div 요소를 선택한다.

```xml
<div test="valuetesting">1</div>
<div test="tester">2</div>
<div test="testingvalue">3</div>
<div test="testervalue">4</div>
```

우리는 "test" 속성을 가진 네 개의 div 요소를 가지고 있습니다. 이제 CSS 선택기를 따라 divs 1, 3, 4만 영향을 받습니다. 속성 "test" 값에는 "val" 문자열이 포함되어 있으며, "test" 속성 값에는 "val" 문자열이 포함되어 있지 않으므로 div 2는 영향을 받지 않습니다.

## [➡~="값"]

이 선택기는 공백으로 구분된 속성 값을 대상으로 합니다. attr 속성 값 목록이 "값"과 일치하는 요소를 선택합니다.

```undefined
div[test~="val"] {
    color: orangered;
}
```

위의 예에서 선택기는 값 목록이 "val"과 일치하는 "test" 특성을 가진 div 요소에 영향을 미칩니다.

```xml
<div test="tester value">1</div>
<div test="tester val">2</div>
<div test="testervalue">3</div>
<div test="testingvalue">4</div>
<div test="testing value">5</div>
<div test="val testing">6</div>
```

여기서는 div 2와 6만 선택됩니다. 이는 이산형 "test" 속성 값 중 하나가 "val"이기 때문입니다. Divs 1, 3, 4, 5는 "test" 속성 값에 "val"과 일치하는 공백 구분 값이 없기 때문에 영향을 받지 않습니다.

## [➡➡➡]

속성이 `attr`이고 값이 `value`이거나 하이픈 `-in`으로 바로 이어지는 요소를 선택합니다.

이 속성 선택기는 주로 국제화에 사용됩니다.

```undefined
div[lang|="en"] {
    color: limegreen;
}
```

그러면 값이 "en" 또는 "en"인 "lang" 속성을 가진 div 요소를 하이픈이 있는 "en"으로 선택합니다.

```xml
<div lang="en">English(en)</div>
<div lang="en-EN">English(en-EN)</div>
<div lang="es">Espana(es)</div>
<div lang="es-ES">Espana(es-ES)</div>
```

divs 영어(english)와 영어(en-EN)는 각각 en과 en-이 들어 있어 선정된다.

## 실제 사례

아래 데모에서 다양한 선택기의 구현을 살펴볼 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="600" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/GRZGVzy?height=600&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=philipsz-davido&amp;slug-hash=GRZGVzy&amp;pen-title=css%20attributes&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css attributes" loading="lazy" id="cp_embed_GRZGVzy"></iframe></div>