---
layout: post
title: "HTML, CSS 및 JavaScript가 포함된 웹 애니메이션"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/web_animation_with_html_css_and_js_web.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/web_animation_with_html_css_and_js_web.png?fit=730%2C487&ssl=1)

## 도입

수년 전, 웹사이트들은 사이트를 보다 사용하기 쉽게 만들기 위해 시각적인 경험을 만드는 방법을 고려하지 않고 사용자에게 정보를 표시하는 데 더 초점을 맞췄다. 지난 몇 년 동안, 많은 것들이 바뀌었습니다: 웹 사이트 소유자들은 이제 사용자들을 그들의 사이트에 있게 하기 위해 시각적인 경험을 만들고 있습니다.

개발자들은 인간이 움직임을 알아채기 위한 자연스러운 반사작용 때문에 움직이는 물체에 더 많은 관심을 기울이는 경향이 있다는 것을 발견했다.

또한 웹 사이트나 응용 프로그램에 애니메이션을 추가하는 것은 사용자의 관심을 웹 사이트의 중요한 영역에 끌어들이고 제품에 대한 더 많은 정보를 노출하는 매우 중요한 방법입니다.

> 참고: 효과적인 애니메이션은 사용자와 화면의 콘텐츠 간에 강력한 연결을 구축할 수 있습니다.

## 웹 애니메이션이란 무엇인가?

웹애니메이션은 기본적으로 단지 웹상에서 사물을 움직이게 하는 것입니다.

웹 애니메이션은 더 나은 변환을 가능하게 하고 사용자가 웹 사이트에서 물건을 클릭, 보기 및 구입하도록 유인하는 눈길을 끄는 웹 사이트를 만드는 데 필요합니다.

애니메이션이 제대로 수행되면 소중한 상호 작용을 추가하고, 사용자를 위한 감성적 경험을 향상시키며, 인터페이스에 개성을 더할 수 있습니다.

현재 수백 개의 라이브러리, 도구, 플러그인이 있으며, 간단한 것부터 복잡한 것까지 다양한 애니메이션을 만드는 데 사용될 수 있다. CSS Animation을 사용하면 CSS로 쉽게 할 수 있는 애니메이션을 위해 웹 사이트 속도를 늦추는 플러그인을 사용할 필요가 없습니다.

이 기사에서는 HTML, CSS, JavaScript로 얻을 수 있는 애니메이션을 보여드리겠습니다.

## 어떤 CSS 속성을 애니메이션할 수 있습니까?

어떻게 애니메이션을 만드는지를 아는 것과 무엇을 애니메이션화해야 하는지 아는 것은 별개의 문제입니다.

일부 CSS 속성은 애니메이션이 가능하며, 이는 애니메이션과 전환에 사용될 수 있음을 의미한다.

이러한 속성은 크기, 색상, 숫자, 모양, 백분율 등과 같이 한 값에서 다른 값으로 점차 변경될 수 있습니다.

배경, 배경색, 테두리 색, 필터, 플렉스 및 글꼴과 같은 속성을 애니메이션할 수 있습니다.

여기에서 애니메이션할 수 있는 모든 속성의 포괄적인 목록을 얻을 수 있습니다.

## 다른 종류의 애니메이션

웹 사이트에서 매우 잘 사용되고 사용자 경험에 매우 중요한 역할을 하는 매우 다양한 종류의 애니메이션이 있습니다.

여기에는 다음이 포함됩니다.

### 툴팁

도구 추가 정보는 사용자가 요소를 맴돌거나 초점을 맞추거나 만질 때 나타나는 텍스트 레이블입니다.

즉, 사용자가 그래픽 사용자 인터페이스(GUI)의 요소와 상호 작용할 때 나타나는 간략하고 유익한 메시지입니다.

도구 설명에는 기능에 대한 간단한 도우미 텍스트가 포함될 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/a-gif.gif?resize=730%2C701&ssl=1)

### 호버

호버 의사 클래스는 마우스를 위로 이동시킬 때 요소에 특수 효과를 추가하는 데 사용됩니다. 이렇게 하면 사용자가 항목을 가리키면 바로 사용자의 주의를 끌 수 있습니다.

클릭할 수 있는 요소를 보여주는 유용한 방법입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/hover-psuedoclass.gif?resize=730%2C365&ssl=1)

### 싣고 있는

