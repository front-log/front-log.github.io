---
layout: post
title: "CSS에서 논리 속성을 사용하는 방법은 다음과 같습니다."
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/css.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/css.png?fit=730%2C487&ssl=1)

## 도입

CSS는 웹에서 프레젠테이션을 처리하는 언어입니다. 원래 화면의 물리적 좌표(또는 치수)를 대상으로 작성되었습니다. 개발자로서 우리는 마진톱, 마진우측, 마진바텀 등의 속성을 이용하여 화면 차원을 겨냥한 스타일을 작성한다.

하지만 웹은 글로벌하고, 언어는 완전히 같은 방향으로 쓰여지지 않습니다. 예를 들어, 영어는 왼쪽에서 오른쪽으로, 아랍어는 오른쪽에서 왼쪽으로(rtl) 쓴다.

국제화 문제를 해결하기 위해, 우리는 이제 논리적 특성과 가치를 가지고 있습니다. 논리 속성 및 값에 대한 규격의 내용은 다음과 같습니다.

> 논리적 속성 및 값은 작성자(개발자)에게 물리적, 방향 및 차원 매핑이 아닌 논리적 매핑을 통해 레이아웃을 제어할 수 있는 기능을 제공합니다. – W3C 사양

이것이 의미하는 바는 화면의 물리적 부분을 대상으로 하는 대신 CSS 스타일링에서 흐름 상대적인 용어를 사용할 수 있다는 것이다.

예를 들어 너비에 대한 논리적 매핑은 인라인 크기이고, 높이에 대한 논리 매핑은 블록 측입니다. 인라인 크기는 인라인 차원의 길이를, 블록 측면의 길이는 블록 차원의 길이를 결정합니다. 폭과 높이 대신 인라인 사이즈와 블록 사이즈를 사용하면 동일한 레이아웃을 만들 수 있다.

다음은 코드펜에서 두 개의 <div> 스타일링을 보여주는 예입니다. 하나는 폭과 높이, 다른 하나는 인라인 사이즈와 블록 사이즈이다. 출력은 동일합니다.

```js
<div class="container">
  <div class="item physical-section"> 
  </div>
  <div class="item logical-section">
  </div>
</div>
```

```css
.physical-section{
  width: 300px;
  height: 300px;
  background-color: blue;
}

.logical-section{
  inline-size: 300px;
  block-size: 300px;
  background-color: yellow;
}
```

코드펜에서 결과를 볼 수 있습니다.

## 쓰기 모드 및 방향 속성 보기

