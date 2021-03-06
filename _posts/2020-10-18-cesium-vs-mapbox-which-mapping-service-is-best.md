---
layout: post
title: "세슘 대 세슘 지도 상자: 어떤 매핑 서비스가 가장 좋습니까?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/cesium-mapbox.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/cesium-mapbox.png?fit=730%2C487&ssl=1)

3D 지도 또는 요소를 애플리케이션에 통합하고자 할 때 Cesium 및 Mapbox가 좋은 옵션입니다.

이 기사의 목적은 다음과 같은 핵심 요소를 기반으로 이러한 두 가지 인기 있는 매핑 서비스를 설명하고 비교하는 데 있다.

- 사용자 정의 옵션
- API 및 SDK의
- 보고 느껴라

또한 각 매핑 서비스의 장단점을 보다 일반적으로 살펴보겠습니다.

## 세슘이 뭐죠?

Cesium은 3D 지리공간 데이터를 타일링, 시각화, 공유 및 분석할 수 있는 빠르고 간단한 엔드 투 엔드 플랫폼을 제공합니다.

Cesium은 대용량 및 다양한 3D 공간 데이터를 애플리케이션과 다른 환경에서 즉시 사용할 수 있는 스트리밍 가능한 3D 콘텐츠로 변환하도록 지원합니다.

### 세슘 프로

글로벌 뷰 지원

세슘은 3D 지구본 뷰 모델에서 지구를 표현할 수 있는 지원을 제공합니다. 보기 각도와 위치를 변경하여 가상 환경에서 자유롭게 이동할 수 있는 기능을 제공합니다. 지구본을 볼 수 있는 또 다른 기능은 지구 표면의 여러 가지 보기를 나타낼 수 있다는 것입니다.

완전 3D

Mapbox와 달리 Cesium은 박스 밖으로 완전히 3D입니다. 이를 통해 여러 관점에서 개체를 회전하고 시각화할 수 있습니다. 이를 통해 매핑 서비스에서 3D 개체를 구현하고 합성하는 경험을 더욱 원활하게 수행할 수 있습니다.

3D 데이터 타일링 및 스트리밍

Cesium은 3D 타일을 제공하여 대용량 및 다양한 3D 지리공간 데이터를 애플리케이션에 즉시 사용할 수 있는 스트리밍 가능한 3D 콘텐츠로 변환합니다. 또한 여러 소스의 데이터를 정렬하고 조합하여 하나로 시각화할 수 있습니다.

넉넉한 무료 평가 옵션 및 유연한 가격 계획

이 기사를 작성할 때 Cesium은 데이터 스트리밍, 무제한 앱, 최종 사용자 및 3D 컨텐츠를 호스팅하고 공유할 수 있는 5GB 스토리지 공간을 매월 최대 15GB의 커뮤니티(또는 무료 계층) 계획을 제공합니다. 이 옵션은 비상업적 프로젝트에 적합합니다.

### 세슘 콘스

상대적으로 더 큰 SDK 크기

맵박스에 비해 세슘은 비교적 큰 SDK 크기를 가지고 있으며, 최신 미포장 버전인 세슘.js(Cesium.js, 2020년 10월 출시)는 79.4MB까지 무게가 나간다.

불완전한 건물 정보

맵박스와는 달리 세슘은 건물 정보를 제공하지 않는다.

응답 시간 단축

세슘은 안정적인 7.2Mbps 인터넷 연결에서 지구본과 상호작용할 때 업데이트가 약 +3초로 로드되는 등 응답 시간이 느리다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/flow-diagram.png?resize=730%2C282&ssl=1)

## Mapbox는 어때요?

반면, Mapbox는 개발자가 서로 다른 플랫폼에 걸쳐 더 나은 매핑, 탐색 및 검색 환경을 구축할 수 있도록 지원합니다.

Cesium과 달리 Mapbox는 3D 매핑 이외의 방대한 매핑 서비스를 제공하며, 이러한 서비스 중 일부는 다음과 같습니다.

- 증강현실 탐색
- 자동차(운전 경험)

### 맵박스 프로

다른 지도 스타일 중에서 선택

Mapbox는 프로그램에서 직접 사용하거나 Mapbox Studio에서 새 사용자 지정 스타일을 만드는 시작점으로 사용할 수 있는 몇 가지 지도 스타일을 제공합니다.

