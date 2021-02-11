---
layout: post
title: "JavaScript에서 개체를 완전 복제하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/deep-cloning-objects-javascript.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/deep-cloning-objects-javascript.png?fit=730%2C487&ssl=1)

## 도입

JavaScript에서 개체는 키-값 쌍의 저장소 또는 컬렉션과 같습니다. 그것들은 일종의 구조적 데이터 유형이며, 속성 모음으로 볼 수 있다. 이러한 속성은 `부울`, `숫자`, `미정의` 등과 같은 원시 유형을 포함한 다른 데이터 유형의 값 또는 다른 개체일 수 있습니다. 따라서 객체를 통해 훨씬 더 복잡한 데이터 구조를 구축할 수 있습니다.

JS에 있는 개체의 특성 때문에 일반적으로 메모리에 저장되며 참조로만 복사할 수 있습니다. 즉, 변수는 객체 자체를 저장하는 것이 아니라 메모리에 있는 특정 객체에 대한 주소나 참조를 나타내는 식별자를 의미합니다. 이와 같이, 사물은 원시적인 것과 같은 방식으로 취급될 수 없다.

원시 데이터 유형의 경우 변수가 할당되면 해당 변수를 복사할 수 없습니다. 따라서 변수의 값을 변경해도 기본 원시 유형은 변경되지 않습니다. 즉, 이러한 유형의 값은 일단 변수에 할당되면 변경할 수 없습니다. 즉, 불변성이라고 하는 개념입니다. 그러나, 그것들은 새로운 가치를 도출하기 위해 함께 결합될 수 있다.

반면 개체는 가변 데이터 유형입니다. 이 문서에서는 JavaScript에서 개체를 수정하거나 변형하는 방법에 대해 살펴봅니다. 여기에는 일반 개체 동작과 관련하여 얕은 복제나 심층 복제 또는 복사를 수행하는 작업이 포함됩니다.

## 개체 동작 소개

반복하자면, 개체는 참조 유형이며, 따라서 개체 변수를 복사할 때 우리는 간접적으로 컴퓨터의 메모리 어딘가에 저장된 동일한 개체에 대한 참조를 하나 더 만들고 있다. 따라서 개체 변수가 복사되면 개체에 대한 참조만 복사되고 실제 개체는 복사되지 않습니다!

이 개념을 더 잘 이해하기 위한 예를 살펴보겠습니다.

```undefined
let user = { name: "Alexander" }

// this instead copies a reference to the previous object
let newUser = user
```

위의 예에서는 두 개의 변수가 있으며, 각 변수는 메모리에 있는 동일한 개체를 참조합니다. 이 경우 변수 newUser는 메모리에서 처음에 선언된 사용자 변수를 참조한다. 개체 및 배열과 같은 참조 유형에만 사용할 수 있으며 문자열이나 부울과 같은 원시 유형의 경우에는 그렇지 않습니다.

> 참고: Object.is을 사용하여 두 값이 실제로 동일한지 여부를 확인할 수 있습니다. 브라우저 콘솔에서 'buille.log(Object.is(user, newUser)'를 실행하면 부울 값 'true'가 반환됩니다.

## 객체 복사 방법

JavaScript는 개체를 복사하는 여러 가지 방법을 제공하지만 전체 복사본은 제공하지 않습니다. 대부분의 경우 얕은 복사본을 수행하는 것이 기본 동작입니다.

우리는 ES6가 언어의 얕은 복사 개체를 위한 두 개의 짧은 구문을 제공한다는 점에 주목해야 한다. 여기에는 `Object.assign()` 및 열거할 수 있는 모든 자체 속성의 값을 개체 간에 복사하는 스프레드 구문이 포함됩니다.

> 참고: 얕은 복사본은 숫자와 문자열과 같은 원시 유형을 복사하지만 개체 참조는 반복 복사되지 않고 복사된 새 개체는 동일한 초기 개체를 참조합니다.

하나씩 살펴보도록 하죠.

### Object.assign() 메서드를 사용하여 개체 복사

