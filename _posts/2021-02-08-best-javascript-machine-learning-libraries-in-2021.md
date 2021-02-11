---
layout: post
title: "2021년 최고의 JavaScript 머신 러닝 라이브러리"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2021/02/machine-learning-libraries-javascript.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/02/machine-learning-libraries-javascript.png?fit=730%2C487&ssl=1)

JavaScript는 특별한 소개가 필요하지 않습니다. 웹 개발자들 사이에서 가장 인기 있는 크로스 플랫폼 언어 중 하나입니다.

그리고 어떤 사람들은 그것을 프런트엔드 개발용 언어라고 생각하지만, 자바스크립트는 오늘날 만능 프로그래밍 언어로 작용하고 있으며, 그것의 가능성은 무궁무진하다.

머신 러닝 프로젝트에서 사용할 수 있는 최고의 JavaScript 라이브러리를 찾고 계십니까? 여기 있습니다.

## 시냅스

Synaptic은 Node.js 또는 브라우저와 함께 사용할 수 있는 MIT에 의해 만들어진 잘 알려진 자바스크립트 신경망 라이브러리이다. 이 라이브러리의 한 가지 중요한 특징은 아키텍처가 없는 알고리듬과 사전 제작된 구조로 인해 1차 또는 2차 신경망 아키텍처를 구축하고 훈련할 수 있다는 것이다.

또한 네트워크를 독립 실행형 기능으로 가져오거나 내보내 다른 네트워크와 연결하거나 게이트 연결로 연결할 수 있습니다. 시냅틱은 Hopfield 네트워크, 상태 머신, 다층 퍼셉트론, LSTM(장기 단기 메모리 네트워크) 등과 같은 몇 가지 흥미로운 내장 아키텍처를 포함한다. 그리고 시냅틱은 오픈 소스 라이브러리이기 때문에 누구나 무료로 기여하거나 이용할 수 있다.

또한 시냅틱은 임베디드 리버 문법 테스트, XOR 해결, 산만한 시퀀스 호출 작업 완료 및 내장 훈련 작업과 같은 테스트로 특정 신경망을 훈련할 수 있는 강사를 포함한다. 또한 다양한 신경망 아키텍처의 성능을 비교하는 데에도 도움이 된다.

다음 예제는 Synaptic 라이브러리를 사용하여 신경망을 만드는 방법을 보여줍니다.

```js
// creating a network
const { Layer, Network } = window.synaptic;

let innerLayer = new Layer(2);
let hiddenLayer = new Layer(3);
let outerLayer = new Layer(1);

innerLayer.project(hiddenLayer);
hiddenLayer.project(outerLayer);

const Network = new Network({
input: innerLayer,
hidden: [hiddenLayer],
output: outerLayer
});
```

이제, 여러분은 위에서 만들어진 신경망의 테스트 과정에 대해 연구할 수 있습니다.

## 브레인.js

Brain.js는 머신 러닝과 신경망에 사용되는 자바스크립트 기반의 고속 실행 라이브러리이다. 브라우저 또는 Node.js에서 사용할 수 있습니다. 브레인하고.JS, 다양한 유형의 네트워크를 다양한 작업에 사용할 수 있습니다. 그것은 장기 단기 메모리 NN, 반복 NN, 피드포워드 NN과 같은 다양한 신경망에 대한 지원을 제공한다.

Brain.js는 계산을 위한 GPU의 사용 때문에 빠른 처리 라이브러리이다. GPU를 사용할 수 없는 경우에도 순수 JS로 되돌아가서 계속 처리합니다. Brain.js는 여러 신경망 구현을 제공하며 Node.js와 함께 서버 측에서 이러한 신경망을 구축하고 실행하도록 장려한다.

이 도서관의 또 다른 좋은 점은 신경망을 사용하기 위해 엄격하게 익숙할 필요가 없다는 것입니다. 웹 사이트를 이러한 네트워크 모델과 통합하려면 해당 웹 사이트를 함수로 구현하거나 JSON 형식을 사용해야 합니다.

Brain.js는 고급 언어를 사용하여 간단한 신경망을 빠르게 만드는 데 사용될 수 있다. 몇 줄의 코드와 우수한 데이터셋만으로 흥미로운 기능을 구축할 수 있습니다. 더군다나, 브레인JS는 클라이언트 측 javascript에서 실행할 수 있는 기능을 제공합니다.

