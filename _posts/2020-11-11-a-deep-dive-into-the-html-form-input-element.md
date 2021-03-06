---
layout: post
title: "HTML 양식 입력 요소에 대한 심층 분석"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/html-5.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/html-5.png?fit=730%2C487&ssl=1)

MDN Web Docs에 따르면 HTML ml 요소는 정보 제출을 위한 대화형 제어가 포함된 문서 섹션을 나타냅니다. 웹 개발자인 우리가 사용자들에게 정보를 요구할 때마다, 우리는 그들에게 `<형식>` 요소를 제시한다.

상위 레벨에서는 사용자의 전자 메일 주소와 암호가 포함된 일반 로그인 양식 또는 중요한 작업을 이중으로 검사하는 보안 강화 이중 인증(2FA) 양식일 수 있습니다. 어느 쪽이든 양식은 우리의 요구사항과 사용자의 소유물 사이의 계약적 인터페이스를 나타낸다.

<형식> 요소의 기본 구성 요소는 그것의 통제이다. 이러한 제어는 `<input>`와 같은 기본적인 대화형 요소부터 다른 여러 제어장치를 그룹화하는 데 사용되는 `<fieldset>와 같은 구조적 요소에 이르기까지 다양하다. 다른 제어 장치로는 클릭 동작을 수행하기 위한 <버튼>과 다른 컨트롤을 설명하기 위한 <라벨>과 같은 제어 장치가 있다.

모든 제어장치 중에서 입력은 아마도 가장 변형되고 가장 많이 사용될 것이다. 또는 MDN에 문서화된 바와 같이, 입력 유형과 특성의 순수한 조합으로 인해 모든 HTML 요소에서 가장 강력하고 복잡한 요소 중 하나이다.

### 【입력】요소의 복잡성에 관한 짧은 글

<input>의 진정한 복잡성을 이해하는 것은 웹 양식의 주요 목적을 이해하기 위해 한 걸음 뒤로 물러나는 것이다. 즉 사용자와 웹 사이트 간의 상호 작용을 촉진하고 인간과 컴퓨터 간의 상호 작용을 더욱 근본적으로 육성하기 위해서이다. 이러한 종류의 상호작용은 인간-컴퓨터 상호 작용(HCI)의 다원적 분야를 일으킬 만큼 충분히 복잡하고 복잡하다.

여기서 중요한 점은 사용자가 웹 사이트와 상호 작용할 때 기본적으로 그래프QL에서 기본 CRUD(작성, 읽기, 업데이트, 삭제 작업 쿼리, 돌연변이 및 구독) 중 하나를 수행한다는 것이다. 사용자에게 양식을 제공할 때 기본적으로 "대화 시스템"을 통해 사용자와 상호 작용합니다.

웹 양식과 관련하여 HCI를 말할 때, 우리는 종종 사용성과 보안의 길로 인도된다. 많은 웹 사이트에서처럼 기본 양식 요소를 보강할 필요가 없는 경우가 많으며, 수동으로 유효성 검사를 강화할 필요도 없지만, 일반 API를 제공하려고 시도한다고 해서 모든 사람의 최고, 구체적이고 안전한 요구를 충족시킬 수는 없습니다.

이 대화 시스템에서 작업할 때 사용자에게서 얻으려는 데이터 유형은 사용하는 `<입력>` 유형에 따라 달라지며, 학습한 바와 같이 데이터 유형과 입력 유형은 상호 배타적일 수 있습니다.

그럼에도 불구하고 <inputs>의 유연성은 여전히 다른 형식 통제보다 뛰어나다. 대부분의 모든 상호 작용에 대해 사용자가 원하는 데이터 유형을 얻을 수 있는 `<input>`이 있는 경우가 많습니다. 또한 사용자 입력이 이미 가려진 `암호` 입력과 같이 재와이어할 필요가 없는 몇 가지 기본값이 있습니다. 그리고 원하는 대로 하기 위해 특정 입력의 기능을 확장해야 할 때도 있습니다.

<입력> 제어의 유연성은 그것을 강력하면서도 복잡하게 만들며, 그것이 웹 양식의 목적을 달성하기 위해 사용되는 제어의 하나라는 사실과 결합되면, 당신은 그것을 옳고 그름으로 할 수 있는 많은 방법을 기대할 수 있다.

### <입력>형

다른 모든 HTML 요소와 마찬가지로, `<input> 요소는 특정한 필요에 따라 구성하는 데 사용할 수 있는 여러 가지 속성을 가지고 있다. 사용 가능한 모든 속성 중에서 유형 특성이 가장 중요한 이유는 `입력`에 ID를 부여하기 때문이다. 명시적으로 지정되지 않았거나 지원되지 않는 경우 또는 존재하지 않는 유형을 지정한 경우 형식은 `텍스트`로 기본 설정됩니다. 즉, = 또는 = 입력 형식="문자"는 기술적으로 = 입력 형식="문자"와 동일합니다.

