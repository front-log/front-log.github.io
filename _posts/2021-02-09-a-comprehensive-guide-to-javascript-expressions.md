---
layout: post
title: "JavaScript 표현식 및 연산자에 대한 포괄적인 가이드"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/02/js-expressions.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/js-expressions.png?fit=730%2C487&ssl=1)

## 도입

나 같은 웹개발자라면 매일 자바스크립트 코드를 처리한다. 그러나, 일상 업무에서 이 라이브러리를 사용함에도 불구하고, 우리들 중 많은 사람들은 우리의 삶을 더 쉽게 만드는 데 도움이 될 자바스크립트 표현과 연산자를 모두 알지 못한다.

이 기사에서는 자바스크립트에 반드시 알아야 할 표현식과 연산자를 열거했는데, 각각에 대한 간단하고 간결한 예도 포함되어 있다. 안으로 들어가자!

## JavaScript 식

식은 리터럴, 변수, 연산자 및 기타 간단한 식들의 집합으로 만들어진 유효한 코드 단위로서, 해결되었을 때 값을 생성한다. 식은 변수 이름처럼 단순할 수 있으며, 이 값은 x = 5와 같습니다.

이미 알고 계시겠지만, 변수에 할당한 값은 숫자부터 문자열, 부울에 이르기까지 모든 값이 될 수 있습니다.

👇🏼의 다섯 가지 표현 범주가 있습니다. 앞의 세 가지는 상당히 간단한 반면, 마지막 두 가지는 조금 더 복잡합니다.

### 1. 산술:

이 식은 5 또는 5.864와 같은 산술 연산자를 사용합니다.

### 2. 문자열:

이 표현들은 "나다" 또는 "5.864"와 같은 문자 집합을 값으로 가지고 있다.

### 3. 논리:

이러한 표현은 일반적으로 `와 같은 논리 연산자를 통해 참 또는 거짓과 같습니다.

### 4. 기본 표현식:

다음은 JavaScript 코드에서 사용하는 기본 키워드 및 주요 문자입니다(대부분 알고 계실 것입니다).

#### '이것':

this.propertyName에서와 같이. 이 식은 실행 컨텍스트 내에서 개체의 속성을 나타냅니다.

… 이제 여러분은 `이것`의 실행 컨텍스트가 무엇인지 궁금할 것입니다. 일반적으로 전역 컨텍스트(예: 브라우저에서는 `창`)입니다. 개체 메서드(예: `user)` 내에서 사용되는 경우는 예외입니다.전체 이름(`)). 이 경우 메서드(전체 이름)() 내에서 이(this)를 호출하고 개체 컨텍스트(user)를 참조합니다.

자세한 내용은 코드STACKR은 이 비디오에서 `이것`을 더 자세히 설명한다.👈🏼

#### 함수, 함수* 및 c 함수:

아시다시피 `함수`는 함수 표현식(duh)을 정의합니다. 명백하게, 함수는 입력(문 집합)을 취하고 작업 수행의 형태로 출력을 반환하는 코드 절차이다.

반면에 `함수*`는 생성기 함수를 정의하는데, 이는 단일 값 대신 일련의 결과를 생성하여 반복기 쓰기 작업을 단순화한다.

비동기 프로그래밍에서는 함수*를 사용할 수 있지만, 단순하게 sync 함수를 사용하는 것이 좋습니다. 비동기 기능은 약속 체인을 명시적으로 구성할 필요가 없도록 하면서 "비동기적 약속 기반 동작을 보다 깔끔한 스타일로 작성할 수 있도록 한다.

일반적으로 비동기 기능을 사용하면 다음 태스크를 실행하기 전에 태스크가 완료될 때까지 기다릴 필요 없이 일련의 태스크를 수행할 수 있습니다.

자세한 내용은 이 비디오를 확인해 보는 것을 추천합니다.

#### 수율, 수율*, 수율:

우선 수익률과 수익률을 구분해 보자.

`반환`은 정규함수에, `수율`은 발전기 함수(`함수*`)에 사용된다. 차이점은 함수에서 단순히 값을 `반환`할 수 있다는 것이다. 대조적으로, 생성기 함수에서는 값의 시퀀스를 생성하므로, `함수*` 호출을 중지할 때까지 `수율`은 다중 값을 생성하는 데 사용된다.