다음 예는 약 50,000개의 필기 숫자 샘플이 있는 MNIST 데이터 세트를 사용하여 NN을 생성하는 방법에 관한 것이다.

```js
// creating a network
const context = require('brain');


const getData = function(content) {
  const rows = content.toString().split('\n');


  const values = [];
    for (covaluesnst i = 0; i < rows.length; i++) {
      const inputVal = rows[i].split(',').map(Number);


      const outputVal = Array.apply(null, Array(10)).map(Number.prototype.valueOf, 0);
      outputVal[inputVal.shift()] = 1;


      values.push({
            input: inputVal,
            output: outputVal
        });
    }
    return values;
};
```

위의 예에 의해 생성된 신경망의 테스트 프로세스에 대해 작업할 수 있습니다.

## TensorFlow.js

텐서플로우.js는 구글 브레인 수집에 의해 만들어진 오픈 소스 자바스크립트 라이브러리이다. 완전하고 유연한 다양한 툴로 하드웨어 가속화를 촉진합니다. 딥 러닝 레이어와 종합적인 선형 대수학으로 인해, 이 라이브러리는 머신 러닝을 기반으로 하는 모든 자바스크립트 프로젝트의 핵심이 되었다.

TensorFlow.js는 사용자가 브라우저의 도움을 받아 신경망을 훈련하거나 기계 학습 구성 요소를 웹으로 불러오는 동안 추론 모드에서 사전 훈련된 모델을 실행할 수 있도록 한다. 현재 사용 가능한 기본 TensorFlow 모델을 실행하거나 일부 파이썬 모델로 추가로 변환할 수도 있습니다.

또한 TensorFlow.js를 사용하면 Javascript의 낮은 수준의 선형 대수학을 사용하여 처음부터 모델을 매우 쉽게 구축할 수 있다.

TensorFlow.js는 또한 일부 기존 머신 러닝 모델을 포함한다. 사용자 자신의 데이터를 재교육하는 데 사용할 수 있습니다. 또한 사용하는 언어, 사내, 브라우저 또는 클라우드에 관계없이 기기를 포함한 모든 곳에 머신 러닝 모델을 배포할 수 있는 기능도 제공합니다.

다음 Machine Learning 기반 JavaScript 프로젝트에 TensorFlow.js를 사용하는 것을 고려할 수 있습니다. 컴퓨팅 그래프 시각화를 개선하는 동시에 빈번한 새 릴리스, 빠른 업데이트, 원활한 성능 등의 몇 가지 이점을 제공합니다.

게다가 TensorFlow.js는 매우 병렬적이며 ASIC, GPU 등과 같은 수많은 백엔드 소프트웨어와 함께 사용하도록 구축되었다.

또한 TensorFlow.js는 전체 경험을 위한 TensorFlow Extended, 모바일 장치를 위한 TensorFlow Lite, Rust 바인딩 등의 많은 하위 버전을 포함하는 TensorFlow 버전이다.

다음 코드는 간섭을 수행하기 위해 TensorFlow.js를 사용하여 간단한 신경망을 만드는 방법을 설명한다. 이 모델에는 NN을 처리하려면 입력 값 하나와 출력 값이 필요합니다.

여기서는 `tf.sequential` 메서드를 호출하여 새로운 모델 인스턴스를 시도했습니다. 그것으로부터, 우리는 새로운 순차적 모델을 얻을 수 있습니다. 순차적 모델은 한 레이어의 출력이 다른 레이어에 대한 입력으로 사용되는 모델이라고 할 수 있다. 즉, 모델의 토폴로지는 분기나 건너뛰기 없이 레이어의 원시 `스택`이다.

그런 다음 조밀한 레이어를 생성하는 model.add 메소드를 호출하여 첫 번째 레이어를 추가할 수 있다. 다음 예에서는 입력과 출력이 각각 하나씩 있는 밀도 레이어를 신경망에 추가했다.

```undefined
// Defining a  machine learning sequential model
const modelObj = tf.sequential();

// Add a single layer
modelObj.add(tf.layers.dense({units: 1, inputShape: [1]}));
```

그런 다음 모델의 손실 및 최적화 함수를 포함할 수 있습니다.

```coffeescript
// Specify loss and optimizer for model
modelObj.compile({loss: 'meanSquaredError', optimizer: 'sgd'});
```

모델 인스턴스의 컴파일 메서드에 구성 개체를 전달하면 됩니다. 구성 개체에는 다음과 같은 두 가지 속성 `손실`과 `최적화기`가 포함되어 있습니다.

