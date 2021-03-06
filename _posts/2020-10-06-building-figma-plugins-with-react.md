---
layout: post
title: "React를 사용하여 Figma 플러그인 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/building-figma-plugins-react.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/building-figma-plugins-react.png?fit=730%2C487&ssl=1)

## 도입

이 자료에서는 React를 사용하여 UI를 구현하기 위해 웹 팩으로 Figma 플러그인 프로젝트를 설정하는 방법에 대해 설명합니다. 플러그인의 목적은 세 가지 임의 색상을 생성하고 사용자가 Figma 문서에서 선택한 요소에 할당할 색상을 선택할 수 있도록 하는 것입니다.

이 예제 플러그인은 UI와 플러그인 코드 간의 통신을 설정하는 방법과 API를 사용하여 주어진 기본 작업을 달성하는 방법을 보여줍니다.

## 플러그인 프로젝트 생성

먼저 Figma 앱을 통해 Figma 플러그인을 생성해야 합니다. 이렇게 하면 플러그인과 해당 ID의 정보가 포함된 `manifest.json` 파일이 포함된 템플릿 프로젝트가 생성됩니다. 프로젝트를 생성하려면 왼쪽 메뉴에서 커뮤니티 섹션을 클릭한 다음 플러그인 탭에서 새 플러그인 만들기 버튼을 찾을 수 있습니다.

플러그인을 생성하면 필요한 파일이 모두 생성되지만 TypeScript와 Sass 컴파일러를 통합하고 JavaScript, 스타일 파일 및 SVG와 같은 모든 자산을 번들링하려면 번들러가 필요합니다. 이를 위해 우리는 웹팩을 사용할 것이다.

웹 팩을 구성하기 전에 원본 파일에 대한 준비가 필요합니다. /src 폴더를 만들고 이 폴더에서 `ui.html` 및 `code.ts` 파일을 이동합니다. 미리 생성된 `code.js` 파일을 삭제할 수 있습니다. 이제 TypeScript로 작성된 React 코드를 포함하는 `ui.tsx` 파일이 필요합니다. `/src` 폴더에도 이 파일을 만드십시오.

우리가 마지막으로 해야 할 일은 `ui.html` 파일을 편집하는 것이다. 이 파일에는 이미 일부 HTML과 JavaScript가 포함되어 있지만, React에 의해 채워질 HTML 요소만 있으면 됩니다. 따라서 `ui.html`의 전체 내용을 다음 행으로 바꿉니다.

```js
<div id="root"></div>
```

결국 `/src` 폴더는 다음과 같이 나타납니다.

```undefined
-- src
 |- ui.tsx
 |- code.ts
 |- ui.html
```

마지막으로 필요한 것은 manifest.json 파일을 적절하게 구성하는 것이다.

거기서 메인 키와 ui 키를 볼 수 있습니다. 이것은 Figma에게 플러그인 코드와 UI 코드가 포함된 파일을 어디에서 찾아야 하는지 알려준다. 웹팩은 기본적으로 파일 번들을 `/dist` 폴더에 넣기 때문에 메인키와 ui키에 대해서는 해당 폴더 아래의 특정 파일을 가리켜야 한다.

//dist/code.js는 `code.ts`에서 컴파일된 파일이며, `/dist/ui.html`은 `<script> 태그 사이에 인라인 자바스크립트 코드를 포함할 HTML 파일이다.

Figma는 UI에 대해 하나의 단일 파일을 허용하므로, `src` 속성이 JavaScript 파일을 가리키는 `<script> 태그를 가질 수 없습니다. 이것이 바로 `ui.html`이 인라인 JavaScript 코드를 포함해야 하는 이유입니다. 나중에 웹 팩에서 특별히 지시하는 작업이 될 것입니다.

```bash
{
  ...
  "main": "./dist/code.js",
  "ui": "./dist/ui.html"
}
```

### 웹 팩 구성

첫 번째 비즈니스 순서는 react와 react-dom을 종속으로 설치하는 것이다. 종속성 트리에 웹팩, 웹팩-cli, typescript를 devDependencies로 설치한다. 이미 전역으로 `typescript`가 설치되어 있는 경우 건너뛸 수 있습니다.

이제 플러그인 프로젝트의 루트 디렉터리에 `webpack.config.js` 파일을 생성하십시오. 이 파일은 처음에 `mode`와 `devtool`을 정의했던 것과 같아야 합니다. Figma 문서에 따르면 figma의 eval이 일반적인 eval과 다르게 작동하기 때문에 devtool 정의가 필요하다.

```js
const webpack = require('webpack');
const path = require('path');

