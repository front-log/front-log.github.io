---
layout: post
title: "개츠비 블로그에 대한 마크다운 플러그인을 구축하는 방법"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/Untitled-design-4.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/Untitled-design-4.png?fit=730%2C487&ssl=1)

마크다운의 등장 이후, 기사 작성(그리고 일반적으로 텍스트 작성)은 새로운 국면을 맞이했다. 이전에는 HTML을 직접 사용해야 하거나 온라인 텍스트 편집기에서 제공한 텍스트 편집 옵션으로 제한되었습니다. 그러나 이제 Markdown 콘텐츠를 지원하는 모든 서비스는 보다 쉽게 쓸 수 있게 되었습니다.

개츠비와 몇몇 다른 틀은 마크다운을 지지한다. 이러한 Markdown 파일은 웹 페이지 또는 블로그를 만드는 데 사용할 수 있습니다. 게다가, 개츠비는 개발자들이 마크다운 파일에 연결할 수 있고 출력된 HTML을 수정할 수 있는 플러그인이라 불리는 도구를 만들 수 있게 한다.

이 기사에서는 개츠비 블로그를 위한 마크다운 플러그인을 구축하는 방법에 대해 배울 것입니다. 예를 들어 텍스트 강조 표시 플러그인을 빌드하여 텍스트 주위에 정의한 특정 구문을 찾으면 해당 텍스트를 강조 표시된 대로 표시하는 스타일링된 HTML로 처리합니다. 다음은 제 웹 사이트의 플러그인 라이브입니다. 특히 "코드 및 기능 공유" 텍스트입니다.

이 문서에 사용된 예에만 국한되지 않습니다. 이 자료에서는 Markdown 플러그인 구축 방법만 설명합니다. 이 문서의 지식을 사용하여 다른 멋진 플러그인을 빌드하고 다른 개발자를 위해 오픈 소스할 수 있습니다.

## 블로그에 대한 개츠비와 마크다운

마크다운은 문서를 쉽게 작성할 수 있는 특수 구문을 제공합니다. 마크다운 콘텐츠의 결과는 보통 HTML이지만, 사용하는 도구에 따라 다를 수 있습니다.

예를 들어 다음과 같은 HTML이 있습니다.

```xml
<h2>I am a header</h2>
<p>I am a paragraph</p>
<code>
   <pre>
     console.log('I am JavaScript')
  </pre>
</code>
<img src='https://google.com' alt='This is not an image'>
```

…다음 Markdown 콘텐츠로 달성할 수 있습니다.

```undefined
# I am a header

I am a paragraph

```
console.log('I am JavaScript')
```

[!This is not an image](https://google.com)
```

Markdown 콘텐츠를 처리한 후 최종 결과가 HTML이기 때문에 Markdown을 사용하는 것은 정규 콘텐츠를 작성하기 위한 원활한 프로세스가 됩니다.

개츠비는 블로그를 포함한 다른 웹 애플리케이션을 만드는 데 사용되는 정적 사이트 생성자이다. 이 프레임워크는 Markdown을 지원하므로 개발자가 본격적인 페이지로 변환되는 Markdown 파일에 블로그를 쉽게 작성할 수 있습니다. 이 기사는 개츠비가 이 페이지를 어떻게 만드는지에 초점을 맞추지 않았으니, 더 많은 정보를 얻기 위해 그들의 문서를 확인해 보세요.

## 마크다운에 대한 추상 구문 트리

일반적으로 모든 프로그래밍 언어에는 구문이 있습니다. 언어의 구문은 언어의 작동 방식과 언어가 지원하는 키워드를 보여줍니다. 이 구문은 트리의 소스 코드에서 캡처된 모든 노드를 보여주는 추상 구문 트리(AST)로 나타낼 수 있습니다.

마크다운 파일에는 고유한 추상 구문 트리가 있습니다. 이 라이브 AST 탐색기에서 실험할 수 있습니다. 트리에는 Markdown 파일의 모든 키워드가 무엇을 의미하는지, 각 HTML 요소에 매핑되는 방법이 나와 있습니다.

다음 Mark down 텍스트를 검토하겠습니다.

```coffeescript
# I am a header
I am a paragraph
```
console.log('I am JavaScript')
```
```

다음은 라이브 뷰어에서 위의 Markdown 파일의 구문 트리입니다.

