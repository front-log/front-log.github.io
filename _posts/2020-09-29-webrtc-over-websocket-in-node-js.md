---
layout: post
title: "Node.js의 WebRTC over WebSocket"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/09/webrtc-over-websocket-node-js.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/09/webrtc-over-websocket-node-js.png?fit=730%2C487&ssl=1)

## 도입

웹소켓(WebSocket)은 클라이언트 애플리케이션(예: 브라우저, 네이티브 플랫폼 등)과 웹소켓 서버 간의 실시간 통신을 가능하게 하는 프로토콜이다. 전이중 실시간 통신의 경우, 웹소켓 프로토콜은 지연 시간과 오버헤드가 낮기 때문에 HTTP와 비교할 때 권장되는 표준이다.

TCP를 기반으로 구축된 WebSocket은 클라이언트-서버 연결이 항상 열려 있는 양방향 통신 모델을 지원하여 비디오/오디오와 같은 미디어 유형의 원활한 전송을 가능하게 합니다. 연결은 암호화되거나 암호화되지 않을 수 있습니다. 이 규격은 암호화되지 않은 연결의 경우 ws(WebSocket)와 암호화된 연결의 경우 wss(WebSocket Secure)로 URI 체계를 정의합니다.

진행하면서 WebSocket 프로토콜과 노드용 ws WebSocket 라이브러리와 함께 기본 WebSocket 서버를 설정하는 방법에 대해 알아보겠습니다. 지금은 모든 최신 브라우저와 간단한 API를 통해 네이티브 Android 및 iOS 플랫폼에서 사용할 수 있는 프로토콜인 WebRTC를 빠르게 살펴보겠습니다.

## WebRTC 소개

Web RTC(웹 실시간 커뮤니케이션)는 웹을 위한 양방향 및 안전한 실시간 피어 투 피어 통신을 위한 일련의 규칙을 제공하는 프로토콜이다. WebRTC를 사용하면 웹 응용 프로그램 또는 다른 WebRTC 에이전트가 피어 간에 비디오, 오디오 및 기타 종류의 미디어를 전송할 수 있습니다.

WebRTC는 연결 또는 통신 채널을 만든 다음 데이터 및/또는 미디어 유형을 전송/교환하는 목적을 달성하기 위해 여러 프로토콜에 의존합니다. 통신을 조정하기 위해 WebRTC 클라이언트는 메타데이터 정보를 교환하는 데 필요한 일종의 "신호 서버"를 필요로 한다.

Socket.IO–와 ws 기반 Node.js 서버는 WebRTC 클라이언트가 세션 설명과 미디어 정보를 공유하고 실제로 데이터를 교환할 수 있는 영구적이고 실시간 방식으로 신호를 제공하는 대안을 제공한다. 따라서, 이 두 가지 기술은 모두 보완 기술로 사용될 수 있습니다.

> 참고: 기존 기술과의 호환성을 최대화하기 위해 WebRTC 개방형 표준에 의해 신호가 구현되지 않습니다. 우리가 알고 있듯이, 서로 다른 애플리케이션은 서로 다른 신호 프로토콜 또는 서비스를 사용하는 것을 선호할 수 있으므로 코어에 그것을 포함할 필요가 없음을 검증한다.

## WebRTC 프로토콜의 작동 방식

WebRTC 에이전트는 피어와의 연결을 작성하는 방법을 알고 있습니다. 신호 전달은 이 초기 시도를 트리거하여 결국 두 에이전트 간의 통화를 가능하게 합니다. 에이전트는 제안/응답 모델을 사용합니다. 에이전트가 통화를 시작하기 위해 제안을 하고, 다른 에이전트는 제공된 미디어 설명과 호환성 검사를 요청하기 위해 응답합니다.

높은 수준에서 WebRTC 프로토콜은 약 4단계로 작동합니다. 이러한 단계의 경우, 통신은 종속적인 순서로 이루어지며, 여기서 한 단계는 다음 단계를 시작하기 전에 완료되어야 합니다. 다음 네 가지 단계는 다음과 같습니다.

### 1.) 신호

이렇게 하면 데이터를 통신하고 교환하려는 두 개의 WebRTC 에이전트를 식별하는 프로세스가 시작됩니다. 피어가 결국 연결되고 통신할 수 있게 되면 신호 전달은 후드 아래의 다른 프로토콜인 SDP를 이용한다.

세션 설명 프로토콜(일반 텍스트 프로토콜)은 키-값 쌍의 미디어 섹션을 교환하는 데 유용합니다. 이를 통해 둘 이상의 연결 대상 피어 간에 상태를 공유할 수 있습니다.

