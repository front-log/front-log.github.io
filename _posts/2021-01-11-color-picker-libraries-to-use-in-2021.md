---
layout: post
title: "2021년에 사용할 색 선택기 라이브러리"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/01/jscolorpickers.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/jscolorpickers.png?fit=730%2C487&ssl=1)

웹상에서 색채의 중요성은 부인할 수 없다. 브랜드는 선호하는 색상 체계뿐만 아니라 색상의 도움으로 식별된다. 다행히 개발자들은 자바스크립트에 의해 수많은 인기 있는 컬러 라이브러리 때문에 여러 가지 옵션을 제공받는다. JavaScript의 색상 선택 라이브러리는 개발자가 프로젝트에서 사용할 수 있는 광범위한 색상 옵션 또는 색상 코드에 대한 액세스를 제공하는 것으로 알려져 있습니다.

JavaScript에서 지정된 색 선택기 라이브러리의 도움으로 다양한 RGB(빨간색, 녹색 및 파란색) 색조의 주파수 값을 사용하여 원하는 주파수 또는 색 구성표를 얻을 수 있습니다.

이 기사에서는 샘플 사용량을 보여 주는 동안 기능에 대해 알아보기 위해 JavaScript의 여러 JavaScript 색상 선택기 라이브러리 목록을 살펴봅니다. 결국, 우리는 모든 라이브러리의 성능과 페이지 로드 시간에 대한 상대적 영향을 기반으로 모든 라이브러리의 성능을 비교할 것이다.

## 부트스트랩 색상 선택기

부트스트랩 컬러 피커(Bootboot Colorpicker)는 부트스트랩용 컬러 피커 플러그인의 선도적인 모듈형 형태이다. 지정된 플러그인을 사용하면 여러 색 중에서 선택할 수 있습니다. 최종 사용자가 색상을 선택할 수 있는 모든 편집기 기능 또는 제품 변형 시나리오에서 쉽게 사용할 수 있습니다.

최신 버전을 얻고자 하는 경우 다음과 같은 여러 가지 방법으로 동일한 버전을 확인할 수 있습니다.

- 각 릴리스에서 ZIP 파일 다운로드
- GIT의 도움을 받아 복제
- NPM의 도움을 받아 설치
- Composer의 도움을 받아 설치

v2.x 설명서 및 v3.x 설명서에서도 부트스트랩에 사용할 수 있는 다양한 색 선택기 버전이 있습니다. 실행 중인 예제를 살펴보겠습니다.

```xml
<!DOCTYPE html>
<html>
<head>
    <title>Bootstrap Color Picker</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-colorpicker/3.2.0/css/bootstrap-colorpicker.css" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-colorpicker/3.2.0/js/bootstrap-colorpicker.js"></script>
    <style type="text/css">
        body {
        width: 80%;
        height:80%;
        margin:auto;
        }
        #main {
        height:400px;
        }
        #color {
        margin-left: 10%;
        width:50%;
        }
  </style>
</head>
<body>
    <div id="main" class="container">
        <h1>Bootstrap Color Picker</h1>
        <input id="color" type="text" value="#269faf" class="form-control" />
    </div>
    <script type="text/javascript">
      $(function () {
        $('#color')
        .colorpicker({})
        .on('colorpickerChange', function (e) { //change the bacground color of the main when the color changes  
            new_color = e.color.toString()
            $('#main').css('background-color', new_color)
        })     
    });
</script>
</body>
</html>
```

## 반응 색상

리액트 컬러는 포토샵, 크롬, 스케치, 깃허브, 재료 디자인, 트위터 등의 도구에서 나온 컬러 픽커 모음이다. 고를 수 있는 컬러 피커가 13개나 된다. 또한 자바스크립트에서 주어진 색상 선택기를 사용하여, 웹 디자이너들은 기존의 빌딩 블록 구성요소를 사용함으로써 자신만의 색상 범위를 만들기를 기대할 수 있다.

NPM의 도움을 받아 리액트 컬러를 설치하면 설치가 가능합니다. 동시에, 색상 구성요소는 구성요소 상단의 반응 색상으로부터 일부 색상 선택기를 가져온 다음 렌더 함수에서 동일한 색상 선택기를 사용하는 데 도움을 받아 지정된 라이브러리에 포함될 수 있습니다. React Color의 구성 요소 API 중 일부는 다음과 같습니다.

