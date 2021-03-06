---
layout: post
title: "공통 TypeScript 모듈 문제 및 해결 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-29-at-10.16.34-AM.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-29-at-10.16.34-AM.png?fit=730%2C483&ssl=1)

## 도입

TypeScript의 빌드 프로세스는 `tsconfig.json` 파일을 통해 프로젝트 흐름을 수동으로 구성해야 할 때 상당히 복잡해질 수 있습니다. 왜냐하면 이러한 구성에는 TypeScript 컴파일러와 모듈 시스템을 이해해야 하기 때문이다.

많은 TypeScript 프로젝트에서 직접 작업한 결과, TypeScript 모듈을 사용할 때 발생하는 두 가지 일반적인 문제와 이를 효과적으로 해결하는 방법을 찾을 수 있었습니다.

### 전제조건

이 기사를 최대한 활용하려면 다음과 같은 무장을 갖추어야 합니다.

- JavaScript 및 TypeScript의 강력한 배경
- TypeScript 모듈 시스템에 대한 확실한 이해

## 문제 1: 종속성의 위치가 불규칙함

일반적인 경우, `노드 모듈` 디렉토리는 일반적으로 아래와 같이 프로젝트의 루트 디렉토리(즉, baseUrl)에 위치합니다.

```css
projectRoot
├── node_modules
├── src
│   ├── file1.ts
│   └── file2.ts
└── tsconfig.json
└── package.json
```

그러나 모듈들이 baseUrl 아래에 직접 위치하지 않는 경우도 있다. 예를 들어 다음 JavaScript 코드를 살펴보십시오.

```js
// index.js
import express from "express";
```

웹 팩과 같은 로더는 매핑 구성을 사용하여 모듈 이름(이 경우 `express`)을 런타임에 `index.js` 파일에 매핑하고, 따라서 위의 코드 조각을 런타임에 `node_modules/express/lib/express`로 변환한다.

이 때 TypeScript 프로젝트에서 위의 변환된 스니펫을 사용할 때 "paths" 속성을 사용하여 모듈 가져오기를 처리하도록 TypeScript 컴파일러를 구성해야 합니다.

```cpp
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./src", 
    "paths": {
      "express": ["node_modules/express/lib/express"]
    }
  }
}
```

그러나 위의 구성에서 TypeScript 컴파일러는 다음과 같은 오류를 발생시킵니다.
`필수 모듈을 찾을 수 없음`

후드 아래에서 벌어지는 일은 다음과 같습니다. TypeScript 컴파일러는 `node_modules`가 `src` 디렉토리 외부에 있더라도 `src` 디렉토리에서 `node_modules`를 검색하여 해당 모듈을 찾을 수 없다고 판단한다.

## 솔루션 1: 올바른 디렉토리를 찾습니다.

"paths"의 매핑은 "baseUrl"을 기준으로 확인됩니다. 따라서 우리의 구성은 다음과 같아야 한다.

```coffeescript
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./src", // This must be specified if "paths" is.
    "paths": {
      "express": ["../node_modules/express/lib/express"] // This mapping is relative to "baseUrl"
    }
  }
}
```

이 구성을 사용할 때 TypeScript 컴파일러는 `src` 디렉토리에서 디렉토리를 "점프"하고 `node_modules` 디렉토리를 찾는다.

또는 아래의 구성도 유효합니다.

```coffeescript
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".", // This must be specified if "paths" is.
    "paths": {
      "express": ["node_modules/express/lib/express"] // This mapping is relative to "baseUrl"
    }
  }
}
```

이 구성을 사용할 때, TypeScript 컴파일러는 프로젝트의 루트 디렉토리에서 `node_modules` 디렉토리를 검색합니다.

## 문제 2: 여러 예비 위치

우리의 두 번째 공통적인 문제는 위치와도 관련이 있습니다. 다음 프로젝트 구성을 살펴보겠습니다.

```coffeescript
projectRoot
├── view
│   ├── file1.ts (imports 'view/file2' and 'nav/file3')
│   └── file2.ts
├── components
│   ├── footer
│   └── nav
│       └── file3.ts
└── tsconfig.json
```

여기서 `view/file2` 모듈은 뷰 디렉토리에, `nav/file3` 모듈은 컴포넌트 디렉토리에 위치합니다.

코드에서 이 시점에서 컴파일러가 여러 위치에서 이러한 모듈을 해결하는 방법을 모르기 때문에 여러 위치에서 모듈을 확인하는 것은 다소 어려울 수 있습니다. 다음 섹션에서는 이 문제를 해결하는 방법을 검토합니다.

## 솔루션 2: 모듈을 찾고 가져오기를 해결합니다.

