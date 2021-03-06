---
layout: post
title: "녹 압축 라이브러리"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/12/rust-compression-libraries.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/rust-compression-libraries.png?fit=730%2C487&ssl=1)

편집자 참고: 이 게시물은 원래 벤치마크에서 오류를 수정하기 위해 2021년 1월 8일에 업데이트되었습니다.

데이터 압축은 많은 애플리케이션에서 중요한 구성요소입니다. 다행히도, 러스트 커뮤니티에는 이 문제를 처리할 수 있는 상자가 많이 있습니다.

Rust serialization 라이브러리에 대한 검토와 달리 다른 형식 간의 성능을 비교하는 것은 의미가 없습니다. 일부 형식에서는 C 라이브러리 주위에 얇은 포장지만 있습니다. 여기 대부분이 곧 러스트에게 보내질 수 있기를 바랍니다.

본 가이드에서는 다음 내용을 다룹니다.

- Rust 압축 라이브러리는 무엇을 합니까?
- Rust에 가장 적합한 압축 라이브러리는 무엇입니까?
- Rust:DEFLATE/`zlib`, Snappy, LZ4, ZS 표준, LZMA, Zopfli 및 Brotli용 압축 라이브러리 스트리밍
- Rust용 라이브러리 보관: tar, zip 및 rar
- 녹 압축 라이브러리: 벤치마크

## Rust 압축 라이브러리는 무엇을 합니까?

압축 유틸리티에는 스트림 압축기와 보관기의 두 가지 종류가 있습니다. 스트림 압축기는 바이트 스트림을 사용하고 압축 바이트 스트림을 더 짧게 내보냅니다. 보관기를 사용하면 여러 파일과 디렉터리를 직렬화할 수 있습니다. verable.zip과 같은 일부 형식은 파일을 수집하거나 압축할 수 있습니다.

웹의 경우 널리 구현된 스트림 형식은 gzip/deflate와 Brotli 두 가지뿐이다. gzip과 defrate는 동일한 기본 알고리즘을 구현하고, gzip은 체크와 헤더 정보를 더하기 때문에 같은 제목으로 나열한다. 일부 클라이언트에서는 bzip2 압축도 허용하지만 zopfli와 비슷한 압축률(거래 압축 시간)을 얻기 위해 gzip을 만들 수 있고, Brotli를 사용하면 더 작은 압축률을 얻을 수 있기 때문에 더 이상 널리 보급되지 않는다.

## Rust에 가장 적합한 압축 라이브러리는 무엇입니까?

다른 문제와 마찬가지로 압축 및 압축 해제에 대한 런타임, CPU 및 메모리 사용 대 압축 비율, 데이터 스트리밍 기능, 체크섬과 같은 안전 조치 측면에서 서로 다른 절충점을 가진 수많은 솔루션이 있다. 이미지, 오디오 또는 비디오 관련 손실 압축 형식이 없는 무손실 압축에만 초점을 맞춥니다.

다소 현실적인 벤치마크를 위해 다음 파일을 압축 및 압축 해제에 사용할 수 있습니다. 압축이 매우 쉬운 파일부터 압축이 매우 어려운 파일까지 다양합니다.

- `cat/dev/zero | 헤드 -c$[1024 * 1024 * 100] > 0으로 생성된 100MB 파일입니다.빈
- 내 개인 블로그의 연결된 마크다운, 작고 텍스트 무거운(cat llogiq.github.io/_posts/*.md > 블로그로 생성)txt)
- 내 고양이의 이미지
- 현재 안정된 툴 체인(toolchain)의 x86_64 rustc 이진(1.47.0
- 우연히 주변에 놓여진 영화 `해커즈`의 TV 녹화.
- `cat /dev/urandom | head -c$[1024 * 1024 * 10000] > randoms로 생성된 유사 난수 100MB 파일입니다.빈