- `색상`: 색상 선택기에서 활성 상태로 남아 있는 색상을 제어하는 데 사용됩니다. 지정된 구성 요소를 사용하여 특정 색상으로 색상 선택기를 초기화하거나 상위 구성 요소의 상태와 동기화되도록 유지할 수 있습니다.
- `변경 시`: 색상이 변경될 때마다 통화하기 위한 기능을 전달해야 합니다. 그런 다음 상위 구성요소의 상태에 색상을 저장하거나 다른 중요한 변환을 수행하는 경우에도 동일한 색상을 사용할 수 있습니다.

가져오도록 선택한 색 선택기 구성 요소에 따라 사용할 수 있는 다른 몇 가지 소품이 있습니다.

```js
import logo from './logo.svg';
import React from 'react';
import './App.css';
import { ChromePicker } from 'react-color'

class App extends React.Component {
  state = {
    background: 'rgb(0,0,0,1)',
    color:""
  };
  changeHandler = (colors) => {
    let col = 'rgb('+colors.rgb.r+','+colors.rgb.g+','+colors.rgb.b+','+colors.rgb.a+')'
    this.setState({ background: col, color:colors.rgb });
  };

  render() {
    return (
      <div id="main" style={backgroundColor: this.state.background}>
      <h1>React-Color Library</h1>
      <ChromePicker
        className="picker"
        color={ this.state.color }
        onChange={ this.changeHandler }
      />
      </div>
    );
  }
}
export default App;
```

## 피커

Picker는 JavaScript를 위한 단순하고 평면적이며 응답성이 뛰어나며 해킹이 가능하며 다중 테마 색상 선택기 역할을 합니다. 이 색 선택기의 도움을 받아 종속성 또는 jQuery를 사용할 필요가 없습니다. 또한, 주어진 색상 선택기는 사용 가능한 모든 CSS 프레임워크와 매우 호환된다.

Pickr은 웹 사이트 또는 앱에 매우 우아하고 터치 가능하며 사용자 지정이 가능한 컬러 선택기를 만드는 데 도움이 되는 것으로 알려져 있습니다. 주어진 색상 선택기는 RGB, HEX, HSV, HSL, CMYK 색상 코드를 지원할 수 있다. 동시에, 피커 색상 선택기는 기본 색상 코드 또는 값(HSVa)을 각각의 RGBa, HSLa, CMYK 및 HEX 값으로 변환할 수 있는 특정 기능을 제공하는 것으로 알려져 있다. 지정된 색상 선택기는 node.js와 브라우저 모두에 대한 지원을 제공하는 것으로 알려져 있습니다.

JavaScript 라이브러리에서 동일한 색상 선택기를 사용하여 색상을 추가하려면 다음 단계를 수행하십시오.

- NPM을 사용하여 Picker 설치
- 페이지에 Pickr의 JavaScript 추가
- 지정된 페이지에 특정 테마 CSS 추가
- 색상 선택기를 배치하기 위한 잘 정의된 컨테이너 요소 만들기
- 기본 색상 선택기를 생성하기 위한 색상 선택기 초기화
- 트리거 시 색 선택기의 지정된 위치 사용자 지정

아래에 제시된 예를 참조하십시오.

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>Pickr library</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/@simonwep/pickr/dist/themes/classic.min.css"
    />
    <!-- 'classic' theme -->
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/@simonwep/pickr/dist/themes/monolith.min.css"
    />
    <!-- 'monolith' theme -->
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/@simonwep/pickr/dist/themes/nano.min.css"
    />
    <!-- 'nano' theme -->
    <script src="https://cdn.jsdelivr.net/npm/@simonwep/pickr@1.8.0/dist/pickr.min.js"></script>
    <style type="text/css">
      body {
        width: 80%;
        height: 80%;
        margin: auto;
      }
      #main {
        height: 400px;
      }
      .pickr {
        margin-top: -3%;
        margin-left: 30%;
      }
    </style>
  </head>
  <body>
    <div id="main">
      <h1>Pickr library</h1>
      <p>Select Button</p>
      <div id="color_input"></div>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        const pickr = Pickr.create({
          el: "#color_input",
          theme: "monolith",
          components: {
            preview: true,
            opacity: true,
            hue: true,
            // Input / output Options
            interaction: {
              hex: true,
              rgba: true,
              hsla: true,
              hsva: true,
              cmyk: true,
              input: true,
              clear: true,
              save: true
            }
          }
        });
        //change the color of the main div when color changes
        pickr.on("change", function (e) {
          $("#main").css("backgroundColor", e.toRGBA());
        });
      });
    </script>
  </body>
