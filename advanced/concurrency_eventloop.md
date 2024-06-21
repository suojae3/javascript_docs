공식문서 링크<br/>
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop)

<br/>

## The event loop

### Runtime concepts

<img src="https://github.com/suojae3/javascript_docs/assets/126137760/ce0fa8f1-fdf9-4721-8dcb-e129655f0b6d" width="450">

<br/>
<br/>

- 자스의 런타임에서는 아래와 같은 일들이 벌어진다
- 함수가 호출되면 **스택**에 프레임을 쌓는다.
- 프레임에 의해 객체들은 만들어지고 힙메모리 공간에 할당된다.
- 처리해야 할 작업, 이벤트, 메시지 등은 메세지 큐에 FIFO 구조로 순차적으로 저장된다.
- 이때 **이벤트루프는 스택이 비어있는지 먼저 확인하고 비어있다면! 메세지 큐에서 대기하고 있는 작업을 serial하게 스택으로 올린다.**

<br/>

### Event Roop

<img src="https://github.com/suojae3/javascript_docs/assets/126137760/a66d4081-952c-40b0-8ab8-1191fcfe52e7" width="450">

<br/>
<br/>

#### Run-to-completion
- 자스의 Run-to-completion 모델은 각 메시지가 완전히 처리된 후에야 다음 메시지가 처리되는 모델이다.
- 덕분에 함수가 실행될 때 다른 코드에 의해 중단되지 않으며, 해당 함수가 완전히 실행된 후에야 다른 코드가 실행된다.
- C 언어에서 함수가 스레드 내에서 실행될 때, 런타임 시스템에 의해 언제든지 중단되어 다른 스레드의 코드가 실행될 수 있어서 race condition 발생할 수 있는데 자스는 언어차원에서 이를 보완했다.
- 단점도 존재한다. 하나의 메시지가 처리되는 데 너무 오래 걸리면, 애플리케이션이 사용자 상호작용(예: 클릭이나 스크롤)을 처리할 수 없게 된다는 점이다.
- 따라서 메시지 처리를 짧게 유지하는 것이 좋다. 가능한 경우, 하나의 메시지를 여러 개의 메시지로 나누어야한다.

<br/>

#### Adding messages

```javascript
const seconds = new Date().getTime() / 1000;

setTimeout(() => {
  // prints out "2", meaning that the callback is not called immediately after 500 milliseconds.
  console.log(`Ran after ${new Date().getTime() / 1000 - seconds} seconds`);
}, 500);

while (true) {
  if (new Date().getTime() / 1000 - seconds >= 2) {
    console.log("Good, looped for 2 seconds");
    break;
  }
}
```








