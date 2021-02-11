---
layout: post
title: "상대 CSS 단위 이해"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-04-at-1.08.53-PM.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Screen-Shot-2021-02-04-at-1.08.53-PM.png?fit=730%2C482&ssl=1)

이 문서는 상대 길이 단위를 규정하는 것을 목표로 한다. 절대 길이 단위(가장 잘 알려진 대표자로 px인 경우)와 대조적으로 상대 길이 단위는 다른 것에 상대적인 길이를 지정한다. 이 "다른 것"은 상위 요소의 글꼴 크기, 상위 컨테이너의 너비 또는 뷰포트의 높이와 같은 다양한 유형의 것일 수 있습니다.

두 단위 유형 모두 `길이`라는 용어를 공통으로 가지고 있지만, 이 맥락에서 길이 단위는 정확히 무엇인가? 요소의 문자 또는 글꼴 관련 속성에 상대적인 글꼴 상대 길이 단위(예: `em`, `rem)`가 있다. 또한 뷰포트에 상대적인 길이 단위(예: `vw`, `vh`)가 있습니다.

상대 단위의 컨텍스트에서 다른 일반적인 CSS 데이터 유형은 백분율(`%`)입니다. 정수 값을 허용하는 CSS 속성도 있습니다. 이러한 단위 없는 값에 대한 가장 일반적인 사용 사례는 이 값을 선 높이 속성과 함께 사용하는 것입니다.

특히 줌에 의존하는 사용자를 지원하기 위해 유체 레이아웃과 웹 접근성의 관점에서 상대적인 유닛이 중요하다. 이러한 유체 레이아웃은 비례 설계를 기반으로 하며, 여기서 길이는 용기와 관련된 백분율로 정의됩니다.

따라서, 상대적인 단위에 기초한 구성요소는 예를 들어 장치를 회전하거나 브라우저 창의 크기를 줄임으로써 상황별 컨테이너와 관련하여 (재계산)되기 때문에 런타임에 크기가 변경될 수 있다.

## 글꼴 관련 CSS 단위

먼저 가장 일반적인 상대 글꼴 관련 CSS 장치인 `em`과 `rem`의 작동 방식을 살펴보겠습니다.

### CSS 단위 'em'

브라우저는 현재 글꼴 크기 컨텍스트와 관련하여 `em` 값을 `px` 값으로 변환합니다. 예를 들어 보겠습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/doppelmutzi/embed/xqbyZb?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=xqbyZb&amp;pen-title=CSS%20Unit%20em%20-%20Headings&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="CSS Unit em - Headings" loading="lazy" id="cp_embed_xqbyZb"></iframe></div>

h2 요소의 실제 마진톱 값은 얼마입니까? Chrome DevTools를 열고 `h2`를 선택한 다음 CSS 섹션의 Computed 탭으로 이동합니다. 값은 `40px`입니다.

어떻게 이런 가치가 생겨났을까요? HTML 요소(h2)의 고려된 CSS 속성(`margin-top`)에 대한 계산식은 다음과 같습니다.

스타일링할 HTML 요소의 px에서 `em` 값(2)에 실제 `font-size` 값을 곱한다(20).예를 들어, `2 * 20px = 40px`를 의미합니다.

이 예에서 h3 요소의 마진톱은 어떨까. 결국 46.8px다.

왜 그런 것일까요? "h2" 요소에서처럼 "글꼴 크기" 값을 선언하지 않았습니다. 그러나, 모든 요소들은 파생된 가치를 가지고 있다 - 이것은 CSS의 사물의 본질에 있다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Element-derived-value-vin-CSS-visual.png?resize=382%2C548&ssl=1)

우리가 볼 수 있듯이 h3 요소의 글꼴 크기는 1.17em이다. 좋아요, 이것도 px값이 아닙니다. px 값을 찾을 때까지 상위 요소 계층으로 올라가야 합니다.

DevTools의 도움을 받아 사용자 지정 값을 지정하지 않았기 때문에 `body` 요소는 브라우저 기본값에서 파생된 절대 `font-size` 값을 갖는다는 것을 알게 된다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/DevTools-body-element-font-size-value-display.png?resize=730%2C791&ssl=1)

이 정보를 통해 `2.5 * 1.17 * 16 = 46.8px`라는 구체적인 값을 계산할 수 있다.