> 참고: 공유 상태는 피어 간의 연결을 설정하는 데 필요한 모든 매개 변수를 제공할 수 있습니다.

### 2.) 연결

신호를 보낸 후 WebRTC 에이전트는 양방향 피어 투 피어 통신을 달성해야 한다. IP 버전, 네트워크 위치 또는 프로토콜과 같은 여러 가지 이유로 인해 연결을 설정하기가 어려울 수 있지만, WebRTC 연결은 기존의 웹/서버 클라이언트에 비해 더 나은 옵션을 제공합니다. 여기에는 대역폭 감소, 지연 시간 단축, 보안 개선 등이 포함됩니다.

> 참고: WebRTC는 ICE(Interactive Connectivity Setup)를 사용하여 두 에이전트를 연결합니다. ICE는 두 ICE 에이전트 간의 최상의 통신 방법을 찾기 위한 프로토콜입니다. 자세한 내용은 여기에서 확인할 수 있습니다.

### 3.) 확보

모든 WebRTC 연결은 암호화되고 인증됩니다. DTLS 및 SRTP 프로토콜을 사용하여 데이터 계층 간에 원활하고 안전한 통신을 지원합니다. TLS와 마찬가지로 DTLS를 사용하면 세션을 협상한 다음 두 피어 간에 데이터를 안전하게 교환할 수 있습니다. 한편, SRTP는 미디어 교환을 위해 설계되었습니다.

### 4.) 커뮤니케이션

WebRTC는 우리가 무제한의 오디오와 비디오 스트림을 주고 받을 수 있게 해준다. 프로토콜은 옵션이 있기 때문에 특정 코덱과 독립적입니다.

RTP와 RTCP의 두 가지 기존 프로토콜에 의존하며, RTP는 미디어를 전달하는 프로토콜이다. 실시간 영상 전달이 가능하도록 설계됐다. RTCP는 호출에 대한 메타데이터를 전달하는 프로토콜입니다.

> 참고: 대부분의 WebRTC 응용 프로그램의 경우 클라이언트 간에 직접 소켓 연결이 없습니다(동일한 로컬 네트워크에 상주하지 않는 경우). 이러한 문제를 해결하는 일반적인 방법은 Turn 서버를 사용하는 것입니다.
이 용어는 NAT을 중심으로 릴레이를 사용하는 트래버설을 의미하며, 네트워크 트래픽을 릴레이하기 위한 프로토콜입니다. NAT 매핑은 STUN 및 TURN 프로토콜의 도움을 받아 완전히 다른 서브넷의 두 피어가 통신할 수 있도록 지원합니다.

## WebRTC의 사례 사용 사례

WebRTC는 웹 및 모바일 플랫폼에서 실시간 애플리케이션을 위해 일반적이기 때문에 가장 일반적인 사용 사례는 다음과 같습니다.

- 비디오 및 텍스트 채팅
- 분석
- 사회 연결망
- 화면 공유 기술
- 회의(오디오/비디오)
- 생방송
- 파일 전송
- 온라인
- 멀티플레이어 온라인 게임
- 등등...

## WebRTC JavaScript API

WebRTC는 주로 카메라/마이크폰(오디오 및 비디오 모두)에서 사용자 미디어를 가져오거나 획득하는 작업, 채널을 통해 이 미디어 콘텐츠를 전달하는 작업, 마지막으로 채널을 통해 메시지를 보내는 작업 등 세 가지 작업으로 구성됩니다. 이제 각 작업 유형에 대한 요약 설명을 살펴보겠습니다.

### 미디어 스트림 API('getUserMedia')

이 API를 통해 미디어 데이터의 하드웨어 소스에 액세스할 수 있습니다. getUserMedia() 방식은 프롬프트를 통해 사용자의 허락을 받아 카메라 및/또는 마이크를 시스템에 활성화하고 원하는 입력으로 비디오 트랙 및/또는 오디오 트랙을 포함하는 [MediaStream](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream))을 제공한다.

> 참고: 'Navigator.mediaDevices' 읽기 전용 속성은 화면 공유뿐만 아니라 카메라, 마이크와 같은 연결된 미디어 입력 장치에 액세스할 수 있는 'MediaDevices' 개체/인터페이스를 반환합니다.

형식은 다음과 같습니다.

```cpp
const promise = navigator.mediaDevices.getUserMedia(constraints);
```

제약조건 매개변수는 요청된 미디어 유형을 설명하는 비디오와 오디오의 두 멤버가 있는 MediaStreamConstraints 개체이다. 또한 MediaStream의 컨텐츠도 마찬가지입니다.