아래 구성을 사용하여 컴파일러가 프로젝트에서 가져온 모듈 중 두 위치(즉, `["*", "components/*"])를 찾아보도록 할 수 있습니다.

```undefined
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "*": ["*", "components/*"]
    }
  }
}
```

이 예에서 어레이의 "*" 값은 모듈의 정확한 이름을 의미하며, "components/*" 값은 접두사가 추가된 모듈 이름("components")입니다.

이제 컴파일러에 다음과 같이 두 가지 가져오기를 해결하도록 지시할 수 있습니다.

```coffeescript
import 'view/file2'
```

그런 다음 컴파일러는 `view/file2`를 배열의 첫 번째 위치(`*`)로 대체하고 이를 `projectRoot/view/file2.ts`가 되는 `baseUrl`과 결합한다. 이렇게 하면 모듈을 찾을 수 있습니다.

이 단계가 끝나면 컴파일러는 다음 가져오기 단계로 이동합니다.

```coffeescript
import 'nav/file3':
```

마찬가지로 컴파일러는 nav/file3를 배열의 첫 번째 위치(예: k.a. nav/file3)로 대체하고 이를 projectRoot/nav/file3.ts로 만드는 baseUrl과 결합한다. 이번에는 파일이 없으므로 컴파일러가 nav/file3를 두 번째 위치인 components/*로 대체하고 baseUrl과 결합한다. 이렇게 하면 `projectRoot/component/nav/file3.ts`가 발생하여 모듈을 찾을 수 있습니다.

## TypeScript가 모듈을 기본적으로 확인하는 방법

앞에서 설명한 대로 TypeScript 컴파일러를 구성하지 않고, TypeScript는 컴파일 타임에 요청된 모듈을 찾기 위해 기본적으로 Node.js 런타임 해상도 전략을 채택할 것이다.

이를 위해 TypeScript 컴파일러는 .ts 파일, .d.ts, .tsx 및 패키지를 찾습니다.json 파일들 컴파일러가 패키지를 찾는 경우.json 파일에 입력 파일을 가리키는 형식 속성이 포함되어 있는지 여부를 확인합니다. 이러한 속성을 찾을 수 없는 경우 컴파일러는 다음 폴더로 이동하기 전에 인덱스 파일을 찾아봅니다.

여기 예가 있습니다. /projectRoot/view/file1.ts`의 `view/file2`에서 `{b} 가져오기`와 같은 가져오기 문을 사용하면 ".view/file2" 위치를 찾을 수 있습니다.

- /projectRoot/view/file2.ts가 존재합니까?
- /projectRoot/view/file2.tsx`가 존재합니까?
- /projectRoot/view/file2.d.ts가 존재합니까?
- /projectRoot/view/file2/package를 실행합니다.json("type" 속성을 지정하는 경우)이 있습니까?
- `/projectRoot/view/file2/index.ts`가 존재합니까?
- /projectRoot/view/file2/index.tsx`가 존재합니까?
- `/projectRoot/view/file2/index.d.ts`가 존재합니까?

이 시점에서 모듈을 찾을 수 없는 경우, 다음과 같이 가장 가까운 상위 폴더에서 한 단계 건너뛰는 동일한 프로세스가 반복됩니다.

- /projectRoot/file2.ts가 존재합니까?
- /projectRoot/file2.tsx`가 존재합니까?
- /projectRoot/file2.d.ts가 존재합니까?
- /projectRoot/file2/package를 실행합니다.json("type" 속성을 지정하는 경우)이 있습니까?
- /projectRoot/file2/index.ts가 존재합니까?
- /projectRoot/file2/index.tsx`가 존재합니까?
- /projectRoot/file2/index.d.ts가 존재합니까?

## 결론

때로는 모듈이 해결되지 않은 이유를 진단하는 것이 어려운 복잡한 상황에 직면할 수 있습니다. 이러한 상황에서 `tsc --traceResolution`을 사용하여 컴파일러 모듈 해상도 추적을 활성화하면 모듈 해상도 프로세스 중에 발생한 일에 대한 더 많은 통찰력을 얻을 수 있어 앞으로 나아갈 올바른 솔루션을 선택할 수 있다.

일반적인 TypeScript 모듈 문제가 부각되고, 이 게시물에 제공된 솔루션을 통해 TypeScript 프로젝트의 모듈을 처리하도록 TypeScript 컴파일러를 구성하는 것이 좀 더 쉬워졌으면 합니다.

이 게시물이 유익하고 도움이 되었기를 바랍니다. 또한 TypeScript 모듈 해상도에 대한 자세한 내용은 공식 TypeScript 설명서를 참조하십시오.