다음 예에서는 `em`이 텍스트 콘텐츠 스타일링에만 국한되지 않으며 반대로 길이 단위를 사용할 수 있는 모든 곳에서 사용할 수 있음을 보여 줍니다. 처음에는 다소 이질적으로 보일 수 있지만 텍스트 이외의 요소를 문제 없이 스타일링할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/doppelmutzi/embed/qBaKMzE?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=qBaKMzE&amp;pen-title=CSS%20Unit%20em%20(margin)&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="CSS Unit em (margin)" loading="lazy" id="cp_embed_qBaKMzE"></iframe></div>

### CSS 단위 'rem'

글꼴 관련 상대 단위 렘은 루트 em을 의미한다. 브라우저의 루트 요소(일반적으로 `html` 요소)의 `font-size`와 상관 관계가 있다. 다음을 쉽게 결정할 수 있습니다.렘 값을 브라우저의 html 요소의 px에서 실제 폰트 크기 값에 곱한다.

여기 예가 있어요.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_3" scrolling="no" src="https://codepen.io/doppelmutzi/embed/NpqXpR?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=NpqXpR&amp;pen-title=CSS%20Unit%20rem&amp;name=cp_embed_3" style="width: 100%; overflow:hidden; display:block;" title="CSS Unit rem" loading="lazy" id="cp_embed_NpqXpR"></iframe></div>

정의된 렘 값(2)에 html 요소(`16px`)의 절대 글꼴 크기 값(`16px`)을 곱하기 때문에 h2 요소의 계산된 margin-top 값은 32px이다. html 요소의 글꼴 크기를 정의하는 선택기를 제공하지 않았기 때문에 브라우저 기본값입니다.

대부분의 (데스크톱) 브라우저의 기본값은 `16px`입니다. 하지만 확실히 알아보려면 DevTools를 다시 활용할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Default-browser-size-pixels-in-DevTools.png?resize=730%2C656&ssl=1)

브라우저 지원이 좋습니다. 기존 브라우저를 위해 개발해야 하는 경우를 제외하고 문제 없이 사용할 수 있습니다.

### '그들'과 함께 작업

W3C CSS 표준 문서를 수시로 확인하는 것이 좋습니다. 여기서 `font-size` 속성이 HTML 상위 계층에서 상속되는 것을 볼 수 있습니다.

따라서 `그들`과 함께 작업하기가 까다로울 수 있습니다.

- 모든 HTML 요소는 상위 HTML 요소로부터 `font-size` 값을 상속합니다.
- 루트 요소(html)에 대해 `em` 기반 `font-size`를 설정하면 px 값은 브라우저의 기본값과 곱하기 때문에 발생합니다. 일부 브라우저에서는 사용 후 설정에서 사용자 값을 정의할 수 있습니다.

따라서 브라우저 글꼴 설정은 상속을 통해 모든 `em` 값에 영향을 미칠 수 있습니다. 접근성 측면에서, 이것은 확대/축소에 의존하는 사용자를 지원하는 데 중요하다.

그러나 이 경우 `em` 값으로 스타일링된 중첩 요소에서 대부분 원하지 않는 동작이 발생할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_4" scrolling="no" src="https://codepen.io/doppelmutzi/embed/aJOzJR?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=aJOzJR&amp;pen-title=em%20-%20inheritance%20problematic&amp;name=cp_embed_4" style="width: 100%; overflow:hidden; display:block;" title="em - inheritance problematic" loading="lazy" id="cp_embed_aJOzJR"></iframe></div>

우리의 예에서, 더 깊은 내포 요소의 `글꼴 크기`는 예상보다 크다. 왜 그런 것일까요?

- level 1 li의 글꼴 크기는 14px입니다.
1.4 (li selector) * 10px (본문으로부터 글꼴 크기 변경)
- 1.4 (li selector) * 10px (본문으로부터 글꼴 크기 변경)
- level 2 li의 글꼴 크기는 19.6px이다.
`1.4 (li 선택기) * 14px (level 1li에서 분리)`
- `1.4 (li 선택기) * 14px (level 1li에서 분리)`
- 3급 li의 폰트 크기는 27.44px로 더 크다.
`1.4 (li selector) * 19.6px (level 2li에서 분리)`
- `1.4 (li selector) * 19.6px (level 2li에서 분리)`