모든 압축 및 압축 해제는 메모리에서 및 메모리에서 이루어집니다. 내 컴퓨터에는 Linux 5.8.18_1을 실행하는 Ryzen 3550H 4코어 CPU가 있습니다.

참고: 일부 압축기는 왕복 테스트에서 실패했습니다. 즉, 압축된 후 압축되지 않은 데이터가 원본과 일치하지 않습니다. 이는 벤치마크의 구현이 잘못되었거나 라이브러리에 결함이 있음을 의미할 수 있습니다. 나는 추가 테스트를 할 것이다. 일단 각 라이브러리에 별표(*)를 표시해 두었습니다.

## Rust용 스트림 압축 라이브러리

스트림 압축기 부서의 벤치마크는 다음 상자에 대해 설명합니다.

### DEFLATE/'zlib'

DEFLATE는 사전과 허프먼 인코딩을 사용하는 오래된 알고리즘이다. 일반(헤더 없이), gzip(동명의 유닉스 압축 유틸리티로 대중화된 일부 헤더 포함), zlib(다른 파일 형식과 브라우저에서도 자주 사용)의 세 가지 변형이 있다. 우리는 평범한 변형을 벤치마킹할 것이다.

- `야지` 0.1.3은 자체 `벡`을 반환하는 단순한 인터페이스를 가지고 있지만, 우리가 벤치마킹하는 보다 복잡한 인터페이스가 있어 우리가 직접 공급할 수 있다. 위에서는 압축 수준을 선택할 수 있습니다.
- `deflate` 0.8.6은 출력 `Vec`를 제공할 수 없으므로 런타임에 할당이 포함됩니다*
- flate2 1.0.14는 세 가지 가능한 백엔드, 그 중 두 가지는 C 구현을 포함한다. 이 벤치마크는 설치 작업을 피하고 싶었기 때문에 `기본` 백엔드만 사용합니다. 죄송합니다.

### 스냅피

스내피는 LZ77에 대한 구글의 2011년 답변으로, 공정한 압축 비율의 빠른 실행 시간을 제공한다.

- snap 1.0.1
- "buppy_twitters

### LZ4

2011년에 출시된 LZ4는 LZ77 계열의 또 다른 속도 중심 알고리즘이다. 사전 매칭에만 의존하여 산술 및 허프먼 코딩을 없앤다. 이렇게 하면 압축기가 매우 간단해집니다.

구현에 약간의 차이가 있습니다. 이들 모두 기본적으로 동일한 알고리즘을 사용하지만, 일부 구현에서는 속도를 최대화하는 기본값을 선택하는 반면 다른 구현에서는 더 높은 압축비를 선택하기 때문에 결과 압축비는 다를 수 있다.

- lz4-압축 0.7.0
- `lz4_flex` 0.7.0
- lzz 0.8.0
- `lz4-190` 0.1.1
- `lz-fear` 0.1.1

### Z 표준

페이스북이 2016년에 발표한 ZS 표준(또는 `zstd`) 알고리즘은 실시간 애플리케이션을 위한 것이다. zlib 수준 또는 더 나은 압축비로 매우 빠른 압축 및 압축을 제공합니다. BTRFS 파일 시스템 압축과 같이 시간이 필수적인 다른 경우에도 사용됩니다.

- `zstd` 0.5.3은 표준 C 기반 구현을 바인딩한다. 여기에는 헤더를 포함하여 "pkg-config" 및 "libzstd" 라이브러리가 필요합니다*

### LZMA

더 높은 압축을 위해 다른 방향으로 이동하며, LZMA 알고리듬은 마르코프 체인을 사용하여 사전 기반 LZ 알고리듬 클래스를 확장한다. 이 알고리즘은 압축이 훨씬 느리고 압축 해제보다 훨씬 많은 메모리가 필요하다는 점에서 상당히 비대칭적이다. 리눅스 배포판의 패키지 포맷에 종종 사용되며, 네트워크 사용량을 줄이고 압축 해제 CPU 및 메모리 요구 사항을 충족할 수 있다.