```undefined
{
  "type": "root",
  "children": [
    {
      "type": "heading",
      "depth": 1,
      "children": [
        {
          "type": "text",
          "value": "I am a header",
        }
      ],
    },
    {
      "type": "paragraph",
      "children": [
        {
          "type": "text",
          "value": "I am a paragraph",
        }
      ],
    },
    {
      "type": "code",
      "lang": null,
      "meta": null,
      "value": "console.log('I am JavaScript')",
    }
  ],
}
```

위에 나열된 첫 번째 Markdown 파일은 HTML의 중요한 부분을 표시하도록 요약되지만 전체 정보는 라이브 뷰어에서 찾을 수 있습니다.

이 Markdown 내용에서 트리는 모든 부분을 노드로 구분했으며, 각 노드는 유형, 값 등이 서로 다릅니다.

## 마크다운 수정의 배후 도구인 개츠비-트랜스포머-말

개츠비-트랜스포머-리마크는 개츠비 팀이 만든 플러그인이다. 이 플러그인의 목적은 마크다운 콘텐츠를 최종 HTML로 구문 분석하는 것입니다. 플러그인은 추상 구문 트리를 사용하여 이 작업을 수행합니다.

Gatsby-transformer-remark는 Markdown으로부터 AST를 수신하여 다른 플러그인이 내용을 수정할 수 있도록 한다. 본질적으로 최종 HTML을 만드는 것은 플러그인과 개츠바이트랜스포머-리마크의 공동 노력이다.

## 텍스트 강조 플러그인 구축

다른 모든 Markdown 플러그인처럼, 텍스트 하이라이트 플러그인은 Markdown 파일에 연결되고 최종 HTML에 포함될 내용 중 일부 또는 전부를 HTML로 수정할 것이다.

이 플러그인의 경우 Markdown 파일에 연결하고, 정의할 구문과 일치하는 텍스트나 단락을 캡처한 다음 강조 표시되도록 일부 스타일을 포함하는 요소로 바꾸려고 합니다.

수동으로 요소를 Markdown 컨텐츠에 직접 추가하는 방법이 있습니다.

```undefined
# I am a header
I want <span class='highlight'>this text</span> highlighted.
```

그러나 문서에서 강조 표시할 모든 텍스트 또는 여러 문서에 대해 요소를 수동으로 Markdown 파일에 추가하는 것은 지루할 수 있습니다. 그러면, 왜 더 쉽게 만들지 않는가?

플러그인에서 다음 구문을 사용합니다.

"I want - # this text #-

-#과 #-는 시작 및 닫기 기호입니다. 여기서 플러그인은 이 구문과 일치하는 모든 문자열을 선택하고 다음과 같이 포맷합니다.

```undefined
I want <span class="highlight">this text</span>
```

또는

```xml
I want <span style="...styles">this text</span>
```

클래스 이름 방법을 사용할 경우 글로벌 스타일시트에서 클래스 이름을 사용할 수 있습니다. 스타일 방법을 사용할 경우 인라인 스타일을 적용합니다.

## 환경 설정

이상적으로는 이 플러그인이 독립 실행형 프로젝트입니다. 하지만, 우리는 npm을 지속적으로 배포하고, 프로젝트에 설치된 플러그인을 업데이트하고, 만족할 때까지 테스트하고 싶지 않을 것입니다.

고맙게도 개츠비는 로컬 플러그인의 사용을 허락한다. 이것은 플러그인이 개츠비 프로젝트와 함께 살 것이라는 것을 의미하며 우리는 그것을 직접 테스트할 수 있다.

이 플러그인을 테스트하기 위해 이미 존재하는 개츠비 블로그가 있으면 됩니다. 그렇지 않은 경우 이 보고서(개츠별 블로그)를 신속하게 복제하고 필요한 종속성을 설치합니다.

다음 단계는 프로젝트의 루트에 플러그인 폴더를 생성하는 것입니다. Gatsby는 파일을 작성할 때 먼저 이 폴더를 확인하여 지정된 플러그인이 존재하는지 확인한 후 `node_modules`를 확인합니다.

플러그인 폴더에 플러그인 이름을 딴 새 폴더를 생성합니다. 저는 이것을 `개츠바이마크-텍스트-하이라이터`라고 부릅니다.

