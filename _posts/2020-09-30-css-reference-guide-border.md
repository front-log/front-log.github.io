---
layout: post
title: "CSS 참조 가이드: '경계'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-border.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-border.png?fit=730%2C487&ssl=1)

CSS `border` 속성은 요소의 테두리 두께, 스타일 및/또는 색상을 설정하는 데 사용되는 단축키입니다.

#### 앞으로 이동:

- 구문
- 구성 요소 속성
테두리 색
국경식
`경계
- 테두리 색
- 국경식
- `경계

CSS의 요소는 박스 모델로 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/css-box-model.png?resize=566%2C400&ssl=1)

위에서 요소의 테두리가 요소의 내용 및 패딩을 둘러싸는 하나 이상의 선에 불과하다는 것을 알 수 있습니다. 요소의 내용이 패딩에서 중지되므로 요소의 테두리가 여백으로 확장되지 않습니다. 예를 들어 보겠습니다.

```undefined
border: 1px solid green;
```

이렇게 하면 대상 요소의 테두리 두께가 1px, 테두리 모양이 솔리드, 색상이 녹색으로 설정됩니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/zYqWmQJ?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=philipsz-davido&amp;slug-hash=zYqWmQJ&amp;pen-title=css%20borders%20example&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css borders example" loading="lazy" id="cp_embed_zYqWmQJ"></iframe></div>

## 구문

`border`의 일반 구문은 다음과 같습니다.

```xml
border: <border-width> <border-style> <border-color>;
```

일부 값은 `border`에서 생략할 수 있으며, 이로 인해 CSS 엔진이 초기 값을 할당하게 된다.

#### 단일 값:

```undefined
border: <border-style>
```

border가 단일 값만 할당되면 【border-style】이 되어야 하며, border(경계형) 그런 다음 CSS는 테두리 너비(`medium`)와 색상(`current color`)의 초기 값을 취합니다. 너비는 약 3px로 계산되며 색상은 요소의 텍스트 색상 또는 전경색이 됩니다.

```undefined
border: solid;
```