적재 시간은 사용자를 즐겁게 하는 데 도움이 되기 때문에 적재 작업은 매우 중요합니다. 또한 사용자에게 진행 수준 또는 로드가 완료되기까지 남은 시간을 알려줍니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/loading-gif.gif?resize=730%2C547&ssl=1)

### 입력

입력 애니메이션은 매우 유용하며 도구 설명 및 검증과 결합되는 경우가 많습니다. 입력을 통해 사용자는 오류를 신속하게 수정하고 누락된 필드를 채워 양식을 작성할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/click-search.gif?resize=730%2C365&ssl=1)

### 메뉴

메뉴의 애니메이션은 UI/UX에서 큰 역할을 합니다. 메뉴는 사용자가 페이지를 통해 모든 내용을 볼 수 있도록 사용자를 놀라게 하고 대화식으로 유지하는 애니메이션의 유형입니다.

> 참고: 페이지 전환, 시차 등과 같은 다른 애니메이션이 많이 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/drawn-hamburger.gif?resize=730%2C411&ssl=1)

## CSS 애니메이션

지금까지, 우리는 CSS로 얻을 수 있는 매우 다양한 종류의 애니메이션을 봐왔지만, 저는 그것이 어떻게 이루어지는지 설명하지 않았습니다.

CSS를 사용하면 JavaScript를 사용하지 않고도 HTML 요소를 애니메이션할 수 있습니다.

CSS 애니메이션을 사용하려면 먼저 애니메이션에 대한 일부 키 프레임을 지정해야 합니다. 키 프레임은 특정 시간에 요소가 가질 스타일을 유지합니다.

적절한 이해를 위해, 우리가 사용할 기본 속성에 대해 설명하겠습니다.

CSS 애니메이션은 다음 두 가지 기본 구성 요소로 이루어집니다.

### @키프레임

키 프레임은 애니메이션의 시작과 끝을 나타내는 데 사용됩니다(시작과 끝 사이의 중간 단계).

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/keyframes-syntax.jpeg?resize=730%2C339&ssl=1)

기본 3가지로 구성되어 있습니다.

- 애니메이션 이름: 이것은 위의 그림과 같이 단순히 애니메이션에 붙여진 이름입니다.
- 애니메이션 단계: 애니메이션의 단계를 나타냅니다. 위 그림과 같이 대부분 백분율로 표시됩니다.
- 애니메이션 스타일 또는 CSS 속성: 애니메이션 중에 변경될 것으로 예상되는 속성입니다.

### 애니메이션 속성

`@keyframe`이 정의되면 애니메이션이 작동하려면 애니메이션 속성을 추가해야 합니다.

이것은 주로 애니메이션이 발생하는 방식을 정의하는 데 사용됩니다.

애니메이션 속성은 애니메이션할 CSS 선택기(또는 요소)에 추가됩니다.

애니메이션의 효과를 확인하려면 두 가지 속성이 매우 중요합니다. `애니메이션 이름`과 `애니메이션 기간`이 그것이다.

다음과 같은 다른 속성이 있습니다.

