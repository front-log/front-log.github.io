---
layout: post
title: "JavaScript 패키지 관리자 비교: Ran, npm 또는 pnpm?"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/npm-yarn-pnpm-package-managers.png"
tags: 
---


![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/npm-yarn-pnpm-package-managers.png?fit=730%2C487&ssl=1)

현대 애플리케이션 개발에서 우리는 모든 것을 처음부터 쓰지는 않습니다. 대신, 우리는 기존의 오픈 소스 패키지를 사용하는 것을 선호한다. 이러한 패키지는 각각 고유한 유지 관리자와 커뮤니티가 있습니다. 따라서 프로젝트에서 패키지를 사용하면 개발 속도가 빨라지고, 새로운 업데이트에 액세스할 수 있으며, 사용자 지정 스크립트보다 보안이 강화되는 등의 이점을 얻을 수 있습니다.

한 패키지가 올바르게 작동하려면 다른 여러 패키지에 의존하는 것이 일반적입니다. 마찬가지로, 다른 패키지도 로데시 같은 것에 의존할 수 있지만, 로데시 자체는 여러 패키지에 의존합니다. 즉, 중첩된 종속성이 너무 복잡해져서 종속성 관리를 수동으로 처리할 수 없는 경우가 있습니다.

다음은 패키지 관리자가 매우 유용한 경우입니다. 패키지 관리자는 프로젝트의 종속성을 자동으로 처리하는 도구입니다.

예를 들어 패키지 관리자는 단일 명령으로 새 패키지를 설치하거나 기존 패키지를 업데이트할 수 있습니다. 왜냐하면 모든 것이 자동화되어 있기 때문에 사람의 실수 가능성은 없기 때문입니다. 우리는 JavaScript 개발자로서 여러 패키지 관리자에 액세스할 수 있습니다. 이 가이드에서는 가장 인기 있는 세 가지를 비교합니다.

- npm
- 실
- pnpm

## 패키지 관리자 개요

npm은 레지스트리 프로토콜과 포장 표준의 개념을 도입한 최초의 패키지 관리자였다. 2010년에 출시되었으며 Node.js 팀에 의해 공식적으로 채택되었으며, npm의 전환점이 되었다.

Node.js의 대규모 성공 이후, npm은 개발자의 커뮤니티로부터도 관심을 받았다. 자바스크립트 패키지에 대한 온라인 레지스트리와 레지스트리와 연동하여 종속성을 설치하고 업데이트하는 명령줄 도구를 제공한다.

그러나 Npm의 단점은 Warn과 pnpm의 개발을 촉발시킨 몇 가지이다. 예를 들어, npm은 다른 것보다 상당히 느립니다. 또한 심각한 보안 취약성의 이력이 있습니다.

그래서 페이스북이나 구글 같은 거대 기술 회사들은 npm을 계속 사용하는 것을 주저했다. 차례로, 그들은 npm의 더 나은 버전을 개발하기 위한 노력에 동참했고 그것을 "Yarn"이라고 불렀다. 한편 우크라이나 개발사 졸탄 코찬은 pnpm을 개발했다.

## npm, Narn 및 pnpm의 특징

이러한 모든 패키지 관리자는 오픈 소스이므로 각 패키지의 내부 작업을 확인할 수 있습니다. 때로는 엔터프라이즈 수준의 애플리케이션 개발의 요구 사항이기도 합니다.

### npm의 이점:

- 자동으로 `package-lock.json` 파일을 생성합니다. 버전 제어 시스템에 커밋하는 것이 유용합니다. 이러한 방식으로 다른 개발자는 로컬 컴퓨터에 종속성을 쉽게 설치할 수 있습니다.
- 로컬 또는 글로벌 종속성 관리
- npm은 여러 버전의 종속성을 처리할 수 있는 잘 갖추어져 있습니다.
- pypi, rubygems 또는 packagist보다 더 많은 패키지를 가진 공식 레지스트리를 가지고 있다.

### 실의 이점:

