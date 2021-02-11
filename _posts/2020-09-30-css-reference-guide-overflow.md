---
layout: post
title: "CSS 참조 가이드: 'overflow'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-overflow-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-overflow-nocdn.png?fit=730%2C487&ssl=1)

CSS `overflow` 속성은 요소의 콘텐츠가 상위 컨테이너에서 오버플로되거나 커지는 방식을 지정합니다. 요소의 너비 또는 높이가 요소의 둘러싸인 상위 요소보다 클 경우 요소의 x 및 y 방향 모두에서 오버플로가 발생할 수 있습니다.

#### 앞으로 이동:

- 가치
➡➡➡➡➡➡➡➡➡➡ ➡
숨김
auto
➡➡➡➡➡➡➡➡➡➡ ➡
`clip
- ➡➡➡➡➡➡➡➡➡➡ ➡
- 숨김
- auto
- ➡➡➡➡➡➡➡➡➡➡ ➡
- `clip
- ➡➡의 성질.
➡-x
➡-y
- ➡-x
- ➡-y
- "overflow-x" 및 "overflow-y" 모두 사용

너비 300px와 높이 700px의 부모 `div` 요소와 너비 500px와 높이 900px의 자식 요소가 있다고 하자. 자식 div 요소가 x 방향과 y 방향으로 부모 요소를 오버플로하는 것을 볼 수 있습니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;
}
```

아래의 실제 사례를 검토하십시오.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/XWdYvBr?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=philipsz-davido&amp;slug-hash=XWdYvBr&amp;pen-title=css%20overflow&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css overflow" loading="lazy" id="cp_embed_XWdYvBr"></iframe></div>

.child El은 x축과 y축 둘 다 200px만큼 넘치게 된다.

## 가치

오버플로 속성은 가시적, 은닉적, 자동적, 스크롤적, 클립적 등 위의 상황을 처리하는 데 사용할 수 있는 가치를 가지고 있다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

`overflow`의 기본값입니다. 이 값은 오버플로를 표시합니다. 오버플로우를 클리핑하지 않습니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow: visible;
}
```

.childEl의 오버플로우 내용은 잘리지 않습니다.

### 숨김

이 값은 하위 요소의 오버플로 영역/컨텐츠를 클리핑하거나 숨깁니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow: hidden;
}
```

위의 .childEl의 넘치는 내용은 숨겨질 것이다.

### auto

이렇게 하면 요소의 오버플로 콘텐츠에 스크롤 막대가 숨겨지고 제공됩니다. 내용이 오버플로되면 스크롤 막대가 나타납니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow: auto;
}
```

스크롤 막대가 .childEl에 나타납니다.

너비가 상위 너비에 오버플로되지 않도록 설정한 경우:

```undefined
...
.childEl {
    width: 200px;
    ...

    overflow: auto;
}
```

그러면 스크롤 막대가 맨 아래에 표시되지 않습니다. 스크롤 막대는 오른쪽에만 표시됩니다.

### ➡➡➡➡➡➡➡➡➡➡ ➡

이렇게 하면 오버플로우된 내용을 숨기거나 클립하지만 오버플로우된 내용을 스크롤하여 표시하거나 나머지 내용을 볼 수 있습니다. 내용이 오버플로되지 않더라도 요소에 스크롤 막대가 자동으로 나타납니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow: scroll;
}
```

.childEl의 넘치는 내용은 숨겨지고 오른쪽과 아래쪽에 스크롤 막대가 나타난다. 마우스나 키를 사용하여 스크롤하여 오버플로우된 콘텐츠를 표시할 수 있습니다.

### 'clip

숨김이 있는 오버플로우 콘텐츠를 프로그래밍 방식으로 스크롤할 수 있다는 점을 제외하면 숨김과 유사하다. 모든 스크롤을 금지하는 `clip`은 그렇지 않다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow: clip;
}
```

.childEl의 오버플로우 내용은 숨겨지며 JS를 사용하여 스크롤할 수 없습니다.

> N.B. 'overflow: clip;'은 작성 당시에도 여전히 실험적인 것으로 대부분의 주요 브라우저의 지원을 받지 못하고 있다.

## ➡➡의 성질.

➡은 ➡-x와 ➡-y의 줄임말로 ➡-x와 ➡-y를 사용할 때 ➡-x와 ➡-y 속성을 동시에 설정한다는 뜻이다.

`ㄹ`은 한두 개의 값을 가질 수 있습니다. 값이 하나만 있으면 오버플로우 x와 오버플로우 y가 모두 값을 갖습니다.

"overflow: auto;"가 있다면 다음과 같습니다.

```undefined
overflow-x: auto;
overflow-y: auto;
```

두 개의 값이 있으면 첫 번째 값은 `overflow-x`가 되고 두 번째 값은 `overflow-y`가 됩니다.

`overflow: auto hidden;`이 있으면 다음과 같습니다.

```undefined
overflow-x: auto;
overflow-y: hidden;
```

### ➡-x

이렇게 하면 요소의 왼쪽과 오른쪽이 대상이며 오버플로우된 콘텐츠가 처리되는 방식을 제어할 수 있습니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow-x: hidden;
}
```

위의 예에서는 오른쪽에 있는 .childEl의 오버플로우 내용을 숨깁니다. 맨 아래 부분의 오버플로 콘텐츠는 규칙의 영향을 받지 않습니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow-x: scroll;
}
```

위의 예에서는 스크롤 막대를 `.childEl`의 오른쪽에만 추가합니다.

### ➡-y

이렇게 하면 요소의 상단과 하단 부분이 대상이며 오버플로우된 콘텐츠가 처리되는 방법을 제어합니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow-y: hidden;
}
```

이렇게 하면 `.childEl`의 넘쳐나는 하단 부분만 숨겨집니다.

## "overflow-x" 및 "overflow-y" 모두 사용

두 속성을 함께 사용하여 내용의 상단 및 하단 부분에 영향을 줄 수 있습니다.

```undefined
.parentEl {
    width: 300px;
    height: 700px;
    border: 1px solid orangered;
}

.childEl {
    width: 500px;
    height: 900px;
    border: 1px solid black;

    overflow-x: hidden;
    overflow-y: scroll;
}
```

이렇게 하면 넘치는 오른쪽이 숨겨지고 스크롤 막대가 .childEl의 하단에 추가됩니다.