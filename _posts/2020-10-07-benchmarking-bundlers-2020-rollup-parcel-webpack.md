---
layout: post
title: "벤치마킹 번들러 2020: 롤업 대 롤업 구획 대 웹 팩"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/benchmarking-bundlers-2020.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/benchmarking-bundlers-2020.png?fit=730%2C487&ssl=1)

번들러는 모든 최신 웹 앱, 특히 모든 JavaScript 앱의 초석 기술 역할을 합니다. 프런트 엔드 세계가 더 많은 클라이언트 쪽 렌더링 앱으로 발전함에 따라 수많은 JS를 효율적으로 번들링하는 방법에 대한 아이디어가 나오기 시작했습니다.

인지적으로, 옵션의 수가 증가함에 따라, 선택하기가 어려워진다. 여기서는 현재 사용 가능한 상위 번들의 기술 및 비기술 역량을 분석하여 귀하의 결정을 쉽고 정확하게 전달할 수 있도록 하겠습니다.

다음 내용을 다루겠습니다.

- 웹팩
- 롤업
- 구획.js

기술 역량을 비교하기 위해, 우리는 리액트 페이스북 픽셀을 라이브러리로, 그리고 매우 기본적인 리액트 앱을 샘플로 선택하여 이러한 번들러를 각각 벤치마킹했다.

이러한 비교는 이러한 훌륭한 도구 중에서 단 한 개의 승리자를 확보하는 것이 아니라 보다 쉽게 결정을 내릴 수 있도록 도와주는 것입니다. 이 모든 번들러는 훌륭한 사람들이 관리하는 훌륭한 도구이며, 이런저런 면에서 모두 아주 훌륭합니다. 모든 유지관리자, 기여자, 후원자 및 후원자에게 응원 🍻

## 구성

번들을 구성하는 것은 프런트 엔드 세계에서 가장 저주받았지만 가장 정교한 분야 중 하나입니다. 소규모 애플리케이션의 경우 이것이 매우 간단해야 한다고 느낄 수 있다. 그러나 애플리케이션의 규모가 커짐에 따라 애플리케이션의 효율성과 성능을 유지하기 위해서는 보다 정교한 구성이 필요합니다.

우리는 개발자들 사이에서 작은 앱을 위해 오늘날의 기술 스택을 구성하는 것이 얼마나 지루한지에 대한 많은 논쟁을 보아왔다. 이러한 논쟁과 공동체의 대다수에 의해 이후에 채택된 일반적인 패턴은 많은 번들로 하여금 제로 구성 솔루션을 제공하게 만들었다.

거의 모든 번들러가 주장하지만, 어떤 번들러에도 0으로 구성될 수는 없습니다. 구성 가이드를 가능한 한 신속하게 구성하고 최대한 편안하게 유지하는 것이 중요합니다.

이 모든 번더들은 이 지역에 홍조를 띤다. 여기서는 React Facebook 픽셀에 대한 배포 패키지를 생성하기 위한 구성을 공유합니다. 이것은 이 번더들의 각각의 모습을 엿볼 수 있게 해 줄 것입니다.

### 웹팩

```js
const path = require('path');
const TerserPlugin = require('terser-webpack-plugin');

module.exports = {
  entry: ['./src/index.js'],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'fb-pixel-webpack.js',
    libraryTarget: 'umd',
    library: 'ReactPixel',
  },
  module: {
    rules: [
      {
        use: 'babel-loader',
        test: /\.js$/,
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.js'],
  },
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          warnings: false,
          compress: {
            comparisons: false,
          },
          parse: {},
          mangle: true,
          output: {
            comments: false,
            ascii_only: true,
          },
        },
        parallel: true,
        cache: true,
        sourceMap: true,
      }),
    ],
    nodeEnv: 'production',
    sideEffects: true,
  },
};
```

### 롤업

