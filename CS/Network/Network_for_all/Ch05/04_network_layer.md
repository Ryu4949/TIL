

# 5장 - 네트워크 계층: 목적지에 데이터 전달하기

[TOC]



# 네트워크 계층

- 서로 다른 네트워크에 있는 목적지로 데이터를 전달
- 네트워크간의 통신을 가능하게 하는 역할을 함



- 라우터라는 네트워크 장비 필요
- 라우터는 데이터의 목적지가 정해지면 해당 목적지로 가기 위해 어떤 경로로 가는 것이 좋은지를 알려줌
- 하지만 반대로 목적지가 어디있는지 모르면 라우터도 경로를 알려주지 못함
- 즉 네트워크에서도 주소가 필요한데, 이 때 주소를 **IP주소**라고 함



- IP주소를 알고나면 이제 경로를 정할 수 있는데, 이렇게 데이터를 어떤 경로로 보낼지 결정하는 것을 라우팅이라고 함



- 네트워크 계층에는 IP(Internet Protocol)가 있음
- 캡슐화할 때 네트워크 계층에서는 IP헤더를 붙임
  - 여러 정보가 있지만 중요한 것은 출발지와 목적지의 IP주소가 들어있다는 점
- 이렇게 만들어진 것을 IP 패킷이라고 함



# IP 주소의 구조

- IP주소는 인터넷 서비스 제공자(ISP)로부터 받을 수 있음
- IP버전에는 IPv4와 IPv6 두가지가 있음
  - IPv4는 32비트, IPv6은 128비트

- IP주소는 공인IP주소와 사설IP주소가 있음
- 공인 IP 주소는 라우터에만 할당하고 랜(LAN)내에 있는 컴퓨터에는 랜의 네트워크 관리자가 자유롭게 사설 IP주소를 할당하거나 DHCP 기능을 사용하여 주소를 자동적으로 할당
- 공인IP주소와 사설IP주소는 모두 2진수 32비트
  - IP주소 32비트를 8비트 단위로 나누고, 각각을 10진수로 변환
  - 표시는 10진수로 하게되지만 실제로 이루어진건 2진수
- IP주소는 네트워크 ID와 호스트 ID로 나뉘어짐
  - 네트워크ID: 어떤 네트워크인지 나타냄
  - 호스트ID: 해당 네트워크의 어느 컴퓨터인지 나타냄



# IP주소의 클래스

- IP 주소의 클래스는 네트워크 규모에 따라 A~E로 나누어짐
- IP 주소는 네트워크ID와 호스트ID로 구분된다고 했는데, 네트워크 ID를 크게 만들거나 호스트 ID를 작게 만듦으로써 네트워크 크기를 구분할 수 있음
- 일반 네트워크
  - A~C클래스
  - A클래스의 경우 처음8비트가 네트워크 ID, B클래스는 처음 16비트, C클래스는 처음 24비트가 네트워크 ID
  - A클래스의 범위: 1.0.0.0 ~ 127.255.255.255
  - B클래스의 범위: 128.0.0.0 ~ 191.255.255.255
  - C클래스의 범위: 192.0.0.0 ~ 223.255.255.255



# 네트워크 주소와 브로드캐스트 주소의 구조

- 컴퓨터에 할당할 수 없는 IP주소
- 특별한 주소로 컴퓨터나 라우터가 자신의 IP로 사용할 수 없음



- 네트워크 주소: 호스트 ID가 10진수로 0, 2진수로는 00000000인 주소
- 브로드 캐스트 주소: 호스트ID가 10진수로 255, 2진수로는 11111111인 주소



- 네트워크 주소는 전체 네트워크에서 작은 네트워크를 식별하는 데 사용. 쉽게 말해 전체 네트워크의 대표 주소
- 브로드캐스트 주소: 네트워크에 있는 컴퓨터나 장비 모두에게 한 번에 데이터를 전송하는 데 사용되는 전용 IP 주소
  - 이 주소로 데이터를 전송하면 네트워크 안에 있는 모든 컴퓨터가 데이터를 받게 됨



- 결론: 네트워크 주소와 브로드캐스트 주소는 컴퓨터에 설정할 수 없다



# 서브넷의 구조

- 네트워크를 분할하는 것을 의미

- 어떤 클래스의 대규모 네트워크를 작은 네트워크로 분할하여 브로드캐스트로 전송되는 패킷의 범위를 좁힐 수 있음. 이렇게 하면 더 많은 네트워크를 만들 수 있어서 IP 주소를 더 효과적으로 활용 가능
- 이렇게 네트워크를 분할하는 것을 서브넷팅이라고 하고, 분할된 네트워크를 서브넷이라고 함
- 서브넷팅을 하게 되면
  - 기존의 네트워크 ID + 호스트 ID가 네트워크 ID + 서브넷 ID + 호스트 ID로 나누어짐



### 서브넷 마스크

- IP 주소를 서브넷팅하면 어디까지가 네트워크 ID고 어디부터가 호스트 ID 인지 판단이 어려움
- 그럴 때 서브넷 마스크라는 값을 사용
- 서브넷 마스크란 네트워크 ID와 호스트 ID를 식별하기 위한 값

- 보통 네트워크ID+서브넷마스크의 비트수를 슬래시와 함께 표기함
  - ex) 두개 합쳐서 28비트면 /28



# 라우터

- 서로 다른 네트워크와 통신하기 위한 기기
- 라우터로 네트워크를 분리할 수 있음
- 라우터와 달리 스위치와 허브는 네트워크를 분리할 수 없고, 동일한 네트워크에 속하게 됨
- 라우터로 네트워크가 분리된 다음
  - 한 네트워크의 컴퓨터가 다른 네트웤크에 데이터를 전송하려면 라우터의 IP 주소를 설정해야 함
  - 이것을 기본 게이트웨이(default gateway)라고 함
  - 컴퓨터는 다른 네트워크로 데이터를 보낼 때 어디로 보내야 하는지 알지 못하기 때문에 일단 네트워크의 출입구를 지정하고 일단은 라우터로 전송하는 것
- 이렇게 기본 게이트웨이를 설정하고 나서 추가로 라우터의 라우팅이 필요
- 라우팅이란 경로 정보를 기반으로 현재 네트워크에서 다른 네트워크로 최적의 경로를 통해 데이터를 전송하는 것.
- 그리고 이 경로 정보가 등록된 테이블이 라우팅 테이블
- 라우팅 테이블에 등록하는 방법
  - 수동
    - 네트워크 관리자가 수동으로 등록
    - 소규모 네트워크에 적합
    - 수정도 수동
  - 자동
    - 대규모 네트워크에 적합
    - 라우터 간에 경로 정보를 서로 교환하여 라우팅 테이블 정보를 자동으로 수정
    - 이때 라우터 간에 라우팅 정보를 교환하기 위한 프로토콜을 라우팅 프로토콜이라고 함
    - 대표적으로 RIP, OSPF, BGP 등이 있음



---

# Q.

- 멀티캐스트 vs 브로드캐스트
- 옥텟 vs 바이트
  - 일반적으로 동의어이나, 모든 컴퓨터 시스템에서 1바이트가 8비트이지는 않다. 반면 옥텟은 항상 8비트를 의미함
- 