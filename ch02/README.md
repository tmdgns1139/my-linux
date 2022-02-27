## <u>Chapter 2</u>. 리눅스 활용 기초

* 리눅스 명령어의 기본 형식 및 sudo 명령어
* 리눅스 시스템에서 사용자와 그룹에 대한 이해

### <u>2.1.</u> 리눅스 명령어의 형식

##### <u>2.1.1.</u> 리눅스의 터미널 사용

* 리눅스가 서버 운영체제로 사용될 경우, GUI 보다는 CLI 환경인 경우가 압도적임
* 키보드로 명령어를 타이핑하여 시스템을 관리해야함
* 명령어 입력을 위해 터미널을 이용함 (시스템을 보다 효율적으로 사용 및 관리한다.)

<u>(1)</u> 터미널의 시작과 종료

* 터미널의 시작

  * 터미널의 프롬프트에 shell 명령어를 입력하여 해당 명령어를 실행

  ![image](https://user-images.githubusercontent.com/87659486/153194387-7a37faf0-c60d-495c-8954-5a3fc8a3524f.png)

* 터미널의 종료

  ```bash
  $ exit
  ```

  > `Ctrl + D` 로도 터미널 종료 가능

<u>(2)</u> 리눅스의 프롬프트

* 프롬프트: 명령어를 받을 수 있는 상태를 의미
  ( shell prompt: shell 명령어를 받을 수 있는 상태 )
* bash shell prompt: 명령어 입력받기, 호스트 이름, 현재 계정, 현재 디렉토리 정보를 포함하며 우분투의 기본 프롬프트
* `~`: 사용자의 홈 디렉토리 (사용자가 시스템에 최초로 접속하면 있는 곳, 사용자가 생성한 파일 저장 디렉토리)
* `$`: 사용자의 자격을 가졌음을 의미
* `#`: 시스템 관리자의 자격을 가졌음을 의미

<u>(3)</u> 사용자의 홈 디렉토리

* 리눅스는 다중 사용자 시스템임 (한 시스템에 여러 사용자가 동시에 접속하여 시스템을 사용)
* 사용자 생성 시, 시스템은 사용자마다 특정 디렉토리를 생성되고 할당함
* 사용자가 생성하는 파일들은 해당 사용자에게 할당된 디렉토리에 저장시켜 다른 사용자의 파일과 구분함
* 사용자마다 할당된 개인 고유 디렉토리를 홈 디렉토리라고 부름
* 사용자의 홈 디렉토리는 `/home` 디렉토리 아래에 사용자 이름과 동일한 이름의 디렉토리임
* `pwd` 명령어로 현재 위치한 디렉토리 확인 가능

##### <u>2.1.2.</u> 리눅스 명령어 기본 형식

<u>(1)</u> 명령어의 기본 구조

* 시스템 관리를 위한 shell 명령어마다 제공되는 다양한 옵션을 활용하여 최적의 작업을 수행

  ```bash
  $ command [option] [arguments]
  ```

  > option 및 arguments 는 선택사항임

* 명령어 단독 사용

  ```bash
  $ ls
  bin   dev  home  media  opt   root  sbin  sys  usr
  boot  etc  lib   mnt    proc  run   srv   tmp  var
  ```

  > 사실 위 명령어는 `ls .` 과 동일함

* 명령어, 옵션 사용

  * 적절한 옵션을 사용하여 시스템 내부에 원하는 명령을 내림

  ```bash
  $ ls -l
  total 60
  drwxr-xr-x   2 root root 4096 Jan 28 14:53 bin
  ...
  drwxr-xr-x  11 root root 4096 Jan 28 14:53 var
  ```

* 명령어, 옵션, 인자 사용

  * 인자: 명령어의 대상으로 주로 파일이나 디렉토리를 지정해줌

  ```bash
  $ ls -l /usr
  total 32
  drwxr-xr-x  2 root root 4096 Jan 28 14:53 bin
  ...
  drwxr-xr-x  2 root root 4096 Apr 24  2018 src
  ```

<u>(2)</u> 명령어 메뉴얼 활용

* 리눅스에서 제공하는 수많은 명령어마다 지원되는 고유의 옵션을 일일이 알기는 힘듦
* 시스템 내부에서 각 명령어마다 기능과 옵션을 메뉴얼로 제공해줌

```bash
$ man ls
$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
...
```

### <u>2.2.</u> sudo 명령의 이해

##### <u>2.2.1.</u> 사용자와 그룹

<u>(1)</u> 리눅스에서의 사용자

* 리눅스는 다중 사용자 시스템임 (한 시스템에 여러 사용자가 동시에 접속하여 시스템을 사용)
* 한 시스템 내부의 여러 사용자들은 자신만의 작업을 수행
* 작업 수행 시, 시스템이 보유한 자원(저장장치, 프린터 등)을 공유하기도함
* 자연스럽게 시스템, 자원, 사용자들을 관리하는 사람인 시스템 관리자가 필요해짐
* 리눅스 시스템은 **시스템 관리자**(시스템 전반에 대한 모든 권한을 가짐)와 **일반 사용자**(시스템에 대한 일부 권한을 가짐)로 구성된다. (같은 권한을 갖는 사용자가 여러명인 경우, 하나의 그룹으로 묶어 관리하기도함)

<u>(2)</u> 시스템 관리자의 역할

* 시스템 관리자는 "root" 라는 계정을 가지며 아래와 같은 주요 업무를 맡음

1. HW 증설과 설치 및 SW 설치와 업그레이드
   * 서버의 성능과 역할을 명확히 이해하여 나중을 대비하여 HW 증설을 수행
   * 시스템 관리자는 시스템 내부 사용자들에게 최적의 작업환경 제공을 위해 OS 및 SW 들을 적절하게 업그레이드
2. 시스템 보안
   * 네트워크 기술 발전은 시스템 사용자들에게 최적의 작업 환경을 주었으나 외부의 침입에 취약해졌다고 볼 수 있음
   * 시스템 관리자는 외부의 불법 침입을 효과적으로 차단하고 내부 자원을 효율적으로 운영
3. 사용자 계정 관리
   * 시스템 관리자만이 일반 사용자 계정을 생성할 수 있음
   * 사용자 계정 관리를 통해 사용자의 부적절한 자원 접근을 막아야함
4. 하드 디스크 백업
   * 화재 등의 천재지변 및 해커나 바이러스에 의해 시스템 내부의 자료가 유실되지 않도록 하드디스크레 대한 주기적인 백업을 수행해야함

* 이 외에도 시스템 관리자의 역할은 다양하며 그 역할은 시스템과 사용자에 대한 모든 부분을 관리하는 것과 관련되어 있으므로 시스템 자체에 대해 자세히 알고 있어야한다.

<u>(3)</u> 시스템 관지라의 일반 사용자 계정 관리

* 일반 사용자: 본인에게 허락된 범위 내에서 시스템을 이용하는 사용자
* 시스템 관리자만이 일반 사용자 계정을 생성함 (리눅스에서 일반 사용자는 자신의 아이디 생성 권한이 없음)
* 일반 사용자는 자신에게 부여된 계정을 사용하여 시스템에 접근함
* 일반 사용자의 계정 발급 과정
  * 사용자가 시스템 관리자에게 본인의 계정을 요청
  * 시스템 관리자가 계정 아이디가 유효한지 체크하고 임시 비밀번호를 갖는 계정을 만들고 사용자에게 계정 아이디와 비밀번호를 전달 (계정의 설정 권한이 적절한지 꼭 확인해야함)
  * 사용자는 관리자에게 받은 계정 아이디와 비밀번호로 시스템에 접근한 후, 비밀번호를 본인 비밀번호로 변경
* 모든 계정의 비밀번호는 암호화하여 시스템 내부에 저장되어 디코딩이 불가능함 
* 그래서 시스템 관리자라고 하더라도 계정의 비밀번호를 알아낼 수 없음
* 사용자가 계정 비밀번호를 잊어버리면 시스템 관리자 권한으로 비밀번호를 변경해야함
* 일반 사용자의 경우, 관리자가 설정한 권한 내에서만 시스템 내부에서 작업 가능

<u>(4)</u> 리눅스에서의 그룹

* 시스템 내부의 사용자가 많아지면 모든 사용자들을 개별 관리하는 것은 매우 어렵고 복잡함
* 유사한 사용자들을 하나의 그룹으로 묶어 효율적으로 사용자들을 관리함
* **예시.** 하나의 시스템에서 2학년 학생들 200명, 3학년 학생들 300명, 총 500명이 학년별 과제를 확인하는 상황
  * 모든 학생들을 계정 사용자로 생각하지 말고 2학년 200명을 하나의 그룹, 3학년 300명을 하나의 그룹으로 묶어 생각
  * 2학년 그룹은 2학년 과제인 `2nd.pdf` 파일에 읽기 권한을 주고 3학년 과제인 `3rd.pdf` 파일에는 읽기 권한을 주지 않음
  * 3학년 그룹도 마찬가지로 권한을 설정
  * 이런 식으로 권한 설정을 500번하는 것이 아니라 2번만 수행
* 사용자는 최소한 하나의 그룹에 반드시 속해야함
* 그룹 지정 없이 사용자 계정을 생성하면 계정 이름과 동일한 그룹에 속하게됨

##### <u>2.2.2.</u> sudo 명령의 사용

<u>(1)</u> 우분투에서의 시스템 관리자

우분투 관리자

* 시스템 관리자가 항상 "root" 계정으로 작업하지 않고 종종 일반 계정으로 시스템에서 작업을 하기도함
* 그런데 일반 계정으로 있다가도 관리자 권한이 필요한 경우가 있음
* 우분투에서는 "root" 계정으로 전환하기보다는 `sudo` 명령어를 이용하여 관리자 권한을 부여함
* 즉, 일반 사용자 계정에 일부 관리자 권한을 부여하면 `sudo` 명령어를 통해 해당 관리자 권한을 이용할 수 있음
* 시스템 관리자 부재시 생긴 시스템 문제 해결을 일반 사용자에게 맡길때 "root" 계정의 비밀번호를 공개하는 상황을 막을 수 있음

root 계정 생성

* 우분투 최초설치 후, root 계정은 생성되나 비밀번호는 따로 지정해야함

  ```bash
  $ sudo passwd root
  ```

root 계정 전환

* 일반 사용자 계정에서 root 계정으로 전환

  ```bash
  $ su
  password:
  # exit
  $ 
  ```

<u>(2)</u> sudo 명령

* 일반 사용자가 본인의 허용 권한 내에서 root 계정의 권한으로 명령어를 실행할 때 sudo 명령어를 사용

  ```bash
  $ sudo command [option] [arguments]
  ```

* 일반 사용자의 sudo 명령어 사용을 위해서 관리자로부터 권한을 부여 받아야함

* root 계정으로 `/etc/sudoers` 파일을 수정해야함

  ```bash
  $ sudo cat /etc/sudoers
  #
  # This file MUST be edited with the 'visudo' command as root.
  #
  # Please consider adding local content in /etc/sudoers.d/ instead of
  # directly modifying this file.
  #
  # See the man page for details on how to write a sudoers file.
  #
  Defaults        env_reset
  Defaults        mail_badpass
  Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
  # Host alias specification
  # User alias specification
  # Cmnd alias specification
  # User privilege specification
  root    ALL=(ALL:ALL) ALL
  # Members of the admin group may gain root privileges
  %admin ALL=(ALL) ALL
  # Allow members of group sudo to execute any command
  %sudo   ALL=(ALL:ALL) ALL
  # See sudoers(5) for more information on "#include" directives:
  #includedir /etc/sudoers.d
  ```

  > `root ALL=(ALL:ALL) ALL` 의미
  >
  > * root: sudo 명령어를 사용 가능한 계정
  > * 1st ALL: sudo 명령어 실행 가능 호스트 (모든 호스트가 sudo 명령어 실행 가능)
  > * 2nd ALL: sudo 명령어 실행 가능 사용자 (모든 사용자가 sudo 명령어 실행 가능)
  > * 3rd ALL: sudo 명령어 실행 가능 그룹 (모든 그룹이 sudo 명령어 실행 가능)
  > * 4th ALL: 관리자 권한으로 실행할 수 있는 명령어 지정 (모든 명령어는 관리자 권한으로 실행 가능)
  >
  > `%admin ALL=(ALL) ALL` 의미
  >
  > * admin 그룹에 속한 모든 사용자는 sudo 를 통해 모든 명령어를 관리자 권한으로 실행 가능
  >
  > `%sudo ALL=(ALL:ALL) ALL` 의미
  >
  > * sudo 그룹에 속한 모든 사용자는 sudo 를 통해 모든 명령어를 관리자 권한으로 실행 가능

  

  ```bash
  $ sudo -u park mkdir report_data
  ```

  > 관리자 권한으로 park 라는 계정으로 접속해 report_data 라는 이름의 디렉토리를 생성한다.

<u>(Exercise)</u>

1. `/etc/sudoers` 파일 열기

   ```bash
   $ sudo vim /etc/sudoers
   password:
   ```

2. 특정 사용자(kim)에게 useradd, userdel 명령을 수행할 수 있는 권한을 부여

   ```bash
   kim All=/usr/sbin/useradd, /usr/sbin/useradd
   ```

3. useradd 명령어를 사용해 사용자(kim) 생성

   ```bash
   $ sudo useradd kim
   ```

4. 사용자(kim) 비밀번호 지정

   ```bash
   $ sudo passwd kim
   Enter new UNIX password: 
   Retype new UNIX password: 
   passwd: password updated successfully
   ```

5. 생성한 계정으로 전환

   ```bash
   $ su kim
   password:
   (kim)$
   ```

6. 생성한 계정을 통해 새로운 계정(lee) 생성

   ```bash
   (kim)$ sudo useradd lee
   ```

7. 허용되지 않은 명령어 사용해보기

   ```bash
   (kim)$ sudo passwd lee
   Sorry, user kim is not allowed to execute '/usr/bin/passwd lee' as root on aiteen-front.asia-northeast3-a.c.bcaitech2nd-aiteen.internal.
   ```

8. 생성했던 계정(lee) 삭제

   ```bash
   (kim)$ sudo userdel lee
   ```

9. 생성했던 계정(kim) 삭제

   ```bash
   (kim)$ exit
   $ sudo userdel kim
   ```

10. kim 에게 부여했던 권한 삭제

    ```bash
    $ sudo vim /etc/sudoers
    password:
    ```

    > 2번 과정에서 추가했던 내용을 삭제한다.

### <u>2.3.</u> 시스템의 시작과 종료

##### <u>2.3.1.</u> 시스템의 시작

* 시스템 부팅 후, 계정을 선택하고 비밀번호를 입력

##### <u>2.3.2.</u> 시스템의 종료

* 다중 사용자 환경에서 갑작스러운 시스템 종료는 사용자들에게 혼란을 초래할 수도 있음

* sudo 를 통한 shell 명령어(`shutdown`)를 통해 시스템을 여러 방식으로 종료시킬 수 있다.

  ```bash
  $ sudo shutdown option time [message]
  ```

  > option
  >
  > * `-h`: 시스템 종료
  > * `-r`: 시스템 재부팅
  > * `-c`: 예약 종료/재부팅 취소
  > * `-k`: 사용자에게 메시지만 전달 (shutdown 수행X)
  >
  > time
  >
  > * `now`: 지금 즉시 시스템 종료/재부팅
  > * `+시간`: 지정된 시간(분 단위) 후, 시스템 종료/재부팅
  > * `hh:mm`: 지정된 시각에 시스템 종료/재부팅
  >
  > message
  >
  > * 시스템 내부 사용자에게 추가로 전달할 메세지 지정
  > * 미지정시, 기본 메세지만 사용자들에게 전달

<u>(Exercise)</u>

1. 시스템 즉시 종료

   ```bash
   $ sudo shutdown -h now
   ```

2. 시스템 즉시 재부팅

   ```bash
   $ sudo shutdown -r now
   ```

3. 10분 후 시스템 종료

   ```bash
   $ sudo shutdown -h +10
   Shutdown scheduled for Thu 2022-02-10 04:27:20 UTC, use 'shutdown -c' to cancel.
   ```

4. 시스템 종료 취소

   ```bash
   $ sudo shutdown -c
   ```

5. 'Save your files' 라는 메시지를 전송하고 10분 후 시스템 종료 예약 및 취소

   ```bash
   $ sudo shutdown -h +10 Save your files
   Shutdown scheduled for Thu 2022-02-10 04:28:22 UTC, use 'shutdown -c' to cancel.
   $ sudo shutdown -c
   ```

6. 23시 58분에 시스템 종료 예약 및 취소

   ```bash
   $ sudo shutdown -h 23:58
   Shutdown scheduled for Thu 2022-02-10 23:58:00 UTC, use 'shutdown -c' to cancel.
   $ sudo shutdown -c
   ```

