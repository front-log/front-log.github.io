---
layout: post
title: "Retact used.js 및 retact-draft-wysiwyg의 리치 텍스트 편집기 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/reactdraftjs.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/reactdraftjs.png?fit=730%2C487&ssl=1)

## 도입

리치 텍스트 편집기는 특히 콘텐츠 생성에 있어 웹 응용 프로그램과 상호 작용하는 방식에 필수적인 요소가 되었다. 리치 텍스트 편집기를 사용하면 텍스트의 모양을 제어할 수 있으며, 내용 작성자가 HTML을 만들고 어디에나 게시할 수 있는 강력한 방법을 제공할 수 있습니다.

이 기사에서는 Draft.js와 retact-draft-wysiwyg를 사용하여 리치 텍스트 편집기를 구축하고 편집기를 사용하여 만든 텍스트를 표시할 것이다. Draft.js는 개발자가 기본 텍스트 스타일에서 임베디드 미디어와 같은 더 강력한 기능에 이르기까지 다양한 범위의 텍스트 구성 가능성과 풍부한 텍스트 구성 가능성을 만들 수 있도록 지원하는 풍부한 텍스트 프레임워크이다. React-draft-wysiswg는 React 및 Draft.js 라이브러리를 사용하여 빌드된 wysiwyg 편집기입니다. 이 기능을 사용하면 많은 기능을 갖춘 편집기를 즉시 설치할 수 있으므로 직접 만들 필요가 없습니다.

## 전제조건

- 이 튜토리얼에서는 React에 대한 실무 지식이 있다고 가정합니다.
- 기기에 Node, Rany 또는 npm이 설치되어 있는지 확인합니다. 아직 설치하지 않은 경우 설치 방법에 대한 지침을 보려면 링크를 따라갈 수 있습니다.
- 우리는 프로젝트를 부트스트랩하기 위해 생성-반응 앱을 사용할 것이다.

## 시작 중

당신이 원하는 디렉토리에 우리의 프로젝트를 만들자. 아래에서 강조 표시된 모든 명령을 명령줄에 입력하여 프로젝트를 생성할 수 있습니다.

npx:

```shell
$ npx create-react-app draft-js-example
```

npm(`npm in <initializer`)은 npm 6+에서 사용할 수 있습니다.

```shell
$ npm init react-app draft-js-example
```

yarn(`yarn create`는 yarn 0.25+에서 사용 가능):

다음과 같은 폴더 구조를 가진 프로젝트 폴더가 생성됩니다.

```undefined
myapp-|
      |__node_mudles/
      |__public/
      |__src/
      |__.gitignore
      |__pacakge.json
      |__README.md
      |__yarn.lock
```

프로젝트 폴더의 루트로 이동하여 실행하십시오.

```undefined
cd draft-js-example
npm start //or
yarn start
```

그러면 앱이 개발 모드로 실행되고 http://localhost:3000/ 링크를 사용하여 브라우저에서 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/editsrcappjsandsavetoreload.png?resize=2872%2C1644&ssl=1)

## 추가 종속성 설치

```shell
$ yarn add draft-js react-draft-wysiwyg
```

## 설정 편집기

진행하려면 `src/App.js` 파일을 일부 수정해야 합니다. 우리는 react-draft-wysiwyg의 편집기 컴포넌트 및 스타일과 드래프트.js의 에디터스테이트를 요구할 것이다. 편집기 구성 요소는 스타일링 없이 기본 Draft.js 편집기를 사용합니다. Draft.js 편집기는 React의 제어된 입력 API를 기반으로 하는 제어된 `ContentEditable` 구성 요소로 빌드되며, `EditorState`는 편집기 상태의 스냅샷을 제공합니다. 실행 취소/다시 실행 기록, 내용 및 커서가 포함됩니다.

편집기를 표시하기 위해 몇 가지 초기 변경 사항을 추가하겠습니다.

```js
import React, { useState } from 'react';
import { EditorState } from 'draft-js';
import { Editor } from 'react-draft-wysiwyg';
import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css';
import './App.css';
const App = () => {
  const [editorState, setEditorState] = useState(
    () => EditorState.createEmpty(),
  );
  return (
    <div className="App">
      <header className="App-header">
        Rich Text Editor Example
      </header>
      <Editor editorState={editorState} />
    </div>
  )
}
export default App;
```

먼저 `편집자주`의 `create empty` 방식으로 빈 상태를 만들겠습니다. 편집기 구성 요소는 `편집 상태`를 소품으로 사용합니다. 변경 사항 및 업데이트를 저장하면 다음과 같이 보기에 좋지 않습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/texteditorexample.png?resize=2400%2C1644&ssl=1)

