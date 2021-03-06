---
layout: post
title: "이 옷을 입으면 내 보따리가 커 보이니?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/slimmingdownyourbundlesizewithrollup.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/slimmingdownyourbundlesizewithrollup.png?fit=730%2C412&ssl=1)

## 어디로 가고 싶어요?

아래 코드는 롤업과 함께 번들로 제공됩니다. 롤업에서 트리 흔들림이라고 하는 프로세스를 사용하여 코드가 실행해야 하는 최소 가져오기가 결정되었습니다.

```coffeescript
import React from 'react';
import { useFormikContext, useField } from 'formik';
import isEmpty from 'lodash/isEmpty';
import { DatePicker, Checkbox, CheckboxGroup, Radio, RadioGroup, TransitionComponent, TransitionType } from '@my/component-library';
```

나무 흔들기는 죽은 코드를 없애기 위해 사용되는 기술이다. 롤업에서 formik에서 두 개의 구성 요소만 필요한 것으로 확인되었습니다.

## 이 옷을 입으면 내 보따리가 커 보이니?

오늘날 대부분의 자바스크립트 사용량이 많은 응용프로그램이 브라우저에 너무 많은 코드를 전송한다는 것은 비밀이 아니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/bundle.png?resize=934%2C730&ssl=1)

위의 이미지에는 `react-dom.production.min.js`의 방대한 크기가 모두 볼 수 있다.

포믹은 거의 모든 로다쉬를 포함시킴으로써 불필요하게 그것의 크기를 두 배로 늘렸다. Lodash는 수많은 JavaScript 번들의 엄청난 문제입니다.

## 어떻게?

묶음 사이즈를 살찌우는 수입 모듈을 모두 포믹이 사용하고 있는 것 같지는 않다. 필요한 것은 데드 코드를 제거하거나 임포트가 사용하는 모듈만 가져오는 방법입니다.

번들러가 데드 코드를 식별하기 위해서는 코드베이스에 대한 정적 분석을 수행해야 합니다.

## CommonJS로 번들 크기 증가

저는 요즘 모든 일에 TypeScript를 사용하고 있으며, 최근까지 tsconfig.json의 module 설정을 commonjs로 설정했습니다.

CommonJS 모듈은 동적 가져오기 및 내보내기가 지원되므로 최적화하기가 어렵습니다.

```js
const a = require(localStorage.getItem('somekey'));

const b = 'exportThis';

module.exports[b] = (a) => a;
```

require는 실제로 함수 호출이기 때문에 동적으로 계산된 문자열을 사용하여 로드할 모듈을 런타임에 결정할 수 있다.

정적 분석기는 동적 수출입의 판독조차 시도하지 않고 대신 모든 것을 잡을 것이다.

## ESM(Ecmascript) 모듈 구문

import(가져오기)와 export(수출) 모두 ESM 모듈이 있는 언어의 일부이다. 애매모호함이 없고, 역동성의 부족은 정적 분석을 용이하게 한다.

코드 분할을 위해 ESM 모듈에서 동적 가져오기가 여전히 가능합니다.

## 롤업은 데드 코드를 제거하는 시장의 선두 업체입니다.

나는 웹팩을 오랫동안 사용해왔고 웹팩은 각 모듈을 로더와 모듈 캐시를 구현하는 기능으로 포장해서 작동한다. 런타임에 이러한 각 모듈은 차례로 평가되어 모듈 캐시를 채웁니다. 웹 팩 접근 방식은 핫 모듈 교체(HMR)와 같은 것을 가능하게 하지만 이 접근 방식으로는 오버헤드가 발생합니다.

롤업은 다른 방식을 취합니다. 즉, 모든 코드를 스코프 호이스트라고 하는 동일한 수준에 둡니다. 결과 번들은 모듈별 평가가 없기 때문에 훨씬 적은 오버헤드로 더 작아집니다.

그 단점은 롤업이 ESM 모듈 의미론에 의존한다는 것이다. 더 작은 번들로 가는 여정 중 1단계는 모든 공통점을 찾는 것입니다.JS 패키지는 100% ESM 패키지로 제공됩니다.

## 꾸러미.json 모듈 해상도

롤업 또는 웹 팩과 같은 번들러는 일반적으로 패키지의 필드를 지정하는 메커니즘을 가지고 있습니다.json 파일이 진입점이다.

사용 중인 패키지에 다음과 같은 가져오기 기능이 있는 경우:

```coffeescript
import * as D3 from 'd3';
```