- `기수-기수-함수`: 애니메이션의 속도 곡선 또는 속도를 정의합니다. 미리 정의된 타이밍 옵션인 이지, 선형, 이지인, 이지아웃, 이지인, 이니셜, 상속으로 타이밍을 지정할 수 있다.
- ➡-➡: 이 속성은 애니메이션의 시작 시기를 정의합니다. 값은 초 또는 밀리초(ms)로 정의됩니다.
- `백-백-백-백-백-백-백-백: 이 속성은 애니메이션을 재생해야 하는 횟수를 지정합니다.
- `직접 방향`: 이 CSS 속성은 애니메이션이 앞뒤로 시퀀스를 재생해야 하는지, 뒤로 재생해야 하는지, 또는 앞뒤로 번갈아 재생해야 하는지 여부를 설정합니다.
- `스위트 채우기 모드`: 이 속성은 애니메이션이 재생되지 않을 때(시작하기 전, 종료 후 또는 둘 다) 요소의 스타일을 지정합니다.
- `놀이 상태`: 이 속성은 애니메이션이 실행 중인지 일시 중지되었는지 여부를 지정합니다.

다음 중요한 질문은 다음과 같습니다. 요소를 애니메이션화하려면 언제든지 이 모든 속성을 지정해야 합니까?

사실은 그렇지 않아요.

우리는 애니메이션 속기물을 가지고 있다. 각 애니메이션 속성은 개별적으로 정의할 수 있지만 보다 깨끗하고 빠른 코드의 경우 애니메이션 속기를 사용하는 것이 좋습니다.

모든 애니메이션 속성이 동일한 애니메이션: 속성에 다음 순서로 추가됩니다.

```css
animation: [animation-name] [animation-duration] [animation-timing-function] 
[animation-delay] [animation-iteration-count] [animation-direction] 
[animation-fill-mode] [animation-play-state];
```

> 참고: 애니메이션이 올바르게 작동하려면 적절한 속기 순서를 따르고 처음 두 개 이상의 값을 지정해야 합니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/olawanlejoel/embed/LYNmBdP?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=olawanlejoel&amp;slug-hash=LYNmBdP&amp;pen-title=simple%20landing&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="simple landing" loading="lazy" id="cp_embed_LYNmBdP"></iframe></div>

여기 셔츠 가게를 위한 아주 간단한 랜딩 페이지가 있습니다.

저는 이 셔츠에 아주 작은 애니메이션을 추가하기로 했습니다. 그래야 이 링크를 방문하는 즉시 사용자의 관심을 끌 수 있습니다.

제가 한 일은 변환 속성을 적용하고 수직(위아래)으로 변환하는 것이었습니다. 천천히 코드를 확인하실 수 있습니다.

## 왜 자바스크립트인가?

이 항목을 자세히 읽으면서 왜 JavaScript가 이 항목에 포함되었는지 자문하기 시작할 수 있습니다. 이제 왜 그런지 알게 될 거야!

자, 왜 자바스크립트인가?

우리는 자바스크립트를 사용하여 CSS 애니메이션을 제어하고 작은 코드로 훨씬 더 낫게 만든다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_2" scrolling="no" src="https://codepen.io/olawanlejoel/embed/XWdVOZy?height=500&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=olawanlejoel&amp;slug-hash=XWdVOZy&amp;pen-title=Form%20Validation%20with%20Html%2C%20Css%20%26amp%3B%20Javascript&amp;name=cp_embed_2" style="width: 100%; overflow:hidden; display:block;" title="Form Validation with Html, Css &amp;amp; Javascript" loading="lazy" id="cp_embed_XWdVOZy"></iframe></div>

위의 코드에서 사용자 세부사항을 수집하기 위해 양식을 작성했지만, 입력이 없으면 양식 필드가 흔들렸으면 합니다.

CSS의 도움으로 나는 그것들을 흔들 수 있다.

```css
@keyframes inputMove {
    0% {
        transform: translateX(5px);
    }
    25% {
        transform: translateX(-5px);
    }
    50% {
        transform: translateX(5px);
    }
    75% {
        transform: translateX(-5px);
    }
    100% {
        transform: translateX(0px);
    }
}
```

위의 코드에서 입력 필드는 5px로 왔다갔다(왼쪽에서 오른쪽으로)한 다음 마침내 100%에서 초기 위치로 돌아온다(위의 코드에서 보듯이 CSS 변환 속성을 사용하여 달성).

그런 다음 애니메이션 속성을 CSS 선택기 오류에 추가합니다.

```css
.form-control.error input {
    border: 2px solid red;
    animation-name: inputMove;
    animation-duration: .5s;
}
```

다음 사항은 다음과 같습니다. 이 필드가 비어 있고 사용자가 제출 단추를 클릭하는지 어떻게 알 수 있습니까?

여기가 바로 자바스크립트가 나오는 곳입니다. 우리는 애니메이션을 제어하기 위해 JavaScript를 사용합니다.

1단계: 양식 제출 버튼을 클릭했는지 확인합니다.
2단계: 모든 양식 필드를 선택합니다.
3단계: 입력 필드가 비어 있는지 확인합니다.
4단계: JavaScript `classList` 속성을 사용하여 CSS 선택기를 추가합니다. 여기서 `classList` 속성에 대해 자세히 볼 수 있습니다.

> 참고: 임베디드 코드펜에 자바스크립트 및 CSS 코드에 코멘트를 적절히 추가해서 쉽게 이해할 수 있도록 했습니다.

양식이 모든 적절한 정보와 함께 제출되면 일부 버블이 위로 미끄러져 올라가기 시작합니다. 이것은 CSS 애니메이션으로 달성되었다.

## 결론

웹 애니메이션에 대해 알아야 할 몇 가지 사항입니다. 이것은 매우 광범위한 주제이지만 애니메이션의 중요성과 프로젝트에 CSS 애니메이션을 사용해야 하는 이유를 알고 있습니다.