반면 기다림은 아식스 기능에만 쓰인다. `async` 기능의 유일한 임무는 약속을 돌려주는 것이므로 `wait`는 기다렸던 가치에 대해 `약속.해결`을 부를 것이다.

이제 `리턴`과 `수율`과 `기다림`을 구분했으니 `수율`이 도대체 무슨 수작인지 궁금할 것이다. 실제로는 매우 간단합니다. `수익률*` 딜러는 다음과 같은 방식으로 다른 제너레이터 기능을 수행합니다.

```js
function* function1() {
yield "I'm the value from function1 👋 but when function2 is called, it delegates to function1 the task of generating a value";
}
function* function2() {
yield* function1();
}
console.log(function2().next().value);
// expected output: "I'm the value from function1, but when function2 is called, it delegates to function1 the task of generating a value "
```

#### class:

Quora의 한 사용자가 클래스를 "개체에 대한 Blueprint"라고 설명했는데, 저는 이 비교에 더 이상 동의할 수 없었습니다.

ES6에 소개된 클래스 표현 개념에 대해 머리를 감싸려면 예제에서 클래스 표현이 어떻게 작동하는지 확인하는 것이 유용합니다.

```js
class ClothingItem {
constructor(type, season) {
this.type = type;
this.season = season;
}
description() {
return `This ${this.type} is for ${this.season}`;
}
}
console.log(new ClothingItem("dress", "winter"));
// expected output: Object {season: "winter", type: "dress"}
console.log(new ClothingItem("dress", "winter").description());
// expected output: "This dress is for winter"
```

여기에 표시된 것처럼, 생성자()를 사용하여 개체의 인스턴스 속성을 정의한 후, 우리는 `description()을 사용하여 데이터를 메서드에 바인딩할 수 있었다.

#### 배열 이니셜라이저/리터럴 구문 '[]:

배열을 초기화하는 방법은 여러 가지가 있지만 가장 간단한 방법은 `[]`를 사용하는 것입니다.

```js
let myEmptyArray = [];
console.log(myEmptyArray);
// expected output: []
```

그런 다음 어레이 요소를 밀어넣을 수 있습니다(`my Empty`).Array.push(475)`) 또는 초기화 단계에서 정의하거나(`let myArray = [1,100]`)

#### 개체 이니셜라이저/리터럴 구문 '{}:

생성자 구문 대신 리터럴 구문으로 어레이를 초기화할 수 있는 것과 마찬가지로, `{}`만으로 개체를 초기화할 수도 있습니다.

```js
let myEmptyObject = {};
console.log(myEmptyObject);
// expected output: Object {}
```

#### 정규식(정규식 줄임말) '/ab+c/i':

RegExp는 텍스트와 패턴을 일치시키는 데 사용되며, 예를 들어 사용자가 필드에 입력한 내용이 전자 메일 또는 숫자)의 패턴과 일치하는지 확인합니다.

얼마 전, 저는 RegExp를 배우고, 만들고, 테스트할 수 있는 이 훌륭한 도구를 발견했습니다. 단, 필요한 정규 표현식을 빠르게 얻을 수 있도록 도와주는 간단한 치트 시트에는 iHateRegx 😉를 사용합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/IhateRegex.gif?resize=600%2C338&ssl=1)

#### 그룹화 연산자 '('):

우리가 그룹화 연산자라고 부르는 괄호는 주어진 식에서 평가의 우선 순위를 간단히 제어한다.

아시다시피 `1 + 2 * 3`은 `1 + (2 * 3) (7)과 같은 결과를 산출합니다. 그러나 괄호 순서를 변경하면 먼저 평가할 사용자를 변경할 수 있습니다. 예를 들어 `(1 + 2) * 3`은 9를 반환합니다.

이 기능은 터미널 연산자를 사용하여 여러 조건을 평가해야 하는 경우에 유용합니다.

```undefined
condition1 ? "statement 1" : (condition2 ? "statement 2" : "statement 3");
```

### 5. 왼쪽 표현:

좌측(LHS) 식은 특정 표현식 또는 할당의 위치를 나타냅니다. 당연히 코드 블록의 왼쪽에 있습니다. 이러한 구성 요소는 다음과 같습니다.

#### 속성 접근자:

속성 액세스자는 다음 두 가지 구문 중 하나를 사용하여 개체 속성에 액세스할 수 있는 방법을 제공합니다.

- 점 표기법 `object.property` 포함
- 괄호 표기법 `object["property"] 포함

