---
layout: post
title: "Rust 사용 시기 및 Go 사용 시기"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/when-to-use-rust-and-when-to-use-golang.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/when-to-use-rust-and-when-to-use-golang.png?fit=730%2C487&ssl=1)

바로 타석에서, 바둑과 러스트 사이에는 분명한 차이가 있다. 바둑은 특히 바둑의 힘으로 끝없이 확장할 수 있는 웹 API와 소규모 서비스를 구축하는 데 더욱 중점을 두고 있다. 후자는 Rust에서도 가능하지만 개발자 경험의 관점에서 상황이 훨씬 더 어렵다.

Rust는 대량의 데이터와 알고리즘 실행과 같은 기타 CPU 집약적인 작업을 처리하는 데 적합합니다. 이것은 Rust가 바둑을 이기는 가장 큰 강점이다. 고성능을 요구하는 프로젝트는 일반적으로 Rust에 더 적합합니다.

이 튜토리얼에서는 Go와 Rust를 비교 및 대조하여 성능, 동시성, 메모리 관리 및 전체 개발자 경험을 위한 각 프로그래밍 언어를 평가합니다. 또한 프로젝트에 적합한 언어를 한 눈에 선택할 수 있도록 이러한 요소에 대한 개요를 제공합니다.

Rust로 시작하는 거라면 더 읽기 전에 이 초보자 가이드를 다시 작성하는 것이 좋을 것 같습니다.

다 들켰다면, 안으로 들어가자!

## 성과

원래 구글의 엔지니어들에 의해 디자인된 고는 2009년에 대중에게 소개되었다. 학습과 코딩을 더 쉽게 할 수 있는 C++의 대안을 제공하기 위해 만들어졌으며 멀티코어 CPU에서 실행되도록 최적화되었다.

그 이후, 바둑은 언어가 제공하는 동시성을 이용하고자 하는 개발자들에게 훌륭한 역할을 해왔다. 언어는 하위 프로세스로 기능을 실행할 수 있는 Goroutine을 제공합니다.

바둑의 큰 장점은 고루틴을 얼마나 쉽게 사용할 수 있느냐 하는 것이다. 함수에 go 구문을 추가하면 하위 프로세스로 실행됩니다. Go의 동시성 모델을 사용하면 여러 CPU 코어에 워크로드를 배포할 수 있으므로 매우 효율적인 언어입니다.

```undefined
package main

import (
    "fmt"
    "time"
)

func f(from string) {
    for i := 0; i < 3; i++ {
        fmt.Println(from, ":", i)
    }
}

func main() {

    f("direct")

    go f("goroutine")
    time.Sleep(time.Second)
    fmt.Println("done")
}
```

멀티코어 CPU 지원에도 불구하고 Rust는 여전히 Go를 능가한다. Rust는 알고리즘 및 리소스 집약적인 작업을 실행하는 데 더 효율적입니다. 벤치마크 게임은 이진 트리와 같은 다양한 알고리듬에 대해 Rust와 Go를 비교한다. 모든 테스트 알고리즘에서 Rust는 최소 30% 더 빨랐고, 이진 트리 계산의 경우 최대 1,000%까지 빨랐다. Bitbucket에 의한 연구는 Rust가 C++와 동등한 성능을 발휘하는 유사한 결과를 보여준다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/rust-performance-bitbucket.png?resize=720%2C481&ssl=1)

(출처: 벤치마크 게임)

## 동시성

위에서 언급한 바와 같이 Go는 동시성을 지원합니다. 예를 들어, API 요청을 처리하는 웹 서버를 실행한다고 가정해 보겠습니다. Go(이동) 루틴을 사용하여 각 요청을 하위 프로세스로 실행하여 사용 가능한 모든 CPU 코어로 작업을 오프로드하여 효율성을 극대화할 수 있습니다.

