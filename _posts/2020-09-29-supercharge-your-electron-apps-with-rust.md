---
layout: post
title: "러스트를 사용하여 전자 앱 과금"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/supercharge-your-electron-apps-with-rust-1.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/supercharge-your-electron-apps-with-rust-1.png?fit=730%2C487&ssl=1)

Electron은 크로스 플랫폼 데스크탑 애플리케이션을 만드는 최고의 기술이 되었습니다. Atom, VS Code, Spotify, Slack, Discord 등 널리 알려진 플랫폼에서도 사용된다.

JavaScript는 느린 언어이지만, 이 튜토리얼에서는 보다 나은 사용자 환경을 위해 Rust를 사용하여 Electron 앱을 가속화하는 방법을 시연합니다. 녹은 높은 수준의 인체공학을 가진 낮은 수준의 언어이다. 메모리 공간이 매우 낮은 상태로 빠르게 타오르고 있습니다.

## Electron 프로젝트

전자 반응 보일러 플레이트를 사용하여 데모 응용 프로그램을 위한 보일러 플레이트를 만들 것입니다. GitHubrepo를 복제하는 것부터 시작하세요.

```undefined
git clone --depth 1 --single-branch https://github.com/electron-react-boilerplate/electron-react-boilerplate.git md_notes
```

npm 또는 Yarn을 사용하여 모든 종속성을 설치합니다.

```coffeescript
npm run dev
```

우리는 마크다운을 지원하는 노트필기 어플리케이션을 만들 것이다. 마크다운 파서는 Rust로 작성될 것이다. 전자와 러스트의 인터페이스에는 다양한 방법을 사용할 것입니다.

## 전자 프로젝트에 UI 추가

Material-UI를 사용하여 애플리케이션을 위한 기성 구성 요소를 확보할 것입니다.

```coffeescript
import React, { useState, useEffect } from 'react';
import { makeStyles, createStyles, Theme } from '@material-ui/core/styles';
import Grid from '@material-ui/core/Grid';
import Paper from '@material-ui/core/Paper';
import { TextField, Typography, ListItemText, Button } from '@material-ui/core';
import List from '@material-ui/core/List';
import ListItem from '@material-ui/core/ListItem';
import { remote } from 'electron';
// import fs and path nodejs core module for wrting and reading files
const fs = remote.require('fs');
const path = remote.require('path');
const useStyles = makeStyles((theme: Theme) =>
  createStyles({
    root: {
      flexGrow: 1,
      width: '100vw',
    },
    paper: {
      width: '100%',
      minHeight: '100vh',
      padding: '5px',
    },
    control: {
      padding: theme.spacing(2),
    },
    textArea: {
      width: '100%',
      height: '100%',
    },
    heading: {
      textAlign: 'center',
    },
  })
);
const NOTES_FOLDER = 'notes';
const save = (name: string, data: any) => {
  fs.writeFileSync(
    path.join('.', NOTES_FOLDER, `${name}${Date.now()}.json`).toString(),
    JSON.stringify(data)
  );
};
export default function SpacingGrid() {
  const [editorState, setEditorState] = useState('');
  const [previewState, setPreview] = useState('');
  const [notes, setNotes] = useState([]);
  useEffect(() => {
    // we need to add code to parse md and set it to preview
  }, [setPreview, editorState]);
  useEffect(() => {
    const note = fs
      .readdirSync(path.join('.', NOTES_FOLDER))
      .map((file: string) => {
        return JSON.parse(
          String(fs.readFileSync(path.join('.', NOTES_FOLDER, file)))
        );
      });
    setNotes(note);
  }, []);
  const [title, setTitle] = useState('');
  const classes = useStyles();
  return (
    <Grid container className={classes.root} spacing={2}>
      <Grid item xs={12}>
        <Grid container justify="center" spacing={2}>
          <Grid item xs={9}>
            <Paper className={classes.paper}>
              <Grid container justify="space-between">
                <Grid item>
                  <Typography variant="h3" align="center">
                    Add Notes
                  </Typography>
                </Grid>
                <Grid item>
                  <Button
                    variant="outlined"
                    onClick={() => {
                      save(title, {
                        title,
                        createdAt: Date.now(),
                        body: editorState,
                      });
                    }
                  >
                    Save
                  </Button>
                </Grid>
              </Grid>
              <Grid
                container
                direction="column"
                justify="space-between"
                spacing={2}
              >
                <Grid item>
                  <TextField
                    label="Title"
                    value={title}
                    onChange={(e) => setTitle(e.target.value)}
                    variant="outlined"
                    className={classes.textArea}
                  />
                </Grid>
                <Grid item>
                  <TextField
                    multiline
                    label="Content"
                    value={editorState}
                    onChange={(e) => setEditorState(e.target.value)}
                    variant="outlined"
                    className={classes.textArea}
                    rows={20}
                  />
                </Grid>
                <Grid item>
                  <div dangerouslySetInnerHTML={ __html: previewState } />
                </Grid>
              </Grid>
            </Paper>
          </Grid>
          <Grid item xs={3}>
            <Paper className={classes.paper}>
              <List>
                {notes.map((note, i) => (
                  <ListItem
                    key={`key-${i}`}
                    onClick={() => {
                      setEditorState(note.body);
                      setTitle(note.title);
                    }
                  >
                    <ListItemText
                      primary={note.title}
                      secondary={new Date(note.createdAt).toLocaleString('in')}
                    />
                  </ListItem>
                ))}
              </List>
            </Paper>
          </Grid>
        </Grid>
      </Grid>
    </Grid>
  );
}
```