아래 예제를 확인하십시오.

```cpp
const myObject = {
firstObject: "Boku",
secondObject: "Anata",
};
console.log(myObject.firstObject);
// Expected output: "Boku"
console.log(myObject["secondObject"]);
// Expected output: "Anata"
```

#### new:

앞의 [class] 식 예에서 보았듯이, "new" 키워드만 사용하여 객체의 인스턴스를 만들 수 있습니다. 여기서 `신규` 운영자의 세부 정보를 자세히 읽어보십시오.

#### 'new.target':

new.target은 함수나 생성자가 new 키워드를 사용하여 호출되었는지 여부를 감지한다. 이 동영상과 이 문서에서 메타 속성에 대해 자세히 알아보십시오. 👈🏻

#### 'super'super'

슈퍼라는 키워드는 상위 생성자에 액세스하여 호출하는 데 사용됩니다. 예를 들어, 공통 부품을 공유하는 생성자가 두 개 있는 경우 클래스 상속과 함께 사용할 수 있습니다. 코드가 중복되지 않도록 하려면 "super()"를 호출하면 됩니다.

다음은 `super`의 예입니다.

```js
class Movie {
constructor(name, year) {
this.name = name;
this.year = year;
}
MovieDescription() {
return `Movie: ${this.name}, year: ${this.year}.`;
}
}
console.log(new Movie("Ma Rainey's Black Bottom", "2020"));
// expected output: Object { name: "Ma Rainey's Black Bottom", year: "2020"}
console.log(new Movie("Ma Rainey's Black Bottom", "2020").MovieDescription());
// expected output: "Movie: Ma Rainey's Black Bottom, year: 2020."
class TvShow extends Movie {
constructor(name, year, seasons) {
super(name, year);
this.seasons = seasons;
}
TvShowDescription() {
return `Tv Show: ${this.name}, number of seasons: ${this.seasons}, year: ${this.year}.`;
}
}
console.log(new TvShow("F.R.I.E.N.D.S", "1994", 10));
// expected output: Object { name: "F.R.I.E.N.D.S", seasons: 10, year: "1994"}
console.log(new TvShow("F.R.I.E.N.D.S", "1994", 10).TvShowDescription());
// expected output: "Tv Show: F.R.I.E.N.D.S, number of seasons: 10, year: 1994."
```

#### 확산 구문 '...obj'...obj'