Goroutines는 Go의 기본 제공 함수의 일부인 반면 Rust는 동시성을 지원하는 기본 비동기/대기 구문만 수신했다. 이와 같이 동시성에 관한 한 개발자 경험의 우위는 바둑으로 간다. 하지만, Rust는 기억의 안전을 보장하는 데 훨씬 더 뛰어나다.

Rust에 대한 단순화된 스레드의 예는 다음과 같습니다.

```cpp
use std::thread;
use std::time::Duration;

fn main() {
   // 1. create a new thread
   for i in 1..10 {
      thread::spawn(|| {
         println!("thread: number {}!", i);
         thread::sleep(Duration::from_millis(100));
      });
   }
  
  println!("hi from the main thread!");
}
```

동시성은 개발자들에게 항상 어려운 문제였다. 개발자 경험을 훼손하지 않고 메모리 안전 동시성을 보장하는 것은 쉬운 일이 아닙니다. 그러나 이러한 극단적인 보안 초점은 아마도 올바른 동시성을 만드는 것으로 이어졌다. Rust는 메모리 안전 버그를 방지하기 위해 자원의 무단 액세스를 방지하기 위해 소유권 개념을 실험했다.

Rust는 일반적인 메모리 안전 함정을 방지하는 데 도움이 되는 4가지 동시성 패러다임을 제공합니다. 채널과 잠금이라는 두 가지 일반적인 패러다임에 대해 자세히 알아보겠습니다.

### 채널

채널은 한 스레드에서 다른 스레드로 메시지를 전송하는 데 도움이 됩니다. 이 개념은 Go에도 적용되지만 Rust를 사용하면 리소스 경합 조건을 피하기 위해 한 스레드에서 다른 스레드로 포인터를 전송할 수 있습니다. 전달 포인터를 통해 Rust는 채널에 대한 스레드 분리를 강제 적용할 수 있습니다. Rust는 다시 동시성 모델과 관련하여 메모리 안전에 대한 집착을 보인다.

### 자물쇠

잠금이 보류된 경우에만 데이터에 액세스할 수 있습니다. Rust는 코드 대신 데이터를 잠그는 원리에 의존하며, Java와 같은 프로그래밍 언어에서 자주 발견된다.

소유권 개념과 모든 동시성 패러다임에 대한 자세한 내용은 "Rust와의 두려움 없는 동시성"을 참조하십시오.

## 메모리 안전성

초기 소유 개념은 Rust의 주요 판매 포인트 중 하나입니다. 녹은 타입의 안전성을 가져간다. 이것은 메모리 안전 동시성을 가능하게 하는 데도 중요하다.

Bitbucket 블로그에 따르면, "Rust의 매우 엄격하고 현학적인 컴파일러는 사용자가 사용하는 모든 변수와 참조하는 모든 메모리 주소를 확인합니다. 데이터 레이스 상황을 방지하고 정의되지 않은 동작에 대해 알려줍니다."

즉, 메모리 안전에 대한 러스트의 극단적인 집착으로 인해 버퍼 오버플로나 레이스 상태가 발생하지 않습니다. 하지만 이것 역시 단점이 있다. 예를 들어 코드를 작성하는 동안 메모리 할당 원리를 지나치게 인식해야 합니다. 항상 기억의 안전을 지키는 것은 쉽지 않다.

## 개발자 경험

우선, 각 언어와 관련된 학습 곡선을 살펴봅시다. 바둑은 단순함을 염두에 두고 설계되었다. 개발자들은 종종 그것을 "지루한" 언어라고 부르는데, 이것은 제한된 내장 기능들의 집합이 바둑을 쉽게 채택하게 만든다고 말한다.

또한 Go는 메모리 안전성 및 메모리 할당과 같은 측면을 숨기면서 C++에 대한 보다 쉬운 대안을 제공한다. 녹은 또 다른 방식을 취하는데, 기억 안전 같은 개념에 대해 생각하도록 강요한다. 소유의 개념과 포인터 전달 능력은 러스트를 학습에 덜 매력적이게 만든다. 메모리 안전에 대해 끊임없이 고민할 때 생산성이 떨어지고 코드가 더 복잡해지기 마련입니다.

