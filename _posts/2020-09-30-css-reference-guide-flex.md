---
layout: post
title: "CSS 참조 가이드: '가변'"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-flex.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/lr-css-reference-guide-flex.png?fit=730%2C487&ssl=1)

CSS 플렉스 속성은 유연한 항목에 플렉스-grow, 플렉스-shrink, 플렉스-basis 속성을 설정하는 데 사용되는 약어입니다. 이러한 속성은 상위 컨테이너 내부의 유연성과 함께 유연한 항목의 정렬 및 정렬 방법을 결정합니다.

#### 앞으로 이동:

- 구문
- 규칙.
- 구성 요소 속성
유연한 성장
굴곡 있는
굴곡 있는
- 유연한 성장
- 굴곡 있는
- 굴곡 있는

여기서 예제를 검토합니다.

<div class="cp_embed_wrapper"><iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="500" width="100%" name="cp_embed_1" scrolling="no" src="https://codepen.io/philipsz-davido/embed/XWdEPQM?height=500&amp;theme-id=dark&amp;default-tab=html%2Cresult&amp;user=philipsz-davido&amp;slug-hash=XWdEPQM&amp;pen-title=css%20flex%20examples&amp;name=cp_embed_1" style="width: 100%; overflow:hidden; display:block;" title="css flex examples" loading="lazy" id="cp_embed_XWdEPQM"></iframe></div>

## 구문

flex 속성의 일반적인 구문은 다음과 같습니다.

```xml
flex: <flex-grow> <flex-shrink> <flex-basis>
```

플렉스 항목의 `Flex-grow` 계수를 정의합니다. 그것의 가치는 단위가 없고 `<숫자>이다. 초기값은 0입니다.

플렉스 항목의 `변형 축소` 계수를 정의합니다. 그것의 가치는 단위가 없고 `<숫자>이다. 초기 값은 1입니다.

플렉스 항목의 초기 길이를 정의합니다. 이 값은 절대적인 ><백분율> 또는 calc() 결과 또는 키워드 auto()로 지정됩니다. 초기값은 auto입니다.

## 규칙.

유연한 속성은 하나, 둘 또는 세 개의 값을 가질 수 있습니다. 규칙은 다음과 같습니다.

#### 단일 값:

- 하나의 값이 이와 같은 단위 없는 `<숫자>인 경우:
유연성: 3;
그런 다음 "Flex-grow"에 값이 할당됩니다. Flex-contract는 기본값이 1이고, Flex-contract는 생략하면 0으로 가정한다.
유연성: 3;

// 폭발 위치:

굴곡: 3 10

// 또는

Flex-grow:
굴곡부: 1
굴곡: 0
- 하나의 값이 <백분율> 또는 <길이>이면 그것은 `변형 기준`이 된다. 생략하면 Flex-grow 및 Flex-contract 기본값이 1로 설정됩니다.
유연성: 3%

// 폭발 위치:
유연성: 13%

// 또는
Flex-grow: 1 Flex-grow: 1 Flex-grow: 3%
유연성: 3px

// 폭발 위치:

유연성: 13px

// 또는
Flex-grow: 1
굴곡부: 1
플렉시블: 3px

#### 두 가지 값:

- flex 속성에 설정된 두 값이 모두 unitless <number>이면 flex-grow는 첫 번째 값을, flex-shrink는 두 번째 값을 갖는다. Flex-contract는 0으로 가정합니다.
유연성: 42

// 폭발 위치:

유연성: 421%
- 그 중 하나가 <백분위수>이고 다른 하나가 <백분위수>라면 <백분위수>는 <백분위수> 값을, 그리고 <백분위수>는 다른 값을 취한다. flex-contract는 1로 기본 설정됩니다.
유연성: 4% 5

// 폭발 위치:

유연성: 514%
유연성: 63%

// 폭발 위치:

유연성: 63%

#### 세 가지 값:

- 이 경우 정규 구문이 사용됩니다. flex 속성의 값은 순서가 맞지 않을 수 있으므로 백분율 값이 첫째 또는 둘째로 설정된 경우를 찾을 수 있습니다. 아래 CSS는 첫 번째 숫자를 Flex-grow로, 두 번째 숫자를 Flex-shrink로, 그 다음 백분율 값을 Flex-bas로 사용합니다.
유연성: 4% 56

// 등가:

유연성: 564%
유연성: 54% 6

// 등가:

유연성: 564%

## 구성 요소 속성

### 유연한 성장

플렉스 컨테이너에 사용 가능한 공간이 있을 경우 플렉시블 항목이 커질 범위를 정의합니다. 성장률은 동일한 플렉스 컨테이너에서 다른 유연한 품목의 성장에 비례합니다.

```undefined
flex-grow: 0
flex-grow: 0
flex-grow: 0
```

위의 플렉스 아이템은 성장인자가 0이기 때문에 공간을 채우지 못합니다. 항목 중 하나를 1의 유연한 성장으로 설정하면:

```undefined
flex-grow: 0
flex-grow: 0
flex-grow: 1
```

세 번째 항목은 오른쪽 공간을 채우기 위해 늘어나거나 커질 뿐입니다.

두 번째 항목도 1의 유연한 성장으로 설정할 수 있습니다.

```undefined
flex-grow: 0
flex-grow: 1
flex-grow: 1
```

두 번째와 세 번째 항목은 오른쪽으로 공간을 채우기 위해 늘어나지만 두 항목 사이에 균등하게 분배됩니다. 세 번째 아이템은 이전만큼 큰 공간을 차지하지 않을 것입니다. 사용 가능한 공간은 항목 2와 항목 3으로 나눈 후 채워집니다.

모든 항목을 1의 유연한 성장으로 설정하는 경우:

```undefined
flex-grow: 1
flex-grow: 1
flex-grow: 1
```

사용 가능한 공간은 세 개로 나눠서 비례해서 채워줄 거예요.

세 번째 항목을 Flex-grow 값 2로 설정하면:

```undefined
flex-grow: 1
flex-grow: 1
flex-grow: 2
```

세 번째 항목은 첫 번째 항목과 두 번째 항목의 두 배의 공간을 차지합니다.

이것은 1:1:2의 간단한 비율이다. 비율의 합은 `(1 + 1 + 2) = 4`입니다.

채울 공간이 700px인 경우 첫 번째 항목은 `(1/4 * 700)` 또는 175px를 차지합니다. 마찬가지로 두 번째 항목은 175px, 세 번째 항목은 175px보다 두 배 긴 `(2/4 * 700) = 350px`를 차지하게 됩니다.

### 굴곡 있는

이것은 형제 플렉스 항목과 비교하여 플렉스 컨테이너에 사용 가능한 공간이 없을 경우 플렉스 항목이 축소되는 범위를 정의합니다.

우리가 700px 폭의 플렉스 컨테이너를 가지고 있고, 300px 폭의 플렉스 아이템이 세 개 있다고 가정해 보자. 이 수치로 미루어 볼 때, 플렉스 항목(특히 세 번째 플렉스 항목)이 상위 컨테이너에 넘쳐날 것으로 확신합니다.

세 번째 항목에 플렉스 축소 기능을 사용하면 플렉스 컨테이너에 맞게 축소됩니다. 본질적으로 세 번째 항목은 플렉시블 축소 값에 따라 좁아지거나 짧아진다.

항목에 대해 다음과 같은 `가변 축소` 값을 설정하는 경우:

```undefined
flex-shrink: 0
flex-shrink: 0
flex-shrink: 1
```

세 번째 항목은 1배 축소됩니다. Flex-grow에서 보았듯이, 이 값들은 과잉 오버플로가 0:0:1의 유연한 항목들 사이에서 어떻게 분할될 것인지를 말해주는 비율이다.

초과 오버플로우는 `900px - 700px = 200px`입니다. 따라서 세 번째 항목은 플렉스 컨테이너에 장착되기 위해 300px에서 100px로 200px 축소해야 합니다.

두 번째 항목의 `가축 축소`가 2로 설정된 경우:

```undefined
flex-shrink: 0
flex-shrink: 2
flex-shrink: 1
```

그러면 두 번째 항목이 세 번째 항목보다 두 배나 줄어들게 됩니다. 과잉 오버플로가 200px인 것을 알고 있기 때문에 두 번째 항목은 (2/3 * 200) = 133.3px를 흘려 166.6px가 될 것이다. 세 번째 아이템은 (1/3 * 200) = 66.6 px가 빠지기 때문에 폭은 233.3 px가 될 것이다.

`300 + 166.6 + 233.3 = 699의 세 항목을 모두 합친다.9" — 약 700px.

### 굴곡 있는

이 속성은 플렉스 항목의 크기를 설정합니다. 사용 가능한 공간을 채우기 위해 성장하거나 오버플로를 방지하기 위해 축소한 후 플렉스 항목의 크기에 추가됩니다.

유연한 항목 크기가 계산되어 사용할 공간 크기에 추가된 다음 다른 항목 크기를 계산합니다.

폭이 700px인 플렉스 컨테이너가 있다고 가정합시다. 이 제품에는 3개의 플렉스 항목이 포함되어 있으며 모두 플렉스 성장 값이 1이고 아래의 플렉스 기준 값이 포함되어 있습니다.

```undefined
flex-basis: 0
flex-basis: 2%
flex-basis: 3%
```

백분율 값은 유연한 항목에 부모 너비의 2% 또는 3%를 증가시키도록 지시합니다. Flex-grow 1이면 모든 플렉스 항목이 사용 가능한 공간을 동일하게 공유하게 됩니다. (700 / 3) = 233.3px

그래서 두 번째 항목은 231px, 세 번째 항목은 236px, 그리고 첫 번째 항목은 225px로 성장할 것입니다.