모든 `li` 요소의 글꼴 크기가 동일한 방식으로 이 문제를 해결하려면 다른 선택기를 추가할 수 있습니다.

```css
li li { font-size: 1em; };
```

CSS 설계 시 참고하십시오. 이러한 시나리오에서는 em을 사용하지 않는 대신 렘을 사용하는 것이 좋습니다.

### 렘으로 작업

rem 값을 사용하여 CSS 속성에 대해 계산된 `px` 값을 결정하는 것은 쉽다. rem 값을 `html` 요소의 `px` 값에 곱해야 합니다. DevTools(Computed 탭)를 사용하여 값을 설정했는지 확인합니다.

아래 목록은 `html` 요소의 `px` 값이 결정되는 방법에 대해 자세히 설명합니다.

- `html` 요소에 대한 사용자 지정 `px` 값을 정의하지 않은 경우 `px` 값은 브라우저 설정 또는 브라우저 기본값에서 상속됩니다.
- `html` 요소의 `font-size`를 `px` 값으로 정의하면 이 값이 계산에 사용됩니다.
- html 요소의 `font-size`를 `em` 또는 `%` 값으로 정의하면 실제 `px` 값은 브라우저 글꼴 설정 또는 기본값을 사용하여 계산됩니다.
- `html` 요소의 `font-size`를 `rem` 값으로 정의하는 경우, `html` 요소의 `px` 값은 브라우저 글꼴 크기 설정 또는 기본값과 곱한 결과입니다.

따라서 브라우저 글꼴 설정은 CSS 설계의 모든 `rem` 값에 영향을 미칠 수 있습니다. 렘을 사용해도 다음 예에서 볼 수 있듯이 글꼴 크기의 상속 문제가 발생하지 않습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_5" scrolling="no" src="https://codepen.io/doppelmutzi/embed/yMNBKr?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=doppelmutzi&amp;slug-hash=yMNBKr&amp;pen-title=CSS%20Unit%20rem%20-%20no%20inheritance%20effects&amp;name=cp_embed_5" style="width: 100%; overflow:hidden; display:block;" title="CSS Unit rem - no inheritance effects" loading="lazy" id="cp_embed_yMNBKr"></iframe></div>

렘 값은 항상 단일 α 요소의 px 값과 상관되기 때문에 상위 계층의 글꼴 크기 정의는 관련이 없습니다.

### 렘과 em을 사용할 경우

렘과 em을 사용하는 것은 반응성 설계, 사용성 및 접근성의 주제와 일치한다.

렘의 장점은 글꼴 크기 상속 없이 통일된 크기 조정이다. 응용프로그램 설계자는 내용을 적절하게 표시하기 위해 사용자 글꼴 설정에 응답할 수 있습니다. 이는 접근 가능한 설계에 의존하는 사용자를 배제하지 않기 위해 중요하다. 사용자가 글꼴 크기를 늘리면 레이아웃의 무결성을 유지할 수 있습니다. 예를 들어 텍스트가 고정된 너비의 작은 용기에 압축되지 않습니다.

em은 모든 CSS 개발자의 벨트에 끼워진 강력한 도구다. 요소를 스타일링하는 기본 원리는 부모 요소의 글꼴 크기 값에 따라 크기 값(예: 여백, 너비)을 결정하는 것이다. 따라서 루트 요소(html)에만 국한되지 않습니다.

`em`의 주요 이점 중 하나는 문맥(예: 버튼 또는 티저 모듈) 내에서 설계 요소 간의 비례 관계를 설정할 수 있다는 것이다. `이들`의 사용은 상황적 확장성 및 대응적 설계의 문제와 관련이 있다. 다음 예제는 반응성분 설계에 `그들`을 어떻게 사용할 수 있는지를 보여준다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_6" scrolling="no" src="https://codepen.io/doppelmutzi/embed/BbNbpy?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=BbNbpy&amp;pen-title=Responsive%20buttons%20with%20em%20unit&amp;name=cp_embed_6" style="width: 100%; overflow:hidden; display:block;" title="Responsive buttons with em unit" loading="lazy" id="cp_embed_BbNbpy"></iframe></div>

버튼 구성요소 응답 구조는 `버튼` 선택기로 정의된다. 경계선(border), 경계선(border-dister)은 정의된 글꼴 크기 값에 관련된 상대적 단위를 사용한다. 다양한 버튼 크기는 콘크리트 클래스 선택기(예: `size-l`)로 정의된다.

