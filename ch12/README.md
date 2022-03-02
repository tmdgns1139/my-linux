## <u>Chapter 12.</u> 시스템의 원격 접속

* `Telnet`(원격에서 호스트 접속) 서버의 설치/운영 방법 숙지
* `SSH`(암호화 키를 사용한 원격 접속) 서버의 설치/운영 방법 숙지
* 그래픽 환경에서 원격 접속을 수행하는 `VNC` 서버 학습

### <u>12.1.</u> Telnet 서버

##### <u>12.1.1.</u> Telnet 서버 설치

<u>(1)</u> Telnet 서버 개요

* 원격지 호스트에 원격접속을 수행하는 TCP/IP 기반 네트워크 서비스 (클라이언트-서버 모델)
  * 원격접속: 클라이언트 컴퓨터를 사용해 원격지의 서버에 접속하는 것
  * Telnet 이 설치된 리눅스 서버로의 원격접속은 클라이언트의 운영체제에 상관없이 가능
* Telnet 을 통한 서버 원격접속을 하는 경우, 사용자 계정으로 접속해야함
  * 계정에 부여된 권한에 따라 서버 내의 파일을 처리하거나 프로그램을 실행함
* 오랫동안 사용된 원격접속 방법이지만 보안에 취약하여 최근에는 잘 사용되지 않음
* Telnet 서버에 원격접속하기 위해서는 Telnet 클라이언트 프로그램이 필요함 (대부분 무료 제공)

<u>(2)</u> Telnet 서버 설치 과정

1. `xinetd` 패키지 설치

   * Telnet 은 `xinetd` 방식으로 운영되는 네트워크 서비스임

   * `xinetd` 라는 슈퍼데몬에 의해 동작하는 네트워크 서비스 (혹은 데몬)
   * `xinetd` 패키지가 설치되지 않은 경우, 설치해야함

2. `Telnet` 서버 설치

   * telnet 설치 여부 확인

     ```bash
     $ sudo dpkg -l | grep telnet
     ii  telnet  0.17-41.2build1  amd64  basic telnet client
     ```

     > 현재는 telnet 클라이언트 프로그램만 설치되어 있고 telnet 서버는 설치되어 있지 않음

   * telnet 서버 패키지 설치

     ```bash
     $ sudo apt-get install telnetd
     ```

3. `Telnet` 서버의 설정 파일 생성

   * `/etc/xinetd.d/telnet`, `/etc/xinetd.conf`: Telnet 서버의 설정 관련 파일
   * 전자 파일의 설정을 우선 적용, 나머지 설정은 후자 파일의 내용에 따라 적용

4. `xinetd` 데몬 재시작

   * Telnet 서버 설정을 바꾸고 나면 `xinetd` 데몬을 재시작하여 설정을 적용

5. 방화벽 설정 변경

   * TCP 프로토콜 전용 포트인 23번 포트를 열어 외부 접속을 허용해야함

6. `Telnet` 사용자 계정 생성

   * `adduser`/`useradd` 명령어를 통해 생성된 사용자 계정을 Telnet 서비스를 이용할 계정으로 사용 가능

7. `Telnet` 클라이언트를 이용한 서버 접속

   * 리눅스에 기본 설치된 혹은 윈도우용 Telnet 클라이언트를 이용하여 리눅스 시스템에 원격접속 가능

##### <u>12.1.2.</u> Telnet 서버 접속

<u>(1)</u> 리눅스 클라이언트에서 Telnet 서버 접속

* telnet 클라이언트로 telnet 서버에 접속하는 방법

  ```bash
  $ telnet 원격지_서버_주소
  ```

  ```bash
  $ telnet
  telnet> 원격지_서버_주소
  ```

  > 원격지 서버 주소는 IP 주소 혹은 도메인 이름으로 지정

* telnet 서버 테스트 (`localhost` 와의 통신)

  ```bash
  $ telnet 127.0.0.1
  $ telnet 0
  $ telnet localhost
  ```

  > 3가지 중 1개 선택

<u>(2)</u> 윈도우 클라이언트에서 Telnet 서버 접속