패키지의 다음 필드입니다.json`은 모듈의 진입점을 결정합니다.

- type – .js로 끝나는 파일은 가장 가까운 상위 패키지가 있을 때 ES 모듈로 로드됩니다.json 파일에 `type` 값의 최상위 필드 `type`이 포함되어 있습니다.
- module – 가져온 파일이 ESM 모듈이 될 것임을 나타내는 경우
- main – 이 필드는 일반적으로 Common(공통)을 해결하는 데 사용됩니다.JS 모듈
- 브라우저 - 노드가 사용하지 않습니다. 이 필드를 사용하여 웹 및 노드에 대해 서로 다른 번들을 생성할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/version.png?resize=293%2C190&ssl=1)

Webpack을 사용하면 `mainFields` 옵션을 사용하여 어떤 필드가 먼저 검색되는지 우선 순위를 설정할 수 있습니다.

```css
resolve: {
  mainFields: ['module', 'browser', 'main'],
```

롤업을 사용하면 @rollup/plugin-node-resolve 롤업 플러그인을 사용할 수 있습니다.

```css
resolve({
  mainFields: ['module', 'browser', 'main'],
```

우리 여정의 첫 단계는 `유형`을 `모듈`로 설정하고 `모듈` 필드를 공급하는 것이다.

```undefined
{
  "name": "@ds/util",
  "version": "6.3.0",
  "type": "module",
  "module": "dist/index",
  "browser": "dist/index",
  "main": "dist/index",
}
```

모듈 메인 브라우저 파일 확장자가 있을 경우 웹팩에 문제가 생겼고 웹팩은 웹빌드를 공략할 때 브라우저 필드의 존재에 의존했다.

## mjs 파일이 있는 가져오기

ES 모듈은 확장자가 .mjs인 파일의 대상입니다. 이후 버전의 노드에서는 `mjs` 파일의 가져오기가 ESM과 호환된다고 가정합니다.

브라우저 월드 또는 더 정확하게는 번들러 월드에서 그것은 규칙에 가깝지만, Webpack과 Rollowling은 이 파일을 다르게 다루고 다른 대상으로 컴파일할 것이다.

React 및 React-router와 같은 공통 종속성의 문제에 부딪혔습니다.JS 종속성. 해결책은 파일 확장명이 .esm.js인 파일을 출력하는 것이었다.

## 'requires'를 부팅하여 연락하십시오.

제가 접한 주요 리팩토링은 `scss` 파일을 가져오는 `requires` 문을 제거하는 것이었습니다.

```js
const styles = require('./Start.module.scss');
```

그러면 다음이 됩니다.

```coffeescript
import styles from './Start.module.scss';
```

## TypeScript

TypeScript를 사용하여 컴파일러에 `ESNext`로 전송하도록 지시해야 했습니다.

tsconfig.json`에서 `module` 필드는 다음과 같이 변경되었습니다.

```bash
"module": "CommonJS",
```

에게

```bash
"module": "ESNext",
```

## 롤업

이것이 제가 결정한 롤업 구성입니다.

```coffeescript
const bundle = await rollup({
    input: inputFile,
    external: (id: string) => {
      return !id.startsWith('.') && !path.isAbsolute(id);
    },
    treeshake: {
      moduleSideEffects: false,
    },
    plugins: [
      resolve({
        mainFields: ['module', 'browser', 'main'],
        extensions: ['.mjs', '.esm.js', 'cjs', '.js', '.ts', '.tsx', '.json', '.jsx'],
      }),
      json(),
      postcss({
        extract: false,
        modules: true,
        use: ['sass'],
      }),
      typescript({
        clean: true,
        typescript: require('typescript'),
        tsconfig: paths.tsConfig,
        abortOnError: true,
        tsconfigDefaults: {
          compilerOptions: {
            sourceMap: true,
            declaration: true,
            target: 'esnext',
            jsx: 'react',
          },
          useTsconfigDeclarationDir: true,
        },
        tsconfigOverride: {
          compilerOptions: {
            sourceMap: true,
            target: 'esnext',
          },
        },
      }),
      babel({
        exclude: /\/node_modules\/core-js\//,
        babelHelpers: 'runtime',
        ...babelConfig,
      } as RollupBabelInputPluginOptions),
      injectProcessEnv({
        NODE_ENV: 'production',
      }),
      sourceMaps(),
      minify === true &&
        terser({
          compress: {
            keep_infinity: true,
            pure_getters: true,
            passes: 10,
          },
          ecma: 2016,
          toplevel: false,
          format: {
            comments: 'all',
          },
        }),
    ],
  });
}
```

사용되는 플러그인은 다음과 같습니다.

- @https/https-node-contracts
- @https/httpson-json
- 롤업-백업-소스 맵
- 롤업 형식 스크립트2
- 롤업식 터어

위의 구성으로 롤업 롤업 기능을 호출하면 다음 코드를 사용하여 번들을 디스크에 쓸 수 있는 번들 개체가 반환됩니다.

```css
await bundle.write({
  file: path.join(paths.appBuild, 'index.js'),
  format: 'esm',
  name: packageName,
  exports: 'auto',
  sourcemap: true,
  esModule: true,
  interop: 'esModule',
});
```

이제 패키지가 ESM을 준수합니다.

## 마무리하기

세계는 천천히 그러나 확실히 ESM 모듈로 눈을 돌리고 있다. 당신을 약속된 땅으로 유인할 작은 보따리 크기의 약속이 있습니다.

롤업을 사용하여 패키지를 번들링하지만 개발 시 핫 모듈 교체에 웹 팩을 사용합니다.

번들러가 무엇이든 CommonJS로부터 멀어지는 것이 점점 더 이치에 맞다.