기술적으로, 모든 입력은 HTMLinputElement 인터페이스를 공유하며, 확장적으로, 동일한 속성 집합을 가지고 있다.

그러나 이러한 속성은 기능적으로 사용되는 입력의 `유형`에 따라 달라집니다. 예를 들어 text와 checkbox 입력 모두 autocomplete 속성을 가지지만 text 입력에서 작동하는 동안에는 checkbox에서 무시된다.

또한 `<input>`의 기능, 설계 및 가용성은 기기 및 사용자 에이전트에 따라 달라진다.

예를 들어, 전화 입력(`=입력 유형=`tel`)을 예로 들 수 있습니다. 전화처럼 화상/가상 키패드가 지원되는 장치에 전화 키패드를 표시하며, 화면 키패드가 없는 장치에 사용할 경우 텍스트 입력보다 실질적인 이점이 없다.

가상 키패드를 구성 또는 사용자 지정하는 방법과 사용하는 유형도 가상 키패드에 따라 달라질 수 있습니다.

type 속성 외에도 required(양식을 제출하기 전에 사용자가 해당 컨트롤에 대한 값을 제공해야 함을 나타냄) 또는 placeholder(`input`을 설명하는 힌트를 나타냄)와 같은 다른 중요한 속성이 있다.

그러나 유형 속성이 가장 중요한데, 이는 단순히 `입력`이 정체성을 부여하기 때문만이 아니라 `입력`을 복잡하게 만드는 순수한 조합의 수에 기여하기 때문이기도 하고, `입력`의 기능적 의존성 때문이기도 하다.

사용되지 않는 `datetime` 유형을 제외하고, 아래 표에 표시된 것처럼 현재 총 22개의 입력 유형이 있습니다.

이러한 `유형`에 대해 자세히 알아보기 전에 다음과 같은 몇 가지 사항을 기억해야 합니다.

- 클라이언트측 검증은 서버측 검증의 대체물이 아닙니다. 이러한 `유형`이 제공하는 검증의 종류에 관계없이, 정보를 모르는 사용자에게만 충분하다고 간주한다.
- 자동으로 유효성을 검사하는 입력의 경우 `:valid` 및 `:valid` CSS 유사 클래스가 이에 따라 적용됩니다.
- 자동으로 유효성을 검사하고 빈 공간을 유효한 것으로 받아들이는 입력의 경우 `필수` 특성을 사용하여 무효화할 수 있습니다.

### '<입력 유형="숨김" >

이것은 비교적 간단합니다. 시각적 표현은 전혀 없지만 DOM에 항상 존재하므로 상호 작용할 수 있습니다.

사용자가 양식을 제출할 때 추가 데이터를 전송하는 데 사용되는 경우가 많지만, 요즘은 실제 또는 실제 용도로 사용되는 것은 아닌지 의심스럽다. 또한 사용자가 볼 수 없다고 해서 DOM의 값을 변경할 수 없는 것은 아닙니다. 따라서 중요한 데이터에 사용할 경우 취약성에 노출될 수 있습니다. 하지 마세요.

### = 입력 유형="text"와 = 입력 유형="search"

텍스트 입력은 아마도 모든 입력 입력 중 가장 보편적이고 유연한 입력일 것이다. 밀어넣기(그럴 경우)가 발생하면 특정 입력은 브라우저에서 지원되지 않습니다. 날짜 입력처럼 텍스트 입력은 대개 가을 남자 또는 영웅이다.

기본 단일 줄 텍스트 필드를 렌더링합니다.

```bash
<input type="text" />
```

가치는 DOM 문자열로 자리 표시자, max length, min length 등 다양한 특성을 지원한다.

```bash
<input type="text" placeholder="Beetwen 1 and 30 characters" minength="1" maxlength="30" />
```

위의 텍스트 입력은 길이가 최소 "min", "1, 최대 "max length"인 "값"이 됩니다. 빈 공간은 유효합니다. 이 경우 `패턴` 특성을 사용하여 RegExp 패턴을 적용하거나 `필수`를 사용하여 빈 공간을 무효화할 수 있습니다.

검색 입력은 엄밀히 말해 텍스트 입력이지만 검색을 위해 암묵적으로 강화된다. 사용하지 않고도 빠져나갈 수 있으며 지원되지 않으면 텍스트 입력으로 되돌아갑니다.

개선 사항 중 일부는 스타일링에 있습니다. 이는 장치 및 사용자 에이전트에 따라 다릅니다. 디자인 면에서도 검색이란 휴대전화와 같이 가상 키패드가 있는 기기와 함께 사용하면 더욱 빛을 발한다. Android의 Swift 키패드에 있는 텍스트와 검색 키보드 디스플레이를 비교했습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/android-swift-key.jpeg?resize=730%2C495&ssl=1)

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/android-input.jpeg?resize=730%2C563&ssl=1)

작동 방식에도 몇 가지 근본적인 차이가 있다. 그러나 현재는 `증분` 또는 `증분`과 같은 비표준 속성 중 하나를 사용하는 경우에만 해당됩니다.

시각적 표현도 개선되었으며 사용자 에이전트마다 다를 수 있습니다.

### = 입력 유형="url" 및 = 입력 유형="tel"

`url` 입력을 통해 사용자는 URL을 입력하고 URL이 존재하는지 아닌 올바른 형식의 URL인지 자동으로 확인할 수 있습니다. 또한 빈 문자열은 `필수` 속성으로 대체되지 않는 한 유효합니다.

```bash
<input type="url" placeholder="Enter your profile URL" required/>
```

텍스트 입력과 마찬가지로 패턴 속성을 사용하여 입력된 URL이 특정 케이스를 준수하는지 확인할 수 있습니다. 아래 정의에서 이 입력은 `HTTPS` URL 체계가 있고 비어 있지 않은 경우에만 유효합니다.

```bash
<input type="url" placeholder="Enter your profile URL" pattern="https://.*" required/>
```

"전화" 입력은 거의 가치가 없지만, 그것들은 이유가 있다. url 입력과 달리 tel 입력은 자동으로 검증되지 않는다. 그러나 사용성에 도움이 되는 사용자 지정 키보드를 표시할 수 있는 가상 키패드가 있는 기기에 유용할 수 있다.

입력 모드 전역 속성(사용자-에이전트에게 필요한 데이터의 종류를 암시하는 데 사용됨)을 사용하여 `텔` 값을 사용하여 `텔` 유형과 동일한 효과를 발생시킬 수도 있다는 점에 유의한다.

```bash
<input type="tel" /> // tel type
<input type="text" inputmode="tel" /> // text type with an inputmode of tel
```

`url` 입력과 마찬가지로 빈 문자열도 유효하므로 `필수` 특성 및 `패턴` 특성을 포함할 수 있습니다.

지원되지 않을 경우 url과 tel은 모두 텍스트로 되돌아갑니다.

### '<입력 유형="전자 메일" >'

이것은 가장 흔한 것 중 하나이다. 값이 올바른 형식의 전자 메일이어야 하므로 e-메일 형식이어야 한다는 점을 제외하면 텍스트 입력에 가깝다. 여러 전자 메일 주소를 수락하는 `다중` 특성을 포함할 수 있습니다.

```cpp
<input type="email" /> // default
<input type="email" multiple /> // multiple, values will be a comma-separated list
```

검증도 자동으로 이루어지므로, 시행 단계부터 어느 정도 진행해야 하는지 안심할 수 있습니다. 이메일 주소의 유효성에 따라 `:valid` 및 `:valid` CSS 유사 클래스가 자동으로 적용됩니다.

`복수` 특성이 지정된 경우 입력한 전자 메일은 쉼표로 구분된 유효한 전자 메일 목록이 됩니다(사용자가 이 사실을 알 필요는 없으므로 힌트를 제공할 수도 있음). 필요한 전자 메일의 모양을 지정하려면 패턴과 같이 전자 메일 입력의 기능을 향상시키기 위해 추가할 수 있는 여러 가지 속성도 있습니다. 또는 "자동 완성"을 통해 컨트롤의 유용성을 향상시킬 수 있습니다.

```undefined
// Only accepts emails that ends with @logrocket.com
<input type="email" pattern=".+@logrocket.com" />
```

### '<입력 유형="암호" >'

비밀번호 입력은 비밀번호를 필요로 하는 웹사이트에서 흔히 볼 수 있다. 사용자가 거의 즉시 사용자-에이전트 정의 기호(대개 점)로 대체되는 문자열을 입력할 수 있습니다. 모바일 장치에서는 암호 문자가 잠시 동안 표시된 후 숨겨져 사용자가 입력한 내용을 계속 알 수 있습니다.

```bash
<input type="password" />
```

브라우저에 암호가 저장되면 브라우저가 암호 필드를 자동으로 완료합니다. 이것은 좋은 일이지만, 자신도 모르게 원치 않는 행동으로 이어질 수 있습니다.

사용자가 로그인하지 않더라도 브라우저는 여전히 등록 페이지와 같은 암호 입력을 미리 채우려고 시도하며, 원인 때문에 사용자의 암호를 노출시킵니다(이미 로그인 페이지에 노출되어 있는데, 등록 페이지에는 어떤 차이가 있습니까? 로그인 페이지에서 사용자가 실제로 로그인할 수 있지만 등록 페이지에는 로그인하지 않습니다.)

off 또는 new-password 자동 완성 값을 사용하여 브라우저에 암호 필드를 자동으로 미리 입력하지 않도록 지시할 수 있습니다(Chrome이 이를 존중하지 않지만). Chromium 페이지에 문제가 있습니다. 새 비밀번호는 브라우저(또는 비밀번호 관리자)가 사용자에게 비밀번호를 제안할 수도 있다.

```js
/* The for values of autocomplete applicable to a password field
 on, off, current-password, new-password */

<input tye="password" autcomplete="on" /> /* Yes, prefill the password field */
<input tye="password" autcomplete="current-password" /> /* Same as on */
<input tye="password" autcomplete="off" />
<input tye="password" autcomplete="new-password" />
```

PIN과 같이 사용자에게 숫자 전용 패스를 요청하는 경우 `암호` 입력과 함께 `입력 모드="➡"를 사용하여 지원 장치에서 숫자 키패드를 활성화할 수 있습니다.

값을 제한하면 최소 길이, 최대 길이 또는 패턴 특성을 사용하여 정보를 모르는 사용자를 견제할 수 있습니다.

### '=입력 유형="날짜" > taph '="datetime-local' > taph '= 입력 유형="월" > 및 '입력 유형="주"

`date` 유형은 사용자 브라우저의 위치에 따라 포맷된 날짜를 선택할 수 있는 입력을 나타냅니다. 지원되지 않는 브라우저에서는 텍스트 입력으로 돌아갑니다. 날짜를 선택하면 구문 분석된 값은 `YYYY-MM-DD` 형식으로 지정됩니다.

일반적으로 달력 보기 UI가 표시됩니다. 그러나 이것은 브라우저마다 다르다.

> Safari(버전 13.1)는 텍스트 입력 대신 만족스러운 입력으로 돌아온다. 크롬은 엣지와 비슷한 것을 보여줍니다. 근본적으로 같기 때문입니다.

최소 및 최대 속성은 날짜 선택까지의 하한 및 상한 경계를 제공하고 해당 경계를 적용하는 데 사용할 수 있습니다. 그러나 위의 Safari의 경우처럼 모든 브라우저가 아직 `날짜` 입력을 지원하지 않기 때문에 제공된 값을 수동으로 확인하는 것이 항상 좋습니다. 항상 유효합니다.

유효성 검사에 대해 말하자면, 한 가지 방법은 `패턴` 특성을 사용하여 허용되는 패턴을 지정하는 것입니다. 따라서 날짜 입력이 지원되지 않더라도 텍스트 입력은 해당 패턴에 따라 유효합니다.

또는 `필수` 특성을 사용할 수 있습니다. 양식을 제출하기 위해 다른 사용자가 DOM에 들어가 모든 제한 속성을 제거할 수 있습니다. 즉, 수동 검증을 수행합니다.

datetime-local은 `date` 입력과 유사하지만, 함께 시간을 선택하거나 입력할 수도 있습니다. 지지가 그렇게 좋은 것만은 아닙니다. 이 글의 내용과 MDN에 대한 광범위한 설명이 있습니다.

Chrome 버전 87(Canary):

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/canary-date-picker.png?resize=730%2C506&ssl=1)