- Narn은 모노레포에 나타나는 많은 문제를 해결합니다. 예를 들어 동일한 리포지토리에서 여러 패키지를 유지 관리하는 경우 해당 패키지는 모두 별도의 `패키지`를 가집니다.json 파일은 저장소에 모든 패키지의 종속성을 설치할 수 있는 작업 공간의 개념으로 인해 모든 패키지를 한 번에 간편하게 업데이트할 수 있습니다. npm에서는 각 패키지 폴더 내에서 `npm install` 명령을 수동으로 실행해야 합니다.
- Warn은 오프라인 캐시 메커니즘을 사용하여 패키지를 처음 설치할 때 Warn이 `~/.yarn-cache` 아래의 캐시 폴더에 추가합니다. 따라서 다음에 이 패키지가 필요할 때 서버에 HTTP 요청을 하지 않고 로컬 캐시에서 패키지를 검색합니다. 이 작은 개선으로 Npm에 비해 Narn의 성능이 크게 향상되었습니다.
- Yarn.lock이라는 잠금 파일도 사용하므로 모든 팀원들에게 프로젝트가 제대로 작동합니다. 이 개념은 결정론적 설치 알고리즘이라고도 합니다.
- 응용 프로그램을 개발할 때 다양한 시나리오에서 편리하게 사용할 수 있는 기본 제공 라이센스 검사기가 포함되어 있습니다.
- npm과는 달리, Yarn은 병렬 다운로드라는 접근 방식을 사용한다. 이를 통해 Ran은 더 많은 리소스를 활용하여 빌드 프로세스를 가속화할 수 있습니다.
- 오류가 발생할 경우 HTTP 요청을 자동으로 재시도할 수 있습니다. 이 기능은 임시 인터넷 문제에 직면할 때 특히 유용합니다.

### pnpm의 이점:

- npm과 호환되지만 훨씬 더 나은 디스크 공간 사용 및 속도도 제공합니다.
- pnpm은 모든 패키지를 단일 위치에 설치한 다음 symlink를 사용하여 참조합니다. Pnpm이 파일 간의 차이를 감지할 수 있도록 하는 컨텐츠 주소 지정 스토리지 시스템이라는 완전히 새로운 개념을 도입했습니다. 따라서 서로 다른 두 버전의 패키지에서 변경되지 않은 파일을 복제하지 않습니다.
- 최신 버전인 5.8.0은 교차 플랫폼 셸 환경인 셸 에뮬레이터라고 하는 새로운 론-바시(Warn-bash) 방식의 설정을 도입했다.
- pnpm은 엄격한 액세스 제어 메커니즘을 가지고 있는데, 이는 패키지가 `pnpm`에 정의된 종속성에만 액세스할 수 있음을 의미한다.json의 파일

## 패키지 관리자 비교

### 사용의 용이성

npm, Rany, pnpm은 다양한 작업에 대해 거의 동일한 명령을 제공하며 모두 사용하기 쉽습니다. 다음은 일반적으로 사용되는 명령의 예입니다.

### 속도

이러한 패키지 관리자의 속도와 성능에 대해서는 pnpm과 비교가 안 됩니다. 다양한 사용 사례를 벤치마킹한 결과 pnpm은 npm보다 최대 3배 빠른 성능을 보였다.

Narn과 npm의 속도는 비교가 된다. 경우에 따라서는 Npm보다 Narn이 훨씬 유리하지만 npm이 더 적합한 시나리오가 있다. 예를 들어 `node_modules`만 사용하여 설치 작업을 수행하고 `cache`와 `lock file` 기능을 건너뛰면 npm이 5배 더 빠른 속도를 제공할 수 있다. 마찬가지로, 세 가지 기능을 모두 사용할 경우, Narn은 성능을 향상시켜 npm보다 11배 더 빨라질 수 있습니다.

### 보안

Narnovernpm의 주요 이점은 체크섬을 사용하여 각 패키지의 무결성을 검증한다는 것입니다. 확인 프로세스는 패키지에서 코드를 실행하기 전에 수행되므로 패키지 가로채기 취약성의 가능성을 모두 무시합니다.