터미널에서 현재 디렉토리를 이 폴더로 변경하고 `npm init`을 실행합니다. 질문에 대답하면 `패키지`가 나옵니다.json이 당신을 위해 만들었습니다.

이 플러그인이 작동하려면 unist-util-visit 및 mdast-util-to-string의 두 가지 종속성이 필요하다. 전자는 추상 구문 트리에서 본 것처럼 마크다운 파일에서 모든 노드를 방문(후크인)하는 데 사용되고 후자는 노드의 텍스트 콘텐츠를 얻는 데 사용된다.

실행:

```undefined
npm install unist-util-visit mdast-util-to-string --save
```

개츠비에서는 `gatsby-config.js`에 사용되는 모든 플러그인을 추가해야 한다. 다음 내용:

```coffeescript
module.exports = {
  plugins: [
    {
      resolve: `gatsby-transformer-remark`,
      options: {
        plugins: [
          {
            resolve: `gatsby-remark-text-highlighter`,
            options: {}
          },
        ]
      }
    }
  ]
}
```

이 플러그인은 앞에서 설명한 바와 같이 이 플러그인이 전원을 공급받으므로 루트가 아닌 `gats by-transformer-remark`의 플러그인으로 추가된다.

## 플러그인 개발

`index.js` 파일을 생성하고 다음을 추가하십시오.

```coffeescript
const visit = require("unist-util-visit")
const toString = require("mdast-util-to-string")

module.exports = ({ markdownAST }, options) => {
  const {useDefaultStyles = true, className = ""} = options;

  visit(markdownAST, "paragraph", (node) => {
    // do something with paragraph
  });
}
```

"gats by-transformer-remark"는 이 플러그인에서 노출되는 함수를 사용하여 Markdown 콘텐츠를 수정한다. 옵션(우리는 `markdownAST`에만 관심이 있다)과 `options`(`gatsby-config.js`에 지정된)로 가득 찬 객체를 인수로 전달한다.

옵션 인수에서 우리는 두 가지 속성, 즉 이 플러그인에 의해 생성된 스타일을 사용할지 여부를 지정하는 useDefaultStyles와 요소에 추가될 클래스를 지정하는 className의 구성을 해제한다.

방문(unist-util-visit에서 내보낸 기능)으로 마크다운의 모든 단락을 방문할 수 있다.마크다운 파일의 AST(Markdown 추상 구문 트리)를 선택하고 콜백 기능을 적용합니다. 콜백 함수에 노드 인수가 지정됩니다.

다음 단계는 구문을 정의하는 것입니다. 구문에 정규식을 사용하여 일치하는 문자열을 선택할 수 있습니다. 다음은 정규식입니다.

```undefined
const syntax = /-#.*#-/
```

위의 정규식은 다음과 같이 표시되는 모든 텍스트와 일치합니다.

```bash
-# The cat caught the mouse #-
I want to be -# highlighted #-. I -# mean #- it.
```

모든 것을 종합하면 다음과 같은 이점이 있습니다.

```js
const visit = require("unist-util-visit")
const toString = require("mdast-util-to-string")

module.exports = ({ markdownAST }, options) => {
  visit(markdownAST, "paragraph", (node) => {
    let para = toString(node)
    const syntax = /-#((?!#-).)*#-/ig
    const matches = para.match(syntax);

    if (matches !== null) {
      console.log(para);
    }
  });
}
```

이 정규식은 -#이 없는 문자열과 -#이 있는 문자열 사이에 일치합니다. (?#-)는 강조 표시된 단어의 여러 예를 선택하는 데 도움이 됩니다.

방문`이 모든 단락을 방문하기 때문에, 우리는 필요한 단락만 수정하기 위해 `bit!== null`이라는 절을 추가해야 한다.

이를 테스트하려면 개츠비 블로그를 열고 새 마크다운 파일(또는 기존 파일)을 빠르게 만든 후 다음을 추가하십시오.

```coffeescript
I want to be -# highlighted #-

I -# have #- different -# highlights #-