시간 입력은 사용자에게 사용자 에이전트에 따라 모양이 다른 시간 선택 필드를 표시합니다. 사용 가능한 제약 조건 때문에 자동 유효성 검사가 없습니다. 사용자는 24시간 클럭 시간 형식의 최소 및 최대 경계를 초과하거나 그 아래로 떨어질 수 없습니다.

```bash
<input type="time" />
```

모양과는 별도로 브라우저마다 형식이 다릅니다. 시간 형식은 다른 브라우저가 아닌 일부 브라우저에서 사용자의 시스템 위치에 따라 달라집니다. 또한 일부 브라우저는 Chrome과 같은 시간 선택기를 제공하는 반면, 그렇지 않은 브라우저도 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/date-picker.png?resize=300%2C586&ssl=1)

시간 입력의 `값`은 항상 24시간 형식이다. 즉, 선행 0을 포함한 "hh:mm"는 01:30과 같습니다. `step` 속성을 사용하여 초를 다음 형식으로 포함할 수 있습니다. hh:mm:ss이지만 브라우저 차이 때문에 완전히 신뢰할 수 있는 것은 아닙니다.

```bash
<input type="time" id="timeOfBirth" name="timeOfBirth" step="2" required />
```

월과 주 입력은 월과 주를 제외한 날짜 입력과 유사하다. 그들은 겉모습과 가치의 형식이 다르다. 월의 형식은 2018-07년과 같은 YYYY-MM, 2020-W24와 같은 주간 YYY-Www이다.