## 마음

자바스크립트에서 스크립트로 작성된 Mind는 신경 네트워킹을 위한 절대적으로 유연한 라이브러리이며 더 나은 예측을 위해 브라우저와 Node.js를 다룬다. Mind의 주요 기능 중 하나는 개발자가 네트워크 토폴로지를 사용자 지정할 수 있도록 하면서 매트릭스 구현을 사용하여 교육 데이터를 처리하는 것입니다.

이 라이브러리는 다른 라이브러리보다 플러그인이 빠르고 쉽게 다운로드 및 업로드할 수 있기 때문에 매우 편리합니다. 사전 훈련된 네트워크를 쉽게 구성할 수도 있습니다.

신경 네트워킹에서 마인드를 사용하는 방법에 대한 아래의 예를 참조하십시오.

```js
let category = [
  'Action',
  'Adventure',
  'Animation',
  'Comedy',
];

let input = [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ];

movie.category.forEach(function(category) {
  let index = category.indexOf(category);
  if (index > -1) input[index] = 1;
});

//  train the network using the ratings provided by a user using Mind()
let mind = Mind()
  .learn([
    { input: input, output: [ ratings / 5 ] }
  ]);
```

## ConvNetJS

ConvNetJS는 딥 러닝 모델을 교육하고 신경망과 함께 작동하도록 특별히 설계된 자바스크립트 라이브러리이다. 이 라이브러리의 가장 중요한 특징은 브라우저에 전적으로 의존하기 때문에 GPU, 컴파일러와 같은 다른 특수 소프트웨어는 전혀 필요하지 않다는 것이다. ConvNetJS는 Node.js도 지원합니다.

ConvNetJS는 완전히 연결된 계층과 비선형성을 가진 공통 신경망 모듈로 구성된다. 이 라이브러리는 일부 공통 네트워크 모듈에 대한 지원을 제공하면서 간단한 JavaScript를 사용하여 신경 네트워크를 공식화하고 해결할 수 있는 기능을 가지고 있다.

또한 신경망 지정, 문제 분류 및 후회, 이미지 처리를 위한 컨볼루션 네트워크, Deep Q Learning을 기반으로 한 실험 강화 학습 모듈 및 아직 실험 수준에 있는 보충 학습 모듈을 제공한다.

신경망에서 ConvNet.js를 구현하는 방법을 이해하려면 다음 간단한 코드 예를 참조하십시오.

```cpp
// create a network
const layer_defs = [];

const network = new convnetjs.Net();
net.makeLayers(layer_defs);
```

경험이 풍부한 개발자 사이에서 인기가 있는 ConvNetJS는 지속적으로 업데이트되는 라이브러리이다. 또 다른 훌륭한 기능은 Node.js에서 이용할 수 있다는 것이다.

## ML5.js

ML5.js는 Node.js와 브라우저로 머신 러닝을 위한 완전 패키지형 종합 오픈 소스 라이브러리이다. Node.js와 함께 사용할 때 자신의 종속성을 추가할 수 있습니다.

TensorFlow를 기반으로 구축되었으며 외부 의존성이 없습니다. 텐서플로우와 유사하게, 이 라이브러리는 머신 러닝 알고리듬에 대한 메모리 관리 외에도 GPU에 의해 가속되는 수학적 연산을 처리할 수 있다.

ML5.js는 인간의 신체 언어와 피치 감지, 이미지 사용자 정의, 텍스트 생성, 영어로 언어 관계 찾기, 음악 트랙 구성 등과 같은 다양한 목적으로 사용할 수 있도록 브라우저에서 사전 훈련된 많은 머신 러닝 알고리듬에 쉽게 접근할 수 있게 한다.

이 도서관은 기계 학습에 대한 집중적인 이해와 더불어 윤리적 컴퓨팅, 데이터 수집 등 다양한 복잡성을 제공하여 초보자에게도 적합하도록 한다.

ML5.js는 비지도 및 감독 문제와 미션 크리티컬하고 간단한 모델에 대한 유틸리티를 제공한다. 또한, Typescript 및 JavaScript 개발자를 위한 일체형 범용 자바스크립트 머신 러닝 라이브러리로서 수학 및 통계, 회귀 알고리듬, 인공 신경망, 비지도 및 감독 학습, 특징 추출, 선형 모델, 배깅, 앙상블, 분해, 클라스 등을 지원한다.테링 등