객체 생성자 메소드 중 `Object.assign()`은 하나 이상의 소스 객체의 값과 속성을 대상 객체로 복사하는 데 사용됩니다. 소스 개체에서 복사된 속성과 값을 가진 대상 개체를 반환합니다.

Object.assign()은 속성 값을 복사하므로 심층 복제에는 적합하지 않습니다. 기본적으로 이 방법을 사용하면 개체를 얕은 복제 및 둘 이상의 개체를 동일한 속성을 가진 하나의 큰 개체로 병합할 수 있습니다.

- 구문:
composed = Object.assign(대상, ...월)
참고: 이 방법을 사용할 때 대상 개체와 소스 개체에 모두 일치하는 키가 있는 경우 두 번째 개체의 일치 키는 복제 후 첫 번째 키를 재정의합니다.

- 매개 변수:
`target` – 값과 속성을 복사할 대상 개체
`➡` – 값과 속성을 복사할 소스 개체
- `target` – 값과 속성을 복사할 대상 개체
- `➡` – 값과 속성을 복사할 소스 개체
- 반환 값:
이 메서드는 대상 개체를 반환합니다.
- 이 메서드는 대상 개체를 반환합니다.

이제 이 방법을 사용하여 두 개체를 병합하는 매우 간단한 예를 살펴보겠습니다.

```js
let objectA = {a: 1, b: 2}

let objectB = {c: 3, d: 4}

Object.assign(objectA, objectB)

console.log(objectA);
// → { a: 1, b: 2, c: 3, d: 4 }
```

여기서 대상 객체는 `개체 A`이고 소스 객체는 `개체 B`입니다. object.assign()을 사용하는 것은 lodash clone 방법을 사용하여 객체를 얕은 복사하는 것과 비슷하다. 다른 예를 살펴보겠습니다.

```js
const clone = require('lodash.clone')
var objA = { 
  a: 1,
  b: {
        c: 2,
        d: {
            e: 3
      }
  }
}
var objB = clone(objA)
objA.b.c = 30
console.log(objA)
// { a: 1, b: { c: 30, d: { e: 3 } } }
console.log(objB)
// { a: 1, b: { c: 30, d: { e: 3 } } }
```

얕은 복사본이므로, 개체 자체가 아니라 값이 복제되고 개체 참조가 복사됩니다. 따라서 원래 개체에서 개체 속성을 편집하면 이 경우 참조된 내부 개체가 동일하므로 복사된 개체에서도 수정됩니다.

### 분산 구문을 사용하여 개체 복사

스프레드 연산자는 객체 리터럴에 스프레드 속성을 추가하는 ES2018 기능입니다. Object.assign()과 동일한 얕은 클론을 수행할 수 있는 매우 편리한 방법을 제공합니다. 개체에서 스프레드 연산자는 새 값 또는 업데이트된 값으로 기존 개체의 복사본을 만드는 데 사용됩니다.

제공된 개체에서 새 개체로 열거 가능한 속성을 복사합니다. 구문에 따라 사용 예제를 살펴보겠습니다.

```cpp
const copied = { ...original }
```

이제 실제 사례를 살펴보겠습니다.

```js
const objA = { 
    name: 'Alexander', 
    age: 26, 
}

const objB = { 
    Licensed: true, 
    location: "Ikeja" 
}

const mergedObj = {...objA, ...objB}
console.log(mergedObj) 

// { name: 'Alexander', age: 26, Licensed: true, location: 'Ikeja' }
```

위에서 보면 병합 오브제가 objA와 objB의 복제품임을 알 수 있다. 실제로 개체의 모든 열거 가능한 속성이 최종 `mengedObj` 개체에 복사됩니다. 스프레드 연산자는 Object.assign() 방법의 줄임말일 뿐, Object.assign()이 setters를 트리거하는 반면 스프레드 연산자는 setters를 트리거하지 않는 등 두 가지 방법에는 미묘한 차이가 있다.