```coffeescript
import babel from '@rollup/plugin-babel';
import { nodeResolve } from '@rollup/plugin-node-resolve';
import { terser } from 'rollup-plugin-terser';
import filesize from 'rollup-plugin-filesize';
import progress from 'rollup-plugin-progress';
import visualizer from 'rollup-plugin-visualizer';

export default {
  input: 'src/index.js',
  output: [
    {
      file: 'dist/fb-pixel.js',
      format: 'cjs',
      name: 'ReactPixel',
      exports: 'named',
    },
  ],
  plugins: [
    terser(),
    babel({ babelHelpers: 'bundled' }),
    nodeResolve(),
    // All of following are just for beautification, not required for bundling purpose
    progress(),
    visualizer(),
    filesize(),
  ],
};
```

### 구획.js

기본 구성으로 라이브러리를 처리하기에 충분했기 때문에 구획에 대한 어떤 구성도 필요하지 않았습니다. 다음은 사용한 명령입니다.

```undefined
bash
    "bundle:parcel": "parcel build src/index.js --experimental-scope-hoisting --out-file fb-pixel-parcel.js",
```

이에 대한 결론은 다음과 같습니다.

- 웹팩은 여전히 우리에게 ES5 구문을 사용하라고 요구하는데, 그것은 약간 문제를 만든다.
- 롤업은 구문이 단순하고 라이브러리 관리에 이상적임
- 정교한 애플리케이션에 맞게 확장할 수 있는 탁월한 기본 구성이 포함된 구성 파일 지원 제공

1️⃣ 롤업 2️⃣ 구획 3️⃣ 웹 팩

## 특징들

새롭고 더욱 정교한 웹 앱에 대한 역량을 유지하기 위해, 이러한 번들러들은 대부분의 최신 앱에 필요한 모든 기능을 제공합니다.

Web.dev 팀은 최근 Tooling이라는 새로운 이니셔티브를 시작했습니다.기능 세트를 직접 비교하여 다음 프로젝트에 적합한 도구를 쉽게 선택할 수 있도록 하기 위한 목적으로 보고하십시오.

번들러가 관련된 경우, 팀은 6차원과 61차원의 특징 테스트를 통해 그것들을 비교했다. 이 보고서는 이 모든 번들러가 무엇을 제공하는지 우리에게 큰 통찰력을 제공합니다. 여기 우리는 이러한 테스트의 결과를 요약했습니다.

### 코드 분할

코드 분할을 통해 공통 종속성 또는 모듈을 공유 번들로 추출하고 페이지에 필요한 코드만 다운로드 및 실행하도록 할 수 있습니다. 코드 분할은 대규모 애플리케이션의 효율성을 유지하는 데 있어 중요한 측면이다. Web.dev 팀은 각 번들러를 8가지 기준에 대해 평가했습니다. 결과는 아래와 같습니다.

#### 결과:

1️⃣ 롤업 [6/8] 2️⃣ 웹팩 [4/8] 3️⃣ 구획 [3.5/8]

이러한 번들러 중 어느 것도 다른 번들이 사용하는 내보내기를 기준으로 모듈을 분할할 수 없습니다. 하지만 그 외에도 롤업이 다른 모든 테스트를 통과하기 때문에 맨 위에 있습니다.

### 해싱

애플리케이션 로드 시간을 줄이려면 리소스를 한 번 다운로드한 후 클라이언트 측에서 캐슁하고 재사용해야 합니다. 리소스의 캐시를 무효화하기 위해 리소스 이름을 변경할 수 있습니다. 이 변경은 버전 식별자를 리소스 이름과 연결하여 수행할 수 있습니다.

빌드 도구는 파일 내용에 따라 버전 식별자를 생성할 수 있습니다. 파일 내용이 변경되면 새 버전 ID를 갖게 되며, 변경되지 않으면 클라이언트가 캐시된 결과를 재사용하게 됩니다.

과도한 캐시 무효화를 방지하기 위해 번들러는 무효화 "cascade"가 제대로 구현되었는지 확인해야 합니다. 즉, 업데이트된 모든 JS 및 비 JS 자산에는 새 해시가 있어야 하며, 해당 자산을 참조하는 모든 JS 번들은 새 URL을 참조하도록 업데이트해야 합니다. 따라서 업데이트된 컨텐츠와 해당 자산을 참조하는 JS에 대한 새 해시 등이 포함됩니다.

