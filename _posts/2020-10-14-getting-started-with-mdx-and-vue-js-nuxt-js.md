---
layout: post
title: "MDX 및 Vue.js/Nux.js 시작"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/gettingstartedwithmdxandvuenuxt.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/gettingstartedwithmdxandvuenuxt.png?fit=730%2C412&ssl=1)

마크다운은 웹에 소개된 이후, 콘텐츠 제작자(예: 기술 작가, 문서 엔지니어 등)가 저작을 수행할 수 있는 보다 단순한 도구를 가질 수 있게 했다. 이 기사에서는 MDX에 대해 살펴보고 Nux.js 응용 프로그램에서 MDX를 시작하는 방법에 대해 알아보겠습니다.

## 전제조건

- 계속 진행하려면 Markdown 및 Vue.js/Nux.js에 익숙해져야 합니다.

## MDX란 무엇입니까?

MDX는 마크다운 문서에 JSX를 쓸 수 있는 구문 확장입니다. 또한 공식 MDX 설명서에 따르면 MDX는 다음과 같습니다.

> 마크다운 문서에 JSX를 원활하게 쓸 수 있는 권한 있는 형식입니다. 대화형 차트 또는 알림과 같은 구성 요소를 가져와 컨텐츠에 포함할 수 있습니다.

기본적으로 MDX는 JSX와 함께 Markdown입니다. MDX 이전에는 마크다운에서 JSX를 쓰는 것이 번거로웠는데, 그 안에 JSX를 뿌리는 과정에서 저작 마크다운의 단순함이 사라졌기 때문이다.

MDX는 마크다운 문서에 많은 재미있는 응용 프로그램의 문을 연다. 예를 들어 Chakra UI 설명서 웹 사이트는 MDX를 사용하여 독서자가 구성요소를 편집하고 변경 내용을 페이지에서 실시간으로 볼 수 있도록 합니다.

MDX는 MDX의 Chakra UI 설명서 사용과 같이 설계 시스템 문서에 유용하며, MDX에서는 Markdown(md) 또는 `.mdx` 파일을 구성 요소로 가져올 수도 있습니다!

## MDX의 기능

- MDX는 Markdown과 JSX를 완벽하게 혼합하여 JX 기반 프로젝트에서 보다 표현력이 뛰어난 저작권을 제공합니다.
- 드롭다운 문서에서 JSX 구성 요소를 쉽게 가져오고 렌더링합니다.
- 각 마크다운 요소에 대해 렌더링되는 구성 요소를 확인할 수 있는 높은 사용자 지정 가능
- 마크다운의 단순성은 유지되며, 원하는 곳에서만 JSX를 사용하면 됩니다.
- 모든 컴파일은 빌드 단계에서 발생하므로 MDX에 런타임이 없습니다. 이것은 MDX를 매우 빠르게 만든다.

## MDX 및 Vue.js

JSX는 원래 React 프로젝트에서 사용되었지만 Vue.js에서도 사용할 수 있습니다. 이렇게 하면 Vue.js 프로젝트에 MDX를 자유롭게 통합할 수 있습니다.

## Vue.js의 MDX 기능

- MDX를 사용하면 `.md` 또는 `.mdx` 파일을 Vue 구성 요소로 가져올 수 있습니다.
- MDX를 사용하면 마크다운 문서 내부에서도 Vue 구성 요소를 가져올 수 있습니다.
- MDX 제공자를 사용하여 드롭다운 요소를 Vue 구성 요소로 바꿀 수 있습니다.

MDX와 Vue를 통합하여 이러한 기능을 실제로 살펴보겠습니다. 우리는 새로운 Vue 프로젝트를 만드는 것으로 시작할 것입니다. Vue CLI를 사용하여 실행:

```undefined
vue create mdx-vue-demo
```

Vue 프로젝트를 만든 후 Vue에서 MDX를 사용할 수 있도록 하는 공식 로더인 `@mdx-js/vue-loader`를 설치하여 MDX 지원을 추가할 예정입니다. 아래에 설치하겠습니다.

```css
npm install @mdx-js/vue-loader
```

로더를 설치한 후 프로젝트의 웹 팩 구성에 로더를 포함합니다. Vue CLI를 사용하므로 프로젝트의 루트에 `vue.config.js` 파일을 생성할 수 있으며, `configureWebpack` 속성에는 다음과 같은 모듈 규칙에 `@mdx-js/vue-loader`가 포함되어 있습니다.

```java
// vue.config.js
module.exports = {
  configureWebpack: {
    module: {
      rules: [
        {
          test: /\.mdx?$/,
          use: ['babel-loader', '@mdx-js/vue-loader'],
        },
      ],
    },
  },
};
```

이 코드가 의미하는 것은.md 또는 .mdx로 끝나는 모든 파일은 babel-loader와 @mdx-js/vue-loader에서 모두 처리된다는 것이다.

웹 팩을 사용하여 Vue 애플리케이션을 구성하는 경우에도 위의 단계는 동일합니다. 다음과 같이 `webpack.config.js`에 로더를 포함하면 됩니다.

