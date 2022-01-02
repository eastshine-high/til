다음은 공식 사이트에 게시된 노드 소개글입니다.

```
Node.js는 크롬 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임입니다. **Node.js는
이벤트 기반, 논블로킹 I/O 모델을 사용해 가볍고 효율적입니다.** Node.js의 패키지 생태계인
npm은 세계에서 가장 큰 오픈 소스 라이브러리 생태계이기도 합니다.
```

**노드는 자바스크립트 런타임**입니다. 런타임은 특정 **언어로 만든 프로그램들을 실행할 수 있는 환경**을 뜻합니다. 따라서 노드는 자바스크립트 프로그램을 컴퓨터에서 실행할 수 있게 해줍니다.

기존에는 자바스크립트 프로그램을 인터넷 브라우저(브라우저도 자바스크립트 런타임입니다) 위에서만 실행할 수 있었습니다. 브라우저 외의 환경에서 자바스크립트를 실행하기 위한 여러 가지 시도가 있었으나, 자바스크립트의 실행 속도 문제 때문에 모두 큰 호응을 얻지는 못했습니다.

하지만 2008년 구글 V8 엔진을 사용하여 크롬을 출시하자 이야기가 달라졌습니다. 당시 V8엔진은 다른 자바스크립트 엔진과 달리 매우 빨랐고, 오픈 소스로 코드도 공개되었습니다. 속도 문제가 해결되자 라이언 달(Ryan Dahl)은 2009년 V8 엔진 기반의 노드 프로젝트를 시작했습니다.

![https://i.stack.imgur.com/QRePV.jpg](https://i.stack.imgur.com/QRePV.jpg)

[https://i.stack.imgur.com/QRePV.jpg](https://i.stack.imgur.com/QRePV.jpg)

노드는 V8과 더블러 libuv라는 라이브러리를 사용합니다. V8과 libuv는 C와 C++로 구현되어 있습니다. 여러분이 코딩한 자바스크립트 코드는 노드가 알아서 V8과 libuv에 연결해주므로 노드를 사용할 때 C와 C++는 몰라도 됩니다.

libuv 라이브러리는 노드의 특성인 이벤트 기반, 논블로킹I/O 모델을 구현하고 있습니다. **노드는 스스로 이벤트 기반, 논블로킹I/O 모델을 사용해 가볍고 효율적이라고 표현했습니다.** 

<br>

### 이벤트 기반

**이벤트 기반(event-driven)이란 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식을 의미합니다.** 이벤트로는 클릭이나 네트워크 요청 등이 있을 수 있습니다.

이벤트 기반 시스템에서는 특정 이벤트가 발생할 때 무엇을 할지 미리 등록해두어야 합니다. **이것을 이벤트 리스너(evnet listener)에 콜백(callback) 함수를 등록한다고 표현합니다.** 버튼을 누르면 경고 창을 띄우도록 설정하는 것을 예로 들어 보겠습니다. 클릭 이벤트 리스너에 경고 창을 띄우는 콜백 함수를 등록해두면 클릭 이벤트가 발생할 때마다 콜백 함수가 실행돼 경고 창이 뜨는 것입니다.

노드도 이벤트 기반 방식으로 동작하므로 이벤트가 발생하면 이벤트 리스너에 등록해둔 콜백 함수를 호출합니다. 발생한 이벤트가 없거나 발생했던 이벤트를 다 처리하면 노드는 다음 이벤트가 발생할 때까지 대기합니다.

이벤트 기반 모델에서는 `이벤트 루프`라는 개념이 등장합니다. **여러 이벤트가 동시에 발생했을 때 어떤 순서로 콜백 함수를 호출할지**를 이벤트 루프가 판단합니다.

이벤트 루프 참조 : [https://nodejs.org/ko/docs/guides/event-loop-timers-and-nexttick/](https://nodejs.org/ko/docs/guides/event-loop-timers-and-nexttick/)

<br>

### 논블로킹 I/O

이벤트 루프를 잘 활용하면 오래 걸리는 작업을 효율적으로 처리할 수 있습니다. 오래 걸리는 함수를 백그라운드로 보내서 다음 코드가 먼저 실행되게 하고, 그 함수가 다시 태스크 큐를 거쳐 호출 스택으로 올라오기를 기다리는 방식입니다. 이 방식이 논 블로킹 방식입니다. **논블로킹이란 이전 작업이 완료될 때까지 멈추지 않고 다음 작업을 수행함을 뜻합니다.**

논블로킹 참조 : [https://nodejs.org/ko/docs/guides/blocking-vs-non-blocking/](https://nodejs.org/ko/docs/guides/blocking-vs-non-blocking/)

<br>
<hr>

### Referece

<Node.js 교과서, 조현영, 길벗>