Rust의 학습 곡선은 바둑에 비해 상당히 가파르다. 그러나 바둑은 파이썬과 자바스크립트 같은 더 동적인 언어보다 더 가파른 학습 곡선을 가지고 있다는 것은 언급할 가치가 있다.

## Go 사용 시기

Go는 다양한 사용 사례에 적합하므로 웹 API를 만드는 Node.js의 훌륭한 대안이 된다. Loris Cro가 지적한 바와 같이, "Go의 동시성 모델은 여러 개의 독립적인 요청을 처리해야 하는 서버측 애플리케이션에 적합합니다." 이것이 바로 바둑이 고루틴을 제공하는 이유이다.

게다가 Go는 HTTP 웹 프로토콜을 기본적으로 지원합니다. 기본 제공되는 HTTP 지원을 사용하여 작은 API를 신속하게 설계하고 이를 마이크로 서비스로 실행할 수 있습니다. 따라서 Go는 마이크로서비스 아키텍처에 잘 적합하고 API 개발자의 요구를 충족시킵니다.

간단히 말해, 개발 속도를 중시하고 성능보다 구문 단순성을 선호한다면 Go가 적합합니다. 여기에다 바둑은 대형 개발팀에게 중요한 기준인 코드 가독성을 더 잘 제공한다.

다음과 같은 경우 이동을 선택합니다.

- 단순성과 가독성에 관심을 기울입니다.
- 코드를 빠르게 쓸 수 있는 쉬운 구문을 원합니다.
- 웹 개발을 지원하는 보다 유연한 언어를 사용하고자 합니다.

## 러스트 사용 시기

대량의 데이터를 처리하는 경우와 같이 성능이 중요한 경우 녹을 선택하는 것이 좋습니다. 또한 Rust는 스레드 동작 및 스레드 간 리소스 공유 방법에 대한 세부적인 제어를 제공합니다.

반면, 러스트는 가파른 학습 곡선과 함께 제공되며, 메모리 안전의 추가적인 복잡성으로 인해 개발 속도가 느려진다. 이것은 반드시 단점이 아닙니다. 또한 컴파일러가 각각의 모든 데이터 포인터를 검사할 때 메모리 안전 버그가 발생하지 않도록 보장합니다. 복잡한 시스템의 경우 이 보증이 유용할 수 있습니다.

녹을 선택할 때:

- 성능에 관심이 많으시군요.
- 스레드 제어를 세분화하려는 경우
- 단순성보다 메모리 안전성을 중요시합니다.

## 가 대 러스트: 나의 솔직한 생각

먼저 공통점을 강조해 보겠습니다. Go와 Rust는 모두 오픈 소스이며 마이크로 서비스 아키텍처와 병렬 컴퓨팅 환경을 지원하도록 설계되었습니다. 둘 다 동시성을 통해 사용 가능한 CPU 코어의 활용도를 최적화합니다.

하지만 결국, 어떤 언어가 최선일까요?

이 문제에 접근하는 방법은 여러 가지가 있다. 어떤 유형의 응용 프로그램을 만들고 싶은지 생각해 보는 것을 추천합니다. Go는 마이크로 서비스 아키텍처를 지원하는 동시에 내장된 동시성 기능을 활용하는 웹 애플리케이션 및 API를 만드는 데 적합합니다.

Rust를 사용하여 웹 API를 개발할 수도 있지만 이 사용 사례를 염두에 두고 설계되지는 않았습니다. Rust는 메모리 안전성에 중점을 두고 특히 상당히 단순한 웹 API의 경우 복잡성과 개발 시간을 증가시킨다. 그러나 코드를 제어하는 양이 많을수록 더 최적화되고 메모리 효율적이며 성능 좋은 코드를 쓸 수 있습니다.

간단히 말해서, Go 대 Rust 논쟁은 사실 단순성 대 보안성의 문제입니다.

자세한 내용은 "Go와 Rust 중 선택"을 참조하십시오.