Jen Simmons의 이 트위터에는 `월`과 `주간`이라는 유형에 대한 좌절감에 대해 훑어보길 권하고 싶은 내용이 많다.

> 사용 가능한 다양한 형식을 이해하려면 HTML에서 사용되는 날짜 및 시간 형식을 읽는 것이 중요합니다.

### = 입력 유형="number" 및 = 입력 유형="range">`

숫자 입력을 통해 사용자는 숫자가 아닌 항목에 대해 자동 유효성 검사를 통해 숫자를 입력할 수 있습니다. 일부 브라우저는 입력 값을 늘리거나 줄이는 데 사용할 수 있는 위쪽 및 아래쪽 화살표도 표시합니다.

제공된 화살표를 사용하여 증가 또는 감소하거나 키보드가 1단계로 이동하지만 `step` 속성으로 변경할 수 있는 기본 단계 크기가 1이다. 최소 및 최대 속성을 사용하여 일부 제약 조건을 적용할 수도 있습니다.

```bash
<input type="number" />
<input type="number" step="2" min="18" max="70" />
<input type="number" step="0.1" min="18" max="70" />
```

`패턴` 특성은 `숫자` 입력에 대해 작동하지 않으며 가상 키패드가 있는 장치에서는 숫자 키패드가 표시될 수 있습니다.