`App.css` 파일의 경우 몇 가지 변경이 필요합니다. 내용을 다음과 같이 바꿉니다.

```css
.App-header {
   background-color: #282c34;
   min-height: 5vh;
   display: flex;
   flex-direction: column;
   align-items: center;
   justify-content: center;
   font-size: calc(10px + 2vmin);
   color: white;
   margin-bottom: 5vh;
   text-align: center;
 }
```

업데이트 후 당사의 `App` 구성 요소는 다음과 같습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/renderededitor.png?resize=730%2C501&ssl=1)

## 편집기 스타일링

위에서 설명한 편집기를 보면 텍스트를 어디에 입력해야 하는지 알 수 없습니다. 편집기의 다른 섹션은 스타일 소품을 사용하여 보다 분명하게 나타낼 수 있습니다. 소품은 특정 섹션에 적용할 클래스 또는 스타일을 포함하는 객체가 될 수 있습니다.

- wrapperClassName="wrapper-class"
- EditorClassName="editor-class"
- toolbarClassName="toolbar-class"
- wrapperStyle={=wrapperStyleObject}
- 편집기 스타일= {=editorStyleObject}
- 도구 모음Style={=toolbarStyleObject}

Editor 구성 요소에 `className` 소품 및 `App.css`에 다음과 같이 관련 스타일을 추가하여 편집기를 스타일링합니다.

### App.js:

```xml
<Editor
  editorState={editorState}
  onChange={setEditorState}
  wrapperClassName="wrapper-class"
  editorClassName="editor-class"
  toolbarClassName="toolbar-class"
/>
```

### App.css:

```css
.wrapper-class {
  padding: 1rem;
  border: 1px solid #ccc;
}
.editor-class {
  background-color:lightgray;
  padding: 1rem;
  border: 1px solid #ccc;
}
.toolbar-class {
  border: 1px solid #ccc;
}
```

애플리케이션을 다시 로드할 때 편집기는 다음과 같은 모양을 해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/styledtexteditor.png?resize=2034%2C1350&ssl=1)

## 편집기 상태

편집기는 제어되거나 제어되지 않는 구성 요소일 수 있습니다. 제어된 편집기는 `편집기 상태`(편집기의 최상위 상태 객체)를 사용하여 수행됩니다. 제어되지 않는 편집기는 편집자 상태 또는 RawDraftContentState(내용의 원시 형식의 예상 구조를 나타내는 흐름 유형)를 사용하여 만들 수 있습니다.

제어된 편집기를 만들려면 다음과 같은 소품을 전달합니다.

- `editorState` — 제어된 방식으로 편집기 상태를 업데이트하는 권한
- `onEditorStateChange` — `EditorState` 유형의 객체 인수를 사용하는 편집기 상태가 변경될 때 호출되는 함수

이 기능을 편집자 구성 요소에 추가하면 다음과 같이 나타납니다.

```js
const App = () => {
  const [editorState, setEditorState] = useState(
    () => EditorState.createEmpty(),
  );
  return (
    <div className="App">
      <header className="App-header">
        Rich Text Editor Example
      </header>
      <Editor
        editorState={editorState}
        onEditorStateChange={setEditorState}
        wrapperClassName="wrapper-class"
        editorClassName="editor-class"
        toolbarClassName="toolbar-class"
      />
    </div>
  )
}
```

`EditorState`는 다음을 전달하여 제어되지 않는 편집기를 만드는 데도 사용할 수 있습니다.

- `defaultEditorState` - `EditorState` 유형의 개체로 편집기 상태를 만든 후 초기화합니다.

```js
const App = () => {
  const [editorState, setEditorState] = useState(
    () => EditorState.createEmpty(),
  );
  return (
    <div className="App">
      <header className="App-header">
        Rich Text Editor Example
      </header>
      <Editor
        defaultEditorState={editorState}
        onEditorStateChange={setEditorState}
        wrapperClassName="wrapper-class"
        editorClassName="editor-class"
        toolbarClassName="toolbar-class"
      />
    </div>
  )
}
```

제어되지 않는 편집기를 달성하는 또 다른 방법은 `원안 컨텐츠 상태`를 사용하는 것이다. 제어되지 않는 편집기는 다음과 같은 소품을 사용합니다.

- `defaultContentState` - `RawDraftContentState` 유형의 개체로 편집기 상태를 만든 후 초기화합니다.
- "onContentStateChange" — "RawDraftContentState" 유형의 객체 인수를 사용하는 편집기 상태가 변경될 때 호출되는 함수

