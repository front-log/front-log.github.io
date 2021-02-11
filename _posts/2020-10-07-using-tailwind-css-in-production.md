---
layout: post
title: "생산 시 Tailwind CSS 사용"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/tailwind-css-production-today-nocdn.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tailwind-css-production-today-nocdn.png?fit=730%2C487&ssl=1)

웹에서의 성능은 당신의 제품을 만들거나 손상시킬 수 있기 때문에 오늘날에는 꽤 큰 문제입니다. 웹 성능이 사용자 경험(UX)의 큰 부분을 차지하기 때문에 제품 출시 후 작업해야 하는 것에서 모든 개발 프로세스에서 고려해야 할 매우 중요한 부분으로 전환되었습니다.

- 이 연구에 따르면 평균 온라인 사용자는 4초 이내에 페이지가 로드될 것으로 예상하며 40%는 3초 후에 사이트를 포기할 가능성이 더 높습니다.
- 이 블로그 게시물은 Pinterest의 웹사이트 성능 최적화를 통해 SEO 트래픽이 15% 증가하고 가입 전환율이 15% 증가했음을 보여줍니다.

## 묶음 사이즈가 어떻게 되나요?

번들 크기가 무엇을 의미하는지 이해하려면 번들이 무엇인지 이해해야 합니다.

번들은 HTML, CSS, JavaScript, 이미지 등의 다양한 자산 그룹을 결합하여 번들로 알려진 더 작은 파일을 생성하는 번들러(웹 팩, 구획 또는 스노팩)의 과정이다. 콘텐츠를 사용자에게 제공하기 위해 브라우저가 수행해야 하는 HTTP 요청 수를 줄이려면 번들링이 필요합니다.

번들 크기는 생성된 번들의 크기이며 웹 사이트와 웹 앱이 로드되는 데 걸리는 시간에 영향을 미칩니다.

이 기사에서는 Tailwind CSS를 트리 흔들기에 의해 Tailwind CSS가 구동되는 앱 번들 크기를 줄이는 방법에 대해 알아보고, 프로덕션에서 Tailwind와 함께 Purge CS를 사용하는 방법에 대해서도 알아보고, Vue 및 React와 같은 JavaScript 프레임워크에서 이를 수행하는 방법에 대해 알아보겠습니다.

## 테일윈드 CSS란?

사용자 지정 설계를 구축하는 데 사용되는 유틸리티 우선 CSS 라이브러리이며, 미리 구성된 스타일과 구성 요소를 제공하지 않으며, 구성 요소를 스타일링하는 데 도움이 되는 유틸리티/도우미 클래스라고 하는 무개념 구성 요소 집합을 제공합니다.

공식 문서에 따르면:

> Tailwind CSS는 재정의하기 위해 싸워야 하는 성가신 주장 스타일 없이 맞춤형 설계를 구축하는 데 필요한 모든 빌딩 블록을 제공하는 고도로 사용자 지정 가능한 저수준의 CSS 프레임워크입니다.

## PurgeCSS란?

PurgeCSS는 프로젝트에서 사용되지 않는 CSS를 제거하기 위한 개발 워크플로우 도구이며, Tailwind CSS 번들 크기를 제어하기 위한 기본 라이브러리이다. 사용하지 않는 스타일을 트리로 흔들고 CSS 빌드 크기를 최적화합니다.

공식 문서에 따르면:

> 삭제 CSS는 내용과 CSS 파일을 분석합니다. 그런 다음 파일에 사용된 선택기와 내용 파일의 선택기와 일치시킵니다. CSS에서 사용되지 않은 선택기를 제거하여 CSS 파일이 작아집니다.

## 전제조건

이 튜토리얼에서는 판독기에 다음이 있다고 가정합니다.

- 해당 PC에 설치된 Node.js 10x 이상
- PC에 설치된 Naron/npm 5.6 이상입니다. 이 튜토리얼은 Yarn을 사용합니다.
- JavaScript의 기본 지식 및 Vue.js 및 React의 작동 방식
- PC에 설치된 Vue CLI
- CSS 작동 방식에 대한 기본 지식
- 원자 CSS 아키텍처의 작동 방식에 대한 기본 이해

