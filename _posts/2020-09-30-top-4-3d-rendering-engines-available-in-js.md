---
layout: post
title: "모든 게임 개발자가 알아야 할 4개의 3D 렌더링 엔진"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/top-4-3d-rendering-engines-available-in-js.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/top-4-3d-rendering-engines-available-in-js.png?fit=730%2C487&ssl=1)

게임을 개발하는 재미의 절반은 게임에 활력을 불어넣고 사용자를 참여시키기 위한 복잡한 애니메이션을 만드는 것입니다. 모션용 게임 엔진, 다이내믹스용 물리학 엔진, 사운드용 오디오 엔진 등을 통해 채굴하는 데 수 많은 시간을 할애할 수 있으며 목록은 계속되며 옵션과 가능한 조합은 상상력, 시간 및 리소스에 의해서만 제한됩니다.

그러나 사용자가 게임에 푹 빠지게 하는 것, 즉 게임 플레이에 모든 노력을 기울인다고 가정해 보겠습니다. 렌더링 엔진은 짧은 시간 내에 놀랍고 정교한 그래픽을 만들 수 있도록 도와주며, 여러분의 게임을 진정으로 독특하고 매력적으로 만드는 것에 집중할 수 있도록 해 줍니다.

이 가이드에서는 JavaScript 커뮤니티에서 제공하는 최고이자 가장 인기 있는 4가지 3D 렌더링 엔진을 자세히 살펴보겠습니다.

- 캐논.js
- Phoria.js
- D3
- Xeogl.js

몇 가지 주목할 만한 기능을 강조하고 각 엔진과 관련된 장단점을 살펴보겠습니다.

## 1. Cannon.js

Cannon.js는 자바스크립트에서 사용할 수 있는 최고의 물리 및 렌더링 엔진 중 하나이다. Three.js와 Ammo.js에서 영감을 받아 특별히 가벼운 것으로 알려져 있다. 무엇보다도, 그것은 무료이고 오픈 소스입니다.

### 프로스

- 가벼운 빌드 크기
- 시작하기 쉬움
- 오픈 소스, 어디서나 자유롭게 사용 가능
- 반복적인 가우스-세이델 해결사를 사용하여 제약 조건 문제 해결
- 내장 충돌 감지
- 박스 밖으로 단단한 차체 다이내믹스

### 콘스

- 마스터하기 어려움
- 단축 광상분리
- 성능이 떨어지고 객체 지향적인 방식으로 작성

### Cannon.js 실행 중

Cannon.js로 시작하려면 간단한 가져오기 장면을 만들고 결과를 콘솔에 인쇄하십시오.

다음 방법 중 하나를 사용하여 Cannon.js를 설치합니다.

```xml
<script src="cannon.min.js"></script>

// OR

npm install --save cannon 
```

이제 우리의 세계를 창조해봅시다.

```cpp
const world = new CANNON.World();
world.gravity.set(0, 0, -9.82); // m/s²
```

구를 만들어 세계에 추가합니다.

```cpp
const radius = 1; // m
const sphereBody = new CANNON.Body({
   mass: 5, // kg
   position: new CANNON.Vec3(0, 0, 10), // m
   shape: new CANNON.Sphere(radius)
});
world.addBody(sphereBody);
```

그런 다음 바닥이나 평면을 만들어 세계에 추가합니다.

```cpp
// Create a plane
const groundBody = new CANNON.Body({
    mass: 0 // mass == 0 makes the body static
});
const groundShape = new CANNON.Plane();
groundBody.addShape(groundShape);
world.addBody(groundBody);

const fixedTimeStep = 1.0 / 60.0; // seconds
const maxSubSteps = 3;
```

초기화 기능을 만들어 모든 것을 설정하고 구 `Z` 위치를 콘솔에 인쇄합니다.

```js
var lastTime;
(function simloop(time){
  requestAnimationFrame(simloop);
  if(lastTime !== undefined){
     const dt = (time - lastTime) / 1000;
     world.step(fixedTimeStep, dt, maxSubSteps);
  }
  console.log("Sphere z position: " + sphereBody.position.z);
  lastTime = time;
})();
```

이 함수는 애니메이션 자체를 만듭니다.

```js
function animate() {
      init();
      requestAnimationFrame( animate );

      mesh.rotation.x += 0.01;
      mesh.rotation.y += 0.02;

      renderer.render( scene, camera );
}
```

코드를 실행하고 콘솔을 열어 `Z` 위치의 값을 볼 수 있습니다. 시작하는 데 도움이 되는 추가 예제를 보려면 여기를 클릭하십시오.

## 2. Phoria.js

