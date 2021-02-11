---
layout: post
title: "프레임워크에 구애받지 않는 JavaScript 플러그인을 생성하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/javascript-plug.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/javascript-plug.png?fit=730%2C487&ssl=1)

## 도입

JavaScript의 플러그인은 언어를 확장하여 우리가 원하는 강력한(또는 그렇게 강력하지 않은) 기능을 얻을 수 있도록 한다. 플러그인/라이브러리는 기본적으로 패키지 코드로, 동일한 기능(기능)을 반복적으로 쓰는 것을 막아줍니다.

그냥 플러그를 꽂고 놀아요!

자바스크립트 생태계에는 수백 개의 프레임워크가 있으며, 이러한 프레임워크 각각은 프레임워크에 새로운 것을 추가하기 위해 플러그인을 만드는 시스템을 우리에게 제공한다.

거의 모든 JavaScript 플러그인이 게시된 NPM 레지스트리를 살펴보면 100만 개 이상의 플러그인이 단순 라이브러리와 프레임워크로 게시됩니다.

각 프레임워크에 대한 플러그인을 생성하는 방법은 크게 다를 수 있습니다. 예를 들어 Vue.js에는 React.js용 플러그인을 생성하는 방법과 다른 자체 플러그인 시스템이 있습니다. 그러나 이 모든 것은 동일한 자바스크립트 코드로 귀결된다.

따라서 Vanilla JavaScript를 사용하여 플러그인을 생성할 수 있으면 해당 프레임워크에 관계없이 작동하는 플러그인을 만들 수 있습니다.

프레임워크에 구애받지 않는 자바스크립트 플러그인은 프레임워크의 컨텍스트 없이도 작동하는 플러그인입니다. 프레임워크 없이도 모든 프레임워크에서 플러그인을 사용할 수 있습니다."

### 라이브러리를 작성할 때 기억해야 할 사항:

- 플러그인에 대한 목표가 있어야 합니다. 플러그인이 달성하고자 하는 핵심 사항입니다.
- 플러그인은 의도한 용도로 사용하기 쉬워야 합니다.
- 플러그인은 크게 사용자 정의할 수 있어야 합니다.
- 플러그인에는 플러그인을 사용할 개발자를 안내하는 문서가 있어야 합니다.

이제 위의 사항들을 염두에 두고 사업을 시작하겠습니다.

## 만들 내용

이 기사에서는 프레임워크에 구애받지 않는 플러그인을 만드는 방법을 보여드리겠습니다. 이 튜토리얼의 목적을 위해, 우리는 플러그인의 목표인 회전식/슬라이더 플러그인을 만들 것이다.

이 플러그인은 플러그인 `.next() 및 `.prev()`의 사용자가 호출할 수 있는 몇 가지 방법을 표시합니다.

## 시작하기

- 플러그인 코드와 기타 필요한 파일을 저장할 새 폴더를 생성합시다. 내 폴더를 `TooSlidePlugin`이라고 부를게요.`
- 이 폴더에서 즐겨찾는 편집기에 새 JavaScript 파일을 만듭니다. 이 파일에는 플러그인의 코드가 포함됩니다. 나는 내 것을 tooSlide.js라고 부를 것이다.

저는 때때로 플러그인을 만들기 전에 (최종 개발자의 관점에서) 플러그인이 어떻게 사용될지 상상하고 싶습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/var-slide.png?resize=730%2C521&ssl=1)

위의 코드 블록을 보면 특정 속성을 가진 객체를 인수로 수신하는 `TooSlides`라는 생성자가 있다고 가정합니다.

개체의 속성은 슬라이드 클래스, 컨테이너, 다음 버튼 및 이전 버튼입니다. 사용자가 사용자 정의할 수 있는 속성입니다.

먼저 플러그인이 네임스페이스를 가질 수 있도록 단일 생성자 함수로 만드는 것으로 시작할 것입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/function-too-slides.png?resize=730%2C454&ssl=1)

### 옵션들

우리의 플러그인인 `TooSlides`는 옵션 인수를 기대하기 때문에, 우리는 몇몇 기본 속성을 정의하여 만약 우리 사용자가 그들 자신의 것을 지정하지 않는다면, 기본 속성을 사용할 것이다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/code-snippet-funciton.png?resize=730%2C365&ssl=1)

일부 속성을 보유하기 위해 `defaultOptions` 개체를 만들었으며, 또한 자바스크립트 스프레드 연산자를 사용하여 수신 옵션을 기본 옵션과 병합했습니다.

우리는 이것을 다른 변수에 할당하여 내부 기능에서 계속 접근할 수 있도록 하였다.

슬라이더로 사용할 이미지를 모두 담을 슬라이드와 현재 슬라이드 두 변수도 만들었다.Index(색인)는 현재 표시되는 슬라이드의 인덱스를 보유하고 있습니다.

다음으로, 슬라이더에는 슬라이드를 앞뒤로 이동하는 데 사용할 수 있는 컨트롤이 있어야 하므로, 다음과 같은 방법을 생성자 기능에 추가할 것입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/this.preparecontrols.png?resize=730%2C594&ssl=1)

.prepareControls() 방법에서는 제어 버튼을 고정할 컨테이너 DOM 요소를 생성했습니다. 우리는 다음 버튼과 이전 버튼을 직접 만들어 컨트롤 컨테이너에 추가했다.

그런 다음 이벤트 수신기를 각각 .next()와 .이전() 메서드라고 하는 두 개의 버튼에 연결합니다. 걱정하지 마십시오. 곧 이러한 방법을 만들 것입니다.

