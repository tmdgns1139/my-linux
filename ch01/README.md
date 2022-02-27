## <u>Chapter 1</u>. 리눅스의 개요와 설치

* 리눅스 탄생, 발전과정 및 리눅스의 특장점
* 리눅스 배포판 및 우분투 배포판의 특징
* 우분투 설치과정

### <u>1.1.</u> 리눅스의 개요

##### <u>1.1.1.</u> 리눅스의 탄생과 발전

1. 리눅스의 탄생
   * 리눅스 운영체제 이전에 유닉스(UNIX) 운영체제가 있었음
   * 엔드류 타넨바움 교수가 UNIX 기반 운영체제인 미닉스(MINIX) 를 만듦 (교육 목적)
   * 리누스 베네딕트 토발즈가 MINIX 를 참고하여 만든 운영체제를 오픈소스로 공개 후, 리눅스(LINUX) 라고 불림
2. 리눅스의 발전
   * 리누스 베네딕트 토발즈가 발표한 것은 운영체제의 핵심인 커널(Kernel)임.
     ( Kernel: 프로세서, 메모리 관리 및 각종 장치의 구동을 관리하는 시스템 프로그램 )
   * 해당 커널에 다양한 유틸리티와 UI, 컴파일러 등을 갖추어 대중적인 형태의 운영체제가 탄생함 
     ( 리처드 스톨만의 GNU 프로젝트 )

##### <u>1.1.2.</u> 리눅스 시스템의 특징과 장점

0. 사용처
   * 리눅스는 주로 중소형 시스템의 서버 운영체제로 사용됨
     ( 서버 운영체제: 네트워크를 통해 받은 클라이언트의 요청을 처리하는 서버 시스템에 장착되는 운영체제 )
   * 리눅스 이전에는 유닉스가 거의 유일한 운영체제였다.
1. **무료** 혹은 저렴한 가격의 운영체제
   * GNU 프로젝트에 의해 개발된 운영체제
   * 리눅스의 다양한 배포판들이 GNU 라이센스를 갖고 배포됨
   * 무료 사용 혹은 유닉스나 윈도우에 비해 저렴하게 구매 가능
2. 안정된 운영체제
   * 리눅스는 오픈소스 운영체제
   * 운영체제 버그가 생기면 전세계의 개발자들이 해당 버그를 고치고 업데이트함
3. 다중 사용자 환경 제공
   * 여러 사용자가 하나의 운영체제 시스템에 접속 가능
   * 사용자는 관리자가 발급한 계정으로만 시스템에 접속 가능
   * 시스템 자원에 대한 엄격한 접근 권한 및 소유 체계 보유로 최적의 보안 유지
