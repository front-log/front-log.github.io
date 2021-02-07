---
layout: post
title: "Deno의 표준 라이브러리: 4개의 핵심 모듈"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/deno-standard-library.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/deno-standard-library.png?fit=730%2C487&ssl=1)

Deno가 제공하는 많은 멋진 기능들 중 하나는 개발자들의 삶을 편리하게 하기 위해 고안된 표준 모듈 세트이다. Go와 Python과 같은 기존 언어에서 주로 영감을 받은 이 모듈들은 Deno에서 승인되었으며 외부 의존성을 가지고 있지 않다.

## 데노의 표준 도서관 안에는 무엇이 있나요?

데노 표준도서관은 모든 데노 프로젝트가 원활하게 사용할 수 있는 고품질 코드를 종합적으로 수집해 제공하겠다는 취지다.

이 튜토리얼에서는 Deno 앱을 한 단계 끌어올리는 데 도움이 되는 4가지 핵심 표준 라이브러리를 살펴봅니다.

- HTTP 서버 설정용 `http`
- 데노의 암호 도서관인 해시
- Deno의 파일 관리 시스템인 fs
- 고유 ID 생성용 `uuid`

## Deno 설치

시작하기 전에 로컬 컴퓨터에 Deno를 설치하는 방법에 대해 간단히 살펴보겠습니다.

### 창문들

Windows 컴퓨터에 Deno를 설치하려면 터미널을 열고 다음 명령을 실행하십시오.

```undefined
iwr https://deno.land/x/install/install.ps1 -useb | iex
```

시스템에 Chocolatey가 설치되어 있는 경우 다음을 실행할 수도 있습니다.

```undefined
choco install deno
```

### 맥

다음을 실행하여 Homebrew를 사용하여 Mac 시스템에 Deno를 설치할 수 있습니다.

```undefined
brew install deno
```

Curl을 사용하여 Deno를 설치할 수도 있습니다.

```undefined
curl -fsSL https://deno.land/x/install/install.sh | sh
```

Deno가 제대로 설치된 경우 위의 명령을 실행하면 다음 출력이 생성됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/install-deno-mac.png?resize=720%2C172&ssl=1)

설치 후 다음 명령을 실행하여 로컬 컴퓨터에 Deno가 제대로 설치되었는지 확인합니다.

```undefined
deno run https://deno.land/std/examples/welcome.ts
```

매우 간단한 Deno 명령은 터미널에 "Welcome to Deno 🦕"를 인쇄합니다.

이제 Deno가 설치되었으니 Deno의 표준 라이브러리에서 가장 인기 있고 유용한 4개의 모듈을 확대해보겠습니다.

## 1. '스파이브'

이 라이브러리는 서버를 설정하는 간단한 방법을 제공합니다.

먼저 `서버` 인스턴스를 응용 프로그램에 가져옵니다. 그런 다음 서버 인스턴스를 사용하여 `PORT`에 연결합니다.

```js
import { serve } from "https://deno.land/std/http/server.ts";
const s = serve({ port: 8000 });
for await (const req of s) {
  req.respond({ body: "Hi,I'm Wisdom Ekpot" });
}
```

서버를 시작하려면 다음을 실행하십시오.

```css
deno run --allow-net http.ts
```

Deno의 보안상 `--allow-net` 플래그를 통과해야 합니다.

앱 방법의 단일 인스턴스를 사용하여 체인 경로를 만들 수 있는 ABC와 같이 Deno의 서버에 연결하는 다른 방법이 있습니다.

ABC를 사용하여 서버를 생성하려면 `http.ts` 파일에 이 서버를 추가하십시오.