> 참고: 객체의 얕은 복사본을 수행할 때 객체가 다른 객체를 참조하는 경우 외부 객체에 참조를 복사합니다. 심층 복사를 수행할 때 이러한 외부 개체도 복사되므로 새로운 복제된 개체는 이전 개체와 완전히 독립적입니다.

### JavaScript에서 개체를 완전 복제하는 데 권장되는 방법

대부분의 경우, 우리가 프로그램에서 개체를 복사하기로 결정할 때, 우리의 의도는 실제로 참조에 의해 복사하는 것입니다. 이것은 개체의 얕은 복사본을 만드는 것입니다. 그러나 중첩된 객체에 대해서는 Object.assign() 또는 스프레드(spread)의 동작이 다릅니다.

본질적으로, 어떻게 객체가 구성되는가에 있어서, 그 구조에 관계없이 언어를 통해 객체를 복제하거나 복사할 수 있는 일관된 방법은 없다.

여기서 생기는 질문은, 예를 들어, 두세 단계 깊이까지 깊게 내포된 물체를 복사하는 것입니다. 만약 우리가 새로운 물체를 변화시킨다면, 그것은 우리의 목표물 역할을 하는 원래 물체에 영향을 주지 않는 것입니다. 그렇다면 어떻게 하면 물체를 정확하게 심층적으로 복제할 수 있을까요?

심층적인 복사를 하기 위해서는 커뮤니티에서 잘 테스트하고, 인기가 있으며, 잘 관리하는 라이브러리에 의존하는 것이 최선책입니다. Lodash. 로다시는 각각 얕은 복제와 깊은 복제를 수행할 수 있는 복제 기능과 클론 딥 기능을 모두 제공한다.

예를 들어 Node.js에서 개체를 심층 복사하는 경우 Lodash `clone Deep()` 방법을 사용할 수 있습니다. 예는 다음과 같습니다.

```js
const cloneDeep = require('lodash.clonedeep')

let objA = {
    a: 1,
    b: {
        c: 2,
        d: {
            e: 3
        }
    }
}

// copy objA save as new variable objB
let objB = cloneDeep(objA)

// change the values in the original object objA
objA.a = 20
objA.b.c = 30
objA.b.d.e = 40

console.log(JSON.stringify(objA))
// → {"a":20,"b":{"c":30,"d":{"e":40}}

// objB which is the cloned object is still the same
console.log(JSON.stringify(objB))
// → {"a":1,"b":{"c":2,"d":{"e":3}}
```

로다쉬 복제 딥() 방식은 개체 상속을 보존하면서 가치를 재귀적으로 복제한다는 점만 빼면 복제와 유사하다. 라이브러리의 장점은 각 기능을 개별적으로 가져올 수 있다는 것입니다. 전체 라이브러리를 프로젝트로 가져올 필요가 없습니다. 이것은 우리의 프로그램 의존성의 크기를 크게 줄일 수 있다.

Node.js에서 Lodash 클론 방식을 활용하기 위해 딥 클론의 경우 npmilodash.cloneeep, 얕은 클론의 경우 npmilodash.clone을 실행하여 설치할 수 있다. 다음과 같이 사용할 수 있습니다.

```php
const clone = require('lodash.clone')
const cloneDeep = require('lodash.clonedeep')

const shallowCopy = clone(originalObject)
const deepCopy = clonedeep(originalObject)
```

> 참고: 내장된 JavaScript 개체에서 파생된 개체를 복사하면 원하지 않는 추가 속성이 생성됩니다.

### 네이티브 딥 클로닝

HTML 표준에는 객체의 심층 클론을 생성할 수 있는 내부 구조화 복제/시리얼라이제이션 알고리즘이 포함되어 있습니다. 여전히 특정 내장형에만 제한되지만 복제된 데이터 내의 참조를 보존할 수 있으며, 그렇지 않으면 JSON에서 오류를 일으킬 수 있는 순환 및 재귀 구조를 지원할 수 있다.