전 세계에는 다양한 종류의 문자 시스템이 있다. 각 문자 체계는 언어에 따라 특정한 방향으로 쓰여질 수 있다. 예를 들어, 영어 쓰기 시스템은 왼쪽에서 오른쪽(`ltr`)입니다. 한편, 아랍어 또는 히브리어는 오른쪽에서 왼쪽으로 쓴다(rtl

### "쓰기 모드"

> 쓰기 모드 CSS 속성은 텍스트의 줄이 수평 또는 수직으로 배치되는지 여부와 블록이 진행되는 방향을 설정합니다.
-MDN

위의 정의가 의미하는 바는 다음과 같다: 텍스트의 행은 `<p> 요소와 같은 블록 요소 안에 들어 있는 텍스트이다. 이러한 텍스트 줄은 문자 체계에 따라 왼쪽에서 오른쪽(`ltr`) 또는 오른쪽에서 오른쪽(`수평 tb`) 또는 왼쪽에서 오른쪽으로(`수직-lr`) 또는 오른쪽에서 왼쪽으로(`수직-lr`) 흐른다.

서로 다른 언어의 쓰기 스타일을 수용하기 위해, 우리는 쓰기 모드 속성을 가지고 있다. 그리고 왜 우리는 쓰기 모드를 가지고 있을까? 우리는 시작점을 목표로 쓰기 모드를 사용합니다.

쓰기 모드도 가로 또는 세로 텍스트 줄의 배치 방법과 블록의 방향(ltr 또는 rtl)을 결정한다.

쓰기 모드를 이해하기 위해 인라인과 블록 레벨 요소를 살펴보자.

HTML `<` 요소는 `블록 레벨` 요소(컨테이너)입니다. 그것이 또 다른 `p` 요소가 앞 단락 아래에 새로운 단락을 시작하는 이유이다. 그러나 <p> 요소는 대개 텍스트를 포함하고 있으며, 이러한 텍스트는 인라인 수준의 내용입니다.

따라서 쓰기 모드는 블록 레벨 컨테이너의 적재를 결정하고, 방향은 인라인 레벨 컨텐츠의 흐름을 결정합니다.

다음은 규격의 쓰기 모드 속성에 대한 복잡한 색인입니다.

### 구문

```undefined
writing-mode: horizontal-tb; 
writing-mode: vertical-rl;
writing-mode: vertical-lr;
writing-mode: sideways-rl;
writing-mode: sideways-lr;
```

다음은 Codepen의 예입니다.

### '방향'

> '방향' CSS 속성은 텍스트, 테이블 열 및 수평 오버플로의 방향을 설정합니다. – MDN

방향 속성은 ltr(왼쪽에서 오른쪽으로) 또는 rtl(오른쪽에서 왼쪽으로)의 두 가지 값을 취할 수 있습니다. `방향`은 텍스트 방향을 정의하는 데 사용됩니다.
구문

```cpp
/* Keyword values*/

direction: ltr;            /* text goes from left to right.*/
direction: rtl;            /* text goes from right to left. */
```

다음은 `방향` `rtl`을 사용하는 히브리어로 된 샘플 텍스트의 예입니다. 코드펜에서 볼 수 있습니다. 텍스트는 Google과 함께 번역되었습니다.

```undefined
<p class="sampleText">
 טקסט לדוגמה בעברית. תורגם באמצעות Google Translate.
</p>
```

```css
.sampleText {
  direction: rtl;
}
```

HTML에는 텍스트 방향을 정의하기 위해 HTML 문서의 루트에서 설정할 수 있는 dir 속성도 있습니다. dir 속성을 사용하려면 다음과 같이 하십시오.

```xml
<html dir="rtl">
<p>This sample text will be read from right to left</p>
</html>
```

## 논리적 속성 및 값

쓰기 모드와 방향과 같은 일부 개념에 대한 기본적인 이해로 이제 우리는 몇 가지 예를 들어 논리적 속성과 가치를 깊이 있게 이해할 수 있다.

논리적 속성은 해당하는 CSS 물리적 속성과 동등한 쓰기 모드입니다.

다음은 논리적 속성과 물리적 속성의 전체 목록이 아닙니다.

논리적 속성의 포괄적인 목록은 여기를 참조하십시오.

논리적 특성을 통해 개발자는 이제 시작/끝 정렬된 사고 방식을 위쪽, 오른쪽, 아래쪽, 왼쪽(화면의 물리적 크기) 대신 쓰거나 심지어 우리가 익숙한 대로 띄울 수도 있습니다.

그래서 우리는 인라인-스타트(inline-start)와 인라인-엔드(inline-end)와 블록-스타트(block-start)-블록-엔드(block-end)가 있다.

논리적 속성을 사용하면 개발자가 레이아웃을 쉽게 변경하거나 변경할 수 있습니다. CSS 그리드(grid-row-start, grid-row-end, grid-column-start 및 grid-column-end)를 생각해 보자.

### 논리 속성에서 블록 및 인라인 차원이 중요한 이유

논리적 속성과 값은 흐름 방향을 추상화하는 데 블록과 인라인을 사용합니다.

블록 치수는 선 내의 선에서 텍스트 흐름 방향을 처리합니다. 수평 `쓰기 모드`의 경우 텍스트가 수직 방향으로 흐릅니다. 수직 `쓰기 모드`의 경우 텍스트가 수평 차원으로 흐릅니다.

인라인 치수는 선 내의 텍스트 흐름과 평행합니다. 반면 가로쓰기 모드의 수평적 차원, 세로쓰기 모드의 수직적 차원.

### 예

다음은 세 가지 언어의 `쓰기 모드`에 대한 논리적 속성을 사용하는 예입니다. 영어, 아랍어, 중국어.

```xml
<!--- HTML index.html -->

<div class="container">
 <div class="english-style item">
   <h2>English</h2>
   <p>
     This is a text sample, for demonstration purposes only in the English, Arabic. and Chinese language, `writing-mode`. Notice how the text is read from left to right (`ltr`) for English, right to left (`rtl`) for Arabic and vertical right to left (`vertical-rl`) for the Chinese language. Interesting how using logical properties and values can help make the web a more open and accessible place. Cheers to logical properties. </p> </div> <div class="arabic-syle item"> <h2>Arabic</h2> <p> هذا نموذج نصي ، لغرض التوضيح فقط في اللغة العربية. لاحظ كيف يُقرأ النص من اليمين إلى اليسار (rtl) للغة العربية. أتمنى أن تكون قد فهمت هذه النقطة. من المثير للاهتمام كيف أن استخدام الخصائص والقيم المنطقية يمكن أن يساعد في جعل الويب مكانًا أكثر انفتاحًا وسهولة. هتاف الخصائص المنطقية. </p> </div> <div class="chinese-style item"> <h2>Chinese</h2> <p> 這是文本示例，僅在中文中顯示。 注意中文是如何從右向左（rtl）讀取的。 我希望你明白這一點。 有趣的是，如何使用邏輯屬性和值可以使網絡變得更加開放和可訪問。 為邏輯屬性加油。 </p> </div> </div>
```

```css
/* Styles.css*/

.container{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 400px;
  gap: 1rem;
}


.item{
  background: #E1E3E4;
  text-align: justify;
}

.english-style{
  writing: horizontal-tb;
  direction: ltr;
}

.arabic-syle{
  writing-mode: horizontal-tb;
  direction: rtl;
}

.chinese-style{
  writing-mode: vertical-rl;
}
```

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/codepen-example.png?resize=730%2C210&ssl=1)

Codepen의 예제를 참조하십시오.

### 브라우저 호환성

논리적 속성과 값에 대한 지원이 모든 브라우저에 의해 완전히 채택되지는 않았지만, 논리적 속성의 기본적인 이해는 레이아웃을 이해하고 작업하는 데 핵심이다.

cani를 사용하여 브라우저 지원 및 논리적 속성 및 값의 호환성을 확인하십시오.

## 결론

웹은 의심할 여지 없이 국제적인 도구이기 때문에 접근성을 위해 논리적인 속성을 사용하여 당신의 스타일을 쓰기 시작하는 것을 추천합니다.

논리적인 특성으로 개발자들은 이제 텍스트를 세로로 실행하고, 레이아웃을 뒤집고, 전 세계 언어의 다른 쓰기 모드를 처리할 수 있다.

아직 모든 브라우저에서 논리 속성이 완전히 지원되지는 않았지만, 물리적 속성보다는 논리 속성을 사용하여 스타일을 작성하는 것이 좋습니다.

### 추가 읽기 및 리소스:

- CSSWG 편집기의 초안 CSS 논리적 속성 및 값
- MDN CSS 논리적 속성 및 값
- Andrew Rachel의 논리적 특성과 가치 이해
- 젠 시몬의 CSS 쓰기 모드