- rust-lzma 0.5.1
- lzma-rs 0.1.3

### 조플리

Zopfli는 zlib 호환 압축 알고리즘으로, 긴 런타임에 탁월한 압축 비율을 교환한다. zlib 감압이 널리 구현되는 웹에서 유용하다.

Zopfli는 압축하는 데 더 많은 시간이 걸리지만, 이는 네트워크 트래픽을 줄이는 데 허용 가능한 절충안입니다. 형식은 DEFLATE와 호환되므로, 이 상자는 압축만 구현합니다.

- zopfli 0.4.0* (`deflate` 또는 `fflate2`를 사용하여 원본과 일치하는 포장을 풀 수 없음)

### 브로틀리

Zopfli를 만든 구글 팀이 개발한 브로틀리는 LZ77 기반 압축 알고리즘을 2차 컨텍스트 모델링으로 확장하는데, 그렇지 않으면 압축하기 어려운 데이터 스트림을 압축할 수 있는 우위를 제공한다.

- `brotli` 3.3.0은 (높은 버전 번호가 증명하듯이) 많은 수의 수정과 최적화를 경험한 원본 C++ 소스를 대략적으로 번역한 것이다. 인터페이스는 다소 단조롭지만 충분히 작동합니다.

## Rust용 라이브러리 보관

보관소의 경우 기준점에는 tar 0.4.30, zip 0.5.8, rar 0.2.0이 있다.

zip이 가장 잘 알려진 형식일 것이다. 1989년에 처음 출시되었으며, 아마도 가장 널리 지원되는 형태일 것입니다. 가장 오래된 포맷으로 1979년 처음 출시되었다. 사실 자체 압축은 없지만 일반적인 UNIX 방식에서는 gzip(DEFLATE), bzip2(LZW), xz(LZMA) 등의 아카이브를 스트리밍해야 한다. rar 포맷은 zip보다 다소 어리고 파일 오버헤드가 적고 압축력이 다소 좋아 파일 공유 서비스에서도 인기를 끌었다.

### 인터페이스

스트림 프로세서와 관련하여, API를 구현할 수 있는 세 가지 옵션이 있습니다. 가장 쉬운 방법은 `를 취하는 것이다.

가장 다재다능한 인터페이스는 분명 `간단한 읽기`와 `간단한 쓰기`를 취하여 전자를 읽고 후자에 쓰는 방식이다. 그러나 기록된 출력에서 블록 길이를 수정해야 하는 경우가 있기 때문에 일부 형식에서는 이 방법이 최적이 아닐 수 있습니다. Vec의 경우 std::io:로 수행할 수 있는 Seek(Seek)를 출력에서 구현하지 않는 한 이 인터페이스는 테이블에 약간의 성능을 남깁니다.:커서"입니다. 반면, 메모리에 맞지 않을 수도 있는 데이터를 사용하여 다소 편안하게 작업할 수 있습니다.

어떤 경우에도 의미 있는 비교를 위해 API가 이를 허용하는 경우 사전 할당된 RAM에서 RAM으로 well compress합니다. 예외는 이와 같이 표시됩니다.

일부 라이브러리를 사용하면 체크섬과 같은 특정 기능을 활성화(비활성화)하는 옵션을 설정하고 특정 크기/런타임 트레이드오프를 선택할 수 있습니다. 이 벤치마크는 API에서 쉽게 사용할 수 있는 경우 기본 구성, 때로는 다양한 압축 수준을 사용합니다.

## 녹 압축 라이브러리: 벤치마크

추가 작업 없이 다음 결과를 6개의 표에 제시하여 디스플레이의 혼동을 방지합니다.

0:

고양이:

rustc:

"해커":

임의:

여느 때처럼, 내 벤치마크는 GitHub에서 이용할 수 있다.