RawDraftContentState를 사용하여 구현된 제어되지 않는 편집기는 다음과 같습니다.

```js
import React, { useState } from 'react';
import { ContentState, convertToRaw } from 'draft-js';
import { Editor } from 'react-draft-wysiwyg';
import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css';
import './App.css';
const App = () => {
  let _contentState = ContentState.createFromText('Sample content state');
  const raw = convertToRaw(_contentState)
  const [contentState, setContentState] = useState(raw) // ContentState JSON
  return (
    <div className="App">
      <header className="App-header">
        Rich Text Editor Example
      </header>
      <Editor
        defaultContentState={contentState}
        onContentStateChange={setContentState}
        wrapperClassName="wrapper-class"
        editorClassName="editor-class"
        toolbarClassName="toolbar-class"
      />
    </div>
  )
}
export default App;
```

렌더링하면 다음과 같이 편집기가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/samplecontentstate.png?resize=730%2C500&ssl=1)

## 데이터 변환

리치 텍스트 편집기와 상호 작용하는 사용자는 의심할 여지 없이 텍스트를 저장하거나 저장된 텍스트를 편집해야 합니다. 즉, `ContentState`를 원시 JavaScript로 변환하거나 그 반대로 변환할 수 있어야 합니다. Draft.js는 다음과 같은 세 가지 기능을 제공합니다.

`원본 콘텐츠` - 원시 상태(원본 콘텐츠 상태)를 `콘텐츠 상태`로 변환

`ContentToRaw` — `ContentState`를 원시 상태로 변환

내용 출처HTML — HTML 조각을 두 개의 키를 가진 개체로 변환합니다. 이 개체 중 하나는 `ContentBlock` 개체 배열을 포함하고 다른 하나는 `entityMap`에 대한 참조를 보유합니다. 그런 다음 이 개체를 사용하여 콘텐츠 상태를 구성할 수 있습니다.

특정 사용 사례에 대해, 우리는 저장 및 디스플레이를 위해 편집자 상태를 HTML로 변환하려고 합니다. 우리는 draftjs-to-html 또는 draft-convert와 같은 라이브러리를 사용하여 이 작업을 수행할 수 있다. draftjs-to-html보다 기능이 더 많기 때문에 우리는 draft-convert를 사용할 것이다. 터미널에서 다음 명령을 실행하여 터미널을 설치합니다.

```undefined
yarn add draft-convert
```

## 리치 텍스트 표시

편집자가 리치 텍스트를 만들 준비가 되어 있지만 입력한 내용을 저장하거나 표시할 준비가 되어 있지 않습니다. 편집기와 다른 도구 모음 옵션을 사용하여 리치 텍스트를 만드는 다양한 방법을 볼 수 있습니다. 다음은 이 단계에서 편집자와 함께 할 수 있었던 여러 가지 일의 예입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/sampleformattedteext.png?resize=730%2C500&ssl=1)

다음 단계는 이 리치 텍스트를 HTML로 변환하는 것입니다. 우리는 `convertTo`를 사용할 것입니다.HTML은 초안 변환에서 변환으로 기능하지만 먼저 현재 내용을 편집기에 입력해야 합니다.

에디터스테이트(EditorState)는 getCurrentContent(게트커런트컨텐츠)라는 방법을 제공하는데, 이 방법은 HTML로 변환할 수 있다. 편집기 상태를 업데이트하고 변환된 HTML을 저장하는 처리기가 호출됩니다. 내용을 가져와서 HTML로 변환하는 구성 요소 방법을 추가해 보겠습니다.

```js
import { convertToHTML } from 'draft-convert';
....

const App = () => {
  ...
  const handleEditorChange = (state) => {
    setEditorState(state);
    convertContentToHTML();
  }
  const convertContentToHTML = () => {
    let currentContentAsHTML = convertToHTML(editorState.getCurrentContent());
    setConvertedContent(currentContentAsHTML);
  }
  return (
    <div className="App">
      <header className="App-header">
        Rich Text Editor Example
      </header>
      <Editor
        editorState={editorState}
        onEditorStateChange={handleEditorChange}
        wrapperClassName="wrapper-class"
        editorClassName="editor-class"
        toolbarClassName="toolbar-class"
      />
    </div>
  )
}
```

> 일부 편집기 형식은 초안 변환에서 지원되지 않을 수 있으며 오류가 발생할 수 있습니다.