Yarn을 사용하여 Vue CLI를 다음 명령으로 설치할 수 있습니다.

```coffeescript
yarn global add @vue/cli
```

터미널에서 `vue`를 실행하여 설치가 성공했는지 확인합니다. 사용 가능한 명령 목록이 표시됩니다.

## Vue에서 생산을 위한 설치 및 건물 Tailwind

CLI 패키지를 전체적으로 설치하면 터미널의 `vue` 명령에 액세스할 수 있습니다. vue create 명령을 사용하면 다음과 같은 새 프로젝트를 만들 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/vuecreateinterminal.png?resize=730%2C428&ssl=1)

먼저 다음 명령을 사용하여 새 Vue 프로젝트를 만듭니다.

```undefined
vue create vue-tailwind
```

프로젝트 이름 "vue-tailwind"는 적합하다고 생각되는 이름으로 대체될 수 있습니다.

이 명령을 실행하면 앱에 필요한 패키지를 선택할 수 있는 대화형 비계 환경을 제공하고 향후 프로젝트에 대한 사전 설정으로 구성을 저장하도록 결정할 수도 있습니다.

그런 다음 디렉터리를 프로젝트 폴더로 변경하십시오.

```bash
cd vue-tailwind
```

그런 다음 다음 다음 명령 중 하나를 실행하여 새로 만든 앱의 브라우저에서 개발 서버를 회전시킵니다.

```coffeescript
yarn serve
or
npm run serve
```

다음 명령을 실행한 후 앱이 기본적으로 http://localhost:8080에서 실행되고 있어야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/localhostvueapp.png?resize=730%2C793&ssl=1)

다음으로, Tailwind CSS 라이브러리를 설치하겠습니다. Warn 패키지 매니저와 함께 Tailwind를 설치하려면 다음을 입력합니다.

```undefined
yarn add tailwindcss
```

그런 다음 Tailwindcs 패키지에 포함된 Tailwind CLI 유틸리티를 사용하여 프로젝트에 대한 기본 구성 파일을 생성하면 Tailwind 설치를 사용자 지정할 수 있습니다.

```undefined
npx tailwindcss init
```

이 명령은 프로젝트의 기본 디렉터리에 `tailwind.config.js` 파일을 생성하며, 파일에는 Tailwind의 기본 구성이 모두 들어 있습니다.

다음으로 src/assets 폴더에 style 폴더를 만든 다음 폴더 내부에 tailwind.css 파일을 생성하면 여기서 tailwind의 base, components, utilities 스타일 구성을 가져올 것이다.

Tailwind 지시문을 사용하여 Tailwind의 base, component 및 utility 스타일을 CSS에 삽입하고 tailwind.css 파일에 다음 내용을 넣으십시오.

```css
@tailwind base;

@tailwind components;

@tailwind utilities;
```

그런 다음 프로젝트의 루트 디렉터리에 `postcs.config.js`를 생성하고 다음을 포함합니다.

```coffeescript
module.exports = {
    plugins: [
        require('tailwindcss'),
        require('autoprefixer')
    ]
}
```

Tailwind.css 파일에서 생성된 스타일을 Tailwind CLI 유틸리티 도구로 컴파일하고 빌드한 후 다음 명령을 실행합니다.

```undefined
npx tailwindcss build src/assets/styles/tailwind.css -o src/assets/styles/index.css
```

이 명령은 다음과 유사한 결과를 산출합니다.

개발 빌드의 출력 크기가 2.26MB로 압축되지 않은 것을 볼 수 있습니다. 이제 엄청난 양입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tailwindcssinvue.png?resize=730%2C490&ssl=1)

`index.css` 파일을 앱으로 가져오려면 `App`에 다음 줄의 코드를 넣으십시오.vue` 파일은 다음과 같습니다.

```xml
<style src="./assets/styles/index.css">
```

`앱`을 엽니다.vue`를 선택한 후 역풍을 통해 제공되는 기본 스타일에서 템플릿에 임의 스타일을 추가합니다.

