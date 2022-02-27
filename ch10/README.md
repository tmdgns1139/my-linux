## <u>Chapter 10.</u> 네트워크 관리

* 네트워크에 대해 이해
* 네트워크 정보 설정 방법 숙지
* 네트워크 관리 방법 숙지

### <u>10.1.</u> 네트워크의 개요와 구성 요소

##### <u>10.1.1.</u> 네트워크의 구성

<u>(1)</u> 네트워크의 정의

* 정보 전달/획득, 각종 파일 송수신, 다른 시스템에 있는 자원 공유 등을 위해 인터넷을 활용함
* 네트워크는 우리가 사용하는 인터넷이 동작할 수 있는 기반을 제공함
* 네트워크의 사전적 의미 
  * `Net`(그물)와 `Work`(일)의 합성어
  * 일들이 그물처럼 엮여 있는 것 혹은 그물처럼 서로 엮여서 일하는 것
* 수많은 종류의 컴퓨터 시스템들이 연결되어 데이터 교환 및 작업을 수행하는 통신망
  (컴퓨터 연결에 있어 여러 네트워크 구성 요소들이 있음)
* "Network is computer. Computer is network" - SUN (UNIX 서버 전문업체)
  * 인터넷 보편화로 수많은 컴퓨터가 네트워크를 통해 서로 연결되어 작업을 수행함

<u>(2)</u> 네트워크의 구성

* 데이터 교환 방식 및 위상에 따라 네트워크를 구분함

*Peer-to-Peer (P2P)*

* `Peer` 뜻
  * 사전적 의미: "능력이나 자격이 동등한 사람"
  * 네트워크에서는 동등하거나 비슷한 성능을 가진 컴퓨터
* P2P는 peer 컴퓨터들이 1:1 대응을 이루며 연결된 네트워크임
* P2P는 서버 개입 없이 사용자 컴퓨터들이 직접 연결되어 정보를 교환하는 방식을 채택함
* P2P 서비스에는 메신저 프로그램, Napster 등이 있음
* Peer 연결 방식에 따라 혼합형(Hybrid) P2P, 순수형(Pure) P2P 로 구분함

*Client-Server*

* 클라이언트와 서버가 주종관계를 이루는 네트워크
  * 클라이언트(Client): 서비스를 요구하는 컴퓨터
  * 서버(Server): 서비스를 제공하는 컴퓨터
* 전용 서버 컴퓨터가 필요함 (고비용/고품질 서비스 제공 가능)
* 네트워크 구조에 따라 1계층(1-tier), 2계층(2-tier), 3계층(3-tier) 네트워크 시스템으로 구분

##### <u>10.1.2.</u> TCP/IP 프로토콜

* 프로토콜(Protocol, 통신규약)

  * 컴퓨터들 사이의 의사 소통을 위한 매커니즘
  * 컴퓨터끼리의 자료 전달 및 정보 교환을 위한 정형화된 약속

* TCP/IP 프로토콜은 인터넷에서 사용되는 표준 프로토콜임

  * 1969년, 미국의 프로젝트(ARPA)에 의해 개발됨 (대학과 연구소 사이의 통신 목적)
  * 1983년, UNIX의 표준 프로토콜로 사용됨
  * 그 이후, 인터넷의 표준 프로토콜로 사용됨

* 5계층 구조

  |       계층 이름        | 설 명                               | 프로토콜 예시     |
  | :--------------------: | ----------------------------------- | ----------------- |
  | 응용(Application) 계층 | 서비스 제공 응용 프로그램           | FTP, SSH 등       |
  |  전송(Transport) 계층  | 응용프로그램으로의 데이터 전달 제어 | TCP, UDT          |
  | 네트워크(Network) 계층 | 주소의 관리와 경로의 탐색           | IP, ICMP          |
  |    링크(Link) 계층     | 네트워크 장치 드라이버              | ARP               |
  |  물리(Physical) 계층   | 전송 매체                           | 유선 케이블, 무선 |

  > 전송계층의 TCP 와 네트워크 계층의 IP 를 합해 TCP/IP 라고 부름

* TCP 프로토콜 (전송계층)

  * Transmission Control Protocol 로 인터넷에서 데이터 전송을 제어하는 프로토콜
  * 송신측은 수신측에 SYN 세그먼트를 전송해 연결 요청
  * 수신측은 송신측에 ACK/SYN 세그먼트를 전송함
  * 송신측은 ACK 세그먼트를 수신측에 보내 연결이 성립됨

* IP 프로토콜 (네트워크계층)

  * Internet Protocol 로 송신측의 패킷을 수신측에 전달하는 프로토콜

##### <u>10.1.3.</u> 네트워크의 주소

<u>(1)</u> IP 주소

* IP(Internet Protocal) 주소
  * 네트워크 상의 컴퓨터를 구분/식별하기 위한 주소
  * TCP/IP 에서 패킷 전송 시, 수신측의 IP 주소를 패킷 앞부분에 붙여 전송
  * 라우터(router)는 패킷 앞부분의 IP주소 정보를 사용해 패킷을 최종 목적지로 전달함
* IP 주소의 구성
  * 네트워크 주소 (Network Address): 네트워크레 부여된 주소
  * 호스트 주소 (Host Address): 네트워크에 연결되어 있는 시스템들에 부여된 주소
* IP 주소 관리
  * 대륙별, 국가별 관리 기관으로부터 관리되고 네트워크 관리자에 의해 부여됨
  * NIC(Network Information Center) 라는 IP 주소 관리 기관이 있음
  * NIC 산하의 APNIC(Asia Pacific NIC) 에 속해있는 KRNIC(KoRea NIC) 가 우리나라의 IP 주소를 관리
* IPv4
  * `.`으로 구분되는 4개의 필드로 구성된 IP 주소
  * 각 필드는 1byte 의 크기를 가짐 (즉, IPv4 주소는 4byte, 32bit 의 크기를 가짐)
  * IPv4 방식으로 표현 가능한 IP 주소의 수는 약 43억개 
* IP 주소의 클래스
  * 43억개에 달하는 IP 주소를 개별 관리하는 것은 불가능
  * 효율적인 IP 주소 관리를 위해 IP 주소를 5개 클래스로 나눔 (`A`~`E` 클래스)

*A 클래스* 

* 처음 8bit 은 네트워크 주소로, 나머지 24bit 은 호스트 주소로 사용