* 초기 윈도우에서부터 telnet 을 기본 제공함
* 최근 윈도우에서는 보안 문제로 설정 변경을 통해 telnet 을 사용할 수 있음
  * [제어판]-[프로그램]-[프로그램 및 기능]-[Windows 기능 켜기/끄기] 선택
  * [Windows 기능] 대화상자에서 `Telnet Client` 체크 후, [확인] 버튼 클릭
  * `telnet`을 윈도우 내에서 검색하여 `telnet.exe` 파일을 실행
* 외부 클라이언트 프로그램인 PuTTy 를 사용하기도함 (한글화 버전: HPuTTy)
  * Host Name: 서버의 주소 혹은 도메인 이름 입력
  * Connection Type: telnet 을 선택 (Port 가 23번으로 변경됨)
  * [open] 버튼을 클릭 후, 아이디와 패스워드를 입력

<u>(Exercise)</u>

1. `xinetd` 패키지 설치

   ```bash
   $ aptitude show xinetd
   Package: xinetd
   Version: 1:2.3.15.3-1
   State: not installed
   ...
   ```

   ```bash
   $ sudo apt-get install xinetd
   ```

2. Telnet 서버 설치 (`telnetd` 패키지 설치)

   ```bash
   $ aptitude show telnetd
   Package: telnetd
   Version: 0.17-41.2build1
   State: not installed
   ...
   ```

   ```bash
   $ sudo apt-get install telnetd
   ```

3. `/etc/xinetd.d` 내에 `telnet` 파일을 생성하고 아래와 같이 작성

   ```bash
   $ sudo vim /etc/xinetd.d/telnet
   $ cat /etc/xinetd.d/telnet
   service telnet
   {
   	disable = no
   	flags = REUSE
   	socket_type = stream
   	wait = no
   	user = root
   	server = /usr/sbin/in.telnetd
   	log_on_failure += USERID
   }
   ```

   |              항목               | 설 명                                                        |
   | :-----------------------------: | ------------------------------------------------------------ |
   |         `disable = no`          | telnet 서버 사용 유무 지정<br />(`yes`: 사용 안함, `no`: 사용함) |
   |         `flags = REUSE`         | 23번 포트가 사용 중인 경우, 해당 포트의 재사용을 허용        |
   |     `socket_type = stream`      | telnet 이 사용하는 TCP/IP 소켓의 형식을 stream 으로 지정<br />(UDP 프로토콜인 경우, dgram 으로 지정함) |
   |           `wait = no`           | 먼저 도착한 요청을 처리하는 도중에 다른 요청을 받은 경우,<br />* `yes`: 먼저 도착한 요청 처리 후, 다음 요청을 허용<br />* `no`: 먼저 도착한 요청이 종료되지 않아도 다음 요청을 허용<br />(socket_type 이 stream 인 경우, `no`로 지정) |
   |          `user = root`          | 해당 서비스를 어떤 사용자 권한으로 서비스할 것인지 지정<br />(데몬 파일의 소유권은 root 에게 있음) |
   | `server = /usr/sbin/in.telnetd` | telnet 서버의 데몬이 지정된 파일에 의해 동작함을 의미        |
   |   `log_on_failure += USERID`    | 서버 자원 부족, 접근 제한 등의 문제로 접속에 실패한 경우,<br />접근을 시도한 사용자의 ID 를 로그파일에 기록함 |

4. 환경 설정 파일(`/etc/xinetd.d/telnet`) 작성 후, `xinetd` 데몬을 시작하고 정상 작동 확인

   ```bash
   $ sudo systemctl start xinetd.service
   $ ps -ef | grep xinetd
   root       12753       1  0 12:59 ?        00:00:00 /usr/sbin/xinetd -pidfile /run/xinetd.pid -stayalive -inetd_compat -inetd_ipv6
   seunghun   13135   11547  0 13:13 pts/0    00:00:00 grep --color=auto xinetd
   $ systemctl status xinetd.service
   ● xinetd.service - LSB: Starts or stops the xinetd daemon.
        Loaded: loaded (/etc/init.d/xinetd; generated)
        Active: active (running) since Tue 2022-03-01 12:59:40 UTC; 12min ago
   ...
   ```

5. 방화벽 설정을 통해 23번 포트의 TCP 프로토콜만 허용

   ```bash
   $ sudo ufw allow 23/tcp
   Rules updated
   Rules updated (v6)
   ```