다음으로, `.GoSlide()`와 `.hideOtherSlide()의 두 가지 방법을 추가하겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/this.gotoslide.png?resize=730%2C474&ssl=1)

.goToSlide() 메서드는 표시할 슬라이드의 인덱스인 인덱스(index) 인수를 사용합니다. 이 메서드는 먼저 현재 표시되는 슬라이드를 숨긴 다음 표시할 슬라이드만 표시합니다.

다음에는 앞으로 한 단계, 뒤로 한 단계(앞서 첨부한 이벤트 청취자 기억)에 도움이 되는 .next()와 .이전() 도우미 방법을 추가하겠습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/this.nextfunction.png?resize=730%2C549&ssl=1)

이 두 가지 방법은 기본적으로 .goToSlide() 메서드를 호출하고 현재 슬라이드를 이동합니다.지수 1씩

이제 생성자 기능이 인스턴스화될 때마다 설정을 도와주는 .init() 방식도 만들 것이다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/this.initfunction.png?resize=730%2C342&ssl=1)

보시는 것처럼 `.init()` 방법은 모든 슬라이드 이미지를 가져와 앞에서 선언한 슬라이드 배열에 저장하고 기본적으로 모두 숨깁니다.

그런 다음 `.goToSlide(0)` 메서드를 호출하여 슬라이드에 첫 번째 이미지를 표시하고 `.prepareControls()를 호출하여 제어 버튼을 설정합니다.

생성자 코드를 마무리하기 위해 생성자 내에서 .init() 메서드를 호출하여 생성자가 초기화될 때마다 항상 .init() 메서드가 호출됩니다.

최종 코드는 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/long-code-snippet.png?resize=730%2C1397&ssl=1)

## CSS 추가 중

플러그인 프로젝트가 들어 있는 폴더에 슬라이더를 위한 기본 스타일링이 포함된 CSS 파일을 추가할 것입니다. 이 파일을 tooSlide.css라고 합니다.

```css
* {box-sizing: border-box}
 
body {font-family: Verdana, sans-serif; margin:0}
.too-slide-single-slide {
    display: none; 
    max-height: 100%;
    width: 100%; 
    
}
 
.too-slide-single-slide img{
    height: 100%;
    width: 100%;
}
img {vertical-align: middle;}
 
/* Slideshow container */
.too-slide-slider-container {
    width: 100%;
    max-width: 100%;
    position: relative;
    margin: auto;
    height: 400px;
}
 
 
.prev, .next {
  cursor: pointer;
  position: absolute;
  top: 50%;
  width: auto;
  padding: 10px;
  margin-right: 5px;
  margin-left: 5px;
  margin-top: -22px;
  color: white;
  font-weight: bold;
  font-size: 18px;
  transition: 0.6s ease;
  border-radius: 0 3px 3px 0;
  user-select: none;
  border-style: unset;
  background-color: blue;
}
 
 
.next {
  right: 0;
  border-radius: 3px 0 0 3px;
}
 
 
.prev:hover, .next:hover {
  background-color: rgba(0,0,0,0.8);
}
 
 
 
.too-slide-fade {
  -webkit-animation-name: too-slide-fade;
  -webkit-animation-duration: 1.5s;
  animation-name: too-slide-fade;
  animation-duration: 1.5s;
}
 
@-webkit-keyframes too-slide-fade {
  from {opacity: .4} 
  to {opacity: 1}
}
 
@keyframes too-slide-fade {
  from {opacity: .4} 
  to {opacity: 1}
}
 
/* On smaller screens, decrease text size */
@media only screen and (max-width: 300px) {
  .prev, .next,.text {font-size: 11px}
}
```

## 플러그인 테스트 중

플러그인을 테스트하기 위해 HTML 파일을 만들 것입니다. 제 것을 index.html이라고 부를게요. 또한 플러그인 JavaScript 코드와 동일한 디렉토리에 슬라이드로 사용할 4개의 이미지를 추가합니다.

내 HTML 파일은 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/doctype.png?resize=730%2C548&ssl=1)

HTML 파일의 헤드 섹션에서 tooSlide.css 파일을 호출했고, 파일 끝에 tooSlide.js 파일을 호출했습니다.

이렇게 하면 플러그인 생성자를 인스턴스화할 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/Our-final-scritp-file..png?resize=730%2C522&ssl=1)

이 펜에서 플러그인의 결과를 확인할 수 있습니다.

### 플러그인 문서화

플러그인에 대한 설명서는 다른 모든 부분만큼 중요합니다.

이 설명서는 플러그 인을 사용하는 방법을 설명합니다. 따라서, 그것에 대해 좀 더 생각해 볼 필요가 있다.

새로 생성한 플러그인의 경우 프로젝트 디렉터리에 README.md 파일을 생성하는 것으로 시작하겠습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tooslides-1.png?resize=1215%2C606&ssl=1)

### 플러그인 게시:

플러그인을 작성한 후 다른 개발자가 새로 생성되어 이익을 얻기를 원할 수 있으므로, 이 방법을 보여드리겠습니다.

다음 두 가지 주요 방법으로 다른 사용자가 플러그인을 사용할 수 있도록 할 수 있습니다.

- Github에서 진행하세요. 이렇게 하면 누구나 레포를 다운로드하고 파일(.js 및 .css)을 포함하며 플러그인을 즉시 사용할 수 있습니다.
- npm에 게시하세요. 안내할 공식 npm 문서를 확인하십시오.

그리고 그게 다야.

## 결론

이 기사의 과정 동안, 우리는 슬라이드 이미지라는 한 가지 일을 하는 플러그인을 만들었습니다. 그것은 또한 의존성이 없다. 이제 우리도 도움을 받은 것처럼 우리의 코드를 사용하여 다른 사람들을 도울 수 있습니다.

이 플러그인 튜토리얼의 코드는 github에서 사용할 수 있다.