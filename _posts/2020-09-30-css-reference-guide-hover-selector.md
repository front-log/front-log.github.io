---
layout: post
title: "CSS 참조 가이드: CSS ':hover' 선택기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-hover-selector-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-hover-selector-nocdn.png?fit=730%2C487&ssl=1)

CSS `:hover` 선택기는 마우스를 요소 위로 이동할 때 요소에 스타일을 적용하는 데 사용되는 의사 클래스 선택기입니다.

구현 예는 다음과 같습니다.

```undefined
a:hover {
    color: orangered;
}
```

우리는 모든 a 요소에 `:hover` 의사 선택기를 적용했다. 이렇게 하면 마우스가 요소 위를 맴돌 때 a 요소의 텍스트 색상이 주황색으로 바뀝니다.

아래의 CodePen에서 라이브 예제로 재생할 수 있습니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="365" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/XWdYvGJ?height=365&amp;theme-id=dark&amp;default-tab=css%2Cresult&amp;user=philipsz-davido&amp;slug-hash=XWdYvGJ&amp;pen-title=css%20%3Ahover&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css :hover" loading="lazy" id="cp_embed_XWdYvGJ"></iframe></div>

또한 차체 내 모든 요소에 `:hover` 선택기를 적용할 수 있습니다.

```undefined
body *:hover {
    background: yellow;
}
```

이렇게 하면 마우스가 요소 위로 이동할 때 신체 요소 내부의 모든 요소가 노란색 배경을 표시합니다.

배경 외에도 테두리, 테두리 반지름, 글꼴, 텍스트 색상, 패딩, 여백 등을 변경할 수 있습니다.

문서의 모든 요소에 `:hover` 선택기를 적용하려면 별표만 사용합니다.

```undefined
*:hover {
    border: 1px solid orangered;
}
```

이로 인해 후미진 원소들의 테두리는 폭이 1px이고, 단색의 빨간색-주황색이 된다.