</html>
```

이러한 인기 있는 색상 선택기 외에도 개발자를 위한 몇 가지 추가 옵션은 다음과 같습니다.

## 컬러 피커

ColorPicer는 매우 직관적이고 가볍고 호환되는 자바스크립트 프레임워크로서 독립적인 색상 선택 도구 역할을 한다. 색상 차이, 레이어 믹스, 콘트라스트와 같은 계산뿐만 아니라 색상 변환을 커버할 수 있는 여러 기능이 있습니다. 컬러 피커 도구는 불량한 색 공간도 완벽하게 지원할 수 있습니다. RGB, HSV, HSL, CMY, CMYK, HEX, XYZ 등과 같은 요구 사항이 무엇이든 간에 이 툴은 원하는 결과를 사용자에게 제공할 수 있습니다. 예를 들어 보겠습니다.

```xml
<!DOCTYPE html>
<html>
<head>
    <title>colorPicker library</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <style type="text/css">
    body {
    width: 80%;
    height:80%;
    margin:auto;
    }
    #main {
    height:400px;
    }
    #color {
      margin-left: 30%;
    }
  </style>
</head>
<body>
    <div id="main">
    <h1>colorPicker library</h1>
    <input  id = "color" type="text">
    </div>

    <script type="text/javascript" src="../colors.js"></script>
    <script type="text/javascript" src="../colorPicker.data.js"></script>
    <script type="text/javascript" src="../colorPicker.js"></script>
    <script type="text/javascript" src="jsColor.js"></script>

    <script  type="text/javascript">
     $(document).ready(function () {

        //find the page loading time
        var loadTime = window.performance.timing.domContentLoadedEventEnd- window.performance.timing.navigationStart
        console.log("Load Time:",loadTime)
        var colors = jsColorPicker('#color', {
            size: 2,
            readOnly: false,
            init: function(elm, colors) {},
            renderCallback: function(colors, mode){ //change the background of the main div when the color is selected
                document.getElementById("main").style.backgroundColor = "#"+colors.HEX
                document.getElementById("color").value = "#"+colors.HEX

            }

        });
    });

</script>
</body>
</html>
```

## 진화색 피커

evol-color picker는 인라인으로 사용할 수 있는 색상 선택 기능을 위한 적응형 JavaScript 라이브러리이다. evol-color picker는 투명 색상 지원, 색상 이력 추적 및 색상 팔레트 선택에 관한 한 오른쪽 버튼을 누르는 것으로 알려져 있다.

지정된 색 선택기는 완전한 UI(사용자 인터페이스) 위젯이므로 환경설정에 맞게 쉽게 조정할 수 있는 구성 및 테마를 사용할 수도 있습니다. 아래 예제를 살펴보십시오.

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>Evol Color Picker library</title>
    <script
      src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"
      type="text/javascript"
      charset="utf-8"
    ></script>
    <script
      src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js"
      type="text/javascript"
      charset="utf-8"
    ></script>
    <script src="https://cdn.jsdelivr.net/npm/evol-colorpicker@3.4.2/js/evol-colorpicker.js"></script>
    <link
      rel="stylesheet"
      type="text/css"
      href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/themes/ui-lightness/jquery-ui.css"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/evol-colorpicker@3.4.2/css/evol-colorpicker.css"
    />
    <style type="text/css">
      body {
        width: 80%;
        height: 80%;
        margin: auto;
      }
      #main {
        height: 400px;
      }
      .evo-cp-wrap {
        margin-left: 30%;
      }
      div.evo-pointer.evo-colorind {
        border-width: 2px;
      }
    </style>
  </head>
  <body>
    <div id="main">
      <h1>Evol Color Picker library</h1>
      <input id="color" type="text" />
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        $("#color").colorpicker({
          color: "#ffffff", //initial color
          defaultPalette: "theme", //theme or web
          transparentColor: true //user can select transparent colors
        });
        //change the main background of the div when the color is selected
        $("#color").on("change.color", function (event, color) {
          $("#main").css("background-color", color);
        });
      });
    </script>
  </body>
</html>
```

## JS컬러