우리의 스타일을 구축하기 위한 빌드 단계는 npm 스크립트로 추상화될 수 있습니다. 당신의 패키지를 여십시오.json은 파일을 작성하고 "style" 스크립트가 "style" 객체에 포함되어 있습니다.

다음 줄의 코드를 `패키지`에 넣습니다.json 파일:

```cpp
"scripts": {
  "style": "tailwindcss build src/assets/styles/index.css -o src/assets/styles/tailwind.css",
// other scripts
}
```

다음으로, 프로덕션용 앱을 구축하겠습니다. 다음 명령을 사용합니다.

```undefined
yarn build
```

이 명령은 `dist/` 디렉토리에서 프로덕션 준비 초최적화된 번들을 생성하는 `vue-cli-service build` 프로세스를 시작합니다.

CLI에서는 다음 정보를 얻을 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/buildcomplete.png?resize=730%2C511&ssl=1)

우리는 CSS의 빌드가 기본 스타일만 포함하고 있는 앱으로서는 상당히 크다는 것을 알 수 있다, 우리가 기존의 스타일을 확장하기 위해 새로운 스타일을 추가하는 시나리오를 상상해보라.

## CSS 제거를 사용하여 파일 크기 축소

다음으로, CSS 제거에 대한 구성을 포함하겠습니다. `tailwind.config.js` 파일을 열고 `module.exports` 사이의 `purge` 배열을 다음과 같이 바꿉니다.

```css
purge: {
  enabled: true,
  content: [
     './src/**/*.vue',
    './public/**/*.html',
  ]
},
```

우리는 PurgeCS가 확인할 수 있는 일련의 경로를 제공했고, 지정된 경로에 있는 파일을 통과한 다음 어레이에 제공된 파일에 정의되지 않은 모든 클래스를 `정리`했습니다.

Tailwind utility 명령을 실행하여 스타일을 다시 만들고 생산을 위해 번들을 하면 CSS 청크 크기가 크게 감소한다.

다음 명령을 실행합니다.

```undefined
yarn style && yarn build
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/compiledsuccesssfully.png?resize=730%2C490&ssl=1)

프로덕션 빌드를 실행할 때 CSS 파일의 크기가 크게 줄어들고 이 크기를 줄이면 앱 로드 속도가 향상되는 것을 볼 수 있습니다.

## React에서 생산을 위한 설치 및 건물 Tailwind

먼저 create-react-app으로 새 프로젝트를 생성하고 터미널에 다음 명령을 입력합니다.

```undefined
npx create-react-app react-tailwind
```

create-react-app은 부트스트래핑 및 비계 리액트 프로젝트를 위한 공식 리액트 빌드 도구입니다. 웹 팩 위에 구축된 이 도구는 프로젝트의 빌드 프로세스를 구성하고 설정하는 번거로움을 해결하고 앱의 전원을 공급하는 코드를 작성하는 데 집중할 수 있도록 해 줍니다.

그런 다음 작업 디렉토리를 프로젝트 폴더로 변경합니다.

```bash
cd react-tailwind
```

그런 다음 프로젝트에 Tailwind CSS를 설치합니다.

```undefined
yarn add tailwindcss
```

그런 다음 `tailwindcs` 패키지에 포함된 Tailwind CLI 유틸리티 도구로 프로젝트에서 Tailwind CSS를 초기화합니다. 이 도움말 도구는 프로젝트의 기본 디렉터리에 `tailwind.config.js` 파일을 생성합니다. 파일에는 Tailwind의 기본 구성이 모두 포함되어 있습니다. Tailwind 설치를 사용자 지정하려면:

```undefined
npx tailwindcss init
```

다음으로/src 폴더에 tailwind.css 파일을 만들어 여기서 Tailwind의 base, component, utility 스타일 구성을 가져올 것이다.

Tailwind 지시문을 사용하여 Tailwind의 base, component 및 utility 스타일을 CSS에 삽입하고 tailwind.css 파일에 다음 내용을 넣으십시오.

```css
@tailwind base;

@tailwind components;