* 네트워크 주소의 첫번째 bit 는 `0` 이어야함 (네트워크 주소 범위: `0.0.0.0` ~ `127.0.0.0`)

  ![image](https://user-images.githubusercontent.com/87659486/155450644-47c89179-44ec-402d-b7c3-819844915a95.png)

* 네트워크 생성 가능 개수: 126개

* 네트워크마다 호스트 연결 가능 개수: 약 1,670만개

*B 클래스*

* 처음 16bit 는 네트워크 주소로, 나머지 16bit 은 호스트 주소로 사용

* 네트워크 주소의 처음 2bit 는 `10` 이어야함 (네트워크 주소 범위: `128.0.0.0` ~ `191.255.0.0`)

  ![image](https://user-images.githubusercontent.com/87659486/155451047-8c818d8c-ef6f-4bf5-891f-c6ccea19ff86.png)

* 네트워크 생성 개수: 16,384개 (64 * 256)

* 네트워크마다 호스트 연결 가능 개수: 65,536개

*C 클래스*

* 처음 24bit 은 네트워크 주소로, 나머지 8bit 은 호스트 주소로 사용

* 네트워크 주소의 처음 3bit 은 `110` 이어야함 (네트워크 주소 범위: `192.0.0.0` ~ `223.255.255.0`)

  ![image](https://user-images.githubusercontent.com/87659486/155451506-ceee28a7-adaf-43f8-a2de-9c35072960f9.png)

* 네트워크 생성 개수: 약 210만개 (32 * 256 * 256)

* 네트워크마다 호스트 연결 가능 개수: 256개

*D 클래스*

* 처음 8bit 은 네트워크 주소로, 나머지 24bit 은 호스트 주소로 사용

* 네트워크 주소의 처음 4bit 은 `1110` 이어야함 (네트워크 주소 범위: `224.0.0.0` ~ `239.0.0.0`)

  ![image](https://user-images.githubusercontent.com/87659486/155452314-a9fc8b16-7745-4dcd-be78-069856846e47.png)

* 네트워크 생성 개수: 16개

* 네트워크마다 호스트 연결 가능 개수: 1,670만개

* 거의 사용되지 않는 클래스임

*E 클래스*

* 처음 8bit 은 네트워크 주소로, 나머지 24bit 은 호스트 주소로 사용

* 네트워크 주소의 처음 4bit 은 `1111` 이어야함 (네트워크 주소 범위: `240.0.0.0` ~ `255.0.0.0`)

  ![image](https://user-images.githubusercontent.com/87659486/155452373-4dff00c0-9800-4d22-aff1-ee93e12e434d.png)

* 네트워크 생성 개수: 16개

* 네트워크마다 호스트 연결 가능 개수: 1,670만개

* 거의 사용되지 않는 클래스임

<u>(2)</u> 포트(Port) 번호

* 네트워크 상의 서버는 클라이언트가 요구하는 다양한 서비스를 제공함

  * 파일 전송(FTP), 원격 접속(SSH), 웹(http) 서비스 등등

* 서버는 포트를 통해 클라이언트의 요구를 처리할 수 있는 데몬 프로세스에 전달

* 즉, 포트는 네트워크 서비스를 수행하는 경로이고 번호로 표현됨

  * 파일 전송(FTP) 서비스는 21번 포트를 사용
  * 원격 접속(SSH) 서비스는 22번 포트를 사용
  * 웹(http) 서비스는 80번 포트를 사용

* `naver.com:80`: `naver.com` 이라는 도메인 이름을 가진 웹서버의 80번 포트를 사용함

* `/etc/services`: 시스템이 사용하는 포트 및 포트 번호를 저장한 파일

  ```bash
  $ cat /etc/services
  ```

  > 국제 표준에 따른 포트번호를 등록해둔 것
  >
  > 사용자가 새로운 포트를 쓰려는 경우, 위 파일에 없는 포트 번호를 써야함

<u>(3)</u> 브로드캐스트(Broadcast) 주소

* 브로드캐스트: 송신 호스트가 전송한 데이터를 한 네트워크에 연결된 모든 호스트에 정보를 전달하는 것
* 브로드캐스트 주소: 브로드캐스트를 수행하는 IP 주소
* 즉, 브로드캐스트 주소에 정보를 전달하면 해당 네트워크 내에 있는 모든 시스템에 정보가 전달됨
* 브로트캐스트 주소의 지정: 호스트 주소의 마지막 1byte 가 `255`임
  * **예.** IP 주소가 `192.168.56.78` 인 호스트가 속한 네트워크의 브로드캐스트 주소는 `192.168.56.255`

<u>(4)</u> 네트워크(Network) 주소

* 네트워크에 부여되는 IP 주소
* 네트워크의 구분/식별이 필요함
  * `A` 네트워크에 속한 컴퓨터에서 `B` 네트워크에 속한 컴퓨터로 정보를 전달하는 상황
  * 정보를 전달하려면 `A`네트워크와 `B`네트워크를 구분/식별할 수 있어야함
* 네트워크 주소의 지정: 호스트 주소의 마지막 1byte 가 `0`임
  * **예.** IP 주소가 `192.168.56.78` 인 호스트가 속한 네트워크 주소는 `192.168.56.0`

<u>(5)</u> 게이트웨이(Gateway) 주소

* 게이트웨이: 네트워크의 출입문 역할을 수행하는 컴퓨터 혹은 네트워크 장비
* 라우터(router)의 역할: 라우팅(routing) 및 서로 다른 네트워크 간의 중계 역할 수행
* 라우팅(routing): 인터넷으로 연결된 컴퓨터들 사이의 데이터 전송 경로를 지정해주는 것
  * 데이터 전달의 이정표 역할을 수행
  * 데이터가 네트워크 내부 컴퓨터에 전달되는지 외부 네트워크의 컴퓨터에 전달되는지에 대한 흐름 제어
  * 같은 네트워크 내의 컴퓨터 송수신은 게이트웨이 없이 이더넷 인터페이스를 통해 전달 가능
  * 서로 다른 네트워크에 속한 컴퓨터 송수신의 경우, 데이터가 반드시 게이트웨이를 거침
  * 게이트웨이를 거치는 라우팅의 경우, 라우팅 테이블(routing table)에 의해 라우팅이 지정됨
* 출입문이 필요한 시나리오
  * `A` 네트워크에 속한 컴퓨터에서 `B` 네트워크에 속한 컴퓨터로 정보를 전달하는 상황
  * `A` 네트워크의 출입문으로 정보가 나오고 `B` 네트워크의 출입문으로 정보가 들어감
* 게이트웨이 주소의 지정: 호스트 주소의 마지막 1byte 가 `1`임
  * **예.** IP 주소가 `192.168.56.78` 인 호스트가 속한 네트워크의 게이트웨이 주소는 `192.168.56.1` 

<u>(6)</u> 서브넷 마스크(Subnet Mask) 주소

* 서브네팅(Subnetting): 하나의 네트워크를 여러 네트워크로 나누는 것
* 서브넷(Subnet): 서브네팅(Subnetting)에 의해 나눠진 각각의 네트워크
* 서브넷 마스크
  * 네트워크를 1개 이상으로 분할하기 위한 기준
  * 네트워크가 어떻게 분할되어 있는지, 네트워크/게이트웨이/브로드캐스트 주소 등에 대한 정보 제공
  * IP 주소와 AND 연산을 통해 네트워크 IP 주소를 얻어냄
  * `IP주소/24`: 서브넷 마스크 주소는 첫 24bit 이 모두 `1`이고 나머지는 0임을 의미 (`255.255.255.0`)
* <u>Q.</u> 네트워크를 왜 나누어 사용하나?
* 네트워크의 효율적인 운영
  * 이더넷을 사용한 LAN(Local Area Network): 네트워크를 구성하는 가장 보편적인 형태
  * 근거리 데이터 전송을 위해 두 호스트 사이에 보조 전송기기(리피터 등)가 있어야함
    (이는 이더넷의 특성과 관련됨)
  * 네트워크 상의 호스트들이 넓게 분포해있는 경우, 네트워크 분할을 통해 위 문제를 해결함
* 보안성 강화
  * 서로 다른 네트워크에 속해있는 호스트에 접근하는 것은 어려움
* `192.168.56.0` 네트워크를 1개로 분할
  * 서브넷 마스크 주소: `255.255.255.0` (`1111111`, `11111111`, `11111111`, `00000000`)
  * 서브넷1
    * Network: `192.168.56.0`
    * Gateway: `192.168.56.1`
    * Broadcast: `192.168.56.255`
    * 연결된 호스트 수: 256개 (`192.168.56.2` ~ `192.168.56.254`)
* `192.168.56.0` 네트워크를 2개로 분할
  * 서브넷 마스크 주소: `255.255.255.128` (`11111111`, `11111111`, `11111111`, `10000000`)
  * 서브넷1
    * Network: `192.168.56.0`
    * Gateway: `192.168.56.1`
    * Broadcast: `192.168.56.127`
    * 연결된 호스트 수: 128개 (`192.168.56.2` ~ `192.168.56.127`)
  * 서브넷2
    * Network: `192.168.56.128`
    * Gateway: `192.168.56.129`
    * Broadcast: `192.168.56.255`
    * 연결된 호스트 수: 128개 (`192.168.56.130` ~ `192.168.56.254`)

<u>(7)</u> DNS 주소

* 인터넷으로 특정 호스트에 접근하려면 해당 IP 주소를 알아야함

* 그러나 3자리 숫자 4개를 외우는 것은 어려운 일임

* 사람은 문자로 구성된 도메인 주소(Domain Name)를 사용해 특정 호스트에 접근하고 싶어했음

* 문제점은 컴퓨터는 문자로 표현된 도메인 주소를 인식하지 못함

* 도메인 주소를 IP 주소로 변환하는 것으로 문제점을 해소하였음

* 도메인 이름 서버(Domain Name Server)는 도메인 주소를 IP 주소로 변환해줌
  (DNS 또한 네트워크 상의 호스트이므로 IP 주소를 가짐 - DNS 주소라고 부름)

* 모든 호스트는 자신의 도메인 주소를 IP 주소로 변환해주는 로컬 DNS 를 반드시 가짐

* 도메인의 체계 (계층적인 트리 구조)

  * 루트(Root) 도메인
  * 최상위 도메인(Top-Level Domain), TLD
    * 루트 도메인의 하위 도메인
    * Generic Domain(`.com`, `.net` 등) 관리
    * National Domain(`.kr`, `.uk` 등) 관리
  * 2단계 도메인(Second-Level Domain), SLD 
    * TLD 의 하위 도메인
    * 호스트의 성격을 나타내는 도메인(`.co`, `.ac` 등) 관리

* **예.** 클라이언트가 http 프로토콜을 이용해 `www.abc.com` 이라는 호스트에 접근하는 과정

  ![image](https://user-images.githubusercontent.com/87659486/155471657-5b66c804-b2d1-4ded-80c5-2c619695b604.png)

  1. 클라이언트는 자신의 로컬 DNS에 "www.abc.com"(이하 `abc`) 이름을 갖는 호스트의 IP 주소를 요청

  2. 로컬 DNS 캐시에 `abc`의 IP 주소가 없다면 루트 DNS 에 `abc`의 DNS 주소를 요청
     * 로컬 DNS 캐시에 `abc`의 IP 주소가 있다면 바로 해당 주소를 반환하고  `9.`로 이동

  3. 루트 DNS 에 `abc`의 DNS 주소가 없다면 `.com` 도메인을 관리하는 TLD 의 DNS 주소를 반환
     * 루트 DNS 에 `abc`의 DNS 주소를 있으면 해당 주소를 반환하고 `6.`로 이동

  4. TLD 에 `abc`의 DNS 주소를 요청
  5. `abc`의 IP 주소 혹은 DNS 주소를 반환
     * `abc`의 IP 주소를 가진 경우, 해당 주소를 반환하고 `9.`로 이동
     * `abc`의 DNS 주소만 가진 경우, 해당 주소를 반환하고 `6.`으로 이동
  6. `abc`의 DNS 에 `abc`의 IP 주소를 요청
  7. `abc`의 IP 주소를 클라이언트 로컬 DNS 에 반환
     (`abc` 로컬 DNS 는 당연히 `abc`의 IP 주소를 알고 있음)
  8. `abc`의 IP 주소를  클라이언트 시스템에 반환
  9. 최종적으로 얻어낸 IP 주소를 이용해 웹 서버 시스템에 접근/요청

<u>(8)</u> MAC 주소

* MAC(Media Access Control) 주소: HW에 대한 주소
* 시스템에 장착된 네트워크 인터페이스 카드에 부여된 주소
* 해당 카드에 부여되는 MAC 주소는 카드 생성 시점에 부여됨
* IP 주소와 마찬가지로 유일한 값을 가져 장치를 식별하는 주소로 사용됨
* `:` 혹은 `-` 기호로 구분되는 6개의 2자리 16진수로 구성됨
* 앞 3자리 수는 제조사의 번호를 나타냄 (제조사 번호는 국제 표준 기구인 IEEE 에서 지정함)
* 뒤 3자리 수는 일련번호를 나타냄

### <u>10.2.</u> 네트워크 정보 설정

##### <u>10.2.1.</u> 네트워크 관리의 개요 

* 관리자는 네트워크 관리 도구를 통해 네트워크 정보의 지정/수정 가능
* 네트워크 정보는 각종 네트워크 서비스를 이용하는데 필수적인 요소임
* 네트워크 정보의 부여
  * 시스템 관리자로부터 고정적인 정보를 부여받음
  * 자동할당 프로토콜에 의해 유동적인 정보를 부여받음
* 고정적인 네트워크 정보를 사용해야할 경우, 현재 네트워크 정보를 정확히 알아야함
  * IP 주소, 서브넷 마스크 주소, 브로드캐스트 주소, 게이트웨이 주소, DNS 주소 등

<u>(1)</u> 네트워크 관리자 데몬 (`network-manager` 패키지)

* 우분투에서 네트워크 관리자 데몬을 이용해 네트워크 정보를 지정 혹은 수정함
  (네트워크 관리자는 우분투의 기본 네트워킹 데몬임)

* 네트워크 관리자(network-manager) 패키지 설치

  ```bash
  $ aptitude show network-manager
  Package: network-manager          
  Version: 1.10.6-2ubuntu1.4
  State: not installed
  $ sudo apt-get install network-manager
  ```

* 네트워크 관리자 데몬 상태 확인

  ```bash
  $ sudo systemctl status NetworkManager.service 
  Unit NetworkManager.service could not be found.
  $ sudo apt-get install network-manager
  ...
  $ systemctl status NetworkManager.service 
  ● NetworkManager.service - Network Manager
     Loaded: loaded (/lib/systemd/system/NetworkManager.service; enabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: man:NetworkManager(8)
  ```

  ```bash
  $ sudo systemctl start NetworkManager.service 
  $ systemctl status NetworkManager.service 
  ● NetworkManager.service - Network Manager
     Loaded: loaded (/lib/systemd/system/NetworkManager.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-02-24 08:50:06 UTC; 4s ago
  ```

* 네트워크 정보 관리

  * CLI: `nmcil` 명령어 사용
  * GUI: GNOME [설정]-[네트워크] 사용 혹은 터미널에서 `nm-connection-editor` 명령어 실행

<u>(2)</u> 네트워크 설정 그래픽 도구 (GUI 환경)

* GNOME [설정]-[네트워크]

  * GCP 클라우드에서 대여받은 시스템은 CLI 만 가능해서 못함

* `nm-connection-editor` 명령어

  * `network-manager-gnome` 패키지 설치

    ```bash
    $ sudo apt-get install network-manager-gnome
    ```

  * `nm-connection-editor` 명령어 실행

    ```bash
    $ nm-connection-editor
    Unable to init server: Could not connect: Connection refused
    (nm-connection-editor:2026): Gtk-WARNING **: 08:54:40.434: cannot open display:
    ```

    > GCP 클라우드 컴퓨터를 대여해서 쓰기 때문에 실행 불가능

##### <u>10.2.2.</u> 네트워크 정보 설정

<u>(1)</u> 네트워크 정보 설정 (`nmcli` 명령어)

* `nmcli`(Network Manager CLI) 네트워크 관리자 패키지에 포함된 네트워크 정보 설정 명령어임

* 네트워크 상태 출력, 네트워크 서비스의 시작/종료, 각종 네트워크 정보 지정

* 명령어 형식

  ```bash
  $ nmcli [내부_명령] [서브_명령]
  ```

* 자주 사용되는 내부/서브 명령

  |     내부 명령     | 서브 명령 및 설명                                            |
  | :---------------: | ------------------------------------------------------------ |
  |  `gen`(General)   | `status`: 네트워크 전체 상태 출력<br />`hostname`: 호스트 이름 출력 |
  |  `net`(Network)   | `on`: 일시정지된 네트워크 다시 시작<br />`off`: 네트워크 일시 정지<br />`connectivity`: 네트워크 연결 정보 출력 |
  |   `dev`(Device)   | `status`: 네트워크 인터페이스 간단한 내용 출력<br />`show`: 네트워크 인터페이스 상세 내용 출력 |
  | `con`(Connection) | `show`: 네트워크 연결 프로파일 내용 출력<br />`up`: 네트워크 연결 시작<br />`down`: 네트워크 연결 종료<br />`add`: 새로운 네트워크 연결 생성<br />`modify`: 네트워크 설정 정보 변경<br />`delete`: 네트워크 연결 제거 |

  > 단독 사용: 현재 네트워크 연결 프로파일의 이름(유선연결1)과 네트워크 정보 출력

*`gen` 내부 명령*

* 네트워크 상태 출력 (`status` 서브 명령)

  ```bash
  $ sudo nmcli gen status
  STATE         CONNECTIVITY  WIFI-HW  WIFI     WWAN-HW  WWAN    
  disconnected  unknown       enabled  enabled  enabled  enabled 
  ```

* 호스트이름 출력 (`hostname` 서브 명령)

  ```bash
  $ sudo nmcli gen hostname
  aiteen-back
  ```

*`net` 내부 명령*

* 네트워크 정지 (`off` 서브 명령)

  ```bash
  $ sudo nmcli net off
  ```

* 네트워크 재시작 (`on` 서브 명령)

  ```bash
  $ sudo nmcli net on
  ```

* 네트워크 및 인터넷 연결 상태 출력 (`connectivity` 서브 명령)

  ```bash
  $ sudo nmcli net connectivity
  unknown
  ```

  | `net` 출력 결과 |                                      |
  | :-------------: | ------------------------------------ |
  |     `none`      | 네트워크 연결 안된 상태              |
  |    `limited`    | 네트워크 연결, 인터넷 연결 안된 상태 |
  |     `full`      | 네트워크 연결, 인터넷 연결 상태      |
  |    `unknown`    | 네트워크 식별 불가능 상태            |

*`dev` 내부 명령*

* 네트워크 인터페이스 상태 출력 (`status` 서브 명령)

  ```bash
  $ sudo nmcli dev status
  DEVICE  TYPE      STATE      CONNECTION 
  ens4    ethernet  unmanaged  --         
  lo      loopback  unmanaged  --  
  ```

* 네트워크 정보(IP/게이트웨이 주소 등) (`show` 서브 명령)

  ```bash
  $ sudo nmcli dev show
  GENERAL.DEVICE:                         ens4
  GENERAL.TYPE:                           ethernet
  ...
  IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 256
  
  GENERAL.DEVICE:                         lo
  GENERAL.TYPE:                           loopback
  ...
  IP6.ROUTE[1]:                           dst = ::1/128, nh = ::, mt = 256
  ```

*`con` 내부 명령*

* 네트워크 연결 프로파일 내용 출력 (`show` 서브 명령)

  ```bash
  $ sudo nmcli con show
  NAME  UUID  TYPE  DEVICE
  ```

* 네트워크 연결 중지 (`down` 서브 명령)

  ```bash
  $ sudo nmcli con down 연결_프로파일_이름
  ```

* 중지된 네트워크 연결 시작 (`up` 서브 명령)

  ```bash
  $ sudo nmcli con up 연결_프로파일_이름
  ```

* 아래 명령어 형식 및 속성을 지정하여 새로운 연결 프로파일 추가 (`add` 서브 명령)

  ```bash
  $ nmcli con add 속성 속성값
  ```

  |   속 성    | 설 명                                                        |
  | :--------: | ------------------------------------------------------------ |
  |   `type`   | 네트워크 유형 (주로 `ethernet` 으로 지정)                    |
  | `con-name` | 새로 생성할 연결 이름 지정                                   |
  |  `ifname`  | 네트워크 인터페이스 이름 지정<br />(우분투에서는 `ens33` 을 지정) |
  |   `ip4`    | 고정 IP 지정                                                 |
  |   `gw4`    | 게이트웨이 주소 지정                                         |

  > 유동 IP 주소를 사용할 경우, `ip4` 와 `gw4` 는 지정 안함
  >
  > 프로파일 추가 후, `$ nmcli con up ...` 명령어로 네트워크 인터페이스에 연결함

*`modify` 서브 명령*

* 아래의 명령어 형식 및 속성을 지정하여 기존 연결 프로파일의 내용을 수정

  ```bash
  $ nmcli con modify 연결_프로파일 ipv4.속성 변경값
  ```

  > 속성
  >
  > * `address`: IP 주소 설정
  > * `dns`: DNS 주소 설정
  > * `gateway`: 게이트웨이 주소 설정
  > * `route`: 네트워크의 라우팅 정보 설정

* 프로파일 설정 후, 네트워크 인터페이스 연결을 활성화해야함

  ```bash
  $ nmcli con up 연결_프로파일 ifname 네트워크_인터페이스
  ```

*`delete` 서브 명령*

* 기존의 네트워크 연결 프로파일을 삭제함

* 명령어 형식

  ```bash
  $ nmcli con delete 연결_프로파일
  ```

<u>(Exercise)</u>

1. 아래와 같이 네트워크 연결 프로파일을 추가

   * 고정 IP 주소: `192.168.20.138`
   * 게이트웨이 주소: `192.168.20.2`
   * 서브넷 마스크 주소: `255.255.255.0`
   * 네트워크 인터페이스 이름: `ens4`
   * 연결 프로파일 이름: `유선 연결 3`

   ```bash
   $ sudo nmcli con add \
   	type ethernet \
   	ip4 192.168.20.138/24 \
   	gw4 192.168.20.2 \
   	ifname ens4 \
   	con-name "유선 연결 3"
   Connection '유선 연결 3' (c9acbf35-5392-4688-bc43-*) successfully added.
   ```

2. 시스템에 생성되어 있는 연결 프로파일 출력

   ```bash
   $ sudo nmcli con show
   NAME         UUID                     TYPE      DEVICE 
   유선 연결 2  e590714e-054f-48df-baf5-*  ethernet  --     
   유선 연결 3  c9acbf35-5392-4688-bc43-*  ethernet  -- 
   ```

3. 추가했던 연결 프로파일을 네트워크 인터페이스에 연결

   ```bash
   $ sudo nmcli con up "유선 연결 2" ifname ens33
   Error: device 'ens33' not compatible with connection '유선 연결 2'.
   ```

   ```bash
   $ sudo nmcli con up "유선 연결 2" ifname ens4
   Error: Connection activation failed: Connection '유선 연결 3' is not available on the device ens4 at this time.
   ```

   ```bash
   $ sudo nmcli con show
   NAME       UUID                       TYPE      DEVICE 
   유선 연결 2  e590714e-054f-48df-baf5-*  ethernet  --     
   유선 연결 3  c9acbf35-5392-4688-bc43-*  ethernet  --  
   ```

4. 현재의 네트워크 정보를 출력

   ```bash
   $ sudo nmcli 
   ens4: unmanaged
           "Red Hat Virtio network device"
           ethernet (virtio_net), 42:01:0A:B2:00:04, hw, mtu 1460
   
   lo: unmanaged
           "lo"
           loopback (unknown), 00:00:00:00:00:00, sw, mtu 65536
   
   Use "nmcli device show" to get complete information about known devices and
   "nmcli connection show" to get an overview on active connection profiles.
   
   Consult nmcli(1) and nmcli-examples(5) manual pages for complete usage details.
   ```

<u>(2)</u>  네트워크 정보 설정 (`ip` 명령어)

* 명령어 형식

  ```bash
  $ ip 내부_명령 서브_명령
  ```

*`addr` 내부 명령*

* IP 주소 설정(네트워크 인터페이스에 추가/제거) 및 정보 출력을 위한 `ip` 명령어의 내부 명령어

* 네트워크 인터페이스 정보 출력 (`show` 서브 명령)

  ```bash
  $ sudo ip addr show
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host 
         valid_lft forever preferred_lft forever
  2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1460 qdisc mq state UP group default qlen 1000
      link/ether 42:01:0a:b2:00:04 brd ff:ff:ff:ff:ff:ff
      inet 10.178.0.4/32 scope global dynamic ens4
         valid_lft 2535sec preferred_lft 2535sec
      inet6 fe80::4001:aff:feb2:4/64 scope link 
         valid_lft forever preferred_lft forever
  ```

  ```bash
  $ sudo ip addr show lo
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host 
         valid_lft forever preferred_lft forever
  ```

  > `lo` (local loopback)
  >
  > * 네트워크 입출력 기능 확인을 위한 시스템 내부 인터페이스
  >
  > * 시스템이 자기 자신과 통신하기 위한 가상의 이더넷 장치임
  >
  > * 표현된 IP 주소: `127.0.0.1`
  >
  > * 실제로 외부 네트워크에 연결되지 않는 SW 입출력 주소
  >
  > * `lo` 비정상 작동을 이더넷 카드 손상과 연관 짓는 경우가 많음

* 네트워크 인터페이스에 IP 주소 추가 (`add` 서브 명령) - dev 속성을 추가 지정

  ```bash
  $ sudo ip addr add 192.168.20.140/24 dev ens4
  ```

  > `255.255.255.0` 서브넷 마스크를 갖는 `192.168.20.140` IP 주소를 `ens4` 네트워크 인터페이스에 추가

  ```bash
  $ sudo ip addr show ens4
  2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1460 qdisc mq state UP group default qlen 1000
  		...
      inet 192.168.20.140/24 scope global ens4
         valid_lft forever preferred_lft forever
      ...
  ```

* 네트워크 인터페이스에서 IP 주소 제거 (`del` 서브 명령)

  ```bash
  $ sudo ip addr del 192.168.20.140/24 dev ens4
  $ sudo ip addr show | grep 192.168.20.140/24
  $
  ```

*`link` 내부 명령*

* 지정한 네트워크 인터페이스 활성화/비활성화

  * 네트워크 지정 (`set` 서브 명령)
  * 활성화(`up` 서브 명령), 비활성화(`down` 서브 명령)

* `ens4` 네트워크 인터페이스 비활성화

  ```bash
  $ sudo ip link set ens4 down
  ```

* `ens4` 네트워크 인터페이스 활성화

  ```bash
  $ sudo ip link set ens4 up
  ```

<u>(3)</u>  네트워크 정보 설정 (`ifconfig` 명령어)

* 네트워크 인터페이스 정보 출력/변경을 위해 리눅스에서 전통적으로 사용된 명령어

* `net-tools` 패키지 설치

  ```bash
  $ sudo aptitude net-tools
  ```

  ```bash
  $ sudo apt-get install net-tools
  ```

* 명령어 형식

  ```bash
  $ ifconfig [네트워크_인터페이스_이름]
  ```

* 시스템의 모든 네트워크 인터페이스 정보 출력

  ```bash
  $ ifconfig
  ens4: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1460
          inet 10.182.0.5  netmask 255.255.255.255  broadcast 0.0.0.0
          inet6 fe80::4001:aff:feb6:5  prefixlen 64  scopeid 0x20<link>
          ether 42:01:0a:b6:00:05  txqueuelen 1000  (Ethernet)
          RX packets 5014  bytes 135059262 (135.0 MB)
          RX errors 0  dropped 0  overruns 0  frame 0
          TX packets 2886  bytes 366263 (366.2 KB)
          TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
  lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
          inet 127.0.0.1  netmask 255.0.0.0
          inet6 ::1  prefixlen 128  scopeid 0x10<host>
          loop  txqueuelen 1000  (Local Loopback)
          RX packets 176  bytes 17659 (17.6 KB)
          RX errors 0  dropped 0  overruns 0  frame 0
          TX packets 176  bytes 17659 (17.6 KB)
          TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
  ```

* 시스템 내부의 특정 네트워크 인터페이스 정보 출력

  ```bash
  $ ifconfig ens4
  ens4: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1460
          inet 10.182.0.5  netmask 255.255.255.255  broadcast 0.0.0.0
          inet6 fe80::4001:aff:feb6:5  prefixlen 64  scopeid 0x20<link>
          ether 42:01:0a:b6:00:05  txqueuelen 1000  (Ethernet)
          RX packets 5035  bytes 135060832 (135.0 MB)
          RX errors 0  dropped 0  overruns 0  frame 0
          TX packets 2907  bytes 369129 (369.1 KB)
          TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
  ```

* 네트워크 활성화 (`up`)

  ```bash
  $ ifconfig 네트워크_인터페이스_이름 up
  ```

* 네트워크 비활성화 (`down`)

  ```bash
  $ ifconfig 네트워크_인터페이스_이름 down
  ```

<u>(4)</u> 게이트웨이 정보 설정 (`route` 명령어)

* 라우팅 테이블의 내용을 출력하거나 변경하기 위한 명령어

  * 라우팅 테이블 내용을 변경한다는 것은 곧 게이트웨이의 정보를 수정한다는 것을 의미함

* 명령어 형식

  |        구 분         | 기능 및 설명                                                 |
  | :------------------: | ------------------------------------------------------------ |
  |       네트워크       | 추가: `$ route add -net 네트워크주소 netmask 서브넷마스크 dev 인터페이스명`<br />삭제: `$ route del -net 네트워크주소 netmask 서브넷마스크 [dev 인터페이스명]` |
  |        호스트        | 추가: `$ route add -host 호스트주소 dev 인터페이스명 `<br />삭제: `$ route del -host 호스트주소` |
  | 기본<br />게이트웨이 | 추가: `$ route add default gw 게이트웨이주소 dev `<br />삭제: `$ route del default gw 게이트웨이주소` |

* 현재 라우팅 테이블의 내용 출력

  ```bash
  $ route
  Kernel IP routing table
  Destination  Gateway   Genmask         Flags Metric Ref  Use Iface
  default      _gateway  0.0.0.0         UG    100    0      0 ens4
  _gateway     0.0.0.0   255.255.255.255 UH    100    0      0 ens4
  ```

  |     구분      | 설 명                                                        |
  | :-----------: | ------------------------------------------------------------ |
  | `Destination` | 라우팅 대상 네트워크 혹은 호스트의 주소 (default: `0.0.0.0`) |
  |   `Genmask`   | 대상 네트워크에 대한 서브넷 마스크 값 <br />(호스트는 `255.255.255.0`, 게이트웨이는 `0.0.0.0`) |
  |    `Flags`    | 네트워크의 상태 <br />(`U`: 네트워크에 대한 경로가 동작 중, `G`: 게이트웨이를 사용함) |
  |   `Metric`    | 네트워크까지의 거리를 홉(hop) 단위로 표현한 것<br />( 홉(hop): 패킷이 출발지에서 목적지까지 가는데 경유한 네트워크 장비 개수 ) |
  |     `Ref`     | 해당 경로나 라우터를 참조한 수 (리눅수에서는 안씀)           |
  |     `Use`     | 해당 경로나 라우터를 탐색한 수                               |
  |    `Iface`    | 테이터가 전달되는 인터페이스 이름을 나타냄                   |

  ```bash
  $ route -n
  Kernel IP routing table
  Destination  Gateway     Genmask         Flags Metric Ref  Use Iface
  0.0.0.0      10.182.0.1  0.0.0.0         UG    100    0      0 ens4
  10.182.0.1   0.0.0.0     255.255.255.255 UH    100    0      0 ens4
  ```

<u>(Exercise)</u>

1. 아래와 같은 정보를 갖는 네트워크를 라우팅 경로에 추가

   * 네트워크 주소: `192.168.1.0`
   * Genmask: `255.255.255.0`
   * 네트워크 인터페이스: `ens4`

   ```bash
   $ sudo route add -net 192.168.1.0 netmask 255.255.255.0 dev ens4
   $ route
   Kernel IP routing table
   Destination  Gateway     Genmask         Flags Metric Ref    Use Iface
   default      _gateway    0.0.0.0         UG    100    0        0 ens4
   _gateway     0.0.0.0     255.255.255.255 UH    100    0        0 ens4
   192.168.1.0  0.0.0.0     255.255.255.0   U     0      0        0 ens4
   ```

2. 방금 추가한 네트워크를 라우팅 경로에서 제거

   ```bash
   $ sudo route del -net 192.168.1.0 netmask 255.255.255.0
   $ route
   Kernel IP routing table
   Destination  Gateway    Genmask         Flags Metric Ref    Use Iface
   default      _gateway   0.0.0.0         UG    100    0        0 ens4
   _gateway     0.0.0.0    255.255.255.255 UH    100    0        0 ens4
   ```

3. 기본 게이트웨이 제거

   ```bash
   $ sudo route del default gw ???.???.???.???
   ```

4. 아래의 기본 게이트웨이 추가

   * 네트워크 인터페이스: `ens33`
   * 주소: `168.192.20.0`

   ```bash
   $ sudo route add default gw 168.192.20.0 dev ens33
   ```

##### <u>10.2.3.</u> 네트워크 정보 출력

<u>(1)</u> 네트워크 상태 출력 (`ping` 명령어)

* 네트워크의 통신 가능 여부 확인

  * 특정 시스템에 ICMP 패킷을 보내 네트워크 연결 상태를 조사함
  * 일정한 간격으로 ICMP 패킷을 다른 시스템에 전송하고 다시 전송 받음

* 네트워크 장애 진단

  * 네트워크 속도 측정 (패킷의 전달 시간 계산)
  * 전달되는 패킷들을 분석해 패킷의 손실 등의 정보를 조사함

* 패킷 송수신에 문제가 있다고 네트워크 장애가 있다고 하면 안됨

  * 관리자가 보안상의 이유로 방화벽 등으로 ICMP 를 제한하는 경우가 있음

* 패킷 송수신 시, 발생 가능한 오류 메세지와 원인

  |     ICMP 오류 메시지     | 설 명                                                        |
  | :----------------------: | ------------------------------------------------------------ |
  | Destination Unreachable  | * 원격 시스템으로 가는 경로를 찾지 못한 경우<br />* 목적지 시스템이 응답할 수 없는경우 |
  |   Network Unreachable    | 라우팅 테이블에서 목적지 네트워크로 가는 경로를 못 찾은 경우 |
  |     Host Unreachable     | 호스트에 전원 미공급, 호스트 비존재, 네트워크 미연결 등의 경우 |
  | Destination Host Unknown | 주로 잘못된 호스트 이름 혹은 IP 주소를 입력한 경우           |

* 명령어 형식

  ```bash
  $ ping [option] 호스트_이름(혹은 IP주소)
  ```

  > options
  >
  > * `-c 전송횟수`: 지정한 전송횟수만큼만 패킷을 주고 받음 (미지정시, `CTRL+C` 입력 해야함)
  > * `-i 간격(초)`: 지정한 초 단위로 패킷을 송수신함
  > * `-n`: 전달받은 패킷을 다시 전송하는 시스템의 IP 주소를 출력
  > * `-q`: 패킷의 전달/도착 상황을 출력하지 않고 통계 정보만 출력
  > * `-s`: 전송할 패킷의 크기를 지정 (기본값: 8byte + 56byte)

<u>(Exercise)</u>

1. 도메인 이름(`www.gbbook.com`)을 갖는 호스트에 패킷 송수신

   ```bash
   $ ping www.gbbook.com
   PING www.gbbook.com (222.239.254.189) 56(84) bytes of data.
   64 bytes from 222.239.254.189 (222.239.254.189): icmp_seq=1 ttl=57 time=138 ms
   ...
   64 bytes from 222.239.254.189 (222.239.254.189): icmp_seq=5 ttl=57 time=138 ms
   ^C
   --- www.gbbook.com ping statistics ---
   5 packets transmitted, 5 received, 0% packet loss, time 4004ms
   rtt min/avg/max/mdev = 138.473/138.746/138.891/0.433 ms
   ```

   > * `icmp_seq`: 전달되는 패킷의 일련번호
   > * `ttl`(time to live): 컴퓨터나 네트워크에서 데이터의 유효기간 (패킷의 무한 순환 방지)
   > * `time`: 응답 시간 (ms 단위)

2. 위 호스트에 패킷 5개 송수신

   ```bash
   $ ping www.gbbook.com -c 5
   ```

3. 위 호스트에 패킷 3개를 2초 간격으로 송수신

   ```bash
   $ ping -c 3 -i 2 www.gbbook.com
   ```

4. 위 호스트에 3개의 패킷 송수신 (호스트 이름 대신 IP 주소로 표현)

   ```bash
   $ ping -c 3 www.gbbook.com -n
   ```

5. 위 호스트에 3개의 패킷 송수신 (통계 정보만 표현)

   ```bash
   $ ping -c 3 -q www.gbbook.com
   PING www.gbbook.com (222.239.254.189) 56(84) bytes of data.
   --- www.gbbook.com ping statistics ---
   3 packets transmitted, 3 received, 0% packet loss, time 2002ms
   rtt min/avg/max/mdev = 138.424/138.966/139.482/0.432 ms
   ```

<u>(2)</u> 네트워크 정보 출력 (`netstat` 명령어)

* 네트워크의 상태/정보 상세히 출력

* `route` 명령어 대비 더 자세한 라우팅 테이블 정보 출력

* 명령어 형식

  ```bash
  $ netstat [option]
  ```

  > options
  >
  > * `-a`: 모든 소켓의 상태 출력
  > * `-c`: `CTRL+C` 입력 전까지 매초마다 정보를 갱신하여 출력
  > * `-i`: 모든 네트워크 인터페이스의 정보를 출력
  > * `-n`: 호스트 이름이 아닌 IP 주소를 출력
  > * `-r`: 라우팅 테이블의 내용 출력

<u>(Exercise)</u>

1. 라이팅 테이블의 정보를 상세히 출력

   ```bash
   $ netstat -r
   Kernel IP routing table
   Destination   Gateway   Genmask         Flags  MSS Window  irtt Iface
   default       _gateway  0.0.0.0         UG       0 0          0 wlp1s0
   192.168.35.0  0.0.0.0   255.255.255.0   U        0 0          0 wlp1s0
   _gateway      0.0.0.0   255.255.255.255 UH       0 0          0 wlp1s0
   ```

2. 현 시스템의 모든 네트워크 인터페이스 정보 출력

   ```bash
   $ netstat -i
   Kernel Interface table
   Iface    MTU  RX-OK RX-ERR RX-DRP RX-OVR  TX-OK TX-ERR TX-DRP TX-OVR Flg
   enp2s0  1500      0      0      0 0           0      0      0      0 BMU
   lo     65536    144      0      0 0         144      0      0      0 LRU
   wlp1s0  1500  13232      0      0 0       11016      0      0      0 BMRU
   ```

   > * `Iface`: 네트워크 인터페이스 이름
   > * `TX-OK`/`RX-OK`: 송신/수신에 성공한 패킷
   > * `TX-ERR`/`RX-ERR`: 송신/수신에 실패한 패킷
   > * `TX-DRP`/`RX-DRP`: 송신/수신 도중에 손실된 패킷
   > * `TX-OVR`/`RX-OVR`: 송신/수신 도중에 유실(오버런 등으로)된 패킷
   > * `Flg`: 네트워크 인터페이스의 현재 상태
   >   * `B`: 브로드캐스팅 주소 지정됨
   >   * `M`: 라우팅 데몬으로 변경됨
   >   * `R`: 인터페이스가 동작 중임
   >   * `U`: 인터페이스가 활성화됨
   >   * `L`: 루프백 인터페이스임

3. 매초마다 현재 시스템에 장착된 네트워크 인터페이스의 네트워크 정보를 출력

   ```bash
   $ netstat -ic
   ```

<u>(3)</u> 라우팅 과정 출력 (`traceroute` 명령어)

* 3개의 검사용 패킷을 송신하여 라우팅 과정을 추적하여 추적 내용을 출력

  * 각 게이트웨이의 DNS 를 통해 도메인 주소를 IP 주소로 변환시켜 출력

* 패킷의 목적지인 호스트까지 전달되는 경로를 화면에 출력

* 패킷이 어느 경로에서 유실되는지, 어떤 네트워크에서 트래픽이 얼마나 발생하는지 등을 조사할 수 있음

* `traceroute` 패키지 설치

  ```bash
  $ sudo aptitude show traceroute
  ```

* `gbbook.com` 이라는 호스트까지의 패킷 전달 경로 출력

  ```bash
  $ traceroute gbbook.com
  traceroute to gbbook.com (222.239.254.189), 30 hops max, 60 byte packets
   1  _gateway (192.168.35.1)  1.174 ms  1.136 ms  1.095 ms
   2  * * *
   3  211.186.35.129 (211.186.35.129)  7.719 ms  7.697 ms  7.656 ms
   4  100.90.5.85 (100.90.5.85)  6.231 ms  6.211 ms  6.760 ms
   5  100.90.181.125 (100.90.181.125)  4.688 ms 100.90.182.125 (100.90.182.125)  4.610 ms 100.90.181.125 (100.90.181.125)  4.596 ms
   6  10.101.9.2 (10.101.9.2)  4.561 ms * *
  ...
  11  * * *
  12  * 10.40.4.2 (10.40.4.2)  8.184 ms  8.151 ms
  ...
  30  * * *
  ```

  > * 30개의 게이트웨이(30개의 홉)를 거쳐 라우팅이 정상 수행되었음
  > * `* * *`: 검사용 패킷이 손실된 경우 혹은 게이트웨이가 명령어에 응답하지 않도록 설정된 경우

* 특정 게이트웨이 확인 (`whois` 명령어)

  * `whois` 패키지 설치

    ```bash
    $ sudo apt-get install whois
    ```

  ```bash
  $ whois 211.186.35.129
  query : 211.186.35.129
  
  
  # KOREAN(UTF8)
  
  조회하신 IPv4주소는 한국인터넷진흥원으로부터 아래의 관리대행자에게 할당되었으며, 할당 정보는 다음과 같습니다.
  
  [ 네트워크 할당 정보 ]
  IPv4주소           : 211.186.0.0 - 211.186.255.255 (/16)
  기관명             : 에스케이브로드밴드주식회사
  서비스명           : broadNnet
  주소               : 서울특별시 중구 퇴계로 24
  우편번호           : 04637
  할당일자           : 20000921
  
  이름               : IP주소 담당자
  전화번호           : +82-80-828-2106
  전자우편           : ip-adm@skbroadband.com
  
  조회하신 IPv4주소는 위의 관리대행자로부터 아래의 사용자에게 할당되었으며, 할당 정보는 다음과 같습니다.
  --------------------------------------------------------------------------------
  
  
  [ 네트워크 할당 정보 ]
  IPv4주소           : 211.186.35.0 - 211.186.35.255 (/24)
  기관명             : 에스케이브로드밴드주식회사
  네트워크 구분      : CUSTOMER
  주소               : 서울특별시 중구 퇴계로
  우편번호           : 04637
  할당내역 등록일    : 20061214
  
  이름               : IP주소 담당자
  전화번호           : +82-80-828-2106
  전자우편           : ip-adm@skbroadband.com
  
  
  # ENGLISH
  ...
  ```

<u>(4)</u> 도메인 정보 질의 (`nslookup` 명령어)

* 도메인에 대한 질의를 수행

* 상태 DNS 서버의 이름과 주소를 질의하는데 사용됨

* 명령어 형식

  ```bash
  $ nslookup [도메인_이름 | IP_주소]
  ```

  > 도메인 이름이나 IP 주소를 쓰지 않으면 대화형으로 명령어가 수행됨

<u>(5)</u> 시스템 정보 출력 (`uname` 명령어)

* 시스템의 정보 출력

  * 시스템의 HW 타입, 네트워크 호스트 이름, 운영체제의 이름 및 버전 등

* 명령어 형식

  ```bash
  $ uname [option]
  ```

  > * `-m`: 시스템의 HW 타입 출력
  > * `-n`: 시스템의 네트워크 호스트 이름 출력
  > * `-r`: 리눅스 OS 의 릴리즈 번호 출력
  > * `-s`: OS 이름 출력
  > * `-v`: OS 버전 출력
  > * `-a`: 위의 모든 정보 출력

<u>(6)</u> 호스트 이름 출력 (`hostname` 명령어)

* 명령어 형식

  ```bash
  $ hostname
  ```

<u>(7)</u> 호스트 이름 변경 (`hostnamectl` 명령어)

* 호스트 이름뿐만 아니라 OS, 커널의 버전, 플랫폼 종류 등 호스트 관련 정보 출력

* 호스트의 이름 수정

* 명령어 형식

  ```bash
  $ hostnamectl [내부_명령]
  ```

  > 내부 명령
  >
  > * `status`: 현재 호스트에 대한 정보 출력
  > * `set-hostname 호스트이름`: 기존 호스트 이름을 지정한 이름으로 변경

* 호스트 정보 출력

  ```bash
  $ hostnamectl status
     Static hostname: q2452
           Icon name: computer-laptop
             Chassis: laptop
          Machine ID: b97975a906ab48a0b6b1ef4ab3b52f3b
             Boot ID: 72e5905edf644db78cf1c14e8d6ec3a2
    Operating System: Ubuntu 20.04.4 LTS
              Kernel: Linux 5.4.0-100-generic
        Architecture: x86-64
  ```

* 호스트 이름을 `tmdgns1139`로 수정

  ```bash
  $ sudo hostnamectl set-hostname tmdgns1139
  $ cat /etc/hostname
  tmdgns1139
  ```

### <u>10.3.</u> 방화벽 관리

* 시스템 내부와 외부 사이를 오고가는 네트워크 트래픽을 모니터링하고 제어하는 보안 시스템
* 사전에 시스템 내부에서 정의한 보안 규칙에 따라 보안 시스템을 동작시킴
* 대부분의 시스템은 방화벽을 기반으로 시스템 보안을 유지함

##### <u>10.3.1.</u> GUI 에서의 관리

<u>(1)</u> `gufw` 패키지

* `gufw`(**G**UI for **u**ncomplicated **f**ire**w**all) 패키지 설치

  ```bash
  $ sudo apt install gufw
  ```

<u>(2)</u> 방화벽 설정 (`gufw` 패키지)

* 지금 시스템 OS 가 서버 버전이라서 실습 불가능
* https://idchowto.com/gufw%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-gui-%EB%B0%A9%EC%8B%9D%EC%9C%BC%EB%A1%9C-%EB%B0%A9%ED%99%94%EB%B2%BD-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/

##### <u>10.3.2.</u> CLI 에서의 관리

<u>(1)</u> `ufw` 패키지

* 패키지 설치

  ```bash
  $ sudo aptitude show ufw
  ```

  ```bash
  $ sudo apt-get install ufw
  ```

* `ufw` 명령어 활성화

  ```bash
  $ sudo ufw status
  Status: inactive
  ```

  ```bash
  $ sudo ufw enable
  Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
  Firewall is active and enabled on system startup
  $ sudo ufw status
  Status: active
  ```

* `ufw` 명령어 비활성화

  ```bash
  $ sudo ufw disable
  ```

<u>(2)</u> 방화벽 설정

* 명령어 형식

  ```bash
  $ ufw 내부_명령
  ```

  > 내부 명령
  >
  > * `enable`/`disable`: 방화벽 활성화/비활성화
  > * `status`: 방화벽 상태 출력
  > * `allow 서비스(또는 포트/프로토콜)`: 지정한 서비스 혹은 포트를 허용
  > * `deny 서비스(또는 포트/포로토콜)`: 지정한 서비스 혹은 포트를 불허용
  > * `delete allow|deny 서비스`: 지정한 서비스 허용/거부 정책을 제거
  > * `default allow|deny incoming|outgoing`: 방화벽의 기본 정책을 설정

*방화벽 기본 룰의 허가와 거부*

* 기본 룰

  * 처음에 기본 룰은 `deny incoming`, `allow outgoing` 으로 설정되어 있음
  * `deny incoming`: 내부로 들어오는 패킷은 거부
  * `allow outgoing`: 외부로 나가는 패킷은 허용

* 기본 룰 모두 허용

  ```bash
  $ sudo ufw default allow
  ```

* 외부로 나가는 패킷을 기본적으로 허용하기

  ```bash
  $ sudo ufw default allow outgoing
  ```

* 내부로 들어오는 패킷을 기본적으로 허용하기

  ```bash
  $ sudo ufw default allow incoming
  ```

* 기본적으로 시스템 내/외부 패킷을 모두 막음

  ```bash
  $ sudo ufw default deny
  ```

*방화벽 초기화 (`reset` 내부 명령)*

* 방화벽 설정을 초기 상태로 되돌리고 `ufw`를 비활성화

* 명령어 형식

  ```bash
  $ sudo ufw reset
  ```

*포트 기반 방화벽 설정*

* 방화벽 설정에 있어서 시스템의 포트를 지정 가능

* 형식

  ```bash
  $ sudo ufw allow|deny 포트[/프로토콜]
  ```

* `22번` 포트에 대해 `TCP`, `UDP` 프로토콜 모두 허용

  ```bash
  $ sudo ufw allow 22
  ```

* `22번` 포트에 대해 `TCP`프로토콜 허용

  ```bash
  sudo ufw allow 22/tcp
  ```

*서비스 기반 방화벽 설정*

* 방화벽 설정에 있어 네트워크 서비스 이름을 지정 가능

* 형식

  ```bash
  $ sudo ufw allow|deny 서비스_이름
  ```

* `ssh` 서비스 허용

  ```bash
  $ sudo ufw allow ssh
  ```

*IP 주소 기반 방화벽 설정*

* 특정 IP 를 갖는 시스템에 대해 지정한 포트나 서비스를 허용/거부하도록 지정

* 형식

  ```bash
  $ ufw allow|deny from IP_주소 [to any port 포트번호|서비스] [proto 프로토콜]
  ```

* IP 밴 (`192.168.20.22` IP 주소를 갖는 시스템의 접근 거부)

  ```bash
  $ sudo ufw deny from 192.168.20.22
  ```

* `192.168.20.22` IP 주소를 갖는 시스템의 22번 포트를 통한 송수신 거부

  ```bash
  $ sudo ufw deny from 192.168.20.22 to any port 22
  ```

* `192.168.20.22` IP 주소를 갖는 시스템의 `ssh` 서비스만 거부

  ```bash
  $ sudo ufw deny from 192.168.20.22 to any port ssh
  ```

* `192.168.20.22` IP 주소를 갖는 시스템의 `TCP` 프로토콜을 통한 `ssh` 서비스는 허용

  ```bash
  $ sudo ufw allow from 192.168.20.22 to any port ssh proto tcp
  ```



* `ufw` 대신 `firewalld` 사용: https://mebadong.tistory.com/85
* `iptables` 사용: https://ndb796.tistory.com/262