번들러는 10가지 다른 캐싱 기준에 따라 비교되었습니다.

#### 결과:

1️⃣ 구획 [8.5/10] 2️⃣ 웹팩 [8/10] 3️⃣ 롤업 [6/10]

소포는 웹팩을 능가하는 정말 인상적인 기능을 가지고 있다: 최종 컴파일된 코드를 기반으로 한 번들 해시를 능가하기 때문에 코멘트의 변화가 번들 해시에 영향을 미치지 않는다는 것을 의미한다.

### 비JavaScript 리소스

웹 앱은 JavaScript에만 국한되지 않으며 풍부한 컨텐츠, 글꼴, 직렬화된 데이터, HTML 및 CSS를 포함한 많은 다른 리소스를 포함합니다.

최근에는 JS가 이러한 모든 자산을 보유하고 배치하는 중심 지점으로 부상하고 있습니다. JS에서는 이러한 비 JS 자산 가져오기를 허용하지 않지만, 번들로 인해 이러한 작업이 가능해졌습니다. 코드 분할 및 해싱 기능을 염두에 두고 이러한 자산을 처리하는 것이 더욱 복잡해집니다.

번들러는 응용 프로그램을 그래프로 간주합니다. 각 리소스를 가져온 다른 모든 리소스와 연결된 노드로 처리합니다. 이렇게 하면 CSS의 네임스페이스와 같은 해시 및 사용 기반 변환 후 리소스 URL을 보다 쉽게 수정할 수 있습니다. 이 형상 범주의 경우, 16개 기준에서 번들러를 비교하였다.

#### 결과:

1️⃣ 웹팩 [15.5/16] 2️⃣ 롤업 [15/16] 3️⃣ 소포[9.5/16]

자원 처리에 관한 한, 소포는 경쟁에서 한참 뒤처져 있다. 롤업과 웹 팩은 이제 둘 다 비 JS 리소스 번들에 필요한 거의 모든 것을 제공하기 때문에 완전히 완전히 유지됩니다.

### 출력 모듈 형식

최신 브라우저는 이제 ECMA스크립트 모듈(ESM)을 지원하지만, 이전 브라우저 버전을 지원한다는 것은 JS를 CommonJS로 변환해야 한다는 것을 의미한다. 이 섹션에는 세 가지 기준만 있었다.

#### 결과:

1️⃣ 롤업 [3/3] 2️⃣ 웹팩 [2/3] 2️⃣ 구획 [2/3]

다른 어느 것도 ESM 번들을 생성할 수 없기 때문에 롤업이 여기서 우위를 차지합니다.

### 변환

현대 응용 프로그램에서 번들을 채택한 중요한 원동력은 코드와 자산의 변환이었습니다. 이러한 변환의 일부는 압축, 축소 등과 같은 일반적인 목적이고, 다른 일부는 특정 자산 집합에 맞춰져 있다. 이러한 변환은 일반적으로 다양한 버전의 브라우저와 최적화를 지원하는 것을 목표로 한다.

Web.dev 팀은 번들러의 변환 기능을 비교하기 위한 7가지 기준을 식별했습니다.

#### 결과:

1️⃣ 웹팩 [6/7] 1️⃣ 롤업 [6/7] 3️⃣ 구획 [4.5/7]

웹 팩이나 롤업 중 어느 것도 동적으로 가져온 모듈에서 데드 코드를 제거할 수 없지만, 이 두 가지 테스트는 Brotli 압축 지원을 포함하여 다른 모든 테스트를 통과했습니다.

## 벤치마킹

오늘날 웹 번들러는 단순히 프로덕션 빌드를 만드는 데만 사용되는 것이 아닙니다. 오히려, 우리의 일상적인 개발은 그들의 성과에 크게 좌우됩니다. 앞에서 언급했듯이, 우리는 번들의 속도와 생성된 번들의 크기를 벤치마킹하기 위해 작은 Retact 애플리케이션을 만들었습니다.

이러한 벤치마크는 다음에 대해 수행되었습니다.