Phoria는 Canvas 2D 렌더러에서 간단한 3D를 만들기 위한 자바스크립트 라이브러리이자 렌더링 엔진이다. Phoria는 WebGL을 사용하지 않기 때문에 HTML Canvas를 렌더링할 수 있는 모든 장치에서 작동한다.

### 프로스

- 사용하기 쉽고 쉽게 시작하고 놀라운 그래픽을 만들 수 있습니다.
- Phoria가 WebGL을 지원하지 않기 때문에 부드러운 학습 곡선
- 우수한 벡터 및 매트릭스 산술 라이브러리

### 콘스

- WebGL이 부족하면 복잡한 그래픽 렌더링을 처리하기 어려울 수 있습니다.
- HTML 캔버스를 배우려면 꾸준한 연습이 필요하다.
- 소규모 애니메이션 및 그래픽에 더욱 적합
- 문서가 없습니다.

### Poria.js의 동작

Phoria.js를 시작하는 데 도움이 되는 많은 예제와 잘 설명된 샘플 코드가 있습니다.

먼저 라이브러리를 설치합니다.

```xml
<!DOCTYPE html>
<html>
<head>
    <script src="scripts/gl-matrix.js"></script>
    <script src="scripts/phoria-util.js"></script>
    <script src="scripts/phoria-entity.js"></script>
    <script src="scripts/phoria-scene.js"></script>
    <script src="scripts/phoria-renderer.js"></script>
    <script src="scripts/dat.gui.min.js"></script>
</head>
<body>
  // Create a Canvas element
  <canvas id="canvas" width="768" height="512" style="background-color: #eee"></canvas>
  <script>
      // Render animation on page load
      window.addEventListener('load', loadAnimation, false);
  </script>
</body>
</html>
```

다음으로 애니메이션 로드 기능을 만들어 애니메이션과 코드를 로드합니다.

```js
function loadAnimation(){
  const canvas = document.getElementById('canvas');

  // Add all script below here
  // ........
}
```

씬(scene)과 카메라를 설정하고 설정합니다.

```cpp
 const scene = new Phoria.Scene();
 scene.camera.position = {x:0.0, y:5.0, z:-15.0};
 scene.perspective.aspect = canvas.width / canvas.height;
 scene.viewport.width = canvas.width;
 scene.viewport.height = canvas.height;
```

렌더러를 만들고 위에서 만든 `캔버스`를 렌더링합니다.

```cpp
const renderer = new Phoria.CanvasRenderer(canvas);
```

그런 다음 몇 가지 유틸리티와 그리드를 구축한 다음 위에서 만든 씬(scene)에 추가합니다.

```cpp
  const plane = Phoria.Util.generateTesselatedPlane(8,8,0,20);
   scene.graph.push(Phoria.Entity.create({
      points: plane.points,
      edges: plane.edges,
      polygons: plane.polygons,
      style: {
         drawmode: "wireframe",
         shademode: "plain",
         linewidth: 0.5,
         objectsortmode: "back"
      }
   }));
   const c = Phoria.Util.generateUnitCube();
   const cube = Phoria.Entity.create({
      points: c.points,
      edges: c.edges,
      polygons: c.polygons
   });
   scene.graph.push(cube);
   scene.graph.push(new Phoria.DistantLight());
```

이제 애니메이션 마무리하고 시작하겠습니다.

```js
   const pause = false;
   const fnAnimate = function() {
      if (!pause)
      {
         // rotate local matrix of the cube
         cube.rotateY(0.5*Phoria.RADIANS);

         // execute the model view 3D pipeline and render the scene
         scene.modelView();
         renderer.render(scene);
      }
      requestAnimFrame(fnAnimate);
   };

   // key binding
   document.addEventListener('keydown', function(e) {
      switch (e.keyCode)
      {
         case 27: // ESC
            pause = !pause;
            break;
      }
   }, false);

   // start animation
   requestAnimFrame(fnAnimate);
```

최종 결과는 다음과 같습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/phoriajs-sample.gif?resize=540%2C446&ssl=1)

## 3. D3

D3는 데이터 및 데이터 시각화를 조작하고 렌더링하기 위해 설계된 자바스크립트 라이브러리이다. D3를 사용하면 HTML을 사용하여 놀랍고 강력한 변환을 추가하여 데이터를 실제처럼 활용할 수 있습니다.

이 라이브러리는 번창하는 커뮤니티 덕분에 보다 복잡한 데이터 시각화 기능을 처리할 때도 쉽게 시작할 수 있습니다. 또한 사용자 정의가 매우 용이하여 기존 시각화를 수정하고 기능을 확장할 수 있습니다.

### 프로스

- 대규모 커뮤니티 및 포괄적인 문서
- 다양한 시각화 컬렉션
- 사용자 정의 가능한 애니메이션, 상호 작용 및 데이터 기반 그림
- 빠르고 가벼운 시스템 리소스. JavaScript와 시각화로 구축되므로 브라우저를 통해 웹에서 쉽게 호스팅할 수 있습니다.