이제 편집기에 입력된 텍스트가 상태로 변환되어 저장되었으므로 표시할 수 있습니다. 입력된 텍스트를 표시하는 div를 생성하겠습니다. 본문을 표시하기 위해 위험하게 Set Inner를 사용하겠습니다.HTML. 왜 위험한지 자문해 볼 수 있다. 코드에서 HTML을 설정하면 XSS(사이트 간 스크립팅) 공격에 노출될 수 있습니다. 그 이름은 당신이 이 일을 할 때 관련된 위험들을 은밀하게 상기시켜 준다. 페이지에 HTML을 추가하기 전에 HTML이 제대로 구성되고 검사되었는지 확인해야 합니다. 이것을 하는 쉬운 방법은 dompurify와 같은 라이브러리를 사용하는 것이다. 다음 명령을 사용하여 이 패키지를 설치하십시오.

```undefined
yarn add dompurify
```

위험하게 이너 설정HTML은 `__html` 키(밑줄 2개)로 객체를 수신한다. 이 키는 삭제된 HTML을 보관합니다. 이제 HTML을 검사하는 방법과 해당 HTML을 보관하는 요소를 추가할 수 있습니다.

```js
import DOMPurify from 'dompurify';
...
const App = () => {
  ...
  const createMarkup = (html) => {
    return  {
      __html: DOMPurify.sanitize(html)
    }
  }
  return (
    <div className="App">
      ...
      <div className="preview" dangerouslySetInnerHTML={createMarkup(convertedContent)}></div>
    </div>
  )
}
```

`createMarkup` 함수는 HTML 문자열을 인수로 수신하고 검사된 HTML로 개체를 반환합니다. 이 메서드를 `위험하게 SetInner`에서 호출합니다.HTML로 변환된 컨텐츠가 포함된 HTML입니다.

HTML 컨테이너의 일부 스타일:

```css
.preview {
  padding: 1rem;
  margin-top: 1rem;
}
```

저장하고 다시 로드한 후, 우리는 편집기에 텍스트를 입력하고 포맷한 것과 같은 방식으로 편집기 아래에 표시되는 것을 볼 수 있습니다.

다 됐다! 더 이상 어쩔 수 없다! 끝났습니다. 우리는 다양한 방법으로 텍스트를 포맷할 수 있는 풍부한 편집기를 만들었습니다. `App.js` 파일의 최종 코드는 다음과 같아야 한다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/dislayformattext.png?resize=1878%2C1046&ssl=1)

```js
import React, { useState } from 'react';
import { EditorState } from 'draft-js';
import { Editor } from 'react-draft-wysiwyg';
import { convertToHTML } from 'draft-convert';
import DOMPurify from 'dompurify';
import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css';
import './App.css';
const App = () => {
  const [editorState, setEditorState] = useState(
    () => EditorState.createEmpty(),
  );
  const  [convertedContent, setConvertedContent] = useState(null);
  const handleEditorChange = (state) => {
    setEditorState(state);
    convertContentToHTML();
  }
  const convertContentToHTML = () => {
    let currentContentAsHTML = convertToHTML(editorState.getCurrentContent());
    setConvertedContent(currentContentAsHTML);
  }
  const createMarkup = (html) => {
    return  {
      __html: DOMPurify.sanitize(html)
    }
  }
  return (
    <div className="App">
      <header className="App-header">
        Rich Text Editor Example
      </header>
      <Editor
        editorState={editorState}
        onEditorStateChange={handleEditorChange}
        wrapperClassName="wrapper-class"
        editorClassName="editor-class"
        toolbarClassName="toolbar-class"
      />
      <div className="preview" dangerouslySetInnerHTML={createMarkup(convertedContent)}></div>
    </div>
  )
}
export default App;
```

## 결론

이 기사에서는 여러 가지 옵션이 있는 도구 모음을 이미 가지고 있는 편집기를 만들고 텍스트를 입력하고 텍스트를 표시하는 방법을 살펴보았습니다. 우리는 `react-draft-wysiwyg`로 만들어낼 수 있는 경험의 표면을 거의 긁어내지 못했다. `react-draft-wysiwyg` 설명서에서 추가 옵션과 기능을 찾을 수 있으며, 보다 기능이 뛰어난 편집기를 만들 수 있는 모든 방법을 살펴볼 수 있습니다. 추가 작업을 수행하려면 데이터베이스에 텍스트를 저장하고 검색한 후 업데이트하는 기능을 추가할 수 있습니다. 이 기사의 코드는 깃허브에서 찾을 수 있다. 네가 뭘 지었는지 너무 보고 싶어.