---
layout: post
title: "Go(이동)"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/workingwithgoimages.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/workingwithgoimages.png?fit=730%2C412&ssl=1)

웹 애플리케이션은 종종 사용자를 위해 아바타를 렌더링해야 하며, 사용자가 항상 자신의 사진을 업로드하는 것을 좋아하는 것은 아니다. 일반적으로 사용되는 오류백은 이름 이니셜을 기준으로 사용자를 위한 아바타를 생성하는 것입니다. 이 튜토리얼에서는 Go에서 이러한 아바타를 생성하고 chi 라우터를 사용하여 HTTP를 통해 서비스하는 방법에 대해 알아보겠습니다.

## 세우다

계속하려면 최신 버전의 Go(버전 1.14 이상)가 설치되어 있어야 합니다.

> 참고: 아래 셸 명령은 Linux/macOS용입니다. 운영 체제와 동일한 명령이 다를 경우 언제든지 사용하십시오.

먼저 프로젝트의 폴더를 생성하고 새 Go 모듈을 초기화합니다.

```shell
$ mkdir go-avatars && cd go-avatars
$ go mod init gitlab.com/idoko/go-avatars
```

그런 다음 작업 디렉토리(예: `go-avatars`)에서 아래 명령을 실행하여 필요한 종속성을 추가합니다.

```shell
$ go get golang.org/x/image github.com/golang/freetype github.com/go-chi/chi
```

의존성은 바둑에서 이미지로 작업하기 위한 방법을 제공하는 이미지, 글꼴로 작업하기 위한 프리타입, HTTP를 통해 생성된 아바타를 서비스하기 위한 chi 라우터 등으로 구성된다.

## 이미지 캔버스 만들기

아바타의 배경을 준비하기 위해 작업 디렉토리에 main.go 파일을 만들고 main과 create 아바타라는 두 가지 기능을 구현한다. main.go 파일을 열고 아래에 코드 블록을 추가합니다.

```cpp
package main
import (
    "fmt"
    "image"
    "image/color"
    "image/draw"
    "image/png"
    "log"
    "os"
    "time"
)
func main() {
    initials := "LR"
    size := 200
    avatar, err := createAvatar(size, initials)
    if err != nil {
        log.Fatal(err)
    }
    filename := fmt.Sprintf("out-%d.png", time.Now().Unix())
    file, err := os.Create(filename)
    if err != nil {
        log.Fatal(err)
    }
    png.Encode(file, avatar)
}
func createAvatar(size int, initials string) (*image.RGBA, error) {
    width, height := size, size
    bgColor, err := hexToRGBA("#764abc")
    if err != nil {
        log.Fatal(err)
    }
    background := image.NewRGBA(image.Rect(0, 0, width, height))
    draw.Draw(background, background.Bounds(), &image.Uniform{C: bgColor},
        image.Point{}, draw.Src)
    //drawText(background, initials)
    return background, err
}
```

Create Avatar 함수는 함수에 전달된 크기 매개변수와 너비와 높이가 같은 정사각형 캔버스를 생성합니다. HEX 색상 코드를 RGBA 등가물로 변환한 후 캔버스 위에 균일하게 RGBA를 칠한다. 다음으로 색상 간 변환을 위한 hexToRGBA 기능을 구현하겠습니다.

## HEX 코드를 RGBA 색상으로 변환

main.go 파일에 hexToRGBA 기능의 구현을 추가합니다.

```undefined
func hexToRGBA(hex string) (color.RGBA, error) {
    var (
        rgba color.RGBA
        err  error
        errInvalidFormat = fmt.Errorf("invalid")
    )
    rgba.A = 0xff
    if hex[0] != '#' {
        return rgba, errInvalidFormat
    }
    hexToByte := func(b byte) byte {
        switch {
        case b >= '0' && b <= '9':
            return b - '0'
        case b >= 'a' && b <= 'f':
            return b - 'a' + 10
        case b >= 'A' && b <= 'F':
            return b - 'A' + 10
        }
        err = errInvalidFormat
        return 0
    }
    switch len(hex) {
    case 7:
        rgba.R = hexToByte(hex[1])<<4 + hexToByte(hex[2])
        rgba.G = hexToByte(hex[3])<<4 + hexToByte(hex[4])
        rgba.B = hexToByte(hex[5])<<4 + hexToByte(hex[6])
    case 4:
        rgba.R = hexToByte(hex[1]) * 17
        rgba.G = hexToByte(hex[2]) * 17
        rgba.B = hexToByte(hex[3]) * 17
    default:
        err = errInvalidFormat
    }
    return rgba, err
}
```

구현은 gox 라이브러리의 코드를 사용하며 HEX 색상 코드를 바이트 시리즈로 변환하는 방식으로 작동합니다. 16진수 삼중수소의 경우(즉, 6자리, 3바이트 16진수 숫자) 각 바이트의 첫 번째 자릿수에 16을 곱하고, 같은 바이트의 두 번째 자릿수에 추가하여 RGB를 구한다.

16진수 문자열이 3자리(예: #FFF)이면 RGB 등가를 얻기 위해 각 문자열에 17만 곱하면 된다.

이 Wikipedia 페이지에서 웹 색상 간의 변환에 대해 자세히 알아볼 수 있습니다.

go run./main.go와 함께 main.go 파일을 실행하여 프로젝트 디렉토리의 아래 파일(out-xxxxxxxxxx.png)과 같은 균일하게 색상의 파일을 생성합니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/Blankcanvasgeneratedbyrunningthemaingofile.png?resize=200%2C200&ssl=1)