이것은 Electron 응용 프로그램용 보일러 플레이트 코드입니다. 이제 마크다운을 HTML로 변환하는 구문 분석 기능을 구현하겠습니다.

우리는 각각의 장단점을 따져보고, 마크다운을 구문 분석하기 위해 Rust를 사용하는 다양한 방법을 탐구할 것이다. 또한 각 방법에 대한 사용 사례도 살펴볼 것입니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/electron-notes-app.png?resize=720%2C391&ssl=1)

## 전자에서 웹 어셈블리 사용

핵심에서 일렉트로닉 앱은 크롬 브라우저와 노드.js에 불과하다. Chrome 및 Node에서 Web Assembly 지원을 활용할 수 있습니다.

WebAssembly는 브라우저 또는 스택 기반 가상 시스템 내에서 실행되는 이진 명령 형식입니다. Rust in Web Assembly에 쓰여진 우리의 마크다운 파서를 컴파일할 것이다.

### Web Assembly에서 Rustmark down 구문 분석기 작성

Rust는 Web Assembly에 대한 일급 지원을 제공합니다. LLVM을 사용하여 WebAssembly를 대상으로 컴파일합니다. `\wasm-pack\`은 개발자가 Web Assembly를 사용하여 Rust에 노드 모듈을 작성할 수 있도록 Rust 커뮤니티가 만든 툴이다.

### 와시팩 사용

wasm-pack은 Web Assembly에 작성된 노드 모듈을 생성, 빌드 및 게시하는 도구입니다.

```undefined
curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh
```

wasm-pack은 리눅스 및 macOS용 설치 관리자 스크립트를 제공합니다. 시스템에 `wasm-pack`이 설치되면 `wasm-pack new`를 사용하여 Web Assembly 모듈의 보일러판을 생성할 수 있습니다.

이렇게 하면 `md-parser`라는 이름의 디렉토리가 보일러 플레이트로 만들어집니다.

```css
wasm-pack new md_webassembly


├── Cargo.toml
├── LICENSE_APACHE
├── LICENSE_MIT
├── README.md
├── src
│   ├── lib.rs
│   └── utils.rs
└── tests
    └── web.rs
```

lib.rs은 Wasm 라이브러리의 메인 파일이다. lib.rs을 자세히 들여다보면 자바스크립트(JavaScript)가 부르는 e메일 기능을 그대로 내보내는 것으로 나타났다.

```php
// utils set a panic hook so it is console in js
mod utils;
use wasm_bindgen::prelude::*;
// When the `wee_alloc` feature is enabled, use `wee_alloc` as the global
// allocator.
#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;

// function defination for alert. It is dom api for generating a dialog
#[wasm_bindgen]
extern {
    fn alert(s: &str);
}

// wasm_bingen is a pro macro which transfor return type and arguments to the function
#[wasm_bindgen]
pub fn greet() {
// calling alert
    alert("Hello, md-webassembly!");
}
```

와셈비든은 와셈 모듈을 쉽게 만들 수 있도록 러스트 커뮤니티가 개발한 상자다. Wasm은 문자열 또는 배열 데이터 유형을 지원하지 않으며 정수와 부동액만 지원합니다. 따라서 액세스 문자열을 사용하려면 JavaScript 힙에 있는 문자열 주소가 필요합니다. wasm-condergen은 우리의 기능에 모든 보일러 플레이트 코드를 추가한다.

마크다운 구문 분석의 경우 GitHub 맛 마크다운을 지원하는 `comrak` 상자를 사용합니다.

```undefined
[package]
name = "md-webassembly"
version = "0.1.0"
authors = ["anshul <anshulgoel151999@gmail.com>"]
edition = "2018"

# libtype must be cdylib so it can be dynamically linked by electron
[lib]
crate-type = ["cdylib", "rlib"]

[features]
default = ["console_error_panic_hook"]

[dependencies]
wasm-bindgen = "0.2.63"
comrak = "0.8.2"
console_error_panic_hook = { version = "0.1.6", optional = true }
wee_alloc = { version = "0.4.5", optional = true }

[dev-dependencies]
wasm-bindgen-test = "0.3.13"

[profile.release]
# Tell `rustc` to optimize for small code size.
opt-level = "s"

# avoid wasm-opt error while build
[package.metadata.wasm-pack.profile.release]
wasm-opt = false
```

### 'comrak' 상자

`comark`는 HTML로 마크다운하는 `markdown_to_html` 기능을 제공합니다. 그러면 마크다운 문자열을 인수로 가져온 다음 HTML 문자열을 반환하는 구문 분석 함수를 생성합니다.

```php
mod utils;
use comrak::{markdown_to_html, ComrakOptions};
use wasm_bindgen::prelude::*;
#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;