### 콘스

- 사용 가능한 교육용 비디오가 거의 없음
- 보다 혁신적인 시각화 차트를 사용할 수 있음
- 웹 개발 경험 필요
- 대규모 데이터셋을 처리할 때 속도가 느릴 수 있음
- 지도를 만들기에 적합하지 않습니다.

### D3 작동 중

D3를 시작하는 것은 매우 간단합니다. HTML 문서에 스크립트 태그를 추가하십시오.

```xml
<script src="https://d3js.org/d3.v6.min.js"></script>
```

예를 들어 다음과 같이 간단히 게임에 전환을 추가할 수 있습니다.

```js
d3.selectAll("transPage").transition()
    .duration(750)
    .delay(function(d, i) { return i * 10; })
    .attr("r", function(d) { return Math.sqrt(d * scale); });
```

여기서는 `transPage`가 있는 태그를 모두 선택하고 전환을 추가하기만 하면 됩니다.

## 4. Xeogl.js

Xeogl.js는 WebGL에서 3D 시각화를 생성하기 위한 오픈 소스 JavaScript 라이브러리이다. 그것은 인터랙티브 3D 애니메이션과 그래픽을 만드는 것에 초점을 두고 설계되었다.

### 프로스

- 렌더링에 WebGL 사용
- 내장된 구성요소 기반 장면 그래프.
- ECMA 스크립트 6로 작성
- 추가 종속성 또는 라이브러리가 없으므로 크기가 작아집니다.
- 자유 및 오픈 소스
- 다수의 개별 관절형 객체를 신속하게 렌더링하도록 설계

### 콘스

- 다른 렌더링 엔진에 비해 유연성이 떨어짐
- 개발자들 사이에서는 인기가 많지 않기 때문에, 일반적인 문제를 해결하는 데 도움이 되는 자원을 찾는 것은 때때로 어렵다.
- 설명서에서 개념을 명확하게 설명하지 않음

### Xeogl.js 실행 중

Xeogl.js를 시작하려면 먼저 프로젝트에 CDN 라이브러리를 추가하십시오.

```xml
<script src="https://github.com/xeolabs/xeogl/blob/master/build/xeogl.js"></script>
```

그런 다음 3D 개체를 만듭니다. 그런 다음 다음을 사용하여 `기하` 변수를 만듭니다.

```cpp
const geometry = new xeogl.TorusGeometry({
    radius: 1.0,
    tube: 0.3
});
```

다음 코드를 사용하여 `금속 재료`를 만듭니다.

```cpp
const material = new xeogl.MetallicMaterial({
    baseColorMap: new xeogl.Texture({
        src: "textures/diffuse/uvGrid2.jpg"
    }),
    roughnessMap: new xeogl.Texture({
        src: "textures/roughness/goldRoughness.jpg"
    })
});
```

마지막으로 `메시`를 만들고 위의 객체를 추가합니다.

```cpp
const mesh = new xeogl.Mesh({
    geometry: geometry,
    material: material,
    position: [0, 0, 10]
});
```

모든 것이 계획대로 진행되면 다음과 같은 것을 볼 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/xeogl-sample.png?resize=600%2C300&ssl=1)

## 올바른 렌더링 엔진 선택

다른 프로젝트와 마찬가지로 최고의 툴은 고객의 고유한 목표, 요구 및 요구 사항에 따라 달라집니다. 이 개요가 게임에 활력을 불어넣는 데 도움이 되기를 바랍니다.

대개의 경우, 제 생각에는, Cannon.js를 잘못 다루면 안 됩니다. 복잡한 그래픽을 구축하는 경우 특히 유용합니다. 그렇지 않으면 외부 라이브러리가 필요할 수 있는 많은 기능이 내장되어 있기 때문에 캐논의 크기가 작아지므로 빠른 처리가 우선인 경우 가장 적합합니다.

반면, Phoria.js는 단순한 그래픽을 만들고 싶다면 훌륭한 라이브러리이다. 웹GL을 지원하지 않기 때문에 피아로는 복잡한 3D 그래픽을 만들기가 어렵다.

웹 데이터 과학자를 선호하고 놀라운 데이터 시각화를 실현하고자 한다면 D3는 확실한 선택입니다.

마지막으로 Xeogl.js는 웹 또는 모델 시각화에 CAD와 같은 그래픽을 생성하는 것이 목표라면 흥미로운 대안이 될 수 있습니다.

어떤 렌더링 엔진을 가장 좋아하십니까? 우리가 놓친 게 있나요? 자유롭게 한 줄 적어주세요!