## 배경에 텍스트 그리기

다음으로 빈 캔버스에 이니셜을 쓰는 `drawText` 기능을 구현하겠습니다. 편집기에서 `main.go` 파일을 열고 아래 코드를 추가합니다.

```undefined
func drawText(canvas *image.RGBA, text string) error {
    var (
        fgColor  image.Image
        fontFace *truetype.Font
        err      error
        fontSize = 128.0
    )
    fgColor = image.White
    fontFace, err = freetype.ParseFont(goregular.TTF)
    fontDrawer := &font.Drawer{
        Dst: canvas,
        Src: fgColor,
        Face: truetype.NewFace(fontFace, &truetype.Options{
            Size:    fontSize,
            Hinting: font.HintingFull,
        }),
    }
    textBounds, _ := fontDrawer.BoundString(text)
    xPosition := (fixed.I(canvas.Rect.Max.X) - fontDrawer.MeasureString(text)) / 2
    textHeight := textBounds.Max.Y - textBounds.Min.Y
    yPosition := fixed.I((canvas.Rect.Max.Y)-textHeight.Ceil())/2 + fixed.I(textHeight.Ceil())
    fontDrawer.Dot = fixed.Point26_6{
        X: xPosition,
        Y: yPosition,
    }
    fontDrawer.DrawString(text)
    return err
}
```

함수는 `이미지를 수신합니다.RGBA 포인터가 전달된 동일한 이미지를 수정하고 있음을 보장하는 매개 변수입니다. Go 표준 라이브러리에 있는 고글 글꼴을 사용하여 렌더링합니다.

텍스트 자체를 그리면 `font`가 사용됩니다.이미지에 쓰는 기본 작업을 하는 드로어의 구조. 아바타 캔버스를 서랍장(Dst)으로, 완전한 흰색 이미지를 원본(Src) 이미지로 전달하면 텍스트가 흰색으로 렌더링됩니다.

우리는 또한 텍스트의 수평 시작점(`xPosition`)을 계산한다. 먼저 텍스트의 길이를 측정하고 전체 캔버스 폭에서 길이를 뺀 다음 결과를 2로 나눈다(왼쪽과 오른쪽 여백을 계산한다).

마찬가지로 수직 위치(yPosition)는 먼저 캔버스 높이와 텍스트 높이의 차이를 절반으로 줄이고 텍스트 높이를 다시 추가하여 텍스트를 제자리에 밀어넣습니다.

create 아바타 함수에서 drawText(배경, 이니셜) 줄의 압축을 풀어야 합니다.

또한 편집자가 종속성을 가져오지 않은 경우 종속성을 수동으로 가져와야 할 수도 있습니다.

go run./main.go로 main.go 파일을 실행하면 생성된 이미지가 작업 디렉토리에 표시됩니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/avatargeneratedwithinitials.png?resize=200%2C200&ssl=1)

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/LRavatar.png?resize=200%2C200&ssl=1)

## chi 라우터를 사용하여 생성된 이미지 렌더링

브라우저에서 아바타를 사용할 수 있도록 `메인` 기능을 수정하고 생성된 이미지를 HTTP로 렌더링하도록 할 것이다. 기존 `메인` 기능을 아래 코드로 바꾸십시오.

```undefined
router := chi.NewRouter()
        router.Use(middleware.Logger)
        router.Get("/avatar", func(w http.ResponseWriter, r *http.Request) {
                initials := r.FormValue("initials")
                size, err := strconv.Atoi(r.FormValue("size"))
                if err != nil {
                        size = 200
                }
                avatar, err := createAvatar(size, initials)
                if err != nil {
                        log.Fatal(err)
                }
                w.Header().Set("Content-Type", "image/png")
                png.Encode(w, avatar)
        })
        http.ListenAndServe(":3000", router)
```

위의 코드 조각은 먼저 HTTP 요청을 처리하기 위해 ki를 사용하여 `/avata` 경로를 설정합니다. 그런 다음 이 함수는 쿼리 매개 변수에서 이니셜과 크기를 가져와 아바타를 생성하는 데 사용합니다.

`png` 이후로.인코딩()은 `io`를 사용합니다.writer` 인터페이스(http에 의해 구현됨).ResponseWriter), 우리의 새로운 `main` 기능은 브라우저가 렌더링할 수 있도록 생성된 이미지를 직접 서비스할 수 있다.

터미널에서 `go run./main.go`를 실행하여 HTTP 서버를 시작하고 http://localhost:3000/avata?initials=PCM을 방문하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/Firefoxshowingthegeneratedavatarwithinitials.png?resize=1366%2C768&ssl=1)

## 결론

저는 역동적인 아바타가 정적 디폴트보다 미적으로 더 만족스럽다고 생각합니다. 이 글에서 우리는 골랑에서 그것을 구현하는 방법을 보았습니다. 이니셜에 의해 결정된 배경색을 사용하여 한 단계 더 나아갈 수 있습니다. 한 가지 방법은 16진수 색상 문자열의 슬라이스 또는 배열을 만들고 이니셜의 해시를 기반으로 슬라이스에서 색상을 선택하는 것입니다.

튜토리얼의 전체 소스 코드는 GitLab에 있습니다. 여기에 당신의 코드에서 사용할 수 있는 코드를 바탕으로 미니 패키지를 작성했습니다.