6. Telnet 전용 사용자 계정 생성

   ```bash
   $ sudo adduser tel-user
   ```

7. Telnet 클라이언트 프로그램 실행

   ```bash
   $ telnet
   telnet>
   ```

8. Telnet 서버에 접속 (사용자 계정과 패스워드를 입력해 서버에 접속)

   ```bash
   $ telnet localhost
   ```

   ```bash
   telnet> open localhost
   Trying 127.0.0.1...
   Connected to localhost.
   Escape character is '^]'.
   Ubuntu 20.04.4 LTS
   q2452 login: tel-user
   Password:
   Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-100-generic x86_64)
   ...
   tel-user@q2452:~$
   ```

9. 현재 디렉토리를 출력하고 Telnet 접속 종료

   ```bash
   $ pwd
   /home/tel-user
   $ exit
   logout
   Connection closed by foreign host.
   ```

### <u>12.2.</u> SSH 서버

##### <u>12.2.1.</u> SSH 서버 설치

<u>(1)</u> SSH 란?

* Secure SHell 의 약자로 telnet 과 유사한 역할과 기능을 수행하는 네트워크 서비스
  * 암호화: ID, Password 등의 중요 정보에 대해 암호화를 수행하여 정보 유출 방지
  * 일반적으로 22번 포트를 사용함 / TCP 프로토콜 이용
* 클라이언트가 SSH 서버에 최초 접속 시, 서버는 클라이언트에 암호 키를 전달함 (사용자 동의 하에)
* 암호 키 수용 이후, 접속과정에서 전달되는 데이터는 해당 암호 키를 통해 암호화되어 전송됨

<u>(2)</u> SSH 서버 설치 과정 (OpenSSH)

1. OpenSSH 서버 설치

   * 설치 확인

     ```bash
     $ sudo dpkg -l | grep openssh
     [sudo] password for seunghun:
     ii  openssh-client       1:8.2p1-4ubuntu0.4  amd64  secure shell (SSH) client, for secure access to remote machines
     ii  openssh-server       1:8.2p1-4ubuntu0.4  amd64  secure shell (SSH) server, for secure access from remote machines
     ii  openssh-sftp-server  1:8.2p1-4ubuntu0.4  amd64  secure shell (SSH) sftp server module, for SFTP access from remote machines
     ```

     ```bash
     $ aptitude show openssh-server
     Package: openssh-server
     Version: 1:8.2p1-4ubuntu0.4
     State: installed
     ...
     ```

   * OpenSSH 서버 패키지 설치 (`openssh-server`) 

     ```bash
     $ sudo apt-get install openssh-server
     ```

2. OpenSSH 서비스 시작

   * 클라이언트가 SSH 서버에 원격접속할 수 있도록함

   ```bash
   $ sudo systemctl start ssh.service
   ```

   ```bash
   $ systemctl status ssh.service
   ● ssh.service - OpenBSD Secure Shell server
        Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
        Active: active (running) since Tue 2022-03-01 21:35:14 KST; 2h 10min ago
   ...
   ```

   ```bash
   $ ps -ef | grep ssh
   root        980      1  0 21:35 ?      00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
   root      11459    980  0 21:37 ?      00:00:00 sshd: seunghun [priv]
   seunghun  11546  11459  0 21:37 ?      00:00:00 sshd: seunghun@pts/0
   seunghun  15417  11547  0 23:48 pts/0  00:00:00 grep --color=auto ssh
   ```

3. 방화벽 설정 변경

   * 방화벽에서 SSH 가 기본적으로 사용하는 22번 포트를 허용

   ```bash
   $ sudo ufw allow 22/tcp
   ```

4. OpenSSH 서비스용 사용자 계정 생성

5. SSH 클라이언트를 이용한 서버 접속

   * SSH 서버 접속을 위해 SSH 클라이언트 프로그램을 클라이언트 호스트에 적절하게 설치

   * 접속 형식

     ```bash
     $ ssh USERID@SSH_SERVER_IP_ADDRESS
     ```

   * SSH 서버에 최초 접속 시, RSA 키 전송 메세지가 나타남

   * RSA 키는 클라이언트마다 다르고 클라이언트와 서버를 오고가는 데이터 암호화 용도로 사용됨

### <u>12.3.</u> VNC 서버

##### <u>12.3.1.</u> VNC 서버 설치