관대한 무료 평가판

또한 Mapbox는 웹을 위한 최대 50,000/월 무료 지도 로드와 모바일 SDK를 위한 최대 25,000명의 월간 활성 사용자를 제공하는 넉넉한 무료 계층을 제공합니다.

로드/업데이트 시간 단축

샘플 3D 건물 디스플레이 테스트에서 Mapbox는 Cesium보다 더 빨리 지도를 업데이트하는 것 같습니다. 안정적인 7.2Mbps 인터넷 연결 상태에서 2초 미만으로 로딩된다.

Mapbox는 건물 정보를 제공합니다.

Mapbox는 세슘과 달리 3D 건물 정보를 지원합니다(외관과 느낌 부분 아래).

## 맵박스 콘스

Mapbox, 3D 대신 2.5D 사용

세슘은 개봉 즉시 3D를 사용하는 반면, 맵박스는 2.5D(2D 높이)를 사용한다. 이것은 Three.js와 같은 다른 타사 라이브러리를 사용하여 여러 3D 개체를 수집하고 결합하는 것을 약간 복잡하게 만든다.

## 사용자 정의 옵션

Cesium은 3D 데이터를 여러 장치에서 쉽게 호스팅, 편집 및 스트리밍할 수 있는 강력하고 안전한 클라우드 플랫폼인 Cesium 이온을 보유하고 있습니다.

또한 플랫폼에서는 Cesium World Terrain, Bing Maps 이미지, Cesium OSM Building 등의 큐레이션된 3D 컨텐츠에 액세스할 수 있습니다.

세슘이온의 또 다른 놀라운 특징은 코드를 쓰지 않고도 지도 기반의 이야기를 만들고 공유할 수 있다는 것입니다.

Mapbox는 Cesiumion과 같은 바로 사용할 수 있는 스타일 템플릿에서 선택할 수 있지만 Mapbox Studio와 유사한 플랫폼도 제공합니다. 하지만, 그것은 더 많은 기능을 가지고 있습니다. 예를 들어, 지도에 관심 지점 레이블을 추가하고, 지도 색상을 변경하고, 도로 폭을 조정할 수 있습니다. 또한 모든 SDK에 손쉽게 설계를 통합할 수 있습니다.

API/SDKs

세슘은 주로 웹을 위해 만들어진다. 주요 SDK는 Cesium.js이며, Cesium 이온에서 데이터를 스트리밍하여 3D 글로브와 맵을 만드는 오픈 소스 자바스크립트 라이브러리이다.

반면, Mapbox는 Mapbox GL을 제공합니다. Mapbox GL은 웹, 모바일 및 데스크톱 애플리케이션에 사용자 정의 가능하고 응답성이 뛰어난 클라이언트측 맵을 내장하기 위한 오픈 소스 라이브러리 제품군입니다.

Mapbox GL과 함께 다른 모든 서비스에 대한 상용 SDK도 제공합니다.

### 보기, 느낌 및 지도 상호 작용

맵박스

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/cesium-map-black-and-white.png?resize=730%2C413&ssl=1)

위의 이미지는 맵박스에 있는 3D 건물의 예로서, 지도를 더 쉽게 탐색할 수 있는 주변 건물 정보를 명확하게 제공합니다.

아래는 세슘에 있는 유사한 3D 건물의 예입니다. 맵박스와는 달리 건물 정보가 없어 지도를 탐색하기가 더 복잡할 것이다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/cesium-sample.png?resize=730%2C343&ssl=1)

## 결론

이 게시물에서는 세슘과 맵박스의 차이점을 다루었습니다. 우리는 그들 각각에 얽힌 장단점을 다루고, 두 지도 모두의 외관을 비교하고, 어느 것이 더 인기 있는지 확인했다. 두 플랫폼 모두 훌륭한 매핑 서비스를 제공하지만, 분명히 맵박스는 세슘보다 훨씬 더 많은 기능을 제공한다.

여러 플랫폼에서 3D 컨텐츠를 호스팅하고 공유하려는 경우 Cesium을 선택할 수 있습니다. 반면에 3D 데이터를 표시하고 더 많은 매핑 기능을 활용하는 것이 목표라면 Mapbox가 가장 좋습니다.