글꼴 크기 값도 em 단위이므로 구체적인 상황별 값은 주 선택기와 바닥 선택기로 정의된 px 값에서 파생된다.

이 접근 방식의 이점은 이러한 구성요소를 다른 컨텍스트에서 쉽게 재사용할 수 있다는 것입니다. 다른 컨텍스트 컨테이너에 대해 서로 다른 `글꼴 크기` 값을 정의하면 나머지는 상속이 됩니다.

## 상대 CSS 단위 '%'

CSS에서 백분율(`%`)은 길이 단위가 아니라 데이터 유형입니다. px, em 등 어디에나 적용할 수 있어 길이 단위처럼 느껴진다. 이런 맥락에서 언급하는 것이 타당합니다.

이 데이터 유형은 항상 상위 구성 요소의 일부를 나타냅니다. 백분율로 정의된 길이는 상위 요소의 동일한 속성의 길이(px의 계산된 값)를 기준으로 합니다. 다음 예에서는 이를 설명합니다.

```xml
main {
  width: 400px;
  height: 50%;
}

h1 {
  width: 50%;
}

<body>
  <main>
    <h1>hello world</h1>
  </main>
</body>
```

h1 요소의 계산된 폭은 상위 요소인 주 요소인 200px의 절반이다. 상위 컨테이너가 `본문` 요소인 경우 백분율은 항상 브라우저 뷰포트의 크기를 나타냅니다. 우리의 경우, `주` 요소의 `높이` 속성의 계산된 값은 뷰포트 컨테이너의 계산된 값의 절반이다.

자, 이제 좀 더 자세히 살펴보도록 하죠. 다음 코드펜에서 회색 배경 컨테이너는 `h2` 요소(파란색 배경)의 상위 요소입니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_7" scrolling="no" src="https://codepen.io/doppelmutzi/embed/BmmqQb?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=BmmqQb&amp;pen-title=CSS%20percentage%20demo&amp;name=cp_embed_7" style="width: 100%; overflow:hidden; display:block;" title="CSS percentage demo" loading="lazy" id="cp_embed_BmmqQb"></iframe></div>

사용자 정의 너비가 없는 블록 레벨 요소의 기본 동작은 데모 1에서 볼 수 있듯이 상위 컨테이너의 사용 가능한 수평 공간을 차지하는 것입니다. h2의 가로 패딩(암청색 배경)은 전체 너비에 추가되지 않습니다.

그러나 데모 2에서 볼 수 있듯이 명시적 `폭: 100%`를 정의하면 수평 스크롤 막대가 표시됩니다. 왜 그런 것일까요?

디폴트 폭(default width)이 auto(자동) 값을 갖는 데모 1과 달리 백분율 값 정의에는 패딩(padding), 여백(margin), 테두리(border)가 포함되어 있어 자녀들이 부모 용기의 너비를 과도하게 늘릴 수 있다. 당신은 CSS 규격에서 그것에 대해 읽을 수 있다.

데모 3에서는 `폭: 100%`도 지정하지만 수평 스크롤 막대가 없다. 기본 설정을 박스사이징:콘텐츠박스(content-box)에서 박스사이징:보더박스(box-size: border-box)로 변경해 아이들의 테두리(border)와 패딩(padding)이 수평 간격을 더하지 못하고 있기 때문이다. CSS 설계 전체에서 `경계 상자`를 사용하도록 글로벌 선택기를 정의하는 것은 드문 일이 아닙니다.

데모 4 및 데모 5를 사용하면 백분율이 직접 상위 요소와 관련이 있는 것이 아니라 상위 트리의 상위 요소와 관련이 있음을 알 수 있습니다.

## 뷰포트 단위

백분율 값(`%`)은 항상 상위 요소를 나타냅니다. 그러나 뷰포트 단위 값은 현재 브라우저 뷰포트의 백분율을 나타냅니다.

뷰포트 단위의 값은 뷰포트 컨테이너의 폭(`vw`)과 높이(`vh`)를 기준으로 결정됩니다. 값 범위는 1에서 100 사이입니다.

- 1vw는 뷰포트 폭의 1%이며, 1vh는 뷰포트 높이의 1%이다.
- 100vw는 뷰포트 폭의 100%이며 100vh는 뷰포트 높이의 100%이다.

