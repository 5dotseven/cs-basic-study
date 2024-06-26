### TCP와 UDP 헤더 분석

- **Header란**
    - TCP, UDP, IP와 같은 프로토콜은 각자 담당하는 역할이 있으며 보내고자 하는 데이터에 자신의 헤더를 붙여서 데이터 정보를 표현하는데 사용
      <br></br>
- **TCP Header**

  ![Untitled](https://github.com/5dotseven/cs-basic-study/assets/118906074/13a6878e-5ad5-435c-97c1-81aa79ea2817)

    - ***송신 포트 번호(Source Port) - 16bit***
        - 응용 서비스에 따라 포트 번호가 정해져 있는 것도 있지만, 대부분 처음 전송하는 측에서 임의의 번호를 사용합니다.<br></br>
    - **수신 포트 번호(Destinatbn Port) -16 bit**
        - 수신 측의 포트 번호를 나타냅니다.
        - 응용 서비스에 따라 포트 번호가 정해져 있습니다.<br></br>
    - **시퀀스 번호(Sequence Number) -32bit**
        - 순서화된 번호를 나타내며 데이터의 모든 바이트에는 고유한 일련번호 부여
            - 네트워크가 불안하여 패킷을 분실, 지연 등으로 세그먼트가 순서가 어긋나게 도착할 수 있기 때문에 시퀸스 번호의 고유 일련번호를 이용하여 데이터를 올바른 순서로 재배열할 수 있습니다.
        - 통신을 시작하는 양 단의 장비들과 별개로 임의의 번호부터 시작<br></br>
    - ***확인 응답 번호(Acknowledgement Number) -32bit***
        - 상대방이 보낸 세그먼트를 잘 받았다는 것을 알려주기 위한 확인 응답번호를 나타냄<br></br>
    - ***데이터 오프셋(Data Offset) -4bit***
        - 전체 세그먼트 중에서 헤더가 아닌 데이터의 시작되는 위치가 어디부터인지를 표시
        - TCP 헤더 길이를 4바이트 단위로 표시
          TCP 헤더는 최소 20, 최대 60 byte
        - 이 오프셋을 표기할 때는 32bit 워드 단위를 사용하며, 32 bit 체계에서의 1word = 4bytes를 의미
          →이 필드의 값에 4를 곱하면 세그먼트에서 헤더를 제외한 실제 데이터의 시작 위치를 알 수 있습니다.
        - 이 필드에 할당된 4bits를 표현할 수 있는 값의 범위는 0000 ~ 1111, 즉 0 ~ 15word이므로 기본적으로 0 ~ 60bytes의 오프셋까지 표현할 수 있습니다.
          하지만 옵션 필드를 제외한 나머지 필드는 필수로 존재해야 하기 때문에최솟값은 20bytes, 즉 5 word로 고정되어 있습니다.<br></br>
    - ***예약 (Reserved) - 4bit***
        - 앞으로의 확장을 위해 마련되어 있는 필드→사용 x 필드
        - 모두 0으로 표시합니다.
          단, 0이 아닌 패킷을 수신했다고 해서 파기해서는 안됨<br></br>
    - ***컨트롤 플래그(Control Flag) -8bit***
        - 제어 비트(Control bits) 라고도 하며, 세그먼트의 종류를 표시하는 필드입니다.
        - 각 비트는 CWR, ECE, URG, ACK, PSH, RST, SYN, FIN
      <details>
      <summary>상세설명</summary>
      <div markdown="1">
    
              ***CWR(Congestion Window Reduced)***

              송신 측 호스트에 의해 설정되는 것으로, 호스트가 ECE 플래그가 포함된 TCP 세그먼트를

              수신했으며 혼잡 제어 메커니즘에 의해 응답했음을 알리는 역할을 합니다.

              ***ECE(ECN-Echo)***

              ECE 플래그는 ECN-Echo를 의미하는 플래그로, 네트워크의 혼잡 가능성을 미리 탐지하여

              이를 송신측에 알려 전송속도를 조절할 경우 사용됩니다.

                - SYN 플래그가 1일 경우

              TCP 상대가 명시적 혼잡 통지(Explicit Congestion Notification, ECN)가 가능함을 의미합니다.

                - SYN 플래그가 0일 경우

              IP 헤더 셋에 혼잡 경험(Congestion Experienced) 플래그가 설정된 패킷이 정상적인 전송 중에

              수신되었다는 것을 의미합니다.

              ***URG(Urgent Flag)***

              이 비트가 ‘1’인 경우에는 긴급히 처리해야 할 데이터가 포함되어 있다는 것을 의미합니다.

              ***ACK(Acknowledgement Flag)***

              이 비트가 '1'인 경우에는 확인 응답 번호의 필드가 유효하다는 것을 의미합니다.

              ***PSH(Push Flag)***

              수신한 데이터를 바로 상위 애플리케이션에 전달해달라는 플래그로, '0' 인 경우에는

              수신한 데이터를 바로 애플리케이션에게 전달하지 않고 버퍼가 채워질 때까지 기다리고

              이 플래그가 '1'이라면 이 세그먼트 이후에 더 이상 연결된 세그먼트가 없음을 의미하기도 합니다.

              ***RST(Reset Flag)***

              이 비트가 '1' 인 경우에는 연결을 강제적으로 리셋 해달라는 요청의 의미입니다.

              ***SYN(Synchronize Flag)***

              연결 확립에 사용됩니다. 여기에 ‘1’이 설정된 경우에는 연결을 확립하고 싶다는 뜻을

              나타냄과 동시에, 시퀀스 번호의 동기화를 맞추기 위한 플래그입니다.

              ***FIN(Fin Flag)***

              여기에 ‘1’이 설정된 경우에는 연결을 종료하고 싶다는 요청을 의미합니다.
        </div>  
      </details>
  <br></br>
    - ***윈도우 사이즈(Window Size) -16bit***
        - 상대방의 확인 없이 전송할 수 있는 최대 바이트 수를 표시
        - 동일한 TCP 헤더에 포함되는 확인 응답 번호가 가리키는 위치로부터 수신 가능한 데이터 크기(옥텟 수)를 통지하는 데에 사용
          여기서 가리키고 있는 데이터량을 넘어서 송신할 수는 없습니다.
          하지만 윈도우가 0 이라고 통지받은 경우에는 윈도우의 최신 정보를 알아내기 위해 ‘윈도우 프로브’를 송신할 수 있습니다. 이 경우 데이터는 1옥텟이어야 합니다<br></br>
    - ***체크섬(Checksum) -16bit***
        - 헤더와 데이터의 에러를 확인하기 위한 필드
        - 데이터를 송신하는 중에 발생할 수 있는 오류를 검출하기 위한 값<br></br>
    - ***긴급 포인터(Urgent Pointer) -16bit***
        - 현재의 순서 번호부터 긴급포인트에 표시된 바이트까지가 긴급한 데이터임을 표시
        - 긴급 데이터를 처리하기 위한 것으로 컨트롤 플래그 URG가 '1'인 경우에만 유효<br></br>
    - ***옵션(Options) -*** 0~40 byte
        - 최대 세그먼트 사이즈 지정 등 추가적인 옵션이 있을 경우 표시합니다.
        - 옵션 필드는 TCP의 기능을 확장할 때 사용하는 필드들이며, 이 필드는 크기가 고정된 것이 아니라 가변적입니다.
          → 그래서 수신 측의 어디까지가 헤더이고 어디서부터 데이터인지 알기 위해 데이터 오프셋을 사용<br></br>
- **UDP Header**

  ![Untitled (11)](https://github.com/5dotseven/cs-basic-study/assets/118906074/980683c7-7b5e-4684-ab40-525065c3092e)


- ***송신 포트 번호(Source Port) -16bit***
    - 응용 서비스에 따라 포트 번호가 정해져 있는 것도 있지만, 대부분 처음 전송 측에서 임의의 번호 사용<br></br>
- ***수신 포트 번호(Destinatbn Port) -16bit***
    - 응용 서비스에 따라 포트 번호가 정해져 있습니다.<br></br>
- ***패킷 길이(Length) -16bit***
    - 헤더와 데이터를 포함한 전체 길이를 바이트 단위로 표시합니다.
    - 단위는 옥텟 길이로 UDP 데이터그램의 헤더인
      8바이트 ~ 65507바이트 사이의 값<br></br>
- ***체크섬(Checksum)***
    - 헤더와 데이터를 모두 포함한 데이터그램 전체에 대해 오류를 탐지하기 위해 사용
    - UDP 헤더는 에러 복구를 위한 필드가 불필요하기 때문에 TCP 헤더에 비해 간단

출처
- https://go-ahead.tistory.com/7

- https://hiflo.tistory.com/24