<u>(1)</u> VNC 서버란

* **V**irtual **N**etwork **C**omputing 의 약자
* `telnet`, `SSH`은 CLI 에서 동작하는 원격접속 서비스임
* GUI 에서 동작하는 프로그램, 유틸리티 등을 실행하는 상황에서 VNC 서버를 사용
* VNC는 X윈도우 환경에서 원격으로 시스템에 접속하게 해줌
* VNC 서버는 GUI 를 사용하기 때문에 환경 설정이 복잡하고 `telnet`, `SSH` 대비 속도가 느림

<u>(2)</u> VNC 서버의 설치 과정

1. VNC 서버 설치

   * 데스크톱 관리자 역할을 하는 패키지 설치 (`xfce4`)

     ```bash
     $ aptitude show xfce4
     Package: xfce4
     Version: 4.14
     State: not installed
     ...
     ```

     ```bash
     $ sudo apt-get install xfce4
     ```

   * VNC 서버 패키지 설치 (`vnc4server`)

     ```bash
     $ aptitude show vnc4server
     No candidate version found for vnc4server
     Package: vnc4server
     State: not a real package
     ```

     ```bash
     $ sudo apt-get install vnc4server
     ```

     > 우분투 CLI OS(20.04 LTS 버전) 에서는 패키지 설치가 불가능한듯
     >
     > https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04

2. VNC 패스워드 생성

   * 패스워드 지정 시, `.vnc` 디렉토리 및 `.vnc/passwd` 파일이 생성됨

   ```bash
   $ vncserver
   $ vncpasswd
   ```

   > 패스워드는 최대 8글자여야하고 너무 짧으면 안됨

   ```bash
   $ ls -lF ~/.vnc/
   합계 24
   -rw------- 1 seunghun seunghun   8  3월  2 19:20 passwd
   ...
   -rw-rw-r-- 1 seunghun seunghun 700  3월  2 19:19 seunghun-ThinkPad-T495s:3.log
   -rw-rw-r-- 1 seunghun seunghun   6  3월  2 19:19 seunghun-ThinkPad-T495s:3.pid
   -rwxr-xr-x 1 seunghun seunghun 225  3월  2 19:19 xstartup*
   ```

3. 환경 설정

   * `.vnc/xstartup` 파일을 수정

   ```bash
   $ cp ~/.vnc/xstartup ~/.vnc/xstartup.org
   ```

   ```bash
   $ vi ~/.vnc/xstartup
   $ cat ~/.vnc/xstartup # 책에서 요구한 내용
   #!/bin/sh
   
   unset SESSION_MANAGER
   unset DBUS_SESSION_BUS_ADDRESS
   
   xsetroot -solid grey
   vncconfig -iconic &
   
   startxfce4 &
   ```

4. VNC 재시작

   * 설정 내용 적용을 위해 VNC 재시작 수행

   ```bash
   $ vncserver -kill :1
   Killing Xtightvnc process ID 11529
   ```

5. 해상도, 픽셀의 비트 수 지정

   * VNC 서버를 통해 접속할 화면의 해상도 및 픽셀의 비트 수 지정

   ```bash
   $ vncserver :1 -geometry 800x600 -depth 24
   New 'X' desktop is seunghun-ThinkPad-T495s:1
   
   Starting applications specified in /home/seunghun/.vnc/xstartup
   Log file is /home/seunghun/.vnc/seunghun-ThinkPad-T495s:1.log
   ```

   > 기본 해상도는 1024x768 임

6. 방화벽 설정 변경

   * 방화벽에서 VNC 서비스를 위한 포트(5901번)를 열어둠

   ```bash
   $ sudo ufw allow 5901/tcp
   규칙이 업데이트됐습니다
   규칙이 업데이트됐습니다(v6)
   ```

7. VNC 클라이언트를 통한 서버 접속

   * 주로 오픈소스 VNC 클라이언트 프로그램인 `tigervnc` 를 사용함

   * 패키지 설치 (`tigervnc-viewer`)

     ```bash
     $ sudo apt-get install tigervnc-viewer
     ```

   * 서버 접속

     ```bash
     $ vncviewer IP_ADDRESS
     ```

   * https://tigervnc.org 에서 `.exe` 파일 등 다운로드 가능