```js
import { Application } from "https://deno.land/x/abc@v1.1.0/mod.ts";
const app = new Application();
const PORT = 8000;
let homepage = () => {
  return "This is the home page";
};
let contactPage = () => {
  return "You can contact Wisdom Ekpot with this contact Page";
};
app
  .get("/", homepage)
  .get("/contact", contactPage)
  .start({ port: PORT });
console.log(`🔤 Abc server running at http://localhost:${PORT}`);
```

이제 응용 프로그램의 다른 경로를 생성할 수 있습니다.

작성 당시 https://deno.land/x/abc@v1.1.0/mod.ts는 ABC의 최신 버전이다. 최신 버전의 공식 문서를 확인하십시오.

## 2. "기억"

Node.js와 마찬가지로 Deno도 해시 라이브러리와 함께 제공됩니다. 해시 모듈은 암호 해싱이나 메시지 해싱과 같은 것을 구현하고자 할 때 유용합니다.

이 모듈을 사용하려면 `createHash` 인스턴스를 가져온 다음 이 인스턴스를 저장할 변수를 만들어야 합니다. 이 인스턴스에는 다음과 같은 알고리즘 유형이 포함됩니다.

```java
import { createHash } from "https://deno.land/std/hash/mod.ts";
const hash = createHash("md5");
hash.update("All You need to know about deno");
const final = hash.digest();
console.log(final);
```

이 응용 프로그램을 실행하려면 `deno run <ofile`을 실행하십시오. 이 코드는 해시된 데이터를 ArrayBuffer 형식으로 기록합니다.

`createHash` 인스턴스의 해시 알고리즘을 매개 변수로 전달할 수 있습니다.

## 3. fs

Deno는 자체 파일 시스템 매니저와 함께 제공됩니다. `fs` 모듈을 사용하면 원하는 모든 종류의 파일 조작을 쓰기, 읽기, 복사 및 수행할 수 있습니다.

### 파일 내용 읽기

Deno.open() 방법을 사용하여 Deno에서 파일 내용을 읽을 수 있습니다. 이 메서드는 읽을 파일의 이름(경로)인 매개 변수를 사용합니다.

`파일을 작성합니다.txt 파일 및 일부 더미 콘텐츠를 저장합니다. 이 파일에서 내용을 읽습니다.

```js
const file = await Deno.open("file.txt");
const decoder = new TextDecoder("utf-8");
const text = decoder.decode(await Deno.readAll(file));
console.log(text);
```

Deno.open() 방법은 약속을 반환합니다. 이 코드는 데이터를 사람이 읽을 수 있는 형식으로 디코딩하는 디코더 인스턴스를 만들고 `readAll` 메서드는 `파일`에 저장된 텍스트를 반환합니다.txt 파일

응용 프로그램을 시작하려면 다음을 실행하십시오.

```undefined
deno run --allow-read
```

Deno는 "-읽기 허용" 플래그를 전달하지 않으면 파일을 읽을 수 없습니다.

### 파일에 쓰기

`Deno`를 사용할 수 있습니다.파일에 텍스트를 쓰기 위해 TextFile() 메서드를 씁니다. 이 방법은 쓰려는 파일의 경로와 파일에 쓸 내용의 두 가지 매개 변수를 사용합니다.

```js
const file_path = "file.txt";
const data = "This is the new content in my filex";
await Deno.writeTextFile(file_path, data);
// Read the file data 
console.log(await Deno.readTextFile(file_path));
```

변수에 저장된 `정의` 경로가 없는 경우 Deno가 자동으로 파일을 만듭니다.

이 프로그램을 실행하려면 파일에 쓰고 읽으면서 내용이 실제로 변경되었는지 확인하기 때문에 `---쓰기 허용`과 `---읽기 허용`이라는 두 개의 플래그를 전달해야 합니다. `write`에서 다른 매개 변수를 추가할 수 있습니다.파일에 데이터를 추가하는 TextFile` 메서드:

```coffeescript
await Deno.writeTextFile(file_path, data, {"append": true});
```

파일 데이터를 한 파일에서 다른 파일로 복사하는 등의 다른 작업도 수행할 수 있습니다. 다음 `fs` 모듈에서 복사 인스턴스를 가져와야 합니다.

```js
import { copy } from "https://deno.land/std/fs/mod.ts";
copy("file.txt", "new.txt"); // void
```

코드를 실행하려면 먼저 `deno run --allow-write --allow-read --unstable fs.ts`를 실행하여 `new`를 만드십시오.txt 파일 및 내용을 저장합니다. 파일을 기존 파일에 복사하려면 해당 파일의 기존 내용을 덮어쓰려면 `{덮어쓰기:true}` 매개 변수를 전달해야 합니다.

```coffeescript
import { copy } from "https://deno.land/std/fs/mod.ts";
copy("file.txt", "test.txt", {
  overwrite: true,
});
```

## 4. UUID

데노는 또한 보편적으로 고유한 식별자를 가지고 있다. 이 라이브러리를 사용하여 고유 ID를 생성하고 검증할 수 있습니다.

```js
import { v4 } from "https://deno.land/std/uuid/mod.ts";
const generatedID = v4.generate();
const isValid = v4.validate(generatedID);
console.log(isValid);
```

이 메서드는 유효한 UUID이기 때문에 콘솔에 `true`가 기록됩니다. 하드코드를 찍으면 콘솔에 false가 반환됩니다.

## 결론

Deno에는 개발자 경험을 향상시키는 많은 멋진 표준 라이브러리와 타사 모듈이 포함되어 있습니다. 이러한 모듈을 사용하여 Deno 응용 프로그램에서 암호 해시 및 데이터베이스의 ID 생성과 같은 복잡한 논리를 작성할 수 있습니다.

다음 프로젝트에서 가장 사용하고 싶은 Deno 표준 라이브러리 모듈은 무엇입니까?