뷰포트 유닛의 가장 분명한 사용 사례는 뷰포트 크기와 관련하여 점유 공간을 차지하는 최상위 컨테이너에 그것들을 사용하는 것이다; 관련되는 상위 요소에 의한 계단식이나 영향력은 없다. 뷰포트 유닛의 %s과(와) 대조적으로, 스타일링할 요소가 마크업에서 어디에 위치하는지는 중요하지 않다.

그러니까 100vw는 전체 폭에 사용할 수 있는 거죠? 네, 하지만 조심해서 말씀하세요.

요소의 `경계`와 `여백`은 고려되지 않으므로 다음 예에서 볼 수 있듯이 헤더 컨테이너가 브라우저 뷰포트를 초과하고 수평 스크롤 막대가 표시됩니다. 기사 컨테이너는 박스 사이징 속성을 콘텐트 박스(content-box) 기본값에서 보더 박스(border-box)로 설정하여 이 문제를 해결한다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_8" scrolling="no" src="https://codepen.io/doppelmutzi/embed/MWjGQgd?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=doppelmutzi&amp;slug-hash=MWjGQgd&amp;pen-title=Viewport%20units%20demo&amp;name=cp_embed_8" style="width: 100%; overflow:hidden; display:block;" title="Viewport units demo" loading="lazy" id="cp_embed_MWjGQgd"></iframe></div>

주 컨테이너의 경우, 우리는 `calc()`를 사용하여 양쪽의 테두리 폭과 여백을 빼서 이 문제를 방지한다.

전체 너비 섹션을 얻는 경우 바닥글 컨테이너에서 볼 수 있듯이 기존 `폭:100%`(기본값 `디스플레이: 블록`)를 사용하는 것이 더 쉽습니다. 스크롤바가 표시될 때 오래된 브라우저가 문제를 겪을 수 있기 때문에 `폭: 100%`를 사용하는 것이 더 우수하다.

보다 유익한 사용 사례는 뷰포트의 높이와 관련하여 vh 단위를 사용하는 것입니다. 컨테이너를 뷰포트 높이로 확장하려면 vh가 상위 포트와 관련되므로 "%"보다 높습니다. 따라서 `%` 단위를 사용하면 상위 컨테이너가 뷰포트의 높이를 채우도록 고정 레이아웃 기법을 사용해야 합니다.

다음 스티커 바닥글 예제에서는 vh를 사용하여 뷰포트 하단에 구성 요소를 포함합니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_9" scrolling="no" src="https://codepen.io/doppelmutzi/embed/bqNoBK?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=doppelmutzi&amp;slug-hash=bqNoBK&amp;pen-title=sticky%20footer%20with%20flexbox&amp;name=cp_embed_9" style="width: 100%; overflow:hidden; display:block;" title="sticky footer with flexbox" loading="lazy" id="cp_embed_bqNoBK"></iframe></div>

관련 단위는 `vmin` 및 `vmax`입니다.

- 20vmin은 20vw 또는 20vh 중 더 작은 것과 관련이 있습니다.
- 10µ는 10vw 또는 10vh 중 더 큰 것과 관련이 있습니다.

예를 들어 10vmin은 세로 방향에서 현재 뷰포트 폭의 10%, 가로 방향에서 뷰포트 높이의 10%로 결정된다는 설명을 다시 한 번 설명하겠습니다.

다음 그림에서는 두 장치의 최종 픽셀 값이 어떻게 계산되는지 보여 줍니다.

무슨 용도가 있죠? 많은 사용 사례가 떠오르지는 않지만 웹 개발자로서 가방에 들어 있는 또 다른 도구입니다.

경우에 따라서는 미디어 쿼리보다 더 도움이 됩니다. 미디어 쿼리는 "게이트"로 생각해야 합니다. 그러나 거의 무한한 수의 장치와 폼 팩터로 인해 이 접근 방식이 항상 확장되는 것은 아닙니다. vmin과 vmax를 유체 길이 단위로 생각할 수 있다.