예를 들어 `minWidth` 및 `minHeight` 기능으로 카메라를 열도록 제약을 설정할 수 있습니다.

```bash
  'video': {
    'width':  {'min': minWidth},
    'height': {'min': minHeight} 
  }
```

또는 마이크로폰에 에코 취소를 설정할 수 있습니다.

```bash
'audio': {'echoCancellation': true},
```

따라서 본질적으로 제약 조건 변수를 다음과 같이 선언할 수 있습니다.

```undefined
const constraints = {
    'video': true,
    'audio': true
}
```

마지막으로 사용자 브라우저에 대한 권한 요청을 트리거하기 위해 `getUserMedia()`를 적용하는 방법에 대한 샘플을 살펴보겠습니다.

```js
const openMediaDevices = async (constraints) => {
    return await navigator.mediaDevices.getUserMedia(constraints);
}

try {
    const stream = openMediaDevices({'video':true,'audio':true});
    console.log('Got MediaStream:', stream);
} catch(error) {
    console.error('Error accessing media devices.', error);
}
```

Media Streams API에서 사용할 수 있는 다른 방법은 다음과 같습니다.

- `장치 열거()`
- `지원되는 제약 조건 가져오기()`
- `디스플레이 미디어 가져오기()`

### RTCeerConnection

RTCerConnection` 인터페이스는 로컬 컴퓨터와 원격 피어 사이의 WebRTC 연결을 나타냅니다. 원격 피어에 연결하고, 연결을 유지 및 모니터링하며, 더 이상 필요하지 않으면 연결을 닫을 수 있는 방법을 제공합니다.

원격 피어에 `RTCeerConnection`이 만들어지면 이들 간에 오디오와 비디오를 스트리밍할 수 있다. 이 계층에서는 getUserMedia()에서 RTCpeerConnection(RTCPeerConnection)

`RTCPeerConnection` 방법은 다음과 같습니다.

- 얼음 후보 추가()
- `동료 정체성`
- 신호국
- `로컬 설명 설정()`
- `원격 설명 설정()`

> 참고: 미디어 스트림은 하나 이상의 미디어 트랙으로 구성되어야 하며, 이 트랙은 미디어를 원격 피어로 전송하려고 할 때 'RTCPeerConnection'에 추가해야 합니다.

### RTC 데이터 채널

RTC Data Channel 인터페이스는 임의 데이터의 양방향 피어 투 피어 전송에 사용할 수 있는 네트워크 채널을 나타냅니다. 데이터 채널을 만들고 원격 피어의 가입을 요청하기 위해 RTCpeerConnection의 createData Channel() 메서드를 호출할 수 있습니다. 예는 다음과 같습니다.

```cpp
const peerConnection = new RTCPeerConnection(configuration);
const dataChannel = peerConnection.createDataChannel();
```

방법에는 다음이 포함됩니다.

- close() – RTCData Channel.close() 메서드는 RTCData Channel을 닫습니다. 어느 피어가 이 메서드를 호출하여 채널 닫기를 시작할 수 있습니다.
- `send() – `RTC Data Channel` 인터페이스의 `send()` 메서드가 데이터 채널을 통해 원격 피어로 데이터를 전송합니다.

노출된 API 집합을 기반으로 하는 WebRTC 프로토콜의 구현은 여기에서 확인할 수 있다. 다양한 WebRTC 실험을 위한 저장소를 포함하고 있다. 예를 들어 `getDisplayMedia() 사용량에 대한 라이브 데모는 여기에서 확인할 수 있습니다.

## 샘플Node.js WebSocket 기반 서버

WebRTC 연결을 작성하려면 클라이언트가 WebSocket 신호 즉, 두 끝점 간의 양방향 소켓 연결을 통해 메시지를 전송할 수 있어야 합니다. Node.js를 통한 WebSocket의 전체 데모 구현은 GitHub에서 Muaz Khan의 도움을 받아 찾을 수 있다. 더 나은 컨텍스트를 위해 `server.js` 파일에서 몇 가지 중요한 항목을 살펴보겠습니다.

첫째, 객체를 인수로 받아들이는 HTTP 서버를 설정할 수 있습니다. 이 개체에는 원활한 연결을 설정하는 데 필요한 보안 키가 포함되어 있어야 합니다. 또한 연결 요청이 있을 때 실행할 콜백 기능과 발신자에게 돌아가기 위한 응답을 지정해야 합니다.

```js
// HTTPs server 
var app = require('https').createServer(options, function(request, response) {
  // accept server requests and handle subsequent responses here 
});
```

그 후 아래와 같이 WebSocket 서버를 설정하고 연결 요청이 들어오면 수신 대기할 수 있습니다.

```coffeescript
// require websocket and setup server.
var WebSocketServer = require('websocket').server;

