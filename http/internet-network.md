# HTTP - 인터넷 네트워크

##1. IP (Internet Protocol)

![image](https://user-images.githubusercontent.com/53935439/208127660-39812103-f9ac-4d1c-86cd-0101c1fb7776.png)

- IP 인터넷 프로토콜의 역할
    - 지정한 IP 주소에 데이터를 전달
    - 패킷이라는 통신 단위 데이터를 전달
- IP 패킷
    - 출발지 IP, 목적지 IP, 전송 데이터, 기타 등

클라이언트에서 서버에 데이터를 전달하기 위해서는 IP 패킷을 통해 전달해야하며 반대도 마찬가지이다. 지나치는 노드는 일정하지 않다.

- IP 프로토콜의 한계
    - 비연결성 : 패킷을 받을 대상이 서비스 불능 상태 또는 대상이 없어도 패킷을 전송
    - 비신뢰성 : 패킷이 사라지거나 순서대로 오지 않을 수 있음
    - 프로그램 구분 : 같은 IP를 사용하는 서버에 통신하는 애플리케이션이 둘 이상일 경우..? (port 로 해결)

##2. TCP

![image](https://user-images.githubusercontent.com/53935439/208128002-ad65e03e-2b9d-4b76-ad77-b4e309b1df5c.png)

IP 프로토콜의 한계를 극복하기 위해 IP위에 TCP나 UDP를 올려서 보완해준다.

TCP의 과정은 아래와 같이 이루어진다.

![image](https://user-images.githubusercontent.com/53935439/208128076-dbe018c7-a28b-4c7e-b50b-ee8a9bad0c27.png)

1. 프로그램이 메시지를 생성하고 Socket 라이브러리를 통해 TCP/IP로 3 way handshake를 실행해 서버와 연결
2. 연결을 확인한 뒤 전송 데이터에 TCP 정보를 한 번 캡슐화
3. IP로 캡슐화
4. 전송


TCP 를 붙여주면 다음과 같은 특징이 있다.
- 데이터 전달 보증 (신뢰성 확보)
- 순서 보장 (순서가 틀리면 그 부분 부터 다시 재전송)

3 way handshake의 과정은 아래와 같다.

![image](https://user-images.githubusercontent.com/53935439/208128197-038276c0-5a15-440f-accc-9767da14640d.png)

##3. UDP

- 사용자 데이터그램 프로토콜
  - TCP는 이미 구축되어있어서 수정 불가능 -> UDP를 사용해 수정
  - 원하는 대로 만들어낼 수 있음
- 기능이 거의 없고 IP와 같음
  - IP + PORT + 체크썸 정도만 추가된 것

##4. PORT

![image](https://user-images.githubusercontent.com/53935439/208135596-19da85ee-6f5b-479a-91a3-e726b5cfe286.png)
![image](https://user-images.githubusercontent.com/53935439/208135674-b0783a58-9674-47db-99cf-9b4ee7ebdfc9.png)
pc에는 1개의 ip 가 할당되어 있지만 여러가지 어플리케이션을 수행하다보면 하나의 ip로 여러 패킷을 받아야 한다. 이때 구분할 수 있는 것이 port이다.
TCP/IP 패킷 정보에는 출발지 PORT, 도착지 PORT 정보가 담겨있기 때문에 서버 쪽에서도 이것을 이용해서 데이터를 전달할 수 있게 된다.

![image](https://user-images.githubusercontent.com/53935439/208135776-e3d1140e-8206-4d80-a0ca-479d9119da15.png)

##5. DNS

DNS란 도메인 네임 시스템이며 사용자들이 기억하기 힘든 IP를 도메인 명으로 변환시켜 매핑해주는 시스템이다.

![image](https://user-images.githubusercontent.com/53935439/208135946-ecac56f9-a09e-4b18-98c7-eda047d39c04.png)

