# JSL Library for Python
JSL은 [WebSocket](https://ko.wikipedia.org/wiki/%EC%9B%B9%EC%86%8C%EC%BC%93) 암호화 통신 프로토콜입니다.
SSL 작동 원리에 기반하여 RSA, AES 암호화 알고리즘을 통해 SSL 인증서 없이도 안전한 통신을 할 수 있도록 해줍니다.

현재 지원되는 서버 언어로는 [Python](python.org), 클라이언트 언어로는 [JavaScript](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)가 있습니다.

해당 프로젝트는 JSL Server를 Python3에서 구축하도록 하는 라이브러리입니다.

# 사용법

### 라이브러리 설치
```
pip install pyJSL
```

### Echo Server 예제
```python3
from pyJSL import *

class simpleEcho(JSLserver):
    def onconn(self):
        print('connected', self.address)

    def onmsg(self, msg):
        print('msg:', msg)
        self.send(msg)  # 클라이언트에게 그대로 메시지 전송

    def onclose(self):
        print('closed', self.address)


# 서버 시작
serve('', 8000, simpleEcho)
```

### 설명
`onconn()`은 클라이언트와 연결되었을 때 실행됩니다.
```python3
def onconn(self):
    print("connected")
```
`onopen()`은 클라이언트와의 JSL 핸드셰이킹이 성공했을 때 실행됩니다.
```python3
def onopen(self):
    print("handshaking was succeeded")
```
`onmsg(msg)`는 클라이언트로부터 메시지를 받았을 때 실행되며, `msg` 인자를 통해 메시지 내용이 전달됩니다.
```python3
def onmsg(self, msg):
    print("client said:", msg)
```
`onclose()`는 클라이언트와의 연결이 끊어졌을 때 실행됩니다.
```python3
def onerror(self):
    print("closed")
```
`self.address`에는 클라이언트의 주소(호스트, 포트)가 담깁니다.

`self.send(msg)` 명령어로 클라이언트에게 메시지를 보낼 수 있습니다. 물론 자동으로 암호화됩니다!