브래드 프로스트는 다음과 같은 기사를 썼다. "여러분은 아마 입력형="숫자"는 여러분에게 읽으라고 권하고 싶은 이유는 "숫자"라는 입력은 종종 여러분이 `숫자`가 정말 필요한 것인지 정확히 결정하는 것으로 시작하는 슬픔을 불러일으킬 수 있기 때문이다.

나는 가격 필드처럼 내 키보드에서 숫자 이외에는 아무것도 입력할 수 없는 사용자 지정 숫자 필드를 본 적이 있다. 신뢰와 신뢰성이 크게 작용한다는 것은 두말할 나위도 없고, 사용하기 전에 완벽하게 잘 작동하는지 확인해야 한다.

범위 입력은 사용자가 미리 정의된 최소값과 최대값 중 하나를 선택할 수 있도록 슬라이더 컨트롤로 표시된다.

이 옵션을 지정하지 않으면 최소값과 최대값이 각각 0과 100으로 설정됩니다. 기본 동작에서 벗어나지 못할 가능성이 높기 때문에 고민할 만한 가치가 있다고 판단한 후에는 취향에 맞게 기능을 조절해야 합니다. 숫자 입력과 마찬가지로 기본 스텝 크기는 1이지만 스텝 속성을 사용하여 변경할 수 있습니다.

범위 입력은 부정확하며 볼륨 컨트롤이나 색 테마 선택기처럼 정확한 값이 필요하지 않은 경우 사용하는 것이 좋습니다. 사용자 에이전트마다 어떻게 표시되는지는 다르지만, 멋지고 능숙하게 보이도록 하기 위해 몇 가지 노력을 기울여 사용자 정의할 수 있습니다.

### '<입력 유형="색상>'

HTML에 네이티브 컬러 선택기가 있다는 것을 알고 있었나요? 글쎄요, 아닐 수도 있습니다. 조금 복잡합니다. 브라우저마다 표현이 다르기 때문에 색 선택기라고 부르지도 않을 것입니다. 일부 브라우저는 색상 선택기를 표시하는 반면, 일부는 색상 형식을 자동으로 검증하는 `텍스트` 입력을 보여준다. 알파 색상은 허용되지 않는다는 또 다른 한계도 있습니다. RGB는 가능하지만 RGBA 등은 사용할 수 없습니다.

```bash
<input type="color" />
```

Mac용 색상 선택 도구입니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/color-picker.png?resize=730%2C1133&ssl=1)

`color` 유형과 함께 사용할 수 있는 두 가지 이벤트가 있습니다.

- `변경` – 색상 선택기가 닫히면 발화
- `입력` – 색상이 계속 변경되는 한 발화

### = 입력 유형="filename" 및 = 입력 유형="라디오">`

확인란은 확인(참) 또는 선택 취소(거짓)할 수 있는 상자로 표시됩니다. 체크된 상태를 알려주는 체크된 속성(checked)과 체크되지 않은 세 번째 확정되지 않은 상태(true 또는 false)를 갖고 있다.

확인란의 사용 사례 중 하나는 사용자가 열거할 수 있는 옵션 목록에서 여러 값을 선택하도록 하려는 경우입니다(이 때문에 라디오 단추와 다릅니다). 라디오 버튼을 사용하면 섹션이 여러 개가 아니라 단일 섹션입니다. 알고 있는 프로그래밍 언어 수와 같은 질문으로 양식을 작성하거나 방문한 수신처 목록을 확인하는 방법을 생각해 보십시오.

```bash
<input type="checkbox" value="Toronto" />
<input type="checkbox" value="Ilorin" />
```

확인란도 흥미롭다. 그것들은 확인/확인되지 않은 부울 상태를 가지고 있기 때문에, 우리가 그것들을 클릭 이벤트로 생각한다면 몇 가지 이국적인 것들을 만들 수 있다.