4. 가상 터미널 및 다중 작업 환경 제공
   * 가상 터미널(Virtual Terminal): 하나의 시스템을 마치 여러 시스템이 있는 것처럼 가상화하여 다중 작업을 지원![virtual-terminal](https://user-images.githubusercontent.com/87659486/153176816-52f61d88-0b9b-4a9b-955b-b1239adbf593.jpeg)
5. 그래픽 환경 지원
   * 서버용 운영체제로 주로 이용되어 CLI 위주였으나 윈도우처럼 GUI 제공
     ( **CLI**: 명령어 이용으로 빠르게 컴퓨터 다루기 vs. **GUI**: 마우스 이용으로 직관적인 컴퓨터 다루기)
   * GUI 환경인 GNOME 제공
6. 강력, 안정적, 경제적인 네트워크 지원
   * 리눅스가 서버 운영체제로 사용되는 주된 이유
   * 적은 비용으로 안정적이고 강력한 네트워크 서버 구축 가능
   * 낮은 사양의 시스템에서도 거의 모든 종류의 네트워크 서비스 운영 가능
     ( 네트워크 종류: 웹 서버, 메일 서버, 네임 서버, FTP 서버 등 )
7. 거의 없는 플랫폼 제약
   * 리눅스는 32bit, 64bit 을 모두 지원하고 x86 기반인 Intel 계열의 CPU 모두를 지원
   * 낮은 사양의 시스템 환경에서도 리눅스는 원활하게 돌아감 (가볍다!)

##### <u>1.1.3.</u> 리눅스의 배포판과 우분투

<u>(1)</u> 리눅스의 배포판

* 리누스가 개발 및 공개한 리눅스는 시스템 운영 및 자원 관리를 위한 커널(Kernel)임
* 리눅스 커널에 적용할 수 있는 애플리케이션 개발 및 운영체제 구성은 어려운 일
* 특정 단체, 커뮤니티, 연구소, 회사 등에서 **리눅스 커널에서 실행될 수 있는 응용 프로그램 (리눅스 배포판)**을 개발, 패키지화, 배포 수행
* 리눅스 배포판은 크게 데비안(Debian) 계열, 레드햇(Redhat) 계열, 슬랙웨어(Slackware) 계열로 구분됨
* 데비안(Debian)
  * 데비안 프로젝트에 의해 개발된 리눅스 배포판
  * 공식 이름은 "Debian GNU/Linux" 이나 일반적으로 데비안이라고 부름
  * 배포판 릴리즈의 구분: Stable, Testing, Unstable 등 3가지
* 레드햇(Redhat)
  * 전 세계적에서 가장 큰 위상을 차지하고 있는 리눅스 배포판
  * 우리나라 한글 프로그램 배포판은 대부분 레드햇 리눅스를 기반으로 했었음
    * https://www.mk.co.kr/news/home/view/2000/06/76923/
  * 그래픽 설치환경(아나콘다)을 통해 쉽게 설치 가능
  * 패키징 매커니즘인 레드햇 패키지 매니저(RPM) 을 통해 패키지 설치, 제거, 업그레이드의 편의성 제공
  * 릴리즈 9를 끝으로 무료 제공을 중단하고 상용 배포판 릴리즈
  * 비영리 커뮤니티인 페도라 프로젝트 지원 (2003년부터 페도라 리눅스 공개)
* 슬랙웨어(Slackware)
  * 레드햇 등장 전, 가장 주목을 받았던 최초의 리눅스 배포판
  * 높은 수준의 안정성과 보안성 및 비교적 적은 버그로 서버에 설치하기 적합함
* 리눅스 배포판의 발전과정
  * https://futurist.se/gldt/wp-content/uploads/12.09/gldt1209.png

<u>(2)</u> 우분투 리눅스

* **데비안 계열**의 리눅스로 영국의 캐노니컬사의 공식 지원을 받고 있음
* 쉬운 설치와 쉬운 사용이 기본 철학
* 11버전부터 유니티(unity)를 자체 데스크톱 환경으로 사용
* 17버전부터 GNOME 을 기본 데스크톱 환경으로 사용
* 장기버전(LTS): 4번의 새로운 버전마다 1번씩, 2년에 1번씩 발표 및 5년의 버전 지원 기간
* 일반버전: 9개월의 버전 지원 기간
* 우분투 기반 배포판 중 캐노니컬사에서 공식 인정한 패포판과 그렇지 않은 배포판이 있음
  * 공식 패포판: 쿠분투, 주분투, 루분투, 우분투 그놈, 에듀분트 등
  * 비공식 배포판: 코분투, 이지피지, 그뉴센스, 리눅스 민트, 넥센타 OS 등

### <u>1.2.</u> 우분투 설치

##### <u>1.2.1.</u> 우분투 리눅스 다운로드

* https://www.ubuntu.com 에서 iso 파일 다운로드

##### <u>1.2.2.</u> 우분투 리눅스 설치

* 기존 시스템의 하드 디스크를 2개의 논리적 파티션으로 나누어 한쪽에 리눅스 설치
  ( 이 경우, 부팅시 사용할 운영체제를 선택함 - 멀티 부팅 )

* VMware 와 같은 가상환경 이용

  * 윈도우에 VMware SW 를 설치하고 가상머신을 이용해 리눅스 설치
  * 윈도우는 HostOS, 리눅스는 GuestOS 라고 부름
  * 가상머신 다운로드
    * https://www.VMware.com/kr
  * 가상머신 설치
  * 가상머신 생성
    * "Create a New Virtual Machine" 실행
    * GuestOS 설치
      * 방법1. Install disc: CD/DVD 를 사용하여 설치
      * 방법2. Installer disc image file(iso): iso 파일을 이용하여 설치
      * **방법3**. I will install ther os later: 비어있는 하드디스크만 생성 후, 나중에 설치
    * 설치할 게스트 OS 지정 (linux - Ubuntu 64-bit)
    * 가상머신 이름 및 설치 위치 지정
    * 하드 디스크의 크기와 저장 방식 지정
    * 가상머신 생성 설정 내용 출력
    * 가상머신 생성 완료

* docker 컨테이너 이용

  * 가상머신과 달리 docker 컨테이너는 일종의 프로세스임

  * docker image 받아오기

    * https://hub.docker.com/_/ubuntu

    ```bash
    $ docker pull ubuntu:18.04
    ```

  * docker daemon 관련 문제 해결

    * https://somjang.tistory.com/entry/Docker-Cannot-connect-to-the-Docker-daemon-at-unixvarrundockersock-Is-the-docker-daemon-running-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95

  * docker 컨테이너 생성

    ```bash
    $ docker run -it --name my-ubuntu-1804 ubuntu:18.04 /bin/bash
    ```

    > Ctrl + P, Q : 컨테이너 종료 없이 빠져나오기

  * docker 컨테이너 진입 및 운영체제 확인

    ```bash
    $ docker attach my-ubuntu-1804
    root@asdfasdfds:#  
    ```

    ```bash
    $ docker attach my-ubuntu-1804
    You cannot attach to a stopped container, start it first
    $ docker start my-ubuntu-1804
    $ docker attach my-ubuntu-1804
    root@asdfasdfsa:# /
    ```

    ```bash
    root@asdfasdfsa:# uname -a
    Linux 6b9f42ba57d8 5.10.76-linuxkit #1 SMP PREEMPT Mon Nov 8 11:22:26 UTC 2021 aarch64 aarch64 aarch64 GNU/Linux
    
    root@asdfasdfsa:# cat /etc/lsb-release
    DISTRIB_ID=Ubuntu
    DISTRIB_RELEASE=18.04
    DISTRIB_CODENAME=bionic
    DISTRIB_DESCRIPTION="Ubuntu 18.04.6 LTS"
    ```

* GCP, AWS 등 클라우드 서비스 이용

  * GCP 의 경우, 가입 후 90일간 E계열 CPU 을 탑재한 서버를 무료로 사용 가능

* 안쓰는 노트북에 `ubuntu server 20.04` 설치해서 원격으로 사용하기

  * 우분투 서버 설치 방법: https://varins.com/library/server/install-ubuntu-server/ 

    * [ISO 파일](https://releases.ubuntu.com/focal/) 및 [rufus](https://rufus.ie/downloads/) 를 다운로드 받아 우분투 서버 설치용 USB 를 준비
    * 노트북에 USB 를 꽂아 블로그 글을 참고하여 운영체제 설치 
      (와이파이 혹은 유선연결 등으로 네트워크 확보해야함)

  * 노트북 덮개 닫았을때 정상 동작: https://dontdiethere.tistory.com/27 

    * `/etc/systemd/logind.conf` 파일에 24번째 줄을 `HandleLidSwitch=ignore` 로 변경

      ```bash
      $ sudo vim /etc/systemd/logind.conf
      ...
      HandleLidSwitch=ignore
      ...
      ```

      > `HandleLidSwith=` 가 포함된 줄이 있음

    * 로그인 관련 데몬 프로세스 재시작 

      ```bash
      $ systemctl restart systemd-logind.service
      ```

    * 이제 노트북 덮개까지 닫아 어딘가 짱박아두고 개인 서버로 사용함

  * `SSH` 접속 시도

    ```bash
    $ ssh ???.???.???.???
    ```

    > * 만약 서버 호스트에 변화(OS 재설치 등)가 발생 시 생기는 오류는 아래 블로그를 참고하여 해결
    >   * https://visu4l.tistory.com/entry/ssh-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D-%EC%97%90%EB%9F%ACWARNING-REMOTE-HOST-IDENTIFICATION-HAS-CHANGED
    >   * `$ ssh-keygen -R IP_주소|도메인_이름`

  * IP 대신 도메인 이름으로 접근하기

    ```bash
    $ sudo vim /private/etc/hosts
    ...
    ???.???.??? YOUR_OWN_DOMAIN_NAME
    ...
    ```

    > Mac OS 혹은 리눅스 OS 가 깔린 클라이언트 시스템에서 할 것

    ```bash
    $ ssh YOUR_OWN_DOMAIN_NAME
    ```

  * 서버에 `nginx` 서버 띄우고 클라이언트 시스템에서 접속해보기

    * `nginx` 설치

      ```bash
      $ sudo apt-get install nginx
      ```

    * `nginx` 서버 구동

      ```bash
      $ sudo service start nginx
      ```

    * `nginx` 서버 구동 확인

      ```bash
      $ systemctl status nginx.service
      ...
      Active: active (running)
      ...
      ```

      ```bash
      $ netstat -lntp
      ...
      Proto Recv-Q Send-Q Local Address  Foreign Addressc  State       PID/Program name
      tcp        0      0 0.0.0.0:80     0.0.0.0:*         LISTEN      -
      ...
      ```

      > `netstat` 명령어가 안먹히면 `$ sudo apt install net-tools` 실행

    * 클라이언트 시스템의 브라우저로 접속해서 `nginx` 페이지 확인