사용 사례를 검색하면 응답성이 뛰어난 히어로 텍스트 구성 요소를 자주 볼 수 있습니다. 다음 코드펜은 vmin이 vw와 대조적으로 대응 행동을 개선한다는 것을 보여준다. 이 경우 가로 모드에서 화면 폭에 대한 텍스트의 비율이 더 일관적이다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_10" scrolling="no" src="https://codepen.io/doppelmutzi/embed/MWjqaOa?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=doppelmutzi&amp;slug-hash=MWjqaOa&amp;pen-title=Hero%20text%20with%20vmin&amp;name=cp_embed_10" style="width: 100%; overflow:hidden; display:block;" title="Hero text with vmin" loading="lazy" id="cp_embed_MWjqaOa"></iframe></div>

`vmax`를 사용하고자 하는 경우를 제외하고 Internet Explorer 9에서도 브라우저 지원이 매우 좋습니다.

하지만, 모바일 기기라면 악마가 자세한 내용을 담고 있습니다. 특히 vh의 계산은 모든 모바일 브라우저에서 일관되지 않는다. 하지만 이에 대처하기 위한 몇 가지 묘책이 있다.

## 공통 사용 사례

이 섹션에서는 서로 다른 상대 단위를 조합하여 가능한 반응 패턴을 보여드리고자 합니다.

### 국내외 확장을 위해 렘과 렘 결합

이 코드펜은 슬라이더의 도움을 받아 글로벌(즉, 웹사이트 전체) 및 로컬(즉, 모듈 내부) 스케일링에 `rem`과 `em`을 어떻게 활용할 수 있는지를 잘 보여준다. 사용자의 글꼴 크기 설정에 대응하여 포함 설계를 염두에 두고 레이아웃을 작성하는 방법을 보여줍니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_11" scrolling="no" src="https://codepen.io/chriscoyier/embed/tvheK?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=chriscoyier&amp;slug-hash=tvheK&amp;pen-title=Em%20AND%20Rem&amp;name=cp_embed_11" style="width: 100%; overflow:hidden; display:block;" title="Em AND Rem" loading="lazy" id="cp_embed_tvheK"></iframe></div>

이 접근 방식과 앞에서 설명한 버튼 예 사이에는 미묘한 차이가 있습니다. px 값 대신 rem 값을 사용하고 루트 요소에 대해 고정된 `font-size` 값을 정의하지 않는 것은 접근성을 향상시키기 위해 사용자 글꼴 크기 설정을 존중한다.

### 제한된 너비 상위 영역의 전체 너비 컨테이너

마크업 구조가 완전히 관리 상태에 있지 않은 시나리오를 고려해 보십시오. 어떤 종류의 CMS를 사용한다면, 여러분은 제한된 너비의 컨테이너를 가지고 있지만, 어린이 요소들을 발생시키고자 하는 상황에 처하게 될 수도 있습니다.

다음 코드펜은 `vw`의 도움을 받아 경계 컨테이너에서 탈출하여 전체 수평 공간을 차지하는 방법을 보여준다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_12" scrolling="no" src="https://codepen.io/doppelmutzi/embed/abmMWgY?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=doppelmutzi&amp;slug-hash=abmMWgY&amp;pen-title=%20Full-width%20containers%20in%20limited-width%20parents&amp;name=cp_embed_12" style="width: 100%; overflow:hidden; display:block;" title=" Full-width containers in limited-width parents" loading="lazy" id="cp_embed_abmMWgY"></iframe></div>

중요한 코드는 .break-out 선택기의 일부이다. 코드가 궁금하면 다음 파생상품인 margin-left를 살펴보십시오.

```undefined
.break-out {
  margin-left: calc(-100vw / 2 + 500px / 2); // 500px because of the max width of the container
  margin-left: calc(-50vw + 50%); // replace 500px with 100%
}
```

CSS-Tricks 및 Cloud Four에서 다양한 시나리오와 기법으로 자세한 설명을 읽을 수 있습니다.

### 유체 타이포그래피

간단히 말해, 대부분의 경우 텍스트 콘텐츠는 페이지에서 매우 중요한 역할을 합니다. 다양한 화면 크기로 인해 유체 타이포그래피는 점점 더 중요해졌다.

뷰포트 단위(vw, vh)를 사용하는 유체 타이포그래피는 뷰포트에 대한 글꼴 크기를 동적으로 조정합니다. 이는 중단점에 기초한 반응형 타이포그래피와 대조적으로 보다 조화롭고 적절한 프레젠테이션으로 이어진다.

다음은 순진한 예입니다.

```css
h1 { font-size: 4vw; }
p { font-size: 2vw; }
```