확인란은 기본적으로 다음과 같은 `체크된` 특성을 사용하여 `체크된` 상태로 설정될 수 있습니다.

```undefined
<input type="checkbox" value="Toronto" />
<input type="checkbox" value="Ilorin" checked/>
```

체크박스와 함께 주목해야 할 한 가지는 그들이 양식 제출에 어떻게 반응하는가이다.

확인란을 선택하지 않으면 실제 값이 없기 때문에 서버로 전송되지 않습니다. 서버에 선택되지 않은 값이 없는 경우 선택되지 않은 값을 서버로 보내는 것은 의미가 없습니다.

라디오 입력은 한 그룹에서 한 가지 옵션만 선택할 수 있다는 점에서 체크박스 입력과 다르다. 그들은 그룹에 속하지 않을 때 거의 쓸모가 없다.

```bash
<input type="radio" />
```

라디오 버튼 그룹을 생성하려면 모두 유사한 `값`을 가진 `이름` 특성을 가지고 있어야 합니다.

```bash
<input type="radio" name="accent-color" value="orange" />
<input type="radio" name="accent-color" value="yellow" />
```

라디오 입력은 체크박스와 같은 무기정 상태는 아니지만 체크박스는 갖고 있다. 그러나 한 번에 하나의 라디오 버튼만 선택할 수 있다.

### '<입력 유형=" 파일" >'

이 입력을 통해 사용자는 컴퓨터에서 파일을 선택할 수 있으며, 이 파일은 프로그래밍된 대로 사용할 수 있습니다.

```bash
<input type="file" />
```

`accept` 속성을 사용하면 허용되는 파일 형식을 지정할 수 있습니다. 유효하거나 허용되는 MIME 유형의 쉼표로 구분된 목록입니다.

```bash
<input type="file" id="profile-img" accept="image/png, image/jpeg" />
```

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/data-vis-basics.png?resize=730%2C359&ssl=1)

위에서 파일 입력에 `승인`을 적용한 결과를 볼 수 있습니다.

상단 이미지에는 수락이 없습니다. 하단 이미지에서는 accept="image/skips, image/skips"로 지정되며, 이는 사용자가 .skips 또는 .skips(`jpg`는 기술적으로 `skip`와 동일)로 끝나는 영상만 선택할 수 있음을 의미합니다. 파일 폴더를 선택할 수 있다는 것을 알고 계십니까? 사실, 그럴 수 없어요. 허용 가능한 파일이 있을 수 있으므로 활성 상태입니다.

`accept` 속성은 업로드할 수 있는 파일 형식을 강제 적용하는 제약 조건입니다. 실제로 사용자는 이를 무시할 수 있으므로 업로드된 내용을 확인해야 합니다.

### '=입력 유형="버튼" `,apto ="reset" 및 '=입력 유형="filo">`

버튼 입력은 클릭 동작을 수행하기 위해 프로그래밍 방식으로 제어할 수 있는 클릭 가능한 컨트롤을 나타냅니다. 입력 버튼은 다음과 같이 정의할 수 있습니다.

```bash
<input type="button" />
```

위에서 설명한 것처럼 입력 버튼을 정의하면 버튼이 비어 있게 됩니다. 그 이유는 레이블 텍스트가 `값` 특성일 것으로 예상되고, 지정하지 않았기 때문입니다. 값을 지정하는 것은 `값` 속성에 값을 부여하는 것만큼 간단합니다.

```bash
<input type="button" value="click me" />
```

기술적으로는 이것이 절대적으로 옳지만, 이제 새로운 버튼 요소가 버튼을 만드는 가장 좋은 방법이다.

리셋이나 제출 입력과 달리 버튼 입력은 특별히 프로그래밍되지 않는 한 기본 동작이 없다.

재설정 및 제출 버튼은 양식과 함께 사용할 때 유용합니다. 둘 다 각각 양식을 재설정하고 제출하는 기본 동작을 가지고 있습니다.

```bash
<input type="reset" value="Reset form" />
<input type="submit" value="Submit form" />
```

버튼 입력과 달리 사용자-에이전트에 종속된 기본값도 있습니다. 언급된 차이점과는 별개로, 그것들은 버튼 입력과 매우 비슷하게 동작한다.

### '<입력 유형="이미지">`

만약 우리가 `img/` 요소를 가지고 있다면, 여러분은 이것이 무엇이라고 생각하나요? 값이 이미지인 버튼입니다. 이 경우 값은 `값` 특성이 아닙니다. 사실, 그것은 가지고 있지 않습니다. 하나를 지정하면 무시됩니다. 그러면 이미지를 어떻게 지정할까요?

`image` 입력에 대한 `src` 속성은 적어도 시각적으로 `button` 입력에 대한 `값` 속성이다. = 입력 type="button" value="click me" />가 "click me" 텍스트로 "button" 입력을 렌더링하는 경우, "<input type=" image" src= "/path/to/img" />은 "src"에 지정된 이미지의 버튼을 렌더링합니다.