뿐만 아니라, ML5.js는 배열과 해시 테이블의 임의의 숫자 생성, 정렬, 비트 연산을 허용하고 사용자에게 최적화, 배열 조작, 선형 대수학을 위한 루틴을 제공한다. 이 라이브러리의 또 다른 큰 장점은 교차 검증을 지원하는 것입니다.

```js
// Initialize an Image Classifier method
let classifier;

// A variable to hold an image 
let image;

function preload() {
  classifier = ml5.imageClassifier('MobileNet');
  image = loadImage('images/car.png');
}
```

## Neuro.js

Neuro.js는 AI 기술과 챗봇으로 보조자를 만드는 데 널리 사용되는 강화 학습 모델과 딥 러닝 모델을 개발하고 훈련하기 위한 자바스크립트 프레임워크이다.

많은 개발자들은 이 라이브러리를 사용하여 딥 러닝 및 머신 러닝 모델을 개발, 연습 및 훈련한 다음, JS 스크립트를 사용하여 웹 브라우저 또는 Node.js에 배포한다.

이 라이브러리의 주요 장점 중 일부는 실시간 분류에 관여하도록 돕고, 학습을 위한 온라인 지원을 제공하며, ML 프로젝트를 만들 때 다중 레이블 양식의 분류를 지원한다는 것이다. Neuro.js 라이브러리를 사용하여 빌드된 아래의 색상 분류 코드 예를 살펴보십시오.

Neuro.js 라이브러리의 성능 중심적이고 단순한 특성으로 머신 러닝을 사용하는 모든 사람이 접근 가능하고 실용적으로 사용할 수 있습니다.

```js
//Using an array of input and output pairs:
const neuroModel = require("neuro.js");

const colorsClassifier = new neuroModel.classifiers.NeuralNetwork();

colorsClassifier.trainBatch([
  { input: { red: 0.03, green: 0.7, black: 0.5 }, output: 0 }, // = black
  { input: { red: 0.16, green: 0.09, black: 0.2 }, output: 1 }, // = white
  { input: { redr: 0.5, green: 0.5, black: 1.0 }, output: 1 } // = white
]);

console.log(colorsClassifier.classify({ red: 1, green: 0.4, b: 0 })); 
// 0.99 = nearly white
```

## 케라스.Js

Keras.js는 TensorFlow.js 다음으로 딥 러닝에 가장 널리 사용되는 JS 프레임워크로 간주될 수 있다. 신경망 라이브러리와 함께 작업하는 개발자들 사이에서 매우 인기가 있다. Keras에서 백엔드를 위해 여러 개의 프레임워크를 사용하므로 CNTK, TensorFlow 및 기타 프레임워크에서 모델을 교육할 수 있습니다.

Keras를 사용하여 빌드된 기계 학습 모델은 브라우저에서 실행할 수 있습니다. Node.js에서도 모델을 실행할 수 있지만 CPU 모드만 사용할 수 있습니다. GPU 가속은 없습니다.

넷플릭스나 우버 등 유수의 기업들이 사용자 경험을 높이기 위해 케라스 뉴럴네트워킹 모델과 협력하고 있다. NASA, CERN 등과 같은 많은 과학 단체들은 인공지능 관련 프로젝트에 이 기술을 사용한다. Keras는 인공지능 라이브러리를 위한 JS 대안으로 고려되며, 프로젝트의 다양한 모델을 실행하고 WebGL 3D 설계의 API에서 제공하는 GPU 지원을 활용할 수 있도록 보장합니다.

## 결론

이 기사에서는 머신 러닝 또는 데이터 과학에 참여할 때 사용할 수 있는 몇 가지 JavaScript 라이브러리를 다루었습니다.

자바스크립트는 딥 러닝과 머신 러닝과 같은 과목과 그다지 친밀하지 않지만, 향후 몇 년 안에 ML 개발자 사이에서 가장 두드러진 언어가 될 것으로 예상된다.

위에 언급한 플랫폼과 도서관의 개발이 그 배경의 주요 원인이 될 것이다. 그리고, JS는 기본 프로그래밍 언어이기 때문에 알고리즘과 함께 작업하고 많은 문제에 대한 해결책을 제시하는 것이 매우 편리할 것입니다.

궁극적으로, 자바스크립트로 데이터 과학에 발을 들여놓는 것은 초보자들에게 확실히 큰 이점이 될 것이고 프로그래머들이 머신 러닝을 발전시키는 훌륭한 접근법이 될 것이다.