```js
// webpack.config.js
module: {
  rules: [
    // ...
    {
      test: /\.mdx?$/,
      use: ['babel-loader', '@mdx-js/vue-loader']
    }
  ]
}
```

## Vue 구성 요소에서 Markdown 가져오기

이제 `@mdx-js/vue-loader`를 설치 및 구성했으므로 `src/` 디렉토리에 `md` 파일을 생성하여 `src/App.vue`에서 Vue 구성 요소로 렌더링해 보겠습니다. 구성 요소/` 디렉토리에 `MyFirstMdx.mdx`를 생성하고 다음 내용을 추가합니다.

```coffeescript
# Hello LogRocket

from markdown powered by MDX
```

Vue 구성 요소를 가져올 때 `src/App.vue`에서 `.mdx` 파일을 가져옵니다. 다음과 같은 경우:

```coffeescript
import MyFirstMDX from '@/components/MyFirstMdx.mdx'
```

구성 요소를 등록합니다.

```undefined
components: {
  MyFirstMdx
}
```

그런 다음 `App`에서 템플릿을 교체합니다.<my first-mdx/>가 붙은 vue 앱.vue` 파일은 이제 다음과 같습니다.

```xml
<template>
  <div id="app">
    <my-first-mdx />
  </div>
</template>
<script>
import MyFirstMdx from "./components/MyFirstMdx.mdx";
export default {
  name: "App",
  components: {
    MyFirstMdx,
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

Vue 개발 서버를 시작하면 브라우저에서 앱을 방문하면 `MyFirstMdx.mdx` 콘텐츠가 표시됩니다.

## Markdown에서 Vue 구성 요소 가져오기

MDX가 Vue 구성요소를 마크다운 문서에 쉽게 가져올 수 있도록 사용자가 MDX에 박수를 보낼 수 있는 구성요소/에 구성요소를 만들 것입니다. 우리는 이 구성 요소를 ClapForMdx.vue라고 부를 것이다. 다음 조각을 여기에 추가합니다.

```xml
// components/ClapForMdx.vue
<template>
  <section>
    <p class="claps">{ claps }</p>
    <button class="clap-btn" @click="clap" title="Clap for MDX">👏</button>
  </section>
</template>
<script>
export default {
  data() {
    return {
      claps: 0,
    };
  },
  methods: {
    clap() {
      this.claps += 5;
    },
  },
};
</script>
<style>
.claps {
  font-size: 3rem;
  color: #f0ab1f;
}
.clap-btn {
  padding: 1rem;
  border-radius: 100%;
  font-size: 2.5rem;
  cursor: pointer;
  border: 1px solid #f0ab1f;
  box-shadow: -4px -1px 18px -10px rgba(0, 0, 0, 0.75);
}
.clap-btn:focus {
  outline: none;
}
</style>
```

그런 다음 `MyFirstMdx.mdx`를 이렇게 수정합니다.

```coffeescript
import ClapForMdx from './ClapForMdx.vue';
# Hello LogRocket
from markdown powered by MDX
<ClapForMdx />
```

브라우저에서 다음을 수행할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/clapformdx.png?resize=811%2C467&ssl=1)

버튼과 상호 작용할 수 있으며, 버튼은 예상대로 응답합니다!

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/uistate200claps.png?resize=735%2C465&ssl=1)

## MDX 및 Nux.js

우리는 이제 Vue 프로젝트에서 MDX가 어떻게 작동하는지 보았습니다. 이 마지막 섹션에서는 Nux.js 응용 프로그램에서 설치를 소개한다. Nuxt는 Vue를 위한 프레임워크이기 때문에, 우리의 코드 예제가 그 안에서 작동할 것이기 때문에 우리는 그것을 반복할 필요가 없을 것이다.

시작하려면 Nux.js 프로젝트에서 MDX용 공식 NuxTJS 모듈을 개발 종속성으로 추가해야 합니다. 터미널에서 다음을 실행합니다.

```coffeescript
npm install --save-dev @nuxtjs/mdx
```

그런 다음 `nux.config.js`에서 다음과 같이 모듈을 `buildModule` 속성에 추가합니다.

```undefined
buildModules: ['@nuxtjs/mdx']
```

nxt > 2.9.0을 사용하는 경우 대신 `모듈` 속성을 사용하십시오.

바로 그거야! 위의 예제를 다시 사용할 수 있으며, 효과가 있을 것입니다.

## 결론

MDX를 사용하면 구성요소 중심 시대에 마크다운과 JSX를 원활하게 혼합할 수 있습니다. 이 기사에서는 Vue.js/Nux.js 프로젝트에서 MDX를 설정하고 사용하는 방법을 살펴봤습니다. MDX 또는 Vue.js 또는 Nux.js와의 통합을 계속 시도하려면 다음 리소스를 확인하십시오.

- https://mdxjs.com/
- https://mdx.nuxtjs.org/
- https://codesandbox.io/s/mdx-provider-nuxt-1bsun

데모 VueJS 프로젝트를 위한 GitHubrepo 링크입니다.