여기 테두리 스타일은 견고합니다. 그런 다음 CSS는 테두리 폭을 3px인 medium(중간)으로 설정하고, 색상이 자체 설정되어 있지 않거나 상위 요소에서 상속된 색상이 없는 경우 검은색(rgb(0, 0, 0), 기본 색상인 검은색으로 설정합니다. 그렇기 때문에 요소 주위에 3px의 검은색 테두리가 나타납니다.

```undefined
border: solid;
color: red;
```

위에서 요소의 색상을 빨간색으로 설정합니다. 테두리는 가로 3px, 세로 3px, 빨간색이 될 것이다. 보시는 바와 같이 요소의 색상 또는 상위 요소의 상속된 색상이 설정되지 않은 경우 요소의 테두리 색상에 영향을 미칩니다.

#### 두 가지 값:

```xml
border: <border-width> <border-style>
```

이 구문은 테두리 두께와 스타일을 설정합니다. 그런 다음 CSS는 요소의 색상 속성 또는 상속된 색상 속성에서 테두리 색상을 계산합니다.

```undefined
border: 9px dotted;
```

요소의 테두리 두께는 9px이며 테두리 모양은 점선으로 표시됩니다. 그런 다음 테두리 색상은 요소의 색상 또는 요소의 상속된 색상이 됩니다. 위에서 설명한 대로, CSS는 설정/상속된 색상 속성이 없는 경우 기본 색상을 검은색으로 설정합니다.

```undefined
color: green;
border: 9px dotted;
```

이 경우 요소에 녹색으로 설정된 색상 속성이 있습니다. 그러면 요소의 테두리 색상은 녹색이 됩니다.

```xml
border: <border-style> <border-color>
```

위의 구문에는 테두리 스타일과 색상만 명시적으로 설정되어 있습니다. CSS는 `경계 폭`을 계산하고 약 3px 폭의 중간으로 설정한다.

```undefined
border: solid red;
```

이렇게 하면 요소의 테두리 스타일과 색상이 빨간색과 3px 너비로 설정됩니다.

## 구성 요소 속성

우리가 `border`의 유일한 속기 속성을 사용할 때마다, 우리는 세 가지 CSS border 속성을 설정하고 있다.

- 테두리 색
- 국경식
- `경계

### 테두리 색

이 속성은 요소의 테두리 색을 설정합니다.

```undefined
border-color: red;
```

테두리 색상은 빨간색으로 설정됩니다.

```undefined
border: 9px solid;
border-color: red;
```

첫 번째 규칙은 너비와 스타일을 설정하고 색상은 요소의 기본 텍스트 색상인 검은색으로 계산한다. 그러면 경계색상은 CSS에 기본 텍스트 색상을 무시하고 빨간색 값을 사용하도록 통보한다.

그래서 우리는 원소에 9px 폭의 견고한 빨간색 테두리를 가지고 있습니다.

`경계색`에는 단측 계산 속성이 있습니다.

- 테두리 톱 컬러
- 테두리 밑단색
- 국경우경색
- `경계 좌색`

개별적으로 정의되지 않으면 모두 테두리 색상의 가치를 취하게 된다.

### 국경식

이 속성은 테두리 모양을 스타일링합니다. 다음 10개의 값을 사용할 수 있습니다.

- `없음`: 테두리 두께와 색상이 설정되더라도 요소에 테두리가 없습니다.
- 숨김: 없음에 해당하지만 테이블에 적용했을 때 약간 다른 효과가 있습니다.
- ➡: 점선 경계 할당
- `dashed`: 점선 테두리를 할당합니다.
- solid: 손상되지 않은 견고한 경계선을 할당합니다.
- `double`double`: 이중 테두리를 할당합니다. 선의 폭과 선의 간격은 경계 폭의 값과 같습니다.
- ➡: 끌로 만든 것처럼 보이는 3D 홈 테두리를 할당합니다.
- `리지`: 가장자리에서 능선이 잘린 것처럼 보이는 3D 능선 테두리를 할당합니다.
- inset: 테두리 위, 왼쪽은 어두운 색을 띠며, 아래, 왼쪽은 밝은 색을 띠게 된다.
- ➡: 삽입의 반대쪽, 테두리 아래쪽과 왼쪽은 어두운 색, 오른쪽과 위쪽은 밝은 색입니다.

`border-style`에는 단측 계산 속성도 있습니다.

- 국경 최상급식
- 국경-바닥식
- 국경좌파식
- 국경우파식

이들이 개별적으로 정의되지 않으면 모두 국경형이라는 가치를 갖게 된다.

#### 다중 스타일 설정

우리는 `border-style`의 단측 속성을 사용하여 요소의 경계에 여러 스타일을 설정할 수 있다. 구문은 다음과 같습니다.

```xml
border-style: <top> <right> <bottom> <left>
```

값은 다음과 같이 할당됩니다.

```xml
//one value:
border-style: <top, bottom, right, left>

//two values:
border-style: <top, bottom> <right, left>

//three values:
border-style: <top> <right, left> <bottom>
```

예는 다음과 같습니다.

```undefined
border-style: solid dotted
```

이렇게 하면 경계선 상하부의 스타일이 견고하게 설정되고 좌우측도 점으로 설정된다.

```undefined
border-style: solid dotted dashed
```

이렇게 하면 위쪽은 탄탄하게, 아래쪽은 담그기로, 왼쪽과 오른쪽은 점으로 바뀐다.

### '경계

이 속성은 테두리 두께를 설정합니다. 다음 값을 사용할 수 있습니다.

- <<길이>: 사용자가 명시적으로 설정한 테두리 두께 값입니다. px, em, pt 등의 모든 단위로 사용할 수 있습니다.
- thin: ~1px
- medium: ~3px
- ➡: ~ 5px

> 참고: CSS는 위의 키워드에 대한 정확한 값을 정의하지 않습니다. 구현에 따라 결과가 달라질 수 있습니다.

`경계 너비`에는 단측 계산 속성도 있습니다.

- 테두리-톱 폭
- `경계선 하단 폭`
- `경계-좌파 폭`
- `경계선-오른쪽

개별적으로 정의되지 않으면 모두 테두리 너비의 값을 취하게 된다.