@tailwind utilities;
```

그런 다음 프로젝트의 루트 디렉터리에 `postcs.config.js`를 생성하고 다음을 포함합니다.

```coffeescript
module.exports = {
    plugins: [
        require('tailwindcss'),
        require('autoprefixer')
    ]
}
```

그런 다음, `App.js`를 열고 Tailwind에서 제공하는 기본 스타일에서 일부 임의 스타일을 템플릿에 추가합니다.

그런 다음 `index.css` 파일의 내용을 삭제하고 Tailwind.css 파일에서 생성된 스타일을 Tailwind CLI 유틸리티 도구를 사용하여 `index.css` 파일로 컴파일 및 빌드하고 다음 명령을 실행합니다.

```undefined
npx tailwindcss build src/tailwind.css -o src/index.css
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/tailwindcsstherearebreakingchangesmethod.png?resize=730%2C505&ssl=1)

이 명령은 2.44MB의 개발 빌드를 산출해야 하는데, 이는 매우 큰 규모입니다.

다음 명령을 실행합니다.

```undefined
yarn build
```

또한 우리의 프로젝트를 생산하기 위해 번들은 186.66KB의 CSS 청크 크기를 산출하는데, 이것은 이 크기의 앱으로서는 매우 큰 크기이다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/runyarnbuild.png?resize=730%2C511&ssl=1)

## CSS 제거를 사용하여 파일 크기 축소

다음으로, PurgeCSS에 대한 구성을 포함하여 `tailwind.config.js` 파일을 열고 `module.exports` 사이에 다음을 포함하도록 하자.

```css
purge: {
    content: [
      'src/**/*.js',
      'src/**/*.jsx',
      'public/**/*.html',
    ]
},
```

우리는 PurgeCS가 확인할 수 있는 일련의 경로를 제공했고, 지정된 경로에 있는 파일을 통과한 다음 어레이에 제공된 파일에 정의되지 않은 모든 클래스를 `정리`했습니다.

Tailwind utility 명령을 실행하여 스타일을 다시 만들고 생산을 위해 번들을 하면 CSS 청크 크기가 크게 감소한다.

다음 명령을 실행합니다.

```undefined
npx tailwindcss build src/tailwind.css -o src/index.css && yarn build
```

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/npxtailwindcssbuild.png?resize=730%2C505&ssl=1)

개발 빌드는 약 14KB이고 프로덕션 빌드 크기는 2KB 미만이어야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/filesizesaftergzio.png?resize=730%2C505&ssl=1)

주의: 프로덕션 환경에서만 PurgeCSS를 사용해야 합니다. 개발 중에 삭제 스타일을 사용하면 특히 이전 빌드에서 사용하지 않은 유틸리티 클래스가 필요한 경우 Tailwind를 매우 자주 다시 컴파일해야 합니다. 제거 CSS 구성에 `사용 가능` 옵션을 포함시키고 해당 환경을 기준으로 빌드하도록 설정하여 `제거` 개체에 다음을 포함하도록 프로덕션만 제거를 사용하도록 설정할 수 있습니다.

```bash
enabled: process.env.NODE_ENV === 'production'
```

PurgeCSS를 사용하면 대부분의 크기 제어 요구를 해결할 수 있지만, Tailwind CSS 파일 크기를 줄이는 또 다른 방법은 다음과 같은 `tailwind.config,.js` 파일에서 사용되지 않는 스타일 구성 값을 제거하는 것입니다.

- 프로젝트에 사용된 색상만 포함하도록 기본 색상 팔레트에서 사용되지 않은 색상 제거
- 사용되지 않는 미디어 쿼리 중단점 제거
- 사용되지 않는 유틸리티 및 변형 사용 안 함

이러한 다른 전략은 CSS 빌드를 크게 개선하지만 사용되지 않은 스타일을 트리 흔들기 위해 퍼지 CS를 사용하는 것만큼 효과적이지 않다.

## 결론

이 기사에서는 Vue와 React에서 Tailwind CSS를 어떻게 사용하는지, 또한 우리가 어떤 환경을 위해 만들고 있는지에 따라 CSS 스타일을 조건부로 흔드는 방법도 보았습니다. 자세한 내용은 설명서를 참조하십시오.