MacBook Pro(15인치, 2018) | 2.2GHz 6코어 Intel Core i7 | 16GB 2400MHz DDR4 | Radeon Pro 555X 4GB, Intel UHD 그래픽스 630 1536MB

### 번들링 속도

애플리케이션 개발의 경우 개발 환경과 prod 환경 모두에서 빌드 시간이 가장 빠른 웹 팩 4가 확실한 승자입니다. 소포는 웹팩으로서 거의 절반의 시간에 도서관 번들로 큰 도약을 한다.

### 빌드 크기

크기에 관한 한 롤업이 여기서 선두를 달리고, 구획 v2가 그 뒤를 바짝 따른다.

의견 섹션에서 결과를 공유하거나 당사 리포지토리에서 문제를 열어 이 벤치마크를 개선할 수 있도록 도와주십시오.

## 설명서

웹팩은 그것의 복잡성으로 인해 가장 저주받은 도서관들 중 하나였지만, 그것의 문서는 지난 몇 년 동안 개선되었다. 많은 개발자들이 자신의 경험을 공유해 왔으며, 웹 팩의 복잡성에 대해 배울 수 있는 많은 리소스가 있습니다. 특정 기능은 여전히 문서화되지 않았으며, 대부분은 실제 고급 사용 사례에 필요합니다.

롤업에는 좋은 설명서가 있으며 롤업을 자세히 학습할 수 있는 많은 리소스가 있습니다. 대부분의 플러그인이 공식이 아니기 때문에 플러그인을 선택하는 데 다소 문제가 있을 수 있습니다. 그럼에도 불구하고, 공식 및 활성 플러그인은 대부분의 사용 사례를 커버하기에 충분하기 때문에 라이브러리 개발자들에게는 이동 가능한 솔루션이다.

구획 v2는 아직 베타 버전이며 문서화가 진행 중입니다. 온보드 플러그인에 대한 표준을 설정했기 때문에, 이것은 진행될수록 도움이 될 것입니다.

## 플러그인 및 에코시스템

플러그인에 관해서는 비교할 것이 별로 없다. 대부분의 일반적인 사용 사례에 대한 플러그인은 모든 번들러에 사용할 수 있지만 각 번들러의 품질은 매우 다를 수 있습니다.

웹팩에는 많은 수의 공식 플러그인이 있어서 쉽고 빠르게 선택할 수 있다. 롤업에는 커뮤니티 플러그인이 많이 있으며 활성 유지 및 정지 상태입니다. 사람들은 그들에게 가장 적합한 것을 시험하고 결정하기 위해 약간의 노력을 기울여야 한다.

패키지에는 v1이 포함된 플러그인에 대한 고유한 메커니즘이 있으므로 플러그인을 구성할 필요가 없습니다. 플러그인을 설치하고 실행하기만 하면 됩니다. v2를 사용하면 개발 중인 구성 설정이 있으며 정교한 사용 사례에 더 많은 전력을 제공할 수 있습니다.

## 결론

여러분이 새로운 프런트 엔드 개발자든 노련한 개발자든 간에, 여러분은 아마도 번들에 대한 토론을 듣거나, 혹은 여러분 스스로 참여했을 것입니다. 웹 팩은 유연성으로 칭찬받지만 컴플렉스로 인해 욕을 먹는다. 롤업은 라이브러리에 대해 우수한 것으로 간주됩니다. 소포는 큰 영향을 미쳤고 v2가 베타 버전이 아니면 더 큰 것이 될 수 있다.

무엇을 선택할까요? 앞서 말씀드렸듯이, 고객님의 요구 사항에 따라 다릅니다. 이번 비교가 결정을 쉽게 내리는데 도움이 되기를 바랍니다.

## 존댓말

- 스노우팩은 시내에 새로 생겼지만 미래를 위한 합리적인 기반을 만들고 있다.
- 포이는 웹팩에 관한 인간 친화적인 포장지이다. 이 번들러는 소포와 웹팩 사이의 어딘가에 있다.
- Rust 기반 번들러인 Pax는 더 빠른 속도를 제공할 것을 약속합니다.