반면에, npm은 불량 패키지로 작업할 때 좀 더 관대하다. 보안을 위한 모범 사례를 제공하기 위해 여전히 발전하고 있습니다. 그러나 npm은 일반적으로 보안 측면에서 평판이 좋지 않다.

과거 npm에는 많은 프로젝트에 직접적인 영향을 미치는 일부 보안 취약점이 있었다. 예를 들어 npm 버전 5.7.0에서는 Linux 기반 운영 체제(OS)에서 sundpm 명령을 실행하면 시스템 파일의 소유권을 변경할 수 있어 OS를 사용할 수 없게 됩니다.

마찬가지로 2018년에도 비트코인 도용 사건이 또 발생했다. 기본적으로 인기 있는 Node.js 패키지 EventStream은 버전 3.3.6에 악성 종속성 `플랫맵 스트림`을 추가했다. 이 악성 패키지는 개발자의 컴퓨터에서 비트코인을 훔치려는 암호화된 페이로드로 가득 찼습니다.

pnpm은 npm과 Narn의 긍정적인 속성을 결합하여 훨씬 더 나은 보안을 제공합니다. 또한 패키지에 정의된 자체 종속성만 사용하도록 패키지를 바인딩하는 엄격한 액세스 제어 메커니즘을 구현한다.json의 파일

### 안정성

npm, Yarn, pnpm은 지난 몇 년간 여러 단계를 거쳤다. 시간이 지남에 따라, 그들의 코드 기반은 성숙해졌습니다. 왜냐하면 그들은 오픈 소스 커뮤니티로부터 엄청난 기여를 받았기 때문입니다.

그리고 시간이 흐르면서, 새로운 개념과 아이디어들이 등장하는데, 그것은 변화를 가져올 수 있다. 이 가이드를 작성할 당시 이러한 패키지 관리자의 상태는 양호하며 프로젝트에서 문제 없이 사용할 수 있습니다.

Narn은 Facebook과 Google이 지원하고, npm은 Microsoft와 Node.js가 지원하고, pnpm은 현재 75명 이상의 기여자를 보유하고 있지만 대부분 개인에 의해 개발되므로 이러한 패키지 관리자에게 의존하여 다음 프로젝트를 만들 수 있습니다.

### 모노레포즈 지원

모노레포스는 대규모 코드베이스를 저장하고 관리하는 대기업이 대부분 선호한다. npm은 개별 프로젝트를 관리하기 위한 목적으로만 설계되었습니다. 현재로선 모노레포스를 지원하는 기능이 없다. 그러나 Warn과 pnpm 모두 작업 공간에 대한 개념 덕분에 모노레포스를 완벽하게 지원합니다.

### 결정론적 — 잠금 파일

세 개의 패키지 관리자 모두 잠금 파일의 기능으로 꽉 찼습니다. 여러 개발자가 프로젝트의 동일한 복사본을 설치할 수 있습니다. pnpm은 package-lock.json 파일을, yarn은 yarn.lock을, pnpm은 pnp-lock.yaml 파일을 사용한다.

## 결론

보다 빠르고 효율적인 메모리 사용을 제공하는 솔루션을 찾는 경우 pnpm 사용을 적극 고려해야 합니다.

모노포스를 취급하는 경우 pnpm 또는 Yarn을 사용하여 처리할 수 있습니다. 그러나, Yarn은 Facebook에 사용 데이터를 전송하므로, 일부 시나리오에서는 Yarn이 적절한 선택이 되지 않을 수 있습니다.

Node.js 버전 5도 지원하지 않습니다. 이러한 점에서 npm은 Node.js 팀에 의해 권장되기 때문에 Node.js 기반 프로젝트에 선호되는 옵션이다. 요즘 Node.js는 기본적으로 npm과 함께 제공됩니다.

npm에서는 보안 문제와 함께 그것의 역사를 고려해야 하는데, 이것은 npm에 존재하는 많은 발행자들을 해결하기 위해 만들어진 Narn의 개발을 촉발시켰다. 따라서, 프로젝트의 보안에 관심이 있는 경우 npm 대신 Narn을 사용하는 것을 고려하십시오.