I do not want to be highlighted
```

이제 단말기에서 "gatsby"를 실행하면 "I want to be -# highlighted #-"와 "I-# have have #-different -# have have have ## highlights #-"가 단말기에 기록됩니다. 다음은 스크린샷입니다.

이제 우리는 올바른 텍스트를 잡는다는 것을 확인했고, 다음에 할 일은 포맷하는 것입니다. 나머지 코드는 다음과 같습니다.

```js
const visit = require("unist-util-visit")
const toString = require("mdast-util-to-string")
module.exports = ({ markdownAST }, options) => {
  const {useDefaultStyles = true, className = ""} = options;
  visit(markdownAST, "paragraph", node => {
    let para = toString(node)

    const syntax = /-#((?!#-).)*#-/ig
    const matches = para.match(syntax)

    if (matches !== null) {
      let style = null
      if (useDefaultStyles) {
        style = `
          display:inline-block;
          padding:5px;
          background-color:yellow;
          color:black;
          border-radius: 5px;
        `
      }

      // remove #- and -#
      const removeSymbols = text => text.replace(/-#/g, "").replace(/#-/g, "")

      const putTextInSpan = text =>
        `<span
          ${useDefaultStyles && style ? ` style='${style}'` : ""}
          ${className !== "" ? `class='${className}'` : ""}
        >${removeSymbols(text)}</span>`

      matches.map(match => {
        para = para.replace(match, putTextInSpan(match))
      })
      para = '<p>' + para + '</p>'
      node.type = "html"
      node.children = undefined
      node.value = para
    }
  })
  return markdownAST
}
```

마지막 캣츠비 개발 이후 플러그인에 추가된 새로운 변경 사항을 사용하려면 개츠비가 플러그인을 캐시하기 때문에 먼저 클린(gats by clean)을 실행해야 한다.

위의 코드에서 볼 수 있듯이:

- `사용 기본 스타일`이 `true`인 경우 인라인 스타일이 지정됩니다.
- 구문과 일치하는 텍스트는 주변 기호가 없는 `span` 요소에 배치됩니다.
- 일치 배열의 텍스트가 매핑되고 구문과 일치하는 모든 텍스트가 기호가 없는 범위 요소에 배치됩니다.
- `className`에 값이 지정된 경우 `span` 요소는 해당 값을 클래스로 수신합니다.
- 노드의 `type`이 html로 변경되고, `children`이 정의되지 않으며, `value`가 포맷된 단락입니다.

이제 `개츠 바이 디벨롭`을 다시 실행해 보세요. 다음은 기본 스타일을 사용하는 웹 페이지의 결과입니다.

## 추가 단계

맞춤 스타일도 적용할 수 있습니다. 옵션 속성으로 플러그인을 확장하면 더 재사용이 가능해집니다. gats by-config에 다음을 추가합니다.

```css
{
  resolve: `gatsby-remark-text-highlighter`,
  options: {
    useDefaultStyles: false,
    className: 'text-highlight'
  }
```

글로벌 스타일시트 또는 블로그에 첨부된 스타일시트에서 다음과 유사한 내용을 추가할 수 있습니다.

```css
.text-highlight {
  padding: 10px;
  border-radius: 10px;
  background-color: purple;
  color: white;
}
```

## 플러그인 배포

이 플러그인은 이미 배포했으며 라이브러리에 고유한 이름이 있어야 하므로 npm에 배포할 수 없습니다. 다른 npm 라이브러리와 마찬가지로 이름을 다르게 지정하거나 아직 존재하지 않는 다른 멋진 플러그인을 구축할 수 있습니다.

```coffeescript
npm login
npm publish
```

이제 모든 프로젝트에서 플러그인을 사용할 수 있습니다. 당신의 플러그인은 생산에 있어서 `node_modules`의 개츠비에 의해 사용될 것이기 때문에 아무도 그들의 프로젝트에 플러그인 폴더를 만들 필요가 없다.

소스 코드에서 완전한 코드를 찾을 수 있으며, 기꺼이 기여하십시오!

## 결론

이 글에서, 우리는 마크다운이 무엇이고 개츠비가 어떻게 마크다운 파일에 연결되고 포맷할 수 있게 함으로써 마크다운 파일의 힘을 확장시키는지를 배웠다. 우리는 또한 마크다운 플러그인을 만드는 데 이상적인 방법을 보여주는 텍스트 강조 플러그인을 만들었다.

텍스트 강조 플러그인은 단순해 보일 수 있지만 사용자 자신의 플러그인을 빌드할 수 있는 충분한 통찰력을 제공해야 합니다.

나는 개츠비어 리마크 액체 태그를 만들 때 여기에 나열된 방법들도 사용했다. 부담 없이 확인하시고 원하시면 기부해주세요.