// wait for when a connection request comes in 
new WebSocketServer({
    httpServer: app, 
    autoAcceptConnections: false 
}).on('request', onRequest);

// listen on app port 
app.listen(process.env.PORT || 9449);

//handle exceptions and exit gracefully 
process.on('unhandledRejection', (reason, promise) => {
  process.exit(1);
});
```

위의 내용에서 알 수 있듯이, 우리는 웹소켓 연결을 받을 때 앱 포트에서 듣고 있습니다. (`요청` 이벤트가 발생할 경우) 연결 요청을 `on Request` 콜백으로 처리합니다. `onRequest` 방법의 내용은 다음과 같습니다.

```js
// callback function to run when we have a successful websocket connection request
function onRequest(socket) {

    // get origin of request 
    var origin = socket.origin + socket.resource;

    // accept socket origin 
    var websocket = socket.accept(null, origin);

    // websocket message event for when message is received
    websocket.on('message', function(message) {
        if(!message || !websocket) return;

        if (message.type === 'utf8') {
            try {
                // handle JSON serialization of messages 
                onMessage(JSON.parse(message.utf8Data), websocket);
            }
            // catch any errors 
            catch(e) {}
        }
    });
    // websocket event when the connection is closed 
    websocket.on('close', function() {
        try {
            // close websocket channels when the connection is closed for whatever reason
            truncateChannels(websocket);
        }
        catch(e) {}
    });
}
```

위의 방법에서, 우리는 메시지가 지정된 형식으로 왔을 때, `메시지` 이벤트가 트리거될 때 실행되는 `온 메시지` 콜백 함수를 통해 그것을 처리한다. 콜백 방법에 대한 자세한 내용은 다음과 같습니다.

```js
// callback to run when the message event is fired 
function onMessage(message, websocket) {
    if(!message || !websocket) return;

    try {
        if (message.checkPresence) {
            checkPresence(message, websocket);
        }
        else if (message.open) {
            onOpen(message, websocket);
        }
        else {
            sendMessage(message, websocket);
        }
    }
    catch(e) {}
}
```

> 참고: 위에 사용된 다른 방법인 'sendmessage', 'checkPresence' 및 데모 웹소켓 서버의 전체 구현에 대한 자세한 내용은 GitHub에서 확인할 수 있습니다.

또한 WebSocket 라이브러리를 사용하려면 WebRTC 클라이언트에서 Node.js 서버의 주소를 지정해야 합니다. 완료 후, 우리는 인터브라우저 WebRTC 오디오/비디오 통화를 할 수 있으며, 여기서 신호는 Node.js WebSocket 서버에서 처리된다.

마지막으로, 강조를 위해 WebSocket 연결에서 주의해야 할 세 가지 기본 방법은 다음과 같습니다.

- `ws.on open` – 연결 시 방출
- `ws.send` – WebSocket 서버로 보내기 이벤트 트리거
- `ws.on message` – 메시지 수신 시 이벤트 발생

## 결론

우리가 본 것처럼, WebRTC API는 미디어 캡처, 오디오 및 비디오 스트림 인코딩 및 디코딩, 전송 및 마지막으로 세션 관리를 포함한다. 브라우저에서의 WebRTC 구현은 WebRTC 기능에 대한 지원 수준이 다르기 때문에 여전히 진화하고 있지만 Adapter.js 라이브러리를 사용함으로써 호환성 문제를 피할 수 있다.

이 라이브러리는 심과 폴리필을 사용하여 이를 지원하는 다양한 환경에서 WebRTC 구현 간의 차이를 해결합니다. 다음과 같이 일반 속성을 가진 `index.html` 파일에 추가할 수 있습니다.

```xml
<script src="https://webrtc.github.io/adapter/adapter-latest.js"></script>
```

우리는 이 링크에서 WebRTC API의 다양한 부분을 보여주는 작은 샘플 컬렉션을 찾을 수 있다. GetUserMedia()it`RTCerConnection()itam 및 RTCDataChannel()을 비롯한 주요 WebRTC API에 대한 구현 세부 정보와 데모를 포함하고 있다.

마지막으로, GitHub의 웹 소켓-오버-노드 js 및 소켓-오버-노드 js에서 웹 실험에 대한 자세한 정보를 찾을 수 있습니다.