module.exports = (env, argv) => ({
  mode: argv.mode === 'production' ? 'production' : 'development',
  devtool: argv.mode === 'production' ? false : 'inline-source-map',
})
```

이제 웹 팩에 번들을 알려주는 진입점을 정의하겠습니다.

```coffeescript
module.exports = (env, argv) => ({
  mode: argv.mode === 'production' ? 'production' : 'development',
  devtool: argv.mode === 'production' ? false : 'inline-source-map',

  entry: {
    ui: './src/ui.tsx',
    code: './src/code.ts',
  },
})
```

다음으로, 특정 파일 확인 규칙을 정의하고 로더로 로드하겠습니다. 스타일링을 위해서는 `tsx` 로더와 로더가 필요하며, UI에 SVG 파일을 포함시키고 싶을 가능성이 높기 때문에 SVG 로더도 있으면 좋습니다.

다음 npm 패키지를 `devDependencies`로 설치합니다.

```css
ts-loader
style-loader
css-loader
sass-loader
@svgr/webpack
node-sass
```

로더가 추가된 경우 구성 파일은 다음과 같아야 합니다.

```coffeescript
module.exports = (env, argv) => ({
  mode: argv.mode === 'production' ? 'production' : 'development',
  devtool: argv.mode === 'production' ? false : 'inline-source-map',

  entry: {
    ui: './src/ui.tsx',
    code: './src/code.ts',
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      },
      {
        test: /\.sass$/,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader',
        ],
      },
      {
        test: /.svg$/,
        use: '@svgr/webpack',
      },
    ]
  },
})
```

이제 웹 팩에 사용할 플러그인을 알려드리겠습니다. 우리는 html-webpack-inline-source-plugin과 html-webpack-plugin 플러그인이 필요하다.

devDependencies로 설치한 다음 webpack.config.js 파일의 맨 위에 파일을 저장해야 합니다.

```js
const HtmlWebpackInlineSourcePlugin = require('html-webpack-inline-source-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
```

첫 번째는 번들 파일을 제공하는 HTML 파일을 만듭니다. 예를 들어, JavaScript 및 CSS 파일이 번들로 제공된 템플릿 HTML 파일에 자동으로 추가됩니다. 우리의 경우, 스타일링은 이미 자바스크립트에 포함되어 있기 때문에, 우리는 자바스크립트 파일만 포함하면 된다. 하지만, 우리는 그 자바스크립트가 인라인으로 필요하고, 이것이 두 번째 플러그인이 하는 일입니다.

템플릿에 대한 src/ui.html을 가리키고 출력 이름을 ui.html로 지정했으면 한다. 또한 "js" 파일만 인라인으로 설정하기를 원하므로 "inlineSource: ((js)$"가 있습니다. 이 키에는 `HtmlWebpack`이 필요합니다.InlineSourcePlugin`입니다. 마지막으로 chunks: [ui]는 webpack이 ui.html에 code.js가 필요하지 않다는 점을 감안하여 ui.html에 ui.js 파일만 포함하도록 지시한다.

webpack.config.js 파일의 마지막 모양입니다.

```js
const HtmlWebpackInlineSourcePlugin = require('html-webpack-inline-source-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const path = require('path');

module.exports = (env, argv) => ({
  mode: argv.mode === 'production' ? 'production' : 'development',
  devtool: argv.mode === 'production' ? false : 'inline-source-map',

  entry: {
    ui: './src/ui.tsx',
    code: './src/code.ts',
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      },
      {
        test: /\.sass$/,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader',
        ],
      },
      {
        test: /.svg$/,
        use: '@svgr/webpack',
      },
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/ui.html',
      filename: 'ui.html',
      inlineSource: '.(js)$',
      chunks: ['ui'],
    }),
    new HtmlWebpackInlineSourcePlugin(),
  ],
})
```

### 추가 파일 수정

SVG를 React 구성 요소로 포함시키려면 사용자 정의 타이핑이 필요합니다. 루트 디렉토리에 `typings-custom`이라는 폴더를 만든 다음 폴더 내부에 `svg.d.ts`라는 파일을 만듭니다. 이 파일의 내용은 다음과 같아야 합니다.

```js
declare module '*.svg' {
  const content: any;
  export default content;
}
```

이제 tsconfig.json에 typings-custom 폴더의 내용을 포함시켜야 한다.

```bash
{
  ...
  "include": [
    "./typings-custom/**/*.ts"
  ]
}
```

JSX와 협력하고 있기 때문에 tsconfig.json에도 표기해야 합니다. 다음 키-값 쌍을 `compilerOptions`에 추가하십시오.

```bash
"jsx": "react"
```

tsconfig.json의 최종 버전은 다음과 같습니다.

```undefined
{
  "compilerOptions": {
    "target": "es6",
    "jsx": "react",
    "typeRoots": [
      "./node_modules/@types",
      "./node_modules/@figma"
    ]
  },
  "include": [
    "./typings-custom/**/*.ts"
  ]
}
```

### 웹 팩 실행 중

빌드와 워치 스크립트를 추가하기 전에 리액트와 피그마의 유형을 설치해야 한다. 다음 npm 패키지를 `devDependencies`로 설치합니다.

```coffeescript
@figma/plugin-typings
@types/react
@types/react-dom
```

자, 이제 `패키지`에.json` 파일에서 다음 스크립트를 추가하여 프로덕션용 빌드하거나 개발의 변경 사항을 확인할 수 있습니다.

```bash
"scripts": {
  "build": "webpack --mode production",
  "watch": "webpack --mode development --watch"
}
```

콘솔에서 `npm 실행 감시`를 실행해 보십시오. 오류가 발생할 수 있습니다. 제가 조사한 바로는 html-webpack-plugin 최신 버전 때문입니다. 이 문제를 해결하려면 `패키지`에서 이 패키지의 버전을 `3.2.0`으로 변경하십시오.json 파일을 선택한 다음 npm install을 실행하여 이 특정 버전을 가져옵니다.

```bash
"html-webpack-plugin": "3.2.0"
```

이렇게 하면 문제가 해결될 거예요.

모든 것이 잘 되고 있는지 테스트하려면 콘솔.log(`test`)와 같은 로깅 줄을 ui.tsx 파일에 추가한 다음 npm run watch를 실행하면 된다. Figma(피그마)로 이동하여 `Plugins(플러그인)` 아래에 있음개발에서 플러그인을 찾아서 실행합니다. 빈 창이 표시됩니다. 동일한 메뉴로 다시 이동하여 `콘솔 열기`를 클릭하십시오. 그러면 콘솔에서 테스트 메시지가 표시됩니다.

## UI 및 플러그인 코드 통신

기본적으로 두 개의 원본 파일인 ui.tsx와 code.ts. 각각 프런트 코드와 백엔드 코드로 생각할 수 있습니다. ui.tsx는 사용자 인터페이스를 생성하여 code.ts와 code.ts로 메시지를 전송하고 이에 따라 작업을 수행하며 API를 사용하여 Figma 문서를 제어하고 메시지를 다시 전송하여 UI에 알릴 수 있다.

이 양방향 메시징 시스템은 이 두 파일을 서로 연결하는 유일한 방법이기 때문에 플러그인에 사용자 인터페이스가 있는 경우 필수적입니다. UI에서 메시지를 보내면 다음과 같이 수행됩니다.

```php
parent.postMessage({ pluginMessage: 'MESSAGE' }, '*');
```

마찬가지로 플러그인 코드에서 UI로 메시지를 보내는 것은 매우 유사합니다.

```bash
figma.ui.postMessage('MESSAGE');
```

## UI 구성

나는 이 기사를 짧게 유지하고 실제 주제에 초점을 맞추기 위해 모든 기능을 리액트 컴포넌트에 제공할 것이다.

이 구성 요소는 사용자가 색 생성 버튼을 클릭할 때 세 가지 임의 색상을 생성합니다. 이 색상 중 하나를 클릭하면 그림마 문서에서 선택한 요소의 채우기 색상이 설정됩니다. 요소를 선택하지 않은 경우 UI에 Select an Item! 메시지가 표시되어 사용자에게 경고합니다.

```js
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import randomColor from 'randomcolor';
import './styles.sass';

interface Props {
}

interface State {
  colors: string[],
  isItemSelected: boolean,
  isColorGenerated: boolean
}

class App extends React.Component<Props, State> {
  constructor(props) {
    super(props);
    this.state = {
      colors: ['#fff', '#fff', '#fff'],
      isItemSelected: false,
      isColorGenerated: false
    }
  }

  componentDidMount() {
    window.onmessage = (msg) => {
      const { type } = msg.data.pluginMessage;
      if (type === "ITEM_SELECTED") {
        this.setState({ isItemSelected: true })
      } else if (type === "ITEM_NOT_SELECTED") {
        this.setState({ isItemSelected: false })
      }
    };
  }

  sendMessage = (type, data = null) => {
    parent.postMessage({
      pluginMessage:
      {
        type,
        data,
      },
    }, '*');
  }

  mapValues = (x) => {
    return (x - 0) * (1 - 0) / (255 - 0) + 0;
  }

  getRGBValues = (str) => {
  var vals = str.substring(str.indexOf('(') +1, str.length -1).split(', ');
    return {
      'r': this.mapValues(parseInt(vals[0])),
      'g': this.mapValues(parseInt(vals[1])),
      'b': this.mapValues(parseInt(vals[2]))
    };
  }

  generateColors = () => {
    const colors = randomColor({ count: 3, format: 'rgb', hue: 'random' });
    this.setState({ colors, isColorGenerated: true });
  }

  assignColor = (color) => {
    this.sendMessage('ASSIGN_COLOR', this.getRGBValues(color));
  }

  render() {
    const { isItemSelected, isColorGenerated, colors } = this.state;
    return (
      <div className="app">
        <div className="colors">
          {colors.map((color, i) => (
            <button
              key={`${i}-${color}`}
              type="button"
              className="color"
              onClick={
                (isItemSelected && isColorGenerated)
                ? () => this.assignColor(color) 
                : null}
              style={backgroundColor: color}
            />
          ))}
        </div>
        <button
          type="button"
          onClick={this.generateColors}
        >
          Generate color
        </button>
        {!isItemSelected && <div className="alert">Select an Item!</div>}
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

중요한 것은 UI와 플러그인 코드 사이의 통신입니다. 선택적인 데이터로 메시지를 보내는 것은 `sendMessage` 유틸리티 함수에 의해 처리된다. `assignColor` 함수가 플러그인 코드를 알리기 위해 `sendMessage`를 호출하는 방법을 참고하십시오.

```coffeescript
sendMessage = (type, data = null) => {
  parent.postMessage({
    pluginMessage:
    {
      type,
      data,
    },
  }, '*');
}
```

그리고 플러그인 코드가 메시지를 보낼 때 UI가 메시지를 잡을 수 있어야 합니다. 이것이 우리가 컴포넌트 DidMount 라이프사이클 후크에 `window.on message`를 가지고 있는 이유이다.

### UI에서 보낸 메시지

생성된 색상을 클릭하면 UI에서 플러그인 코드로 전송되는 유일한 메시지가 발생합니다. UI는 다음과 같은 모양의 개체와 함께 `ASSign_COLOR` 메시지를 보냅니다.

```css
{
  r: 0.3,
  g: 0,
  b: 1,
}
```

이것은 Figma가 색상을 설정할 때 받아들이는 유효한 객체이며, 생성된 색상을 {r,g,b} 형태의 객체로 변환하고 0-255 범위를 0-1 범위로 매핑하는 getRGB값, mapValues 등 다른 유틸리티 기능이 있는 것도 이 때문이다.

### 플러그인 코드에서 보낸 메시지

Figma 문서의 요소를 선택했는지 여부를 UI에 알리기 위해 플러그인 코드에서 보낸 두 개의 메시지가 있습니다. 그런 다음 구성 요소는 UI 업데이트에 따라 상태 변수를 설정합니다.

## 기본 API 기능

시작하기 전에 이 설명서 페이지는 API에서 사용할 수 있는 모든 항목에 대한 참조입니다. 문제를 해결해야 할 때는 항상 이 문서를 확인하십시오.

`code.ts` 파일에는 프로젝트가 생성될 때 생성된 예제 코드가 이미 있습니다. 이것은 API의 시작 및 사용 방법을 이해하는 데 좋은 참조입니다.

code.ts의 모든 내용을 삭제하고 다음 코드를 시작점으로 사용할 수 있습니다.

```js
figma.showUI(__html__);

figma.ui.onmessage = msg => {};
```

피그마쇼UI(__html__)는 `ui.html` 콘텐츠를 표시하기 위해 필요하며, `figma.ui.onmessage`는 UI에서 수신되는 메시지 수신을 시작하기 위해 필요합니다.

플러그인은 플러그인 코드 쪽에 두 가지 기본 기능을 가지고 있습니다.

- Figma 문서에서 요소가 선택되었는지 여부를 감지하고 있는 경우 UI에 알림
- 선택한 요소의 채우기 속성을 이 메시지와 함께 보낸 색으로 설정하기 위해 UI에서 오는 `ASSign_COLOR` 메시지 수신

### 선택탐지

선택을 감지하기 위해 이벤트 유형을 첫 번째 인수로, 콜백 함수를 두 번째 인수로 사용하는 `그림자.on` 함수를 통해 `선택 변경`을 들을 수 있다.

```coffeescript
figma.on('selectionchange', () => {
  detectSelection();
});
```

여기서 `detect Selection()` 함수는 다음과 같습니다.

```js
const detectSelection = () => {
  const { selection } = figma.currentPage;
  if (selection.length) {
    figma.ui.postMessage({ type: 'ITEM_SELECTED' });
  } else {
    figma.ui.postMessage({ type: 'ITEM_NOT_SELECTED' });
  }
}
```

`figma.currentPage`selection`은 선택된 노드의 배열을 반환합니다. 배열 길이를 확인하여 선택 항목이 발생했는지 여부를 결론을 내릴 수 있습니다. 길이가 `0`보다 크면 UI로 `ITEM_SELECTED` 메시지를 보낼 수 있으며, 그렇지 않으면 `ITEM_NOT_SELECTED` 메시지를 보낼 수 있습니다.

이 메커니즘은 단일 요소의 선택을 감지하지 못합니다. 마찬가지로, 선택한 요소에 설정할 `채움` 속성이 있다는 보장도 없습니다(예: `그룹 노드`에는 `채움` 속성이 없습니다).

이 예에서는 단순성을 위해 사용자가 `채움` 속성을 가진 단일 요소를 선택한다고 가정합니다. 프로덕션 준비 플러그인에서, 이것은 분명히 자동으로 처리되어야 하며, UI는 그에 따라 사용자에게 경고하는 적절한 메시지를 표시하도록 통지되어야 한다.

또한 플러그인을 초기화하기 전에 선택한 요소를 감지하려면 메인 스코프에서 detect Selection()을 호출해야 한다. 그렇지 않으면 사용자가 선택할 때까지 `선택 변경`이 실행되지 않습니다.

선택 탐지를 설정하면 `code.ts`는 다음과 같습니다.

```coffeescript
figma.showUI(__html__);
figma.ui.onmessage = msg => {};

figma.on('selectionchange', () => {
  detectSelection();
});

const detectSelection = () => {
  const { selection } = figma.currentPage;
  console.log(selection)
  if (selection.length) {
    figma.ui.postMessage({ type: 'ITEM_SELECTED' });
  } else {
    figma.ui.postMessage({ type: 'ITEM_NOT_SELECTED' });
  }
}

detectSelection();
```

### 채우기 속성 설정

앞서 언급했듯이 UI에서 색상을 클릭하면 `ASSIGN_COLOR` 메시지가 컬러 데이터 객체와 함께 UI에서 전송됩니다. 그러므로 우리가 가장 먼저 해야 할 일은 `figma.ui.on message =`*msg*`= {} 함수에서 이 메시지를 듣는 것이다.

```js
figma.ui.onmessage = msg => {
  const { type } = msg;
  if (type === 'ASSIGN_COLOR') {
    const { selection } = figma.currentPage;
    const { data } = msg;
  }
};
```

`const {selection} = figma.currentPage;`은(는) 선택한 요소의 배열을 반환합니다. 앞에서 언급한 바와 같이 유효한 요소가 하나만 선택되었다고 가정합니다. 메시지와 함께 데이터를 보냈기 때문에 msg를 파괴하고 데이터를 얻을 수도 있다.

`data`의 키 이름은 `sendMessage` 유틸리티 함수의 정의에서 유래한다.

```coffeescript
sendMessage = (type, data = null) => {
  parent.postMessage({
    pluginMessage:
    {
      type,
      data,
    },
  }, '*');
}
```

`console.log(selection)`를 실행하면 선택한 요소가 어레이 내의 개체이며 개체가 포함된 다른 어레이인 `fill` 속성이 있음을 알 수 있습니다. 이 개체는 앞에서 설명한 대로 다음과 같은 형식의 `color` 속성을 가집니다.

```css
{
  r: 0,
  g: 0,
  b: 0,
}
```

따라서 다음 작업을 수행하여 이 개체를 설정할 수 있습니다.

```undefined
selection[0].fills[0].color = data
```

그러나 fills와 같은 보다 복잡한 속성에는 figma 문서에 명확하게 설명되어 있는 이유로 read only 객체가 있다.

속성 자체는 읽기 전용이 아니라 내용이므로 Figma는 해당 속성을 복제하고 해당 클론을 변경한 후 해당 속성에 다시 할당하라고 지시합니다. 위에 링크된 문서에는 복제에 대해 제안된 두 가지 접근 방식이 있습니다. 가장 쉬운 것은 객체를 문자열로 묶은 다음 객체로 다시 구문 분석하는 것입니다.

```js
const clone = (val) => {
  return JSON.parse(JSON.stringify(val))
}
```

if의 나머지 성명은 이제 다음과 같다.

```js
figma.ui.onmessage = msg => {
  const { type } = msg;
  if (type === 'ASSIGN_COLOR') {
    const { selection } = figma.currentPage;
    const { data } = msg;
    const fills = clone((selection[0] as any).fills)

    fills[0].color = data;
    (selection[0] as any).fills = fills;
  }
};
```

복제된 속성은 객체에 불과하므로 fill[0.color]을 수행하고 figma와 동일한 형식의 색상 정보를 포함하는 객체인 data를 할당할 수 있습니다.

code.ts의 최종 버전은 다음과 같다.

```js
figma.showUI(__html__);

figma.ui.onmessage = msg => {
  const { type } = msg;
  if (type === 'ASSIGN_COLOR') {
    const { selection } = figma.currentPage;
    const { data } = msg;
    const fills = clone((selection[0] as any).fills)
    fills[0].color = data;
    (selection[0] as any).fills = fills;
  }
};

figma.on('selectionchange', () => {
  detectSelection();
});

const detectSelection = () => {
  const { selection } = figma.currentPage;
  console.log(selection)
  if (selection.length) {
    figma.ui.postMessage({ type: 'ITEM_SELECTED' });
  } else {
    figma.ui.postMessage({ type: 'ITEM_NOT_SELECTED' });
  }
}

const clone = (val) => {
  return JSON.parse(JSON.stringify(val))
}

detectSelection();
```

스타일링을 원하는 사용자를 위해 `style.sass` 파일의 내용은 다음과 같습니다.

```css
*
  outline: none
body
  font-family: Arial, Helvetica, sans-serif
.app
  display: flex
  flex-direction: column
  align-items: center
  button
    border: 1px solid black
    text-align: center
    border-radius: 4px
    font-size: 14px
    padding: 4px 8px
    margin-bottom: 10px
  .colors
    margin: 30px 0
    width: 60%
    display: flex
    justify-content: space-between
    .color
      border: none
      width: 50px
      height: 50px
      border-radius: 50%
      cursor: pointer
      border: 1px solid black
  .alert
    font-size: 12px
    color: rgba(red, 0.7)
```

## 결론

이제 이 튜토리얼을 따라 웹 팩을 사용하여 Figma 플러그인 개발에 React를 통합하는 방법을 이해해야 합니다. Figma에서 문서를 읽거나 GitHub 또는 다른 플랫폼에서 다른 플러그인의 소스 코드를 확인함으로써, API에 익숙해지고 Figma를 위한 자신만의 플러그인을 구축하기 시작할 수 있다.