JSColor는 설계자와 개발자에게 주어진 구성요소의 설치와 사용 종료 동안 최고의 경험을 제공하는 것을 목표로 하는 웹 기반 색상 선택기 중 하나이다. 전체적인 사용 편의성과 단순성으로 인해 주어진 색상 선택기가 사용자들 사이에서 매우 선호됩니다. JSColor는 Chrome, Safari, Internet Explorer 7 이상, Mozilla, Opera 등을 포함한 모든 브라우저에 지원을 제공하는 것으로 알려져 있다. 실행 중인 예제를 살펴보겠습니다.

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>Jscolor library</title>
    <script
      src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"
      type="text/javascript"
      charset="utf-8"
    ></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jscolor/2.4.0/jscolor.js"></script>
    <style type="text/css">
      body {
        width: 80%;
        height: 80%;
        margin: auto;
      }
      #main {
        height: 400px;
      }
      p {
        margin-left: 30%;
      }
    </style>
  </head>
  <body>
    <div id="main">
      <h1>Jscolor library</h1>
      <p>
        Color: <input id="color" value="rgba(255,160,0,0.5)" data-jscolor="" />
      </p>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        //change the main background of the div when the color is changed
        $("#color").on("change", function (e) {
          color = $(this).val();
          $("#main").css("backgroundColor", color);
        });
      });
    </script>
  </body>
</html>
```

## 파르비타스틱

Farbtastic은 웹 프로젝트에 단일 또는 심지어 여러 색 선택 위젯을 추가할 수 있는 특수 색 선택 플러그인을 제공하는 데 도움이 됩니다. 이는 JavaScript의 도움으로 달성됩니다. 각 위젯을 텍스트 필드와 같은 기존 요소에 연결할 때 일부 색상을 선택하면 요소 값이 자동으로 업데이트됩니다. 실제로 보겠습니다.

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>Farbtastic Color Picker library</title>
    <script
      src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"
      type="text/javascript"
      charset="utf-8"
    ></script>
    <script src="https://cdn.jsdelivr.net/npm/farbstastic@1.3.0/farbtastic.js"></script>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/farbstastic@1.3.0/farbtastic.css"
    />
    <style type="text/css">
      body {
        width: 80%;
        height: 80%;
        margin: auto;
      }
      #color {
        margin-left: 30%;
      }
      #colorpicker {
        margin-left: 30%;
      }
    </style>
  </head>
  <body>
    <div id="main">
      <h1>Farbtastic Color Picker library</h1>
      <input id="color" type="text" />
      <div id="colorpicker"></div>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        var obj = $.farbtastic("#colorpicker", "#color"); //placeholder, callback
        obj.linkTo(function (event) {
          //linking a callback function that changes the background color of the main div
          $("#main").css("backgroundColor", event);
          $("#color").val(event);
        });
      });
    </script>
  </body>
</html>
```

## 색조

colorjoe는 확장성이 뛰어난 컬러 선택기이다. 이 제품은 즉각적인 색채 고르기 편리함을 제공합니다. 이 도구를 사용하면 사용 가능한 색상 선택 영역을 선택하고 클릭하면 RGB뿐만 아니라 다른 색상 코드나 값도 얻을 수 있습니다. 컬러 조에 의해 제공되는 확장성은 매우 효과적입니다. 영원한 이미지에 의존하지 않고 CSS에 기반한 것으로 알려져 있기 때문이다. 따라서 CSS를 활용하여 Colorjoe의 크기를 수정할 수 있습니다.

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>ColorJoe library</title>
    <script
      src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"
      type="text/javascript"
      charset="utf-8"
    ></script>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/colorjoe@4.1.1/css/colorjoe.css"
    />
    <script src="https://cdn.jsdelivr.net/npm/colorjoe@4.1.1/dist/colorjoe.js"></script>
    <style type="text/css">
      body {
        width: 80%;
        height: 80%;
        margin: auto;
      }
      #main {
        height: 600px;
      }
      #color {
        margin-left: 30%;
      }
    </style>
  </head>
  <body>
    <div id="main">
      <h1>ColorJoe library</h1>
      <div id="colorpicker"></div>
    </div>
    <script type="text/javascript">
      $(document).ready(function () {
        const joe = colorjoe.rgb("colorpicker", "red", [
          "hex",
          ["fields", { space: "RGB", limit: 255, fix: 0 }],
          "currentColor"
        ]);
        //change the background color of the main div when color changes
        joe
          .on("change", function (c) {
            $("#main").css("backgroundColor", c.css());
          })
          .update();
      });
    </script>
  </body>
</html>
```

## 성능시험

위의 색 선택 도구 라이브러리의 페이지 로드 시간은 아래 표에 나와 있습니다. 평균 10개의 테스트를 보여줍니다.

## 원천

https://github.com/casesandberg/react-color

https://github.com/Simonwep/pickr

https://github.com/itsjavi/bootstrap-colorpicker

https://github.com/PitPik/colorPicker

https://github.com/evoluteur/colorpicker

https://github.com/mattfarina/farbtastic

https://github.com/bebraw/colorjoe