Node.js의 지원이 여전히 실험적인 가운데, `v8` 모듈은 구조화된 직렬화 API를 직접 노출시킨다. 예를 들어 개체를 복제하는 방법은 다음과 같습니다.

```js
const v8 = require('v8');

const structuredClone = obj => {
  return v8.deserialize(v8.serialize(obj));
};
```

자세한 내용은 여기에서 확인할 수 있습니다.

### 기타 개체 복제 방법

#### 각 개체 속성을 반복하여 새 빈 개체로 복사

여기에는 소스 개체의 속성을 통해 반복하고 모든 속성을 차례로 대상 개체에 복사하는 작업이 포함됩니다. 그 아이디어는 새로운 객체를 만들고 그것의 속성을 반복하고 복제함으로써 기존 객체의 구조를 복제하는 것이다.

예를 들어 보겠습니다.

```js
let user = {
  name: "Alexander",
  age: 26
};

let clone = {}; // the new empty object

// let's copy all user properties into it
for (let key in user) {
  if (user.hasOwnProperty(key)) {
  clone[key] = user[key];
 }
}

// now clone is a fully independent object with the same content
clone.name = "Chinedu"; // changed the data 

console.log(user.name); // still Alexander in the original object
```

#### JSON.parse/stringify를 사용하여 개체 복제

이렇게 하면 개체를 매우 빠르게 복제할 수 있습니다. 그러나 데이터 손실이 발생함에 따라 신뢰성과 표준성은 떨어집니다.

이 방법을 사용하면 원본 개체가 JSON 안전해야 합니다. 개체 내에서 날짜, 정의되지 않음, 무한성, 함수, 정규식, 맵, 세트 또는 기타 복잡한 유형을 사용하지 않는 경우 개체를 심층 복제하는 매우 간단한 방법은 다음과 같습니다.

```css
JSON.parse(JSON.stringify(object))
```

예를 들어 보겠습니다.

```js
const a = {
  string: 'string',
  number: 123,
  bool: false,
  nul: null,
  date: new Date(),  // string
  undef: undefined,  // lost
  inf: Infinity,  // 'null'
  re: /.*/,  // lost
}

console.log(typeof a.date) // returns  object

const clone = JSON.parse(JSON.stringify(a))

console.log(typeof clone.date)  // returns string 

console.log(clone)
// 
{
  string: 'string',
  number: 123,
  bool: false,
  nul: null,
  date: '2020-09-28T15:47:23.734Z',
  inf: null,
  re: {}
}
```

> 참고: 이 방법은 소스 개체를 JSON으로 변환할 수 없는 경우 안전하게 보호하기 위해 일종의 예외 처리가 필요합니다.

## 결론

기본적으로 JavaScript는 항상 값을 기준으로 전달하므로 변수의 값을 변경해도 기본 원시 유형은 변경되지 않습니다. 그러나 참조로 전달되는 원시적이지 않은 데이터 유형(레이, 함수 및 개체)의 경우 항상 데이터를 변이시킬 수 있으므로 단일 개체 값이 서로 다른 시간에 다른 콘텐츠를 가질 수 있다.

JavaScript 개체 복제는 동일한 개체가 이미 있는 경우 해당 개체를 생성하지 않기 때문에 주로 사용되는 작업입니다. 이제 알 수 있듯이 개체는 참조에 의해 할당되고 복사됩니다. 즉, 변수는 개체 값이 아니라 참조를 저장합니다. 따라서 이러한 변수를 복사하거나 함수 인수로 전달하면 개체가 아니라 해당 참조가 복사됩니다.

숫자와 문자열과 같은 원시적인 유형만 저장하는 단순한 객체에 대해서는 앞에서 설명한 얕은 복사 방법이 효과적일 것이다. 얕은 복사본은 첫 번째 수준이 복사되고 더 깊은 수준이 참조된다는 것을 의미합니다. 그러나 개체 속성이 다른 중첩된 개체를 참조하는 경우 실제 개체는 복사되지 않습니다. 이는 참조만 복사하기 때문입니다.