// take md string return html string
#[wasm_bindgen]
pub fn parse(md: String) -> String {
    markdown_to_html(md.as_str(), &ComrakOptions::default())
}
```

우리는 모두 녹슬지 않은 파서를 사용할 준비가 되어 있다. 컴파일 시 wasm-pack build. build 명령을 사용하면 스크립트 유형이 포함된 wasm 모듈이 생성됩니다.

### 전자에서 웹 어셈블리 사용

우리는 브라우저측과 전자측 Node.js API 모두에서 Rust mark down 파서를 사용할 수 있다.

노드와 크롬 모두 포장에서 꺼낸 웹어셈블리를 지원합니다. 게다가 전자 부트스트랩에 사용되는 보일러도 기본적으로 wasm 모듈 수입을 지원한다. 우리는 Wasm 코드를 어떤 보일러 플레이트도 추가하지 않고 직접 수입할 수 있습니다.

```coffeescript
  useEffect(() => {
    import('../../md-webassembly/pkg/md_webassembly').then((module) =>
      setPreview(module.parse(editorState))
    );
  }, [setPreview, editorState]);
```

Wasm 모듈이 인스턴스화되는 동안 UI 렌더링이 차단되지 않도록 동적 가져오기를 사용해야 합니다. 웹팩은 휴식을 취할 것이다. 이 방법은 웹 앱을 지원하려는 경우에도 유용합니다.

## 전자에서 NAPI 사용

Napi는 Node.js와 통신하는 C 헤더이다. 이 방법은 실행하기가 좀 어렵다.

Node.js에서 직접 사용할 수 있는 Rust 라이브러리를 만들려면 `Node-bindgen`을 사용합니다.

화물을 이용하여 nj-cli를 설치한다.

```undefined
cargo install nj-cli
```

이제 노드 모듈에 대한 보일러 플레이트를 만든 다음 전자 애플리케이션으로 가져올 수 있습니다.

먼저 간단한 Rust 프로젝트를 만들어 보겠습니다.

```coffeescript
cargo new md_napi --lib
```

이제 Node.js에서 사용할 수 있는 동적 라이브러리에서 컴파일하도록 `cargo`를 구성해야 한다. 응용 프로그램에 스레드, SIMD 또는 프로세서별 코드가 필요한 경우 이 방법을 사용해야 합니다.

```undefined
[package]
name = "md_napi"
version = "0.1.0"
authors = ["anshul <anshulgoel151999@gmail.com>"]
edition = "2018"

[lib]
# compile to .so or .dl file based on OS
crate-type = ["cdylib"]

[dependencies]
# Node.js NAPI helper library
node-bindgen = { version = "2.1.1" }
# Parse MarkDown
comrak = "0.8.2"

[build-dependencies]
node-bindgen = { version = "2.1.1", features = ["build"] }
```

우리는 lib.rs 파일의 기능을 내보낼 수 있습니다.

```php
use node_bindgen::derive::node_bindgen;
use comrak::{markdown_to_html, ComrakOptions};

#[node_bindgen]
fn parse(md: String) -> String {
  markdown_to_html(md.as_str(), &ComrakOptions::default())
}
```

이제 nj-cli build 명령을 사용하여 라이브러리를 작성할 수 있습니다. index.node로 dist 디렉토리를 생성한다. 이 파일은 Electron에서 직접 필요합니다.

```coffeescript
import { remote } from 'electron';
const mdModule = remote.require('../md_napi/dist/index.node');

console.log((mdModule.parse('# Heading'))
```

Electron 앱에서는 Web Assembly 모듈 가져오기를 간단히 교체합니다.

```coffeescript
 useEffect(() => {
    setPreview(mdModule.parse(editorState));
  }, [setPreview, editorState]);
```

### 기타 라이브러리

추가 기능을 만드는 데 사용할 수 있는 몇 가지 추가 라이브러리가 있습니다. 아래 나열된 라이브러리는 V8 API 또는 NAPI를 사용합니다.

- 네온
- 내피족
- nodejs-sys

## 비교

위에서 설명한 두 가지 방법 모두 장단점이 있습니다. 특정 프로젝트의 올바른 선택은 사용 사례와 요구사항에 따라 달라집니다. 아래 차트를 빠른 참조 가이드로 사용하여 가장 적절한 접근 방식을 선택할 수 있습니다.

## 결론

JavaScript는 느린 언어입니다. 따라서 전자 앱은 느린 경향이 있습니다. 이 기술은 CPU 사용량이 많은 작업을 다른 빠른 언어로 오프로드하는 것입니다. 자바스크립트는 UI 제어에 잘 적합하지만, Rust는 그러한 복잡한 계산을 처리할 수 있는 더 나은 장비를 갖추고 있다. 이 튜토리얼을 완료했다면 이제 Rust를 사용하여 벨트 아래에서 Electron 앱의 속도를 높이는 두 가지 시도 및 참 방법을 사용할 수 있습니다.