그러나 잠시 기다려 주십시오. 더 나은 방법은 다음과 같습니다.

```css
html { font-size: 3vw; }
```

그 이유는 접근성을 염두에 두고 브라우저의 기본값을 활용하기 때문입니다. h1과 p의 HTML 태그는 글꼴 크기 값(em)으로 정의되므로 문단 텍스트, 헤드라인 등의 상대성을 생각할 필요가 없다. 물론 설계를 최적화하려면 글꼴 크기를 정의할 수 있습니다.

그러나 우리의 접근 방식은 문제가 있다. 작은 뷰포트 폭에 비해 글꼴 크기가 너무 작다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/Fluid-Typography-small-type-adjustment-visual.gif?resize=730%2C281&ssl=1)

따라서 우리는 텍스트가 각각 작은 화면 폭 또는 큰 화면 너비에 대해 너무 많이 축소되거나 커지지 않도록 약간의 하한과 상한이 필요하다. 우리는 미디어 쿼리를 사용하여 그러한 경계를 정의할 수 있지만, 결국 중단점 사이의 이러한 조화롭지 못한 점프를 하게 된다.

더 나은 방법은 우선 미디어 쿼리를 생략하는 것이다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_13" scrolling="no" src="https://codepen.io/doppelmutzi/embed/bGwZXJx?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=doppelmutzi&amp;slug-hash=bGwZXJx&amp;pen-title=Fluid%20typography&amp;name=cp_embed_13" style="width: 100%; overflow:hidden; display:block;" title="Fluid typography" loading="lazy" id="cp_embed_bGwZXJx"></iframe></div>

위의 코드펜에서 사용되는 알고리즘은 CSS-Tricks에 의해 기술된 접근법에 기초한다. 크리스틴 밸러(Christine Vallaure)는 이 알고리즘의 이면에 있는 근거를 잘 설명해준다.

```css
html {
    font-size: calc([minimum size] + ([maximum size] - [minimum size]) * ((100vw - [minimum viewport width]) / ([maximum viewport width] - [minimum viewport width])));
}
```

팀 브라운은 또한 이 개념을 유동적인 `라인 높이`로 옮긴다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_14" scrolling="no" src="https://codepen.io/timbrown/embed/akXvRw?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=timbrown&amp;slug-hash=akXvRw&amp;pen-title=CSS%20calc%20lock%20for%20line-height&amp;name=cp_embed_14" style="width: 100%; overflow:hidden; display:block;" title="CSS calc lock for line-height" loading="lazy" id="cp_embed_akXvRw"></iframe></div>

### 'vh'가 있는 사용자 지정 스크롤 표시기

다음 예에서는 `%`와 `vh` 단위를 결합하여 순수 CSS 스크롤 표시기를 만드는 현명한 방법을 보여 줍니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_15" scrolling="no" src="https://codepen.io/MadeByMike/embed/ZOrEmr?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=MadeByMike&amp;slug-hash=ZOrEmr&amp;pen-title=CSS%20only%20scroll%20indicator&amp;name=cp_embed_15" style="width: 100%; overflow:hidden; display:block;" title="CSS only scroll indicator" loading="lazy" id="cp_embed_ZOrEmr"></iframe></div>

## 결론

CSS가 사실상 무제한의 다양한 뷰포트 크기와 dpi를 처리할 수 있도록 하는 유동적 레이아웃이나 반응성 설계(원하는 대로 불러)의 경우 상대 단위를 이해하는 것이 핵심입니다.

글꼴 기반 단위는 처음에는 이상하게 느껴질 수 있지만, 어느 정도 경험을 쌓은 후에는 절대 단위보다 장점을 인식할 수 있습니다. 뷰포트 단위, 특히 `vh`는 유용하지만, 언제 사용해야 하는지, 그리고 언제 오래된 `%` 데이터 유형을 사용해야 하는지 이해하는 것이 중요합니다.

웹 사이트를 개발하는 데 있어 너무 자주 간과되는 측면은 웹 사이트에 액세스할 수 있게 하는 것입니다. 이러한 상대적 단위는 본질적으로 사용자별 글꼴 크기 및 확대/축소 설정을 존중합니다.

수평선에 더 많은 상대 유닛이 있지만 현재 이를 지원하는 브라우저는 없습니다. 감사하게도, 우리는 이미 현 대표들과 매우 잘 협력할 수 있습니다.