분산 구문 `...itl`을 사용하면 식을 확장할 수 있습니다. 예를 들어 어레이를 어레이에 추가해야 하는 경우 이와 같은 작업이 수행될 수 있습니다(`...`: [a, [b, c, d](으)

분산 연산자를 사용하는 한 가지 방법은 배열 요소를 분산하는 것입니다.

```js
let childArray = ["b", "c"];
let parentArray = ["a", ...childArray, "d"];
console.log(parentArray);
// expected output: [a, b, c, d]
```

이 문서에서 다루는 스프레드 구문을 사용할 수 있는 몇 가지 다른 방법이 있습니다.

## JavaScript 연산자

이제 우리는 표현이 무엇을 할 수 있는지 보았으니, 이제 운영자에 대해 이야기 할 때입니다. 연산자는 완전히 간단한 표현식으로 복잡한 표현식을 만드는 데 사용됩니다. 아래에서 자세히 설명하겠습니다.

연산자는 우측(RHS) 값을 생성하는 데 사용하는 도구입니다. 이들은 생성된 오른쪽 값이 c인 `a + b = c`인 덧셈 연산자처럼 단순하거나, 예를 들어 조건부 연산자가 사용되는 비트 트리키어(` (c > a) ? "c가 a보다 크다: "c가 a보다 크지 않다"가 될 수 있다.

연산자에는 단항, 이항 및 삼항 연산자의 세 가지 유형이 있습니다. 다음 섹션에서는 세 가지 모두에 대해 간단하고 쉽게 따라할 수 있는 예제를 살펴보겠습니다.

### 단항 연산자

단항 연산자는 값을 생성하는 데 하나의 피연산자(식)만 필요한 연산자입니다. 예를 들어 `2++`에서는 값을 생성할 피연산자(`2`)가 하나만 필요합니다.

단항 연산자에는 여러 종류가 있는데, 우리는 아래에서 논의할 것이다.

#### 단항 산술 연산자:

증분 연산자는 매우 간단합니다. 1을 추가합니다. 그러나 피연산자의 사후 수정 또는 접두사 지정 여부에 따라 동작이 달라집니다.

```js
let a = 2;
console.log(a++);
// expected output: 2
console.log(a);
// expected output: 3
let b = 2;
console.log(++b);
// expected output: 3
console.log(b);
// expected output: 3
```

증분 연산자와 동일한 원리가 감소 연산자에 적용된다.

```js
let a = 2;
console.log(a--);
// expected output: 2
console.log(a);
// expected output: 1
let b = 2;
console.log(--b);
// expected output: 1
console.log(b);
// expected output: 1
```

단항 + 연산자 `+`는 한 가지 간단한 작업을 수행합니다. 피연산자를 숫자로 변환합니다(아직 하나가 아닌 경우).

```js
let a = "2";
console.log(a);
// expected output: "2"
console.log(+a);
// expected output: 2
```

이 수법은 숫자에 줄을 끼우는 데 유용하다. 여러분은 이렇게 물을지도 모릅니다: 만약 숫자로 변환되지 않는다면요? 이 경우 +"some_string"은 NaN을 반환합니다.

단항 부정 연산자는 `+`( 문자열을 숫자로 변환)와 동일한 작업을 수행하지만 피연산자를 부정하여 추가 마일을 이동합니다.

```js
let a = "2";
console.log(a);
// expected output: "2"
console.log(-a);
// expected output: -2
```

논리 연산자(logical operator)는 논리 값과 함께 사용되는 연산자이며, 우리가 흔히 알고 있듯이 부울(true/false)이다. 따라서 단항 논리 연산자는 값을 생성하기 위해 하나의 부울 피연산자만 필요로 하는 연산자임을 알 수 있다.

`!` 연산자는 `truthy` 표현에 적용하면 `false`를 반환하고, 👉🏼와 그 반대도 마찬가지입니다.

```js
let a = 2;
let b = 4;
console.log(a < b);
// expected output: true
console.log(!(a < b));
// expected output: false
console.log(!(a > b));
// expected output: true
console.log(!"truthy");
// expected output: false
console.log(!"truthy");
// expected output: false
```

인간으로서, 우리는 십진법(1, 4.5, 5000 등)을 사용하여 숫자를 이해한다. 반면에 컴퓨터는 숫자를 이진 형식(영과 영의 조합)으로 처리한다.

비트 연산자가 하는 일은 피연산자의 십진수 값이 아니라 이진수 32비트 표현을 기반으로 하는 피연산출자는 다음과 같습니다.

```undefined
let decimal = 9;
let binary = decimal.toString(2);
console.log(binary);
// expected output: "1001"
// 32 bit integer : "00000000000000000000000000001001"
```

다행히도, 이 32비트 표현은 커튼 뒤에서 일어납니다. 비트 연산자의 출력은 아래 설명과 같이 여전히 표준 JavaScript 출력입니다.

단항 비트 연산자(`~`)는 피연산자의 비트를 반전시킵니다.

```cpp
const a = 3;
// 32-bit integer: 00000000000000000000000000000011
console.log(~a);
// expected output: -4
// 32-bit integer: 11111111111111111111111111111100
```

여기서 발생하는 것은 NOT 연산자가 피연산자의 (`3`) 32비트 표현인 00000000000000000000000011을 0으로 변환하고 0을 0으로 변환하는 것이다.

십진수를 이진수 또는 32비트 정수로 변환하려면 이 유용한 도구를 확인하십시오.

추측했습니다. 이 연산자는 속성이 개체(레이어 포함)에 속하는 한 적용되는 피연산자를 제거합니다.

```undefined
const programmer = {
alias: "rosen",
age: 30,
};
console.log(programmer.alias);
// expected output: "rosen"
delete programmer.alias;
console.log(programmer.alias);
// expected output: undefined
```

그러나 일반 변수에서는 `삭제`를 사용할 수 없습니다.

```cpp
const programmer = "rosen";
console.log(programmer);
// expected output: "rosen"
delete programmer;
console.log(programmer);
// expected output: "rosen"
```

어떤 이유로 인해 정의되지 않은 상태로 반환하는 식이 필요한 경우(반환해야 하는 식이 있더라도) `void` 연산자를 사용할 수 있습니다.

```js
function notVoid() {
return "I am not void!";
}
console.log(notVoid());
// expected output: "I am not void!"
console.log(void notVoid());
// expected output: undefined
```

마지막으로, 이름에서 알 수 있듯이, `type of` 연산자는 적용되는 식의 유형을 다음과 같이 명명합니다.

```js
console.log(typeof 3);
// expected output: "number"
console.log(typeof "3");
// expected output: "string"
console.log(typeof (3 > "3"));
// expected output: "string"
function three() {}
console.log(typeof three);
// expected output: "function"
array = [];
console.log(typeof array);
// expected output: "object"
```

### 이항 연산자

단항 연산자와는 대조적으로 이항 연산자는 값을 생성하기 위해 두 개의 피연산자가 필요하다.

예를 들어, (>)보다 큰 비교 연산자는 (이 경우, `2 > 5`가 `false`로 평가되는) 두 식에 적용되는 값(`true` 또는 `false`)만 생성할 수 있습니다.

#### 표준 산술 연산자:

```js
let a = 4;
let b = 2;
console.log(a + b);
// expected output: 6
```

```js
let a = 4;
let b = 2;
console.log(a - b);
// expected output: 2
```

```js
let a = 4;
let b = 2;
console.log(a / b);
// expected output: 2
```

```js
let a = 4;
let b = 2;
console.log(a * b);
// expected output: 8
```

지수 연산자는 기준값에 대한 지수를 계산합니다. 아래 예제에서는 4가 기준이고 2가 지수이므로 16이 예상 출력됩니다.

```js
let a = 4;
let b = 2;
console.log(a ** b);
// expected output: 16
```

계수라고도 하는 나머지 (%) 연산자는 두 피연산자의 분할에서 "leftover"를 반환합니다.

```js
let a = 4;
let b = 2;
console.log(a % b);
// expected output: 0
let c = 3;
console.log(a % c);
// expected output: 1
```

#### 비교 연산자:

이름에서 알 수 있듯이 비교 연산자는 피연산자가 적용된 피연산자를 비교한 다음 참 또는 거짓을 반환합니다.

숫자, 문자열, 부울 또는 개체 등 피연산자를 비교할 수 있습니다. 예를 들어 문자열은 유니코드 값에 따라 비교됩니다. 서로 다른 유형의 피연산자를 비교하는 경우, JavaScript는 비교를 위해 피연산자를 호환되는 유형으로 변환합니다.

```cpp
string = "string";
console.log(string.charCodeAt()); // returns a string unicode value
// expected value: 115
console.log(string < 3);
// expected value: false
console.log(false > true);
// expected value: false
console.log(true > false);
// expected value: true
function operand1() {
return "hello";
}
bye = ["zaijian", "matta", "besslama", "tchao"];
console.log(operand1() !== bye);
// expected value: true
```

#### 동등 연산자:

평등 운영자는 ==, =, ===, 4가지로 나뉜다.==. 다음 예에서는 각각의 작동 방식을 정확하게 보여드리겠지만, 먼저 몇 가지 유의해야 할 사항이 있습니다.

- 등가 `==`이고 등가 아닌 `!= 사업자는 피연산자를 비교하기 전에 변환하므로 숫자 하나와 문자열을 비교하더라도 3 == 3은 참으로 평가합니다.
- 반면에 엄한 것은 ===, 엄격한 것은 같지 않다!==의 연산자는 비교되는 피연산자의 유형을 고려합니다. 따라서 이 경우 3 === 3은 거짓을 반환한다.

```coffeescript
console.log(3 == "3");
// expected value: true
console.log(3 == 3);
// expected value: true
```

```coffeescript
console.log(3 != "3");
// expected value: false
console.log(3 != 3);
// expected value: false
```

```coffeescript
console.log(3 === "3");
// expected value: false
console.log(3 === 3);
// expected value: true
```

```coffeescript
console.log(3 === "3");
// expected value: true
console.log(3 === 3);
// expected value: false
```

#### 관계형 연산자:

```coffeescript
console.log(3 > 1);
// expected value: true
```

```coffeescript
console.log(3 >= "3");
// expected value: true
```

```coffeescript
console.log("3" < 1);
// expected value: false
```

```coffeescript
console.log(3 <= 1);
// expected value: false
```

#### 논리 연산자:

더

```coffeescript
console.log(3 > 1 && "3" > 0);
// expected value: true
```

반면에 || 연산자는 피연산자 중 어느 하나가 참이면 true를 반환한다. 따라서 첫 번째 피연산자가 true로 평가하면 두 번째 피연산자를 확인할 필요 없이 기대값이 true로 반환된다.

```undefined
console.log(3 > 1 || "3" == 0);
// expected value: true
```

#### 비트 연산자:

이 가이드에서 앞서 설명한 것처럼 비트 연산자는 32비트 표현을 기반으로 피연산자를 평가한 다음 표준 JavaScript 출력으로 값을 반환합니다.

JavaScript 비트 연산자의 사용 사례에 대한 보다 심도 있는 논의를 위해 이 기사를 읽는 것을 추천한다.

비트 AND 연산자(`

```cpp
const a = 3;
// 32-bit integer: 00000000000000000000000000000011
const b = 7;
// 32-bit integer: 00000000000000000000000000000111
console.log(a & b);
// expected output: 3
// 32-bit integer: 00000000000000000000000000000011
```

여기서 볼 수 있듯이 같은 위치에서 a의 이진수 표현에서 0과 충돌하는 b의 이진수 표현에서 1은 0으로 반전됐다.

XOR 연산자(`^`)는 비트 연산자(`^)와는 상당히 다른 논리를 따릅니다.

```cpp
const a = 3;
// 32-bit integer: 00000000000000000000000000000011
const b = 7;
// 32-bit integer: 00000000000000000000000000000111
console.log(a ^ b);
// expected output: 4
// 32-bit integer: 00000000000000000000000000000100
```

비트 OR 연산자(`|)는 `와 같은 논리를 따른다.

```cpp
const a = 3;
// 32-bit integer: 00000000000000000000000000000011
const b = 7;
// 32-bit integer: 00000000000000000000000000000111
console.log(a | b);
// expected output: 7
// 32-bit integer: 00000000000000000000000000000111
```

#### 비트 시프트 연산자:

이전의 예에서, 우리는 논리 연산자가 피연산자의 32비트를 어떻게 가져와서 평가한 다음 그들이 일부 비트의 값을 되돌리는 결과를 출력하는지 보았다.

다른 한편으로, 비트 시프트 연산자는 LHS 피연산자의 32비트 이진 표현을 취하고 하나의 비트를 특정 위치(RHS 피연산자에 의해 지정)로 이동시킨다.

이 작업 및 각 교대조 작업자의 작동 방식을 더 잘 시각화하려면 아래 예제를 살펴보겠습니다.

### 좌측 시프트 연산자 '<<:

여기서 왼쪽 시프트 연산자는 a의 32비트 이진수 표현을 취하고 왼쪽으로 7자리 이동하며 초과(000000)를 버린다.

```cpp
const a = 3;
// 32-bit integer: 00000000000000000000000000000011
const b = 7;
console.log(a << b);
// expected output: 384
// 32-bit integer: 00000000000000000000000110000000
```

### 우측 시프트 연산자 '>>:

우측 시프트 연산자 `>는 좌측 시프트 연산자 `<<와 동일하지만 두 가지 차이점이 있다.

- 반대 방향으로 움직이며
- 피연산자의 부호를 보존한다.

```cpp
const a = 5;
// 32-bit integer: 00000000000000000000000000000101
const b = 2;
console.log(a >> b);
// expected output: 1
// 32-bit integer: 00000000000000000000000000000001
const c = -5;
// 32-bit integer: -00000000000000000000000000000101
console.log(c >> b);
// expected output: -2
// 32-bit integer: -00000000000000001111111111111110
```

### 서명되지 않은(제로필) 우측 시프트 연산자 '>>>':

부호 없는 (제로필) 우이동 연산자 >><a의 32비트 이진수 표현을 오른쪽으로 b(1위치) 이동시켜 연산자와 유사하다. 여기서 가장 큰 차이점은 `>>가 부정적인 숫자를 긍정적인 숫자로 만들 수 있다는 것이다.

```cpp
const a = 3;
// 32-bit integer: 00000000000000000000000000000011
const b = 1;
console.log(a >>> b);
// expected output: 1
// 32-bit integer: 00000000000000000000000000000001
const c = -3;
// 32-bit integer: -00000000000000000000000000000011
console.log(c >>> b);
// expected output: 2147483646
// 32-bit integer: 01111111111111111111111111111110
```

#### 선택적 체인 연산자 '?`:

대부분의 개발자와 마찬가지로 개체의 체인 깊숙이 값을 가져오려고 했지만 null이거나 정의되지 않았기 때문에 오류가 발생했습니다.

복잡한 객체에서 체인 연산자의 에스코바를 사용하는 대신, 옵션인 체인 연산자 `??와 함께 사용할 수 있다.다음 번에는 이 연산자를 사용하면 체인 내의 모든 참조를 확인하지 않고도 값을 검색할 수 있습니다.

```cpp
const holiday = {
name: "christmas",
companions: "family",
travel: {},
};
console.log(holiday.travel.country);
// expected output: undefined
// Causes errors
console.log(holiday?.travel.country);
// expected output: undefined
// Only returns undefined
```

#### 쉼표 연산자의 어법:

물론 우리 모두는 우리가 사랑하는 조작자를 알고 있지만, 우리의 기억을 되살리는 것은 해로울 수 없다! 쉼표 연산자는 변수 선언(예: `a = [6, 3, 1, 8])과 식을 순서대로 실행할 수 있도록 구분합니다(루프 변수 선언: `vari = 0; i < 100; i++`).

#### in 연산자:

개체가 지정한 속성을 가지고 있으면 in 연산자는 true를 반환합니다.

```js
const holiday = {
name: "christmas",
companions: "family",
travel: {},
};
console.log("name" in holiday);
// expected output: true
```

#### 연산자 'contract of' 연산자:

속성이 특정 연산자의 인스턴스인지 확인하려면 다음과 같이 `instance of`를 사용할 수 있습니다.

```js
class ClothingItem {
constructor(type, season) {
this.type = type;
this.season = season;
}
}
const clothes = new ClothingItem("dress", "winter");
console.log(clothes instanceof ClothingItem);
// expected output: true
console.log(clothes instanceof Object);
// expected output: true
```

#### 할당 연산자:

이름에서 알 수 있듯이, 할당 연산자는 (RHS 피연산자에 기초한) 값을 LHS 피연산자에 할당한다.

주 할당 연산자는 `= b`의 a에 b를 할당하는 등호 기호로 구성됩니다.

```undefined
a = 1;
b = 4;
console.log(a);
// expected output: 1
a = b;
console.log(a);
// expected output: 4
```

구축 할당 구문을 사용하면 먼저 어레이 또는 개체에서 데이터를 추출한 다음 해당 데이터를 서로 다른 변수에 할당할 수 있습니다.

```cpp
const array = [6, 3, 1, 8];
const [a, b, c, d] = array;
console.log([a, b, c, d]);
// expected output: [6, 3, 1, 8]
console.log(a);
// expected output: 6
const object = {
first: 6,
second: 3,
third: 1,
fourth: 8,
};
const { first, second, third: e, fourth } = object;
console.log({ first, second, e, fourth });
// expected output: Object {
//   e: 1,
//   first: 6,
//   fourth: 8,
//   second: 3
// }
console.log(e);
// expected output: 1
```

표현식과 관련하여 논의한 논리 연산자는 피연산자만 평가한 다음 부울을 반환합니다. 반면에, 논리 할당 연산자는 그들의 왼쪽 피연산자를 평가하고 부울의 값에 기초하여 오른쪽 피연산자에 기초한 새로운 값을 할당한다.

```cpp
a = 4;
b = 0;
b &&= a;
console.log(b);
// expected value: 0
// As b = 0 which evaluates to false, b isn't assigned a's value.
a &&= b;
console.log(a);
// expected value: 0
// As a = 4 which evaluates to true, a is assigned b's value.
```

||=는 `와 반대이다.

```cpp
a = 4;
b = 0;
a ||= b;
console.log(a);
// expected value: 4
// As a = 4 which evaluates to true, a isn't assigned b's value.
b ||= a;
console.log(b);
// expected value: 4
// As b = 0 which evaluates to false, b is assigned a's value.
```

처음 들어보시는 분들은 `?= 연산자는 두 가지 작업을 수행합니다. 하나는 왼쪽 피연산자에 값이 있는지 확인하고, 두 가지는 값이 없는 피연산자에 값을 할당합니다.

이 예에서 LHS 운영자는 life.time과 life.money이다. life.money에는 값이 없으므로 논리 null 연산자는 값을 할당합니다. life.time은 값(50)이 있기 때문에 영향을 받지 않는다.=`.

```js
const life = { time: 50, money: null };
console.log((a.time ??= 10));
// expected output: 50
console.log((a.money ??= 25));
// expected output: 25
```

#### 속기 연산자:

다음 섹션에서는 위의 섹션에서 학습한 산술 및 비트별 연산에 대한 짧은 이진 연산자를 살펴봅니다.

```cpp
(a = 4), (b = 2);
a += b;
console.log(a);
// expected output: 6
```

```cpp
(a = 4), (b = 2);
a -= b;
console.log(a);
// expected output: 2
```

```cpp
(a = 4), (b = 2);
a /= b;
console.log(a);
// expected output: 2
```

```cpp
(a = 4), (b = 2);
a *= b;
console.log(a);
// expected output: 8
```

```cpp
(a = 4), (b = 2);
a **= b;
console.log(a);
// expected output: 16
```

```cpp
(a = 4), (b = 2);
a %= b;
console.log(a);
// expected output: 0
```

#### 비트 연산:

```cpp
(a = 4), (b = 2);
a &= b;
console.log(a);
// expected output: 0
```

```cpp
(a = 4), (b = 2);
a ^= b;
console.log(a);
// expected output: 6
```

```cpp
(a = 4), (b = 2);
a |= b;
console.log(a);
// expected output: 6
```

```cpp
(a = 4), (b = 2);
a <<= b;
console.log(a);
// expected output: 16
```

```undefined
(a = 4), (b = 2);
a >>= b;
console.log(a);
// expected output: 1
```

```undefined
(a = 4), (b = 2);
a >>>= b;
console.log(a);
// expected output: 1
```

### 조건부 연산자('조건 ? ifTrue: ifFalse'):

마지막으로, 우리는 세 명의 피연산자, 즉 여성과 신사, 즉 조건부 피연산자에 대해 논의하는 것을 놓칠 수 없다.

```coffeescript
a = 6;
console.log(a > 5 ? "bigger" : "smaller");
// expected output: "bigger"
console.log(a < 5 ? "bigger" : "smaller");
// expected output: "smaller"
```

조건부(즉, 용어) 연산자는 조건이 참인지 거짓인지 확인한 다음(물음표 `?`로 표시된 다음 조건이 참인지 거짓인지 여부에 따라 두 식 중 하나를 실행합니다.

## 요약

축하합니다! 귀하는 자바스크립트의 표현식 및 연산자에 대한 포괄적인 가이드를 통해 작성했습니다. 나중에 참조할 수 있도록 아래 요약 표를 즐겨찾기에 추가하면 이 문서에서 다룬 모든 자료를 다시 캡처할 수 있습니다(새 탭에서 이미지를 열고 확대!).

읽어주셔서 감사드리며, 제 작품을 NadaRifki.com에서 확인해주세요. 나 또한 트위터에서 당신의 코멘트나 메시지를 읽을 수 있어서 항상 기쁩니다. 😜