이미지 입력은 src와 alt 속성 같은 속성을 가진 img 요소와 비슷하다.

### '<입력>

HTML 속성은 요소에 대한 구성입니다. 우리가 `유형` 특성이 `입력` 요소에게 정체성을 부여한다고 언급했듯이, 다른 속성들도 (다른 유형에 대한 기능적 의존성을 고려할 때)` 입력 요소를 보다 유연하고 강력하며 복잡하게 만드는 데 할당량을 기여할 수 있다.

속성에는 두 가지 유형이 있습니다.

- 컨텐츠 속성
- IDL 속성

#### 컨텐츠 속성

컨텐츠 속성은 다음 세 가지 기준을 충족하는 속성입니다.

- class, id, placeholder 등 HTML 요소와 함께 정의할 수 있습니다.
- 요소로 설정할 수 있습니다.속성 메서드를 설정합니다.
- [element.getAttribute] 메서드와 함께 사용할 수 있습니다.

아마도 가장 흔한 것은 클래스 속성이나 id 속성일 것이고, 많은 것들이 있다. 사실, 대부분의 글로벌 속성은 이 범주에 속합니다.

#### IDL 속성

IDL은 Interface Description Language의 약자로 이 범주에 속하는 속성은 일반적으로 체크박스 입력의 `무정` 속성처럼 프로그래밍 방식으로만 액세스할 수 있다.

특히 IDL 속성의 섹션인 IDL 속성의 반영 섹션이 있는 IDL 속성의 왜곡이 있습니다. 일부 속성에는 꺾이는 동작이 있을 수 있습니다. 이 섹션을 자세히 읽고 규격이 업데이트되었는지 여부를 모니터링하는 것이 좋습니다.

아래 표에는 32개의 (표준) 특성이 나와 있습니다.

> W3과 WHATWG 모두 사양 페이지에 아래 표가 있지만, "기능적 종속성"에 대해 자세히 설명하고자 합니다.

기본적으로 표에서 설명하는 것은 `유형`과 해당 속성의 중복이다.

예를 들어 autofocus는 hidden을 제외한 모든 입력 컨트롤에 사용할 수 있다. 자동완성은 체크박스, 라디오 버튼, 파일 입력 등에 영향을 미치지 않는다. 그리고 숨겨진 입력은 외톨이로 악명이 높다.

## 유효성 검사에 대한 토론

검증은 영원히 계속되는 주제가 될 것이다. 양식 검증에 대해 생각할 때 사용성과 보안 측면에서 생각해보고자 합니다. 사용자가 입력 사양을 준수하도록 제한하는 유일한 목적을 가진 많은 속성이 있습니다. Required(필수)는 사용자가 값을 제공하도록 하고, 패턴은 특정 패턴을 적용하며, min과 max는 하한과 상한 경계 등을 적용합니다.

그러나 결국, 이러한 모든 것은 클라이언트측 검증이며, 가장 중요한 것은 모든 것이 DOM에 있는 HTML에서는 쉽게 바이패스될 수 있다는 것입니다. 좀 더 정보에 입각한 사용자는 DOM에 들어가서 제출 단추의 비활성화된 속성을 제거할 수 있습니다. 이전에 수행한 적이 있는데 작동됩니다.

무엇이 위험에 처해 있는지 알고 프로그래밍 방식으로 검증함으로써 검증을 한 단계 끌어올리려고 노력합니다. 이 주제에 대해서는 의견이 엇갈리는데, 제가 어떻게 하고 싶은지 의논하는 게 좋겠어요.

사용자 이름(`text` input)과 이메일(`email` input)으로 로그인 양식을 예로 들어 보겠습니다.

다음은 제가 하는 일의 목록입니다.

- 우선 기본 입력 유효성 검사에 의존하지 않습니다. Formik을 사용하여 Yp for Object 스키마 유효성 검사를 사용하여 양식 관리
- 필수 필드의 유효성을 확인할 때까지 제출 단추를 사용 불가능으로 설정합니다.
- 입력 필드가 두 개 이상일 경우 "onBlur"(입력 필드가 "포커스"를 손실할 때)을 확인합니다. 이렇게 하면 사용자가 입력을 종료한 후 즉시 오류 메시지를 표시할 수 있습니다.
- 단일 입력 필드가 있을 때 "변경 시"를 확인합니다. 이렇게 하면 사용 안 함 단추가 있는 상태에서 양식을 제출하려고 시도하지 않을 가능성이 높기 때문에 오류 메시지(있는 경우)가 나타납니다. 단추가 활성화되지 않은 이유를 인식하지 못할 수 있습니다.

1-4단계로 양식을 사용할 수 있게 만들었습니다. 여전히 보안상의 위험이 있을 것이다. 여기서 인증과 인증이 이루어집니다.

제가 처음에 한 일은 기본적으로 고객측 검증입니다. 물론 기본 입력 검증에 의존하지 않으며 사용자 지정 검증을 통해 검증 작업을 조금 더 진행하고 있습니다. 그래도 이 작업은 바이패스될 수 있으므로 서버 측 검증은 항상 수행되어야 합니다.

사용성과 보안은 광범위한 주제이다. Phil Nash가 기사에서 사용자의 2가지 요인 인증을 개선하기 위해 HTML 속성을 지적했듯이 개발자는 계정 보안의 필요성을 지원하면서도 사용자 경험을 손상시키지 않는 애플리케이션을 구축해야 합니다. 때때로 이러한 요구 사항들이 서로 싸우는 것처럼 느껴질 수 있다.

## 팁과 요령

`input`으로 작업하는 동안, 제가 그 동안 배운 몇 가지 유용한 조언과 요령들이 있었습니다. 일부는 버그 픽스, 디자인, 또는 단지 호기심에서 태어났다. 제가 가장 좋아하는 발견은 제가 "비밀번호를 보여달라"고 부르는 것입니다. 그리고 여러분이 잘 아실 수도 있다는 것을 압니다. 하지만 저는 이것이 정말 자랑스럽습니다.

#### 암호 표시

비밀번호 입력은 로그인 양식에서 등록 양식에 이르기까지 어디에나 있습니다. 사용자가 암호를 입력할 때, 필요한 경우 오류를 수정할 수 있도록 사용자가 입력하는 내용을 볼 수 있도록 하는 것이 좋습니다.

전에도 보셨잖아요. 야생의 예는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/sign-in.png?resize=730%2C1071&ssl=1)

입력 끝에는 종종 암호 표시를 전환하는 전환 버튼이 있습니다. 사용자가 해당 버튼을 클릭하면 어떻게 됩니까? 음, 우리는 프로그래밍 방식으로 입력 유형을 `암호`에서 `텍스트`로 전환합니다.

예는 다음과 같습니다.

#### 확인란을 사용하여 이벤트 클릭

> 우리는 세 개의 상태를 확인했고 결정하지 않았습니다. 점검된 상태만 참조하십시오.

이거 재미있네요. 클릭 상태(부울, 참 또는 거짓, 클릭 또는 클릭 안 함)를 생각해 보십시오. = 입력 유형="filename" / >에는 확인란의 확인 여부를 알려주는 체크된 속성이 있습니다. 이것을 라벨과 결합하면 흥미로운 것을 만들 수 있습니다.

이 조각들을 살펴봅시다:

```js
<div class="div">
  <input type="checkbox" id="toggle" />
  <label for="toggle">toggle dropdown</label>
  <div class="dropdown">
    <p>Lionel Messi</p>
    <p>Cristiano Ronaldo</p>
    <p>Andres Iniesta</p>
    <p>Xavi</p>
  </div>
</div>
```

레이블을 클릭하면 ID 값이 "for" 값과 일치하는 해당 입력에 초점을 맞춘다.

확인란의 경우 관련 레이블을 클릭하면 해당 확인란이 활성화됩니다. 선택한 속성을 설정(참) 또는 해제(거짓)로 전환합니다.

흥미로운 단서네요.

이것에 대한 나의 사용 사례 중 하나는 드롭다운에 대한 것이다. 추가 CSS 비트로 드롭다운 높이를 제어할 수 있습니다.

```css
.dropdown {
  max-height: 0px;
  overflow: hidden;
}

input[type="checkbox"]:checked + label[for="toggle"] + .dropdown {
  max-height: 150px;
}
```

처음에는 드롭다운 높이가 0이 됩니다. 그런 다음 확인란(`유형="➡"]:➡")을 선택하면 다음 형제(라벨)를 찾습니다. 그런 다음, 그 다음 형제자매를 찾습니다. 드롭다운입니다.

다음은 전체 예입니다.

> "체크박스 해킹" (그리고 그걸로 할 수 있는 일들)

Checkbox 해킹은 부작용에 가깝고 완벽과는 거리가 멀다. 드롭다운이 나타나면 드롭다운에 대한 절대 최대 높이가 있어야 하며, 일반적으로 드롭다운 요소는 DOM에 유지됩니다. 따라서 신중하게 사용하고, 사용하기 전에 반드시 전체 사용 사례를 해결하십시오.

## 결론

이러한 HTML 입력으로 작업을 계속하면 매우 유용합니다. 그러나 특정 또는 능률적인 목표를 달성하기 위해 원래 사양에 따라 구축한 사용자 에이전트, 지원 및 기타 모든 사항으로 인해 불완전할 수 있습니다.

이러한 사양은 계속 증가할 것이며 지원도 마찬가지이므로 완전히 무시해서는 안 됩니다. 휠을 재창조하기 전에 항상 현재 기능을 다시 확인하십시오.