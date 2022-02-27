## <u>Chapter 6.</u> 디렉토리와 파일 관리

* 디렉토리와 파일 관리 방법 숙지
* 디렉토리 종류 숙지, 디렉토리 생성/수정/삭제를 통해 디렉토리 관리 방법 숙지
* 파일의 소유/허가 권한 숙지, 파일 생성/수정/삭제를 통해 파일 관리 방법 숙지
* 파일 내용 출력 및 검색을 위한 명령어 숙지

### <u>6.1.</u> 디렉토리 관리

##### <u>6.1.1.</u> 디렉토리의 개요

<u>(1)</u> 디렉토리란

* 윈도우 운영체제의 '폴더'와 같은 것
* 계층적(Hierarchical) 혹은 트리(Tree) 구조
  * 한 디렉토리 하위에 다른 디렉토리가 위치하는 구조
  * 이러한 구조를 갖는 파일 시스템을 계층적 파일 시스템이라고함
* 리눅스는 계층적 파일 시스템을 이용하는 대표적 운영체제임
* 리눅스 시스템 내의 사용자는 자신의 홈 디렉토리 내에 디렉토리와 파일을 생성하고 유지함

<u>(2)</u> 리눅스의 디렉토리 구조

* 리눅스 운영체제 설치 시 생성되는 디렉토리들은 시스템이 생성한 것

* 시스템이 생성한 디렉토리에 대해 이름 변경, 파일 및 디렉토리 삭제 등을 하면 안됨

* `/home`

  * 사용자들의 홈 디렉토리들을 모아두는 디렉토리
  * 시스템이 웹서버로 사용되는 경우, 웹페이지들을 저장시키는 디렉토리
  * 1개의 웹서버가 여러 웹 사이트를 호스팅(hosting)하는 경우, 웹 사이트마다 1개의 홈 디렉토리 할당
  * 각 사이트마다 할당된 홈 디렉토리에 웹 페이지들이 유지됨

* `/root`

  * 시스템 관지자 계정인 'root' 의 홈 디렉토리로 관리자 권한을 가져야만 접근 가능

* `/bin`

  * 리눅스 시스템 내에서 사용되는 shell 명령어들이 존재하는 디렉토리
  * 시스템에 생성된 각 계정은 `/bin` 디렉토리에 대한 경로를 환경변수로 가지면서 shell 명령어 실행 가능

* `/sbin`

  * 시스템 초기화 명령, 커널 모듈 관리 등 시스템 관리를 위한 명령어들이 존재하는 디렉토리
  * 주로 시스템 관리자가 사용하는 명령어들이 존재함

* `/usr`

  * 기본 실행 파일 및 각종 설치 프로그램, 각종 라이브러리 파일들이 존재하는 디렉토리

* `/dev`

  * 각종 장치 파일들이 위치하는 디렉토리
  * 리눅스는 운영체제에 장착된 모든 장치를 파일로 인식함 (`Everything is a file.`)
  * 키보드, 마우스, 프린터 등의 장치들을 파일 혹은 디렉토리 형태로 `/dev` 디렉토리 안에 존재함
    (`cat /boot/vmlinuz > /dev/dsp` 를 통해 읽기 쓰기가 가능)

* `/lib`

  * 공유 라이브러리 파일들이 존재하는 디렉토리
  * 공유 라이브러리 파일: 시스템 운영 및 응용 프로그램 실행에 필요한 파일들

* `/proc`

  * 시스템의 정보를 확인할 수 있는 파일들이 존재하는 디렉토리
  * CPU 정보(cpuinfo), interrupts 정보, I/O 주소 목록(ioports), 시스템 통계 정보(stat) 등의 시스템 정보
  * 이 디렉토리에 있는 파일을 통해 현재 시스템의 상태, 실행되는 프로세스에 대한 정보 및 데이터 확인
  * 실제 디스크 공간에는 존재하지않는 가상의 디렉토리

* `/tmp`

  * 시스템이 수행하는 작업 도중 임시로 이용하는 디렉토리
  * 주로 사용자 프로그램의 처리, 시스템 운영 과정에서 임시로 사용하는 데이터가 저장됨
  * 시스템 종료 시, `/tmp` 디렉토리에 있는 파일들은 모두 삭제됨

* `/var`

  * 시스템이 수행하는 작업 도중 임시로 이용하는 디렉토리
  * 주로 네트워크를 통한 파일 전송 상황, 각종 오류 상황을 모니터링하는 로그(log) 파일들이 존재함
  * 메일, 프린터 등 스풀링(spooling)이 필요한 프로그램들이 존재함

* `/etc`

  * 시스템 관리를 위한 각종 설정 파일이 위치하는 디렉토리
  * 사용자 정보, 파일 시스템 정보, 네트워크 정보 등 시스템 환경설정을 위한 파일들이 존재함

* 우분투에서 추가로 제공되는 디렉토리

  | 디렉토리 이름 | 설 명                                                        |
  | :-----------: | ------------------------------------------------------------ |
  |    `media`    | USB, CD-ROM 등 외부 장치 마운드를 위한 디렉토리              |
  |     `opt`     | 추가로 설치되는 패키지들이 존재하는 디렉토리                 |
  |     `sys`     | 리눅스 시스템의 커널 관련 파일들이 존재하는 디렉토리         |
  |    `boot`     | 시스템 부팅에 필요한 커널 관련 파일들이 존재하는 디렉토리    |
  | `lost+found`  | 문제 발생으로 시스템 복구 시, 문제가 발생한 파일들이 존재하는 디렉토리 |
  |     `mnt`     | 파일 시스템 마운드를 위한 디렉토리                           |
  |     `run`     | 실행 중인 서비스와 관련된 내용이 존재하는 디렉토리           |
  |     `srv`     | FTP 등 시스템에서 제공하는 서비스의 데이터가 존재하는 디렉토리 |


##### <u>6.1.2.</u> 절대경로와 상대경로

<u>(1)</u> 경로

* 특정 파일이나 디렉토리의 위치

* 파일 복사 (`cp` 명령어)

  ```bash
  $ cp /home/tmdgns1139/mynote.txt /home/tmdgns1139/note/mynote.txt
  ```

<u>(2)</u> 절대경로와 상대경로

* 경로를 표현하는 2가지 방식

  * 절대경로: 루트 디렉토리(`/`)가 경로를 표현하는 기준 위치
  * 상대경로: 현재 사용자가 위치한 디렉토리(`.`)가 경로를 표현하는 기준위치

* 예. 

  ```bash
  /
  ├── home/
  |		├── kim/ (현재 위치)
  |		|		└── lecture/
  |		|		 		└── report/
  |		└── lee/
  |				└── subject/
  └── data/
      └── OS/
          └── linux/
  ```

  |                    |          절대경로          |      상대경로      |
  | :----------------: | :------------------------: | :----------------: |
  | `report` 디렉토리  | `/home/kim/lecture/report` | `./lecture/report` |
  | `subject` 디렉토리 |    `/home/lee/subject`     |  `../lee/subject`  |
  |  `data` 디렉토리   |          `/data`           |    `../../data`    |

##### <u>6.1.3.</u> 디렉토리 관련 명령어

<u>(1)</u> 현재 디렉토리 확인 (`pwd` 명령어)

* 사용자가 현재 위치한 디렉토리 절대경로로 알려줌

* 사용자가 현재 위치한 디렉토리: 작업 디렉토리(working directory) 혹은 현재 디렉토리(current directory)

* 명령어 형식

  ```bash
  $ pwd
  ```

<u>(2)</u> 디렉토리 변경 (`cd` 명령어)

* 특정 디렉토리에서 다른 디렉토리로 이동할 때 사용 (작업 디렉토리를 변경하고 싶을 때)

* 디렉토리 이동 시, 이동하려는 디렉토리에 관한 권한이 있어야함

* 명령어 형식

  ```bash
  $ cd [option] [directory]
  ```

  > options
  >
  > * `~`: 자신의 홈 디렉토리
  > * `~사용자_계정`: 해당 사용자의 홈 디렉토리
  > * 단독사용: 자신의 홈 디렉토리로 이동

<u>(3)</u> 디렉토리 검색 (`ls` 명령어)

* 특정 디렉토리의 내용물을 출력

* 파일의 크기, 종류, 생성/수정 일시, 접근 권한, 소유자/소유그룹 등 정보 제공

* 명령어 형식

  ```bash
  $ ls [option] [directory]
  ```

  > options
  >
  > * `-a`(all): 디렉토리에 존재하는 숨겨진 파일까지 출력
  >   * 숨겨진 파일/디렉토리: `.`으로 시작하는 파일/디렉토리
  > * `-F`(Format): 파일 이름 다음에 파일의 형식을 출력 (실행파일은 `*`, 디렉토리는 `/`, 심볼릭 링크는 `@`를 덧붙임)
  > * `-k`(kilobyte): 파일 크기를 KB 단위로 출력
  > * `-m`: 파일의 이름을 `,`로 구분하여 가로로 나열해 출력
  > * `-l`: 파일의 정보를 자세히 출력
  > * `-d`(directory): 인자로 지정한 디렉토리의 정보를 출력
  > * `-r`: 파일의 이름을 알파벳 내림차순으로 정렬하여 출력
  > * `-R`(Recursive): 현재 디렉토리에 존재하는 모든 하위 디렉토리의 파일 목록까지 출력
  > * `-s`: 파일의 이름 앞에 파일의 크기를 KB 단위로 출력
  > * `-S`: 파일의 크기 내림차순으로 정렬하여 출력
  > * `-t`(time): 최근 수정/생성된 파일 순으로 출력
  > * `-u`: `-l` 옵션과 함께 사용할 경우, 파일의 내용을 수정한 시간 출력
  > * `-c`: `-l` 옵션과 함께 사용할 경우, 파일을 변경한 시간을 출력 (소유자, 허가 권한 병경도 포함)
  > * `-x`: 파일의 이름을 알파벳 오름차순으로 정렬해 가로방향으로 출력
  > * `-X`: 파일을 확장자 순으로 출력 (확장자 없는 파일이 앞에 출력됨)

* 예. `$ ls -l`

  ```bash
  $ ls -l
  total 8
  drwxrwxr-x(1) 2(2) tmdgns1139(3) tmdgns1139(4) 4096(5) Feb 11 08:57(6) test(7)
  ```

* 파일의 종류와 접근 권한 (1)

  * (1) 의 첫번째 알파벳은 파일의 종류를 나타냄

    | 기호 | 설 명                                                      |
    | :--: | ---------------------------------------------------------- |
    | `d`  | 디렉토리임을 의미                                          |
    | `l`  | 소프트(soft)/심볼릭(symbolic) 링크(link)임을 의미          |
    | `b`  | 블록 단위 입출력을 수행하는 블록 장치 파일임을 의미        |
    | `c`  | 문자 단위 입출력을 수행하는 문자 장치 파일임을 의미        |
    | `p`  | 프로세스 사이의 통신에 사용되는 파이프(pipe) 파일임을 의미 |
    | `s`  | 네트워크 통신에 사용되는 소켓(socker) 파일임을 의미        |
    | `-`  | 일반 파일임을 의미                                         |

  * (1) 의 나머지 알파벳은 접근 권한을 나타냄

    * 나중에 다룸

* 링크 수 (2)

  * 하드 링크의 수를 나타냄
  * 하드 링크: 해당 파일과 같은 i-node 를 갖는 파일

* 파일 소유자 (3)

  * 파일의 소유자가 `tmdgns1139` 임을 나타냄

* 파일 소유 그룹 (4)

  * 파일의 소유 그룹이 `tmdgns1139` 임을 나타냄

* 파일의 크기 (5)

  * 파일의 크기가 `4096`임을 나타냄

* 파일의 최종 접근 시간 (6)

* 파일의 이름 (7)

<u>(4)</u> 디렉토리 생성 (`mkdir` 명령어)

* 시스템 내의 한 디렉토리 안에 디렉토리를 생성

* 생성 가능한 디렉토리의 수에는 제한이 없음

* 명령어 형식

  ```bash
  $ mkdir [option] 디렉토리_이름
  ```

  > options
  >
  > * `-p`: 하나의 디렉토리와 그 하위 디렉토리를 동시에 생성함
  > * `-m`: 디렉토리를 생성할 때 접근 권한을 지정

<u>(5)</u> 디렉토리 삭제 (`rmdir` 명령어)

* 시스템 내의 디렉토리 삭제

  * 시스템으로부터 바로 삭제되어 복원 불가능
  * 비어있지 않은 디렉토리는 삭제 불가능 (옵션을 주어야함)
  * 현재 위치한 디렉토리는 삭제 불가능 (상위 디렉토리나 다른 디렉토리에서 삭제해야함)

* 명령어 형식

  ```bash
  $ rmdir [option] 디렉토리_이름
  ```

  > option
  >
  > * `-p`: 하나의 디렉토리와 그 하위 디렉토리를 동시에 삭제

<u>(Exercise)</u>

1. 현재 위치한 디렉토리 경로 출력

   ```bash
   $ pwd
   /home/tmdgns1139
   ```

2. `/usr/games` 디렉토리로 이동 후, 현재 위치한 디렉토리 경로 출력

   ```bash
   $ cd /usr/games
   $ pwd
   /usr/games
   ```

3. 자신의 홈 디렉토리로 이동 후, 현재 위치한 디렉토리 경로 출력

   ```bash
   $ cd ~
   $ pwd
   /home/tmdgns1139
   ```

4. 현재 위치한 디렉토리보다 한 단계 상위 디렉토리로 이동 후 경로 출력

   ```bash
   $ cd ..
   $ pwd
   /home
   ```

5. `kim` 이라는 사용자 계정 생성 후, 임의의 패스워드 부여

   ```bash
   $ sudo adduser kim
   ```

6. `kim` 계정의 홈 디렉토리로 이동

   ```bash
   $ cd ~kim
   $ pwd
   /home/kim
   ```

7. 숨겨진 파일을 포함하여 현재 디렉토리의 내용을 자세히 출력

   ```bash
   $ ls -al
   total 76
   drwxr-xr-x 7 tmdgns1139 tmdgns1139  4096 Feb 14 07:45 .
   drwxr-xr-x 7 root       root        4096 Feb 14 08:30 ..
   -rw------- 1 tmdgns1139 tmdgns1139 10691 Feb 13 17:32 .bash_history
   drwx------ 2 tmdgns1139 tmdgns1139  4096 Feb 14 07:48 .ssh
   drwxrwxr-x 6 tmdgns1139 tmdgns1139  4096 Feb 10 10:55 frontend
   drwxrwxr-x 2 tmdgns1139 tmdgns1139  4096 Feb 11 08:57 test
   ```

8. `/lib` 디렉토리에 존재하는 모든 파일의 이름 및 파일 형식을 출력

   ```bash
   $ ls /lib -F
   apparmor/       hdparm/                                libnss_cache_oslogin.so.2@     modules-load.d/  udev/
   console-setup/  init/                                  libnss_oslogin-20210907.00.so  netplan/         ufw/
   crda/           klibc-wBFLvVtxy4xJqEadIBJMa78iJz8.so*  libnss_oslogin.so.2@           open-iscsi/      x86_64-linux-gnu/
   cryptsetup/     libhandle.so.1@                        lsb/                           recovery-mode/
   ebtables/       libhandle.so.1.0.3
   ```

9. `/usr` 디렉토리에 존재하는 모든 파일의 이름을 `,`로 구분하여 출력

   ```bash
   $ ls /usr -m
   bin, config, games, include, lib, local, sbin, share, src
   ```

10. `/usr` 디렉토리에 존재하는 파일의 이름만 알파벳 오름차순으로 정렬하여 가로방향으로 출력

    ```bash
    $ ls /usr -x
    bin  config  games  include  lib  local  sbin  share  src
    ```

11. 현재 디렉토리에 존재하는 파일의 이름을 출력하고 하위 디렉토리 내용까지 모두 출력

    ```bash
    $ ls -R
    .:
    frontend  test
    
    ./frontend:
    Dockerfile  README.md  __pycache__  confirm_button_hack.py  front.py  log  requirements.txt  utils.py
    
    ./frontend/__pycache__:
    confirm_button_hack.cpython-36.pyc  utils.cpython-36.pyc
    
    ./frontend/log:
    frontend.log
    
    ./test:
    firstfile.txt  secondfile.txt  thirdfile.txt  vi_test1.txt  vi_test2.txt  vi_test3.txt
    ```

12. `/boot` 디렉토리에 존재하는 파일을 자세히 출력 (파일의 생성 일시에 따라 내림차순 정렬)

    ```bash
    $ ls /boot -lt
    total 155581
    drwxr-xr-x 6 root root     4096 Feb  9 06:06 grub
    -rw-r--r-- 1 root root 37737008 Feb  8 06:31 initrd.img-5.4.0-1064-gcp
    -rw------- 1 root root 10998016 Feb  6 05:40 vmlinuz-5.4.0-1064-gcp
    -rw------- 1 root root  4629904 Feb  6 05:22 System.map-5.4.0-1064-gcp
    -rw-r--r-- 1 root root   233127 Feb  6 05:22 config-5.4.0-1064-gcp
    -rw-r--r-- 1 root root 37738285 Feb  4 06:41 initrd.img-5.4.0-1063-gcp
    -rw------- 1 root root 10998016 Jan 18 11:04 vmlinuz-5.4.0-1063-gcp
    -rw------- 1 root root  4629904 Jan 18 10:53 System.map-5.4.0-1063-gcp
    -rw-r--r-- 1 root root   233127 Jan 18 10:53 config-5.4.0-1063-gcp
    -rw-r--r-- 1 root root 37726992 Dec 14 17:11 initrd.img-5.4.0-1058-gcp
    -rw------- 1 root root  9498880 Nov 15 07:32 vmlinuz-5.4.0-1058-gcp
    -rw------- 1 root root  4628090 Nov 15 07:16 System.map-5.4.0-1058-gcp
    -rw-r--r-- 1 root root   233071 Nov 15 07:16 config-5.4.0-1058-gcp
    drwx------ 3 root root      512 Jan  1  1970 efi
    ```

13. `/home` 디렉토리 내부의 내용이 아닌 디렉토리 자체에 대한 자세한 정보 출력

    ```bash
    $ ls /home -ld
    drwxr-xr-x 7 root root 4096 Feb 14 08:30 /home
    ```

14. 현재 디렉토리에 `aa`라는 이름의 디렉토리 생성 후 확인

    ```bash
    $ mkdir aa
    $ ls -F
    aa/  frontend/  test/
    ```

15. 현재 디렉토리에 `bb`, `cc` 라는 이름의 두 디렉토리를 동시에 생성 후 확인

    ```bash
    $ mkdir bb cc
    $ ls -mF
    aa/, bb/, cc/, frontend/, test/
    ```

16. 위에서 생성한 `bb` 디렉토리의 하위 디렉토리로 `bb-1` 디렉토리 생성

    ```bash
    $ mkdir ./bb/bb-1
    $ ls -l ./bb
    total 4
    drwxrwxr-x 2 tmdgns1139 tmdgns1139 4096 Feb 14 08:49 bb-1
    ```

17. `dd` 디렉토리와 `dd/dd-1` 디렉토리를 동시에 생성 후 확인

    ```bash
    $ mkdir -p dd/dd-1
    $ ls -mF
    aa/, bb/, cc/, dd/, frontend/, test/
    $ ls -l ./dd/
    total 4
    drwxrwxr-x 2 tmdgns1139 tmdgns1139 4096 Feb 14 08:51 dd-1
    ```

18. `aa` 디렉토리 삭제 후 확인

    ```bash
    $ rmdir aa
    ```

19. `bb` 디렉토리의 한 단계 하위 디렉토리인 `bb-1` 디렉토리 삭제 후 확인

    ```bash
    $ rmdir bb/bb-1
    ```

20. `bb`, `cc` 디렉토리 동시 삭제 후 확인

    ```bash
    $ rmdir bb cc
    ```

21. `dd` 디렉토리와 그 한 단계 하위 디렉토리 `dd-1` 을 동시 삭제 후 확인

    ```bash
    $ rmdir dd
    rmdir: failed to remove 'dd': Directory not empty
    $ rmdir -p dd/dd-1
    $ ls -mF
    frontend/, test/
    ```

### <u>6.2.</u> 파일 관리

##### <u>6.2.1.</u> 파일의 개요

<u>(1)</u> 리눅스 파일의 특징

* 리눅스 시스템에서 디스크에 존재하는 자료들은 파일 형태로 유지됨
* 리눅스에서의 파일과 윈도우에서의 파일에는 많은 차이가 있음

1. 큰 의미가 없는 확장자
   * 리눅스에서 파일의 확장자는 파일의 종류를 나타내지 않으며 확장자가 없는 파일도 있음
   * 텍스트 파일을 `.jpg` 확장자로 지정하더라도 리눅스 시스템은 해당 파일을 텍스트 파일로 인식함
   * C언어 소스 파일(`.c`), HTML 파일(`.htm`, `.html`), 이미지 파일(`.gif`, `.jpg`, `png` 등), MP3 파일(`.mp3`) 등의 파일에 대해서는 확장자를 인식함
2. 대소문자를 구별함
   * 대소문자 구별이 엄격함 (단 한 글자라도 대소문자 차이가 있으면 별개의 파일로 간주)
   * 피일 이름은 256글자까지 가능
   * 파일 이름에 공백, 특수 문자 포함은 가능하나 권장하지는 않음
3. 엄격한 소유/허가 권한을 가짐
   * 윈도우와 리눅스의 가장 큰 차이점 중 하나
   * 소유권한: 파일의 소유자가 누구인지 명시
   * 허가권한: 어떤 사용자에게 어떤 권한이 허가되었는지 명시
   * 모든 파일은 생성시점부터 소유/허가 권한을 가짐

<u>(2)</u> 리눅스 파일의 종류

* 리눅스는 모든 것을 파일로 간주함 (`Everything is a file`)
  (사용자가 생성한 파일, 디렉토리 및 시스템에 장착된 장치들까지)

1. 디렉토리 파일
   * `mkdir` 명령어를 통해 디렉토리 파일을 생성할 수 있음
   * `d` 라는 문자로 디렉토리 파일을 표시함
2. 장치 파일
   * 시스템에 존재하는 각 장치의 구동을 위한 드라이버들이 존재함
   * 장치 파일: 장치 운용을 위해 사용되는 파일
   * 블록 장치 파일(block device file) 과 문자 장치 파일(character device file) 2종류가 존재함
     * 블록 장치 파일(`b`): 디스크와 같은 블록형 장치 운용을 위한 파일
     * 문자 장치 파일(`c`): 단말기와 같은 문자형 장치 운용을 위한 파일
3. 특수 목적 파일
   * 심볼릭 링크 파일(`l`): 파일의 링크를 의미하는 파일
   * 파이프(pipe) 파일(`p`): 프로세스/명령어의 수행 결과를 다른 프로세스/명령어의 입력으로 연결하는 파일
   * 소켓(socket) 파일(`s`): 네트워크 통신에 사용되는 파일
4. 일반 파일
   * 위 3가지 종류가 아닌 대부분의 파일들
   * 문서 파일, 음악 파일, 이미지 파일 등으로 `-`로 표현됨

##### <u>6.2.2.</u> 파일의 소유/허가 권한

<u>(1)</u> 소유/허가 권한의 개요

* 리눅스 파일 시스템의 가장 큰 특징

* 소유 권한: 특정 파일이 누구 소유인지를 나타냄

* 허가 권한: 사용자별 파일의 읽기/쓰기/실행 허용 권한을 명시

* 소유/허가 권한 이해를 위한 사용자의 분류

  ```bash
  drwxr-x-r-- 2 tmdgns1139 student 4096 May 1 23:46 testdir
  ```

  * 파일 소유자 (owner - tmdgns1139)
    * 해당 파일을 소유한 계정을 명시
    * 파일 생성 시, 파일을 생성한 사람이 파일 소유자로 지정됨
    * 위 예시에서는 `testdir`이라는 디렉토리의 소유 계정은 `tmdgns1139`임을 명시 
    * `chmod` 명령어를 통해 파일의 소유자 변경이 가능
  * 파일 소유 그룹 (group - student)
    * 해당 파일을 소유한 그룹을 명시
    * 위 예시에서는 `testdir`이라는 디렉토리의 소유 그룹은 `student`임을 명시
      (`student` 그룹에 속해있는 사용자들은 `testdir` 에 대해 `r-x` 권한을 가짐)
    * `chgrp` 명령어를 통해 파일의 소유 그룹 변경이 가능함
  * 기타 사용자 (other)
    * 파일 소유자도 아니고 파일 소유 그룹에도 속하지 않은 사용자들
    * 위 예시에서는 `tmdgns1139` 계정도 아니고 `student` 그룹에 속하지도 않은 계정들

* 사용자 분류를 통해 파일을 효율적으로 관리하고 허가되지 않은 접근으로부터 파일을 보호하기 좋음

* 중요한 파일의 경우, 사용자의 유형별로 별도의 권한을 지정하여 불법적인 접근을 막기 좋음

* 특정 파일, 디렉토리에 대해 사용자들의 접근 권한을 쉽게 지정할 수 있음

<u>(2)</u> 파일의 소유 권한 변경 (`chown`, `chgrp` 명령어)

* `chown`(change owner): 시스템 관리자가 파일이나 디렉토리의 소유자를 변경하는 명령어

* 명령어 형식

  ```bash
  $ chown [option] 소유자명 파일/디렉토리
  ```

  > option
  >
  > * `-R 디렉토리`: 지정한 디렉토리와 해당 디렉토리 하위의 모든 디렉토리/파일의 소유자를 모두 변경

* `chgrp`(change group): 시스템 관리자가 파일이나 디렉토리의 소유 그룹을 변경하는 명령어

* 명령어 형식

  ```bash
  $ chgrp [option] 소유그룹명 파일/디렉토리
  ```

  > option
  >
  > * `-R 디렉토리`: 지정한 디렉토리와 해당 디렉토리 하위의 모든 디렉토리/파일의 소유 그룹을 모두 변경

* `chown` 명령어만으로 소유자 혹은 소유 그룹 변경

  ```bash
  $ chown 소유자명.소유그룹명 파일/디렉토리
  ```

  ```bash
  $ chown .소유그룹명 파일/디렉토리
  ```

  ```bash
  $ chown 소유자명 파일/디렉토리
  ```

<u>(3)</u> 파일의 허가 권한

* 특정 파일 관련, 누구(**소유자/소유 그룹/기타 사용자**)에게 어떤 권한(**읽기/쓰기/실행** 가능 여부)이 부여되었는가를 표현

  * 읽기 권한(`r`): 해당 파일을 읽을 수 있는 권한
  * 쓰기 권한(`w`): 해당 파일을 수정 할 수 있는 권한
  * 실행 권한(`x`): 해당 파일을 실행할 수 있는 권한
  * 금지(`-`): 읽기/쓰기/실행 등의 권한을 주지 않는 것

* 3가지의 권한이 파일에 적용되는 경우와 디렉토리에 적용되는 경우에는 차이가 있음

  |   권 한   | 파일에 지정된 경우               | 디렉토리에 지정된 경우                                     |
  | :-------: | -------------------------------- | ---------------------------------------------------------- |
  | 읽기(`r`) | 지정된 파일을 읽을 수 있음       | 디렉토리 내의 내용 출력 가능 <br />(`ls` 명령어 사용 가능) |
  | 쓰기(`w`) | 파일 수정/삭제 가능              | 디렉토리 내에 파일/디렉토리를 생성/수정/삭제 가능          |
  | 실행(`x`) | 실행 가능 파일인 경우, 실행 가능 | 해당 디렉토리로 이동 가능<br />(`cd` 명령어 사용 가능)     |
  | 금지(`-`) | 파일에 권한을 부여하지 않음      | 해당 디렉토리로 이동 가능<br />(`cd` 명령어 사용 가능)     |

* 사용자 유형별 허가 권한은 `r`, `w`, `x`, `-` 문자를 조합하여 표현

* `d(파일유형)rwx(소유주_권한)r-w(소유그룹_권한)r--(기타사용자_권한)`의 의미

  * 파일 유형은 `d` 로 디렉토리임
  * 파일 소유주에게 주어진 권한은 `rwx` 로 읽기/쓰기/실행 권한이 모두 주어져 있음
  * 파일 소유 그룹에 속한 사용자에게 주어진 권한은 `r-x`로 읽기/실행 권한만 주어져 있음
  * 파일 소유주도 아니고 소우 그룹도 아닌 기타 사용자에게 주어진 권한은 `r--`로 읽기 권한만 주어져 있음

* 기호를 이용한 허가 권한 변경 (`chmod` 명령어)

  * 관리자는 명령어를 통해 파일/디렉토리의 허가 권한 변경을 수행

  * 기존의 허가 권한 중 일부를 추가/삭제하는 방법으로 기호를 사용

  * 명령어 형식

    ```bash
    $ chmod [option] user_range operator auth file/directory
    ```

    > option
    >
    > * `-R`: 하위 디렉토리의 모든 파일에 대해 같은 허가 권한을 지정
    >
    > user_range
    >
    > * `-u`(user): 파일의 소유자를 의미
    > * `-g`(group): 파일의 소유 그룹을 의미
    > * `-o`(other): 소유주도 아니고 소유 그룹에 속하지도 않는 기타 사용자 의미
    > * `-a`(all): 모든 사용자를 의미
    >
    > operator
    >
    > * `+`: 새로운 권한 추가
    > * `-`: 기존 권한 제거
    > * `=`: 권한을 새로 지정
    >
    > auth
    >
    > * `r`: 읽기 권한을 의미
    > * `w`: 쓰기 권한을 의미
    > * `x`: 실행 권한을 의미

    ```bash
    $ chmod u=rw,g+x,o+r ./myfile
    ```

* 8진법을 이용한 허가 권한 변경 (`chmod` 명령어)

  * 새로운 권한을 기존 권한에 덮어씌움

  * 권한 지정을 위한 8진수 허가 모드 표현

    | 권 한 | 2진수 표현 | 8진수 표현 |
    | :---: | :--------: | :--------: |
    | `---` |   `000`    |     0      |
    | `--x` |   `001`    |     1      |
    | `-w-` |   `010`    |     2      |
    | `-wx` |   `011`    |     3      |
    | `r--` |   `100`    |     4      |
    | `r-x` |   `101`    |     5      |
    | `rw-` |   `110`    |     6      |
    | `rwx` |   `111`    |     7      |

    > `rwxrw-r--` 의 경우 `764` 로 표현할 수 있음


<u>(4)</u> mask 를 이용한 기본 허가 권한 지정

* 파일 생성 시, 지정되는 기본 허가 권한: `0666 - mask`

* 디렉토리 생성 시, 지정되는 기본 허가 권한: `0777 - mask`

* 기본으로 지정되는 허가 권한은 마스크(mask)에 의해 결정됨

  ```bash
  $ umask
  0002 # 기타 사용자의 w 권한 삭제
  ```

* umask 값을 설정을 통해, 파일/디렉토리 생성 시 부여되는 허가 권한을 설정 가능

  ```bash
  $ umask 0022 # 소유그룹과 기타 사용자의 w 권한 삭제
  ```

  > mask 값을 일시적으로 `0022`로 변경

<u>(Exercise)</u>

1. root 계정으로 전환 후, 전환 사실 확인

   ```bash
   $ su
   Password:
   # whoami
   root
   ```

2. `greeting` 이라는 파일 생성

   ```bash
   # vim greeting
   ```

3. 위에서 만든 파일의 소유/허가 권한 출력

   ```bash
   # ll greeting 
   -rw-r--r-- 1 root root 33 Feb 14 11:39 greeting
   ```

4. 위에서 만든 파일의 소유자를 `tmdgns1139` 로 변경 후, 변경 사실 확인

   ```bash
   # sudo chown tmdgns1139 greeting
   # ll ./greeting 
   -rw-r--r-- 1 tmdgns1139 root 33 Feb 14 11:39 ./greeting
   ```

5. 위에서 만든 파일의 소유 그룹을 `tmdgns1139` 로 변경 후, 변경 사실 확인

   ```bash
   # sudo chgrp tmdgns1139 greeting
   ```

   ```bash
   # sudo chown .tmdgns1139 greeting
   # ll ./greeting 
   -rw-r--r-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
   ```

6. 위에서 만든 파일의 소유 그룹만 `root`로 변경 후 사실 확인

   ```bash
   # sudo chgrp root greeting
   # ll ./greeting 
   -rw-r--r-- 1 tmdgns1139 root 33 Feb 14 11:39 ./greeting
   ```

   ```bash
   # sudo chown .root greeting
   ```

7. 위에서 만든 파일의 소유자와 소유 그룹을 `tmdgns1139` 로 변경 후, 변경 사실 확인

   ```bash
   # sudo chown tmdgns1139.tmdgns1139 greeting
   # ll ./greeting 
   -rw-r--r-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
   ```

8. `data` 라는 디렉토리 생성하고 해당 디렉토리 내에 `file2`, `file3` 라는 이름의 파일 생성

   ```bash
   # mkdir data
   # touch data/file2
   # touch data/file3
   # ll ./data
   total 8
   drwxr-xr-x 2 root       root       4096 Feb 14 11:47 ./
   drwxrwxr-x 3 tmdgns1139 tmdgns1139 4096 Feb 14 11:47 ../
   -rw-r--r-- 1 root       root          0 Feb 14 11:47 file2
   -rw-r--r-- 1 root       root          0 Feb 14 11:47 file3
   ```

9. `data` 디렉토리와 디렉토리 아래의 모든 파일/디렉토리의 소유자와 소유 그룹을 `tmdgns1139`로 변경

   ```bash
   # sudo chown tmdgns1139.tmdgns1139 -R ./data
   # ll ./data
   total 8
   drwxr-xr-x 2 tmdgns1139 tmdgns1139 4096 Feb 14 11:47 ./
   drwxrwxr-x 3 tmdgns1139 tmdgns1139 4096 Feb 14 11:47 ../
   -rw-r--r-- 1 tmdgns1139 tmdgns1139    0 Feb 14 11:47 file2
   -rw-r--r-- 1 tmdgns1139 tmdgns1139    0 Feb 14 11:47 file3
   ```

10. `greeting` 파일의 소유/허가 권한 출력

    ```bash
    $ ls -l ./greeting
    -rw-r--r-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

11. 기호를 통해 위 파일의 소유그룹에 쓰기 권한 추가

    ```bash
    $ chmod g+w ./greeting
    $ ls -l ./greeting 
    -rw-rw-r-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

12. 기호를 통해 위 파일의 소유자와 소유그룹에 각각 실행 권한 추가

    ```bash
    $ chmod u+x,g+x ./greeting # chmod ug+x ./greeting
    $ ls -l ./greeting 
    -rwxrwxr-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

13. 기호를 통해 위 파일의 소유자와 소유그룹에 실행 권한 제거 & 기타 사용자에 쓰기 권한 추가

    ```bash
    $ chmod ug-x,o+w ./greeting
    $ ls -l ./greeting
    -rw-rw-rw- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

14. 기호를 통해 위 파일의 소유자, 소유그룹, 기타 사용자에 실행 권한 추가

    ```bash
    $ chmod ugo+x ./greeting # chmod a+x ./greeting
    $ ls -l ./greeting 
    -rwxrwxrwx 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

15. 기호를 통해 소유자에게 읽기/쓰기/실행 권한을, 소유그룹에게 읽기/쓰기 권한을, 기타 사용자에게 읽기 권한 주기

    ```bash
    $ chmod u=rwx,g=rw,o=r ./greeting
    $ ls -l ./greeting 
    -rwxrw-r-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

16. `greeting` 파일의 소유자에게 읽기/쓰기 권한을 소유그룹과 기타 사용자에게 읽기 권한 주기

    ```bash
    $ chmod 644 ./greeting
    $ ls -l ./greeting 
    -rw-r--r-- 1 tmdgns1139 tmdgns1139 33 Feb 14 11:39 ./greeting
    ```

17. `data` 디렉토리와 그 하위 모든 파일에 대해 소유자는 읽기/쓰기/실행 권한을, 소유그룹과 기타 사용자에게 읽기/실행 권한 주기

    ```bash
    $ chmod -R 755 ./data
    $ ls -l ./data
    total 0
    -rwxr-xr-x 1 tmdgns1139 tmdgns1139 0 Feb 14 11:47 file2
    -rwxr-xr-x 1 tmdgns1139 tmdgns1139 0 Feb 14 11:47 file3
    ```

##### <u>6.2.3.</u> 파일의 복사/이동/삭제

<u>(1)</u> 파일의 복사 (`cp` 명령어)

* 명령어 형식

  ```bash
  cp [option] source_file/directory target_file/directory
  ```

  > options
  >
  > * `-r`(recursive): 디렉토리 복사 시, 사용하는 옵션
  > * `-a`: 파일의 모든 속성을 함께 복사 (생성 일시 등)
  > * `-f`(force): 복사하려는 파일이 있는 경우, 덮어쓰기 강제 진행
  > * `-i`: 복사하려는 파일이 있는 경우, 사용자에게 복사 여부를 물어봄
  > * `-u`: 복사하려는 파일이 있는 경우, 복사하려는 파일에 비해 최근에 수정된 파일이면 복사를 하지 않음
  > * `-v`(verbose): 파일 복사 과정 출력

<u>(2)</u> 파일의 이동 (`mv` 명령어)

* 파일/디렉토리를 옮기거나 이름을 변경할 때 사용

* 명령어 형식

  ```bash
  $ mv [option] source_file/directory target_file/directory
  ```

  > options
  >
  > * `-f`(force): 목표 파일과 같은 이름의 파일이 존재할 경우, 기존 파일에 덮어씌움 
  > * `-i`: 목표 파일과 같은 이름의 파일이 존재할 경우, 사용자에게 명령어 수행 여부를 물어봄
  > * `-v`(verbose): 파일의 이동 과정을 출력

<u>(3)</u> 파일의 삭제 (`rm` 명령어)

* 파일/디렉토리 삭제 후, 복구 불가능

* 명령어 형식

  ```bash
  $ rm [option] file/directory
  ```

  > options
  >
  > * `-f`(force): 파일/디렉토리 강제 삭제 (삭제 과정에서 메세지 출력 없음)
  > * `-i`: 파일/디렉토리 삭제 전, 사용자에게 삭제 여부 물어봄
  > * `-r`(recursive): 디렉토리 제거 시, 사용되는 옵션
  > * `-v`(verbose): 파일 삭제 과정 출력

##### <u>6.2.4.</u> 파일 링크

<u>(1)</u> `i-node`와 파일 링크의 개요

* 사용자는 커널을 통해 파일이 저장된 하드 디스크에 접근함

* 그래서, 커널은 파일을 관리할 수 있는 자료구조를 가지고 있어야함 (대표적으로, `i-node`)

* 시스템 상의 모든 파일/디렉토리는 `i-node`를 가지고 있고 `$ ls -il` 명령을 통해 확인 가능

  ```bash
  $ ls -ailF
  total 76
  258062 drwxr-xr-x 7 tmdgns1139 tmdgns1139  4096 Feb 15 04:34 ./
    1637 drwxr-xr-x 7 root       root        4096 Feb 14 08:30 ../
  296139 -rw-r--r-- 1 tmdgns1139 tmdgns1139  3808 Feb 10 10:25 .bashrc
  323088 drwxrw-rw- 2 tmdgns1139 tmdgns1139  4096 Feb 15 04:34 test/
  ```

  > 제일 앞의 숫자들이 파일의 i-node 임

* 파일의 이름이 달라도 `i-node`가 같다면 같은 파일임을 의미함

* 파일의 링크: 기존의 파일에 새로운 이름을 부여하는 것

<u>(2)</u> 하드 링크

* 원본 파일과 같은 내용을 갖고 같은 `i-node`를 가짐

* 하드 링크를 수정하면 원본 파일도 같이 수정됨

* 원본파일의 위치가 변경되거나 삭제되는 경우에도 하드 링크는 계속 유지됨

* 그래서, 하드 링크는 중요한 파일의 복사본 유지를 위해 주로 사용됨

* 명령어 형식

  ```bash
  $ ln target_file link_file
  ```

<u>(3)</u> 소프트 링크 (심볼릭 링크)

* 소프트 링크는 원본 파일의 정보만 갖고 다른 `i-node`를 가짐

* 원본 파일에 대한 포인터를 의미함

* 원본 파일을 이동시키거나 삭제하면, 소프트 링크의 기능은 상실됨

* 여러 디렉토리에 흩어져있는 각종 설정 파일들을 하나의 디렉토리에 모아 관리

* 자주 이용되는 설정 파일들을 소프트 링크로 생성하고 한 디렉토리에 모아 관리

* 명령어 형식

  ```bash
  $ ln -s source_file link_file
  ```

<u>(Exercise)</u>

1. `member` 파일을 아래와 같이 생성

   ```bash
   $ cat > member
   1. hong gildong
   ```

2. `member` 파일에 대한 하드링크 `member_hl`, 소프트링크 `member_sl` 를 생성 및 원본과 링크의 i-node 확인

   ```bash
   $ ln member member_hl
   $ ln -s member member_sl
   $ ls -ilF
   total 8
   260070 -rw-rw-rw- 2 tmdgns1139 tmdgns1139 16 Feb 15 05:17 member
   260070 -rw-rw-rw- 2 tmdgns1139 tmdgns1139 16 Feb 15 05:17 member_hl
   260071 lrwxrwxrwx 1 tmdgns1139 tmdgns1139  6 Feb 15 05:17 member_sl -> member
   ```

3. 원본 파일에 아래와 같이 문장을 추가 후, 링크들의 내용 출력

   ```bash
   $ cat >> member
   2. Lee Sunshin
   $ cat member_hl
   1. hong gildong
   2. Lee Sunshin
   $ cat member_sl
   1. hong gildong
   2. Lee Sunshin
   ```

4. `person` 디렉토리 생성 후, 디렉토리에 `member` 파일을 옮기고 링크들의 내용 출력

   ```bash
   $ mkdir person
   $ mv ./member ./person
   $ cat member_hl
   1. hong gildong
   2. Lee Sunshin
   $ cat member_sl
   cat: member_sl: No such file or directory
   ```

##### <u>6.2.5.</u> pipe 와 redirection

* 프롬프트에서 `;` 구분자를 통해 한번에 2개 이상의 shell 명령어를 독립적으로 수행

  ```bash
  $ ls -l member_hl; cat member_hl
  -rw-rw-rw- 2 tmdgns1139 tmdgns1139 65 Feb 15 05:21 member_hl
  1. hong gildong
  2. Lee Sunshin
  3. hl: hard link
  4. sl: soft link
  ```

<u>(1)</u> pipe 를 이용한 명령어 수행

* 1개 이상의 shell 명령어를 수행

* 한 명령어의 수행 결과를 다른 명령어의 입력으로 사용

* `|` 구분자를 통해 shell 명령어들을 구분함

* 예시

  ```bash
  $ cat member_hl | grep hong
  ```

  > `cat` 명령어 수행 결과를 `grep` 명령어의 입력으로 전달하여 최종 결과를 얻게됨 

<u>(2)</u> redirection 을 이용한 입출력 방향 지정

* 리눅스에서의 입출력은 표준 입력과 표준 출력으로 이루어짐

  * 표준 입력: 키보드와 같은 표준 입력장치에 의해 이루어지는 것
  * 표준 출력: 모니터와 같은 표준 출력장치에 의해 이루어지는 것
  * 표준 오류: 정상적인 결과를 출력하지 않고 오류를 출력하는 경우

* redirection 을 통해 입출력 방향을 변경할 수 있음

  * (키보드 사용을 통한 데이터 입력을) 파일을 통한 데이터 입력으로 변경 가능
  * (모니터가 아닌) 파일에 수행 결과를 출력하도록 변경 가능

* file descriptor: 리눅스가 파일을 처리하는데 있어서 파일에 부여하는 번호

  * 리눅스에서는 입출력 장치도 파일이므로 file descriptor 번호가 부여되어 사용됨
  * 표준 입력은 0번, 표준 출력은 1번, 오류 출력은 2번의 file descriptor 가 부여됨

* redirection: 각 장치에 대한 file descriptor 와 출력 방향을 나타내는 기호를 사용한 것

  * `>` vs `>>`: 지정된 파일이 존재할 경우, 전자(overwrite)는 덮어쓰기를 수행하고 후자(append)는 내용을 추가함

* 출력 redirection 형식

  ```bash
  $ command 1> filename
  $ command > filename
  ```

  ```bash
  $ command 1>> filename
  $ command >> filename
  ```

  > * `1`은 표준 출력 file descriptor 이고 지정한 파일을 표준 출력으로 사용하겠다는 의미
  > * 표준 출력에 나오는 command 의 결과를 filename 의 입력으로 사용

* 오류 redirection

  * 오류 출력: 명령어 수행에 오류 발생으로 명령이 정상적으로 수행되지 않은 결과

  * 오류 출력을 나타내는 file descriptor(`2`)를 생략할 수 없음

  * 형식

    ```bash
    $ command 2> filename
    ```

    ```bash
    $ command 2>> filename
    ```

* 입력 redirection

  * 형식

    ```bash
    $ command 0< filename
    ```

    ```bash
    $ command < filename
    ```

    > `0`은 표준 입력 file descriptor 로 지정한 파일을 표준 입력으로 사용하겠다는 의미

  * `<`: 지정한 파일을 읽어 명령어의 입력으로 전달

<u>(Exercise)</u>

1. 디렉토리 내에 member 로 시작하는 파일의 정보를 파일에 출력하고 해당 파일 확인

   ```bash
   $ ls -l member* 1> result_stdin
   $ cat result_stdin 
   -rw-rw-rw- 1 tmdgns1139 tmdgns1139 74 Feb 15 05:27 member
   -rw-rw-rw- 2 tmdgns1139 tmdgns1139 65 Feb 15 05:21 member_hl
   lrwxrwxrwx 1 tmdgns1139 tmdgns1139  6 Feb 15 05:17 member_sl -> member
   ```

2. `nation` 파일을 아래와 같이 생성

   ```bash
   $ cat 1> nation
   1. korea
   2. japan
   3. china
   ```

3. 생성된 `nation` 파일의 내용아래에 `result_stdin` 내용을 추가

   ```bash
   $ cat result_stdin 1>> nation
   $ cat nation
   1. korea
   2. japan
   3. china
   -rw-rw-rw- 1 tmdgns1139 tmdgns1139 74 Feb 15 05:27 member
   -rw-rw-rw- 2 tmdgns1139 tmdgns1139 65 Feb 15 05:21 member_hl
   lrwxrwxrwx 1 tmdgns1139 tmdgns1139  6 Feb 15 05:17 member_sl -> member
   ```

4. 존재하지 않는 디렉토리의 내용을 출력하는 명령어의 결과를 표준 redirection 을 통해 `result_stderr` 라는 이름의 파일로 출력하고 생성된 파일 내용 확인

   ```bash
   $ ls -l ./abc 1> result_stderr
   ls: cannot access './abc': No such file or directory
   $ cat result_stderr
   $
   ```

5. 위에서 표준 redirection 을 오류 redirection 으로 바꾸어 다시 수행

   ```bash
   $ ls -l ./abc 2> result_stderr
   $ cat result_stderr 
   ls: cannot access './abc': No such file or directory
   ```

6. 표준 redirection 과 오류 redirection 을 모두 적용시켜보기

   ```bash
   $ ls -l .abc 1> result_stdout 2> result_stderr
   $ cat result_stdout
   $ cat result_stderr
   ls: cannot access '.abc': No such file or directory
   ```

   ```bash
   $ ls -l member* 1> result_stdout 2> result_stderr
   $ cat result_stdout
   -rw-rw-rw- 1 tmdgns1139 tmdgns1139 74 Feb 15 05:27 member
   -rw-rw-rw- 2 tmdgns1139 tmdgns1139 65 Feb 15 05:21 member_hl
   lrwxrwxrwx 1 tmdgns1139 tmdgns1139  6 Feb 15 05:17 member_sl -> member
   $ cat result_stderr
   ```

7. 입력 redirection 을 통해 `member_hl` 파일의 내용 출력

   ```bash
   $ cat < member_hl
   1. hong gildong
   2. Lee Sunshin
   3. hl: hard link
   4. sl: soft link
   ```

8. 출력 redirection 을 통해 `member_hl` 파일의 내용을 `member_hl_input_redirection` 파일로 출력 후 확인

   ```bash
   $ cat < member_hl > member_hl_input_redirection
   $ cat member_hl_input_redirection
   1. hong gildong
   2. Lee Sunshin
   3. hl: hard link
   4. sl: soft link
   ```

### <u>6.3.</u> 기타 파일 관련 명령

##### <u>6.3.1.</u> 파일 내용 출력 관련 명령어

<u>(1)</u> 파일 내용 전체 출력 (`cat` 명령어)

* 파일의 내용 전체를 화면에 출력

* 내용이 많은 경우, 옵션을 통해 일부만 출력

* pipe 를 통해 화면 단위 출력이 가능한 명령어와 같이 사용

* 명령어 형식

  ```bash
  $ cat [option] text_file
  ```

  > options
  >
  > * `-b`: 파일의 각 (공백이 아닌)문장에 1부터 시작하는 번호를 붙임
  > * `-E`(EOL): 각 문장의 끝에 `$`기호를 추가하여 문장의 끝을 표시
  > * `-n`(number): 파일의 각 문장에 1부터 시작하는 번호를 붙임
  > * `-s`(strip): 하나 이상의 연속된 공백 문장이 존재할 경우, 하나의 공백 문장으로 표현

<u>(2)</u> 파일 내용 화면 단위 출력 (`more` 명령어)

* `cat` 과 달리 파일의 내용을 화면 단위로 출력할 수 있음

* 명령어 형식

  ```bash
  $ more [option] filename
  ```

  > options
  >
  > * `-n`: 처음부터 `n`번째 문장까지 출력
  > * `+n`: `n`번째 문장부터 출력

* `more` 명령어 수행 상태에서 사용 가능한 명령어

  |      명령어       | 설 명                                                        |
  | :---------------: | ------------------------------------------------------------ |
  |       SPACE       | 한 화면만큼 아래로 스크롤                                    |
  |       ENTER       | 한 라인씩 아래로 스크롤                                      |
  |        `d`        | 한 화면의 반만큼 아래로 스크롤                               |
  |        `b`        | 한 화면의 반만큼 위로 스크롤                                 |
  |        `v`        | vi 편집기로 전환 (편집기 종료 시, `more` 명령어 수행상태로 돌아옴) |
  |       `:f`        | 파일 이름과 현재 라인의 번호 출력                            |
  |        `=`        | 현재 라인의 번호만 출력                                      |
  | `! shell_command` | shell 로 이동하여 지정한 명령어 수행<br />(명령어 수행 이후, `more` 명령어로 수행상태로 돌아옴) |
  |        `q`        | `more` 명령어 종료                                           |

<u>(3)</u> 파일의 앞부분 출력 (`head` 명령어)

* 파일 앞부분의 일부 문장 출력 시, 유용함

* 기본적으로 `10`개의 문장을 출력

* 명령어 형식

  ```bash
  $ head [option] filename
  ```

  > options
  >
  > * `-c 단위`: 출력할 용량을 지정 (단위로 b[512Bytes], k[1024Bytes] 지정 가능, 기본 단위는 Bytes)
  > * `-n`: 처음문장을 포함하여 위에서 `n`개의 문장까지만 출력

<u>(4)</u> 파일 뒷부분 출력 (`tail` 명령어)

* 파일 뒷부분의 일부 문장 출력 시, 유용

* 기본적으로 `10`개의 문장을 출력

* 명령어 형식

  ```bash
  $ tail [option] filename
  ```

  > options
  >
  > * `-c 단위`: 출력할 용량을 지정 (단위로 b[512Bytes], k[1024Bytes] 지정 가능, 기본 단위는 Bytes)
  > * `-n`: 마지막문장을 포함하여 아래에서 `n`개의 문장까지만 출력

<u>(5)</u> 빈 파일 생성 및 기존 파일의 시간 정보 변경 (`touch` 명령어)

* 지정한 파일명을 갖지 않는 파일 생성 시, 크기가 `0`인 파일이 생성됨

* 파일에는 생성 시간, 수정 시간, 변경 시간, 접근 시간 등이 존재함

  * 생성 시간: 파일이 생성되어 저장장치에 저장된 시간 (파일이 막 생성된 시점에서 생성 시간은 수정 시간과 같음)

  * 수정(modify) 시간: 파일의 내용이 수정된 시간

    ```bash
    $ ls -l member
    -rw-rw-rw- 1 tmdgns1139 tmdgns1139 74 Feb 15 05:27 member
    ```

  * 변경(change) 시간: 파일의 내용이 아닌 소유/허가 권한 등의 속성이 수정된 시간

    ```bash
    $ ls -lc member
    -rw-rw-rw- 1 tmdgns1139 tmdgns1139 94 Feb 15 07:12 member
    ```

  * 접근(access) 시간: 파일을 참조한 시간

    ```bash
    $ ls -lu member
    -rw-rw-rw- 1 tmdgns1139 tmdgns1139 94 Feb 15 07:13 member
    ```

* 파일의 시간 정보 확인

  ```bash
  $ stat member
    File: member
    Size: 94              Blocks: 8          IO Block: 4096   regular file
  Device: 801h/2049d      Inode: 291575      Links: 1
  Access: (0666/-rw-rw-rw-)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
  Access: 2022-02-15 07:13:19.945525643 +0000
  Modify: 2022-02-15 07:12:31.554352615 +0000
  Change: 2022-02-15 07:12:31.554352615 +0000
   Birth: -
  ```

* 명령어 형식

  ```bash
  $ touch [option] filename
  ```

  > options
  >
  > * `-a`: 파일의 접근 시간만 현재시간으로 변경함
  > * `-m`: 파일의 수정 시간만 현재시간으로 변경함
  > * `-t`: 파일의 수정 시간을 직접 지정 (`[YY]YY-MM-DDmm[.ss]` 형식)
  > * 옵션 미지정시, 파일의 수정/변경/접근 시간을 모두 변경

<u>(Exercise)</u>

1. 출력 redirection 을 통해 아래와 같이 `user_list` 파일 생성

   ```bash
   $ cat > user_list
   a. Hong Gildong
   
   b. Lee Sunshin
   
   
   
   c. Kang Kamchan
   ```

2. 생성한 `user_list` 파일의 각 문장에 문장 번호 출력

   ```bash
   $ cat user_list -n
        1  a. Hong Gildong
        2
        3  b. Lee Sunshin
        4
        5
        6
        7  c. Kang Kamchan
   ```

   ```bash
   $ cat user_list -b
        1  a. Hong Gildong
        2  b. Lee Sunshin
        3  c. Kang Kamchan
   ```

3. 생성한 `user_list` 파일을 출력 (각 문장의 문장 번호 표시, 연속된 공백 문자는 하나로 출력)

   ```bash
   $ cat user_list -ns
        1  a. Hong Gildong
        2
        3  b. Lee Sunshin
        4
        5  c. Kang Kamchan
   ```

   ```bash
   $ cat user_list -bs
        1  a. Hong Gildong
        2  b. Lee Sunshin
        3  c. Kang Kamchan
   ```

4. `more` 명령어를 통해 `/etc/passwd` 파일의 내용을 화면에 출력

   ```bash
   $ more /etc/passwd
   ```

5. `/etc/passwd` 파일의 30번째 문장부터 마지막까지 화면에 출력

   ```bash
   $ cat -n /etc/passwd | more +30
       30  _chrony:x:111:115:Chrony daemon,,,:/var/lib/chrony:/usr/sbin/nologin
       31  ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
       32  aiteen-front-rsakey:x:1001:1002::/home/aiteen-front-rsakey:/bin/bash
       33  tmdgns1139-rsakey:x:1002:1003::/home/tmdgns1139-rsakey:/bin/bash
       34  tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
       35  kim:x:1004:1005:,,,:/home/kim:/bin/bash
   ```

6. `/etc/passwd` 파일의 1번째 문장부터 5번째 문장까지 화면에 출력

   ```bash
   $ cat -n /etc/passwd | more -5
        1  root:x:0:0:root:/root:/bin/bash
        2  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
        3  bin:x:2:2:bin:/bin:/usr/sbin/nologin
        4  sys:x:3:3:sys:/dev:/usr/sbin/nologin
        5  sync:x:4:65534:sync:/bin:/bin/sync
   --More--
   ```

7. `/etc/passwd` 파일의 처음 3문장 출력

   ```bash
   $ head -3 /etc/passwd
   root:x:0:0:root:/root:/bin/bash
   daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
   bin:x:2:2:bin:/bin:/usr/sbin/nologin
   ```

8. `/etc/passwd` 파일의 첫 50Bytes 의 내용 출력

   ```bash
   $ head -c 50 /etc/passwd
   root:x:0:0:root:/root:/bin/bash
   daemon:x:1:1:daemo
   ```

9. `/etc/passwd` 파일의 첫 1KBytes 의 내용 출력

   ```bash
   $ head -c 1k /etc/passwd
   root:x:0:0:root:/root:/bin/bash
   daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
   bin:x:2:2:bin:/bin:/usr/sbin/nologin
   sys:x:3:3:sys:/dev:/usr/sbin/nologin
   sync:x:4:65534:sync:/bin:/bin/sync
   games:x:5:60:games:/usr/games:/usr/sbin/nologin
   man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
   lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
   mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
   news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
   uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
   proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
   www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
   backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
   list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
   irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
   gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
   nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
   systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
   systemd-resolve:x:101:103:systemd Resolver,,,:/run/sys
   ```

10. `/etc/passwd` 파일의 마지막 3문장 출력

    ```bash
    $ tail -3 /etc/passwd
    tmdgns1139-rsakey:x:1002:1003::/home/tmdgns1139-rsakey:/bin/bash
    tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
    kim:x:1004:1005:,,,:/home/kim:/bin/bash
    ```

11. `/etc/passwd` 파일의 마지막 `1 block`(512Bytes) 의 내용 출력

    ```bash
    $ tail -c 1b /etc/passwd
    /sbin/nologin
    landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
    sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
    pollinate:x:110:1::/var/cache/pollinate:/bin/false
    _chrony:x:111:115:Chrony daemon,,,:/var/lib/chrony:/usr/sbin/nologin
    ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
    aiteen-front-rsakey:x:1001:1002::/home/aiteen-front-rsakey:/bin/bash
    tmdgns1139-rsakey:x:1002:1003::/home/tmdgns1139-rsakey:/bin/bash
    tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
    kim:x:1004:1005:,,,:/home/kim:/bin/bash
    ```

12. `myfile2` 파일을 생성하고 파일의 시간 정보를 출력

    ```bash
    $ touch myfile2
    $ stat myfile2
      File: myfile2
      Size: 0               Blocks: 0          IO Block: 4096   regular empty file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0666/-rw-rw-rw-)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:24:20.740854289 +0000
    Modify: 2022-02-15 07:24:20.740854289 +0000
    Change: 2022-02-15 07:24:20.740854289 +0000
     Birth: -
    ```

13. `myfile2` 파일의 접근 시간을 현재 시간으로 변경

    ```bash
    $ touch -a myfile2
    $ stat myfile2
    $ stat myfile2 
      File: myfile2
      Size: 0               Blocks: 0          IO Block: 4096   regular empty file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0666/-rw-rw-rw-)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:26:09.523987244 +0000
    Modify: 2022-02-15 07:24:20.740854289 +0000
    Change: 2022-02-15 07:26:09.523987244 +0000
     Birth: -
    ```

14. `myfile2` 파일의 수정 시간을 현재 시간으로 변경

    ```bash
    $ touch -m myfile2
    $ stat myfile2
    $ stat myfile2 
      File: myfile2
      Size: 0               Blocks: 0          IO Block: 4096   regular empty file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0666/-rw-rw-rw-)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:26:09.523987244 +0000
    Modify: 2022-02-15 07:27:09.571924625 +0000
    Change: 2022-02-15 07:27:09.571924625 +0000
     Birth: -
    ```

15. `cat` 명령어를 통해 `myfile2`의 내용을 출력하고 접근 시간 확인

    ```bash
    $ cat myfile2
    $ stat myfile2
      File: myfile2
      Size: 0               Blocks: 0          IO Block: 4096   regular empty file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0666/-rw-rw-rw-)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:28:38.065727198 +0000
    Modify: 2022-02-15 07:27:09.571924625 +0000
    Change: 2022-02-15 07:27:09.571924625 +0000
     Birth: -
    ```

16. redirection 을 통해 `myfile2` 파일의 내용에 문장을 추가하고 수정 시간 확인

    ```bash
    $ cat >> myfile2
    korea
    $ stat myfile2
    $ stat myfile2 
      File: myfile2
      Size: 6               Blocks: 8          IO Block: 4096   regular file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0666/-rw-rw-rw-)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:28:38.065727198 +0000
    Modify: 2022-02-15 07:30:31.785183802 +0000
    Change: 2022-02-15 07:30:31.785183802 +0000
     Birth: -
    ```

    > Modify implies Change

17. `myfile2` 파일의 허가 권한을 `755`로 변경 후, 수정 시간 확인

    ```bash
    $ chmod 755 myfile2
    $ stat myfile2
      File: myfile2
      Size: 6               Blocks: 8          IO Block: 4096   regular file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0755/-rwxr-xr-x)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:28:38.065727198 +0000
    Modify: 2022-02-15 07:30:31.785183802 +0000
    Change: 2022-02-15 07:31:37.017461075 +0000
     Birth: -
    ```

    > Change doesn't imply Modify

18. `touch myfile2` 명령어 수행 후, 시간 정보 확인

    ```bash
    $ touch myfile2
    $ stat myfile2
      File: myfile2
      Size: 6               Blocks: 8          IO Block: 4096   regular file
    Device: 801h/2049d      Inode: 296140      Links: 1
    Access: (0755/-rwxr-xr-x)  Uid: ( 1003/tmdgns1139)   Gid: ( 1004/tmdgns1139)
    Access: 2022-02-15 07:33:47.842039223 +0000
    Modify: 2022-02-15 07:33:47.842039223 +0000
    Change: 2022-02-15 07:33:47.842039223 +0000
     Birth: -
    ```

##### <u>6.3.2.</u> 파일 검색 관련 명령어

<u>(1)</u> 문자열 검색 (`grep` 명령어)

* 파일에 존재하는 내용 중 인자로 지정된 문자열을 포함한 문장들만 출력

* 파이프와 함께 사용되어 이전 명령어의 수행 결과 중, 인자로 지정된 문자열이 포함된 문장만 출력

* 파일의 전체 내용이나 명령어 수행 결과 중 보고 싶은 문장만 뽑아낼 때 유용

* 명령어 형식

  ```bash
  $ grep [option] search_string filename
  ```

  > options
  >
  > * `-A 숫자`: 검색 문자열 포함 지정한 숫자만큼 아래쪽 문장을 함께 출력
  > * `-B 숫자`: 검색 문자열 포함 지정한 숫자만큼 위쪽 문장을 함께 출력
  > * `-c`(count): 검색 문자열을 포함한 라인의 수를 출력
  > * `-i`: 검색 문자열의 대소문자를 구분하지 않음
  > * `-n`: 검색 결과에 라인 번호를 붙여 출력

<u>(2)</u> 파일 검색 (`find` 명령어)

* 시스템 내에서 원하는 파일/디렉토리를 쉽게 찾도록 도와주는 명령어

* 명령어 형식

  ```bash
  $ find 시작경로 [option] 검색대상 [action]
  ```

  > options
  >
  > * `-maxdepth 숫자`: 시작 경로 기준, 몇 단계의 하위 디렉토리까지 검색할지 지정
  > * `-name 파일명`: 검색 조건으로 파일명을 지정
  > * `-size 파일크기`: 검색 조건으로 파일크기를 지정 (인자에 `+` 포함시 이상, `-` 포함시 이하)
  > * `-user 사용자`: 검색 조건으로 파일의 소유자를 지정
  > * `-group 그룹`: 검색 조건으로 파일의 소유그룹을 지정
  > * `-type 파일형식`: 검색 조건으로 파일의 형식을 지정 (파일 형식)
  >
  > action
  >
  > * `-print`: 검색 결과를 표준 출력장치로 출력 (기본값)
  > * `-fprint 파일명`: 검색 결과를 지정한 파일로 저장 (이름이 같은 파일이 있다면 덮어쓰기)
  > * `-exec 명령어 {}\;`: 검색 결과에 대해 수행할 명령을 지정 (`{}` 위치에 검색 결과를 넣게됨)

* 파일 크기 단위: `c`[Byte], `w`[Word], `b`[Block], `k`[KBytes]

* 파일 형식: `b`[블록 장치 파일], `c`[문자 장치 파일], `d`[디렉토리], `f`[파일], `l`[심볼릭 링크], `s`[소켓 파일]

<u>(3)</u> 명령어 위치 검색 (`whereis`, `which` 명령어)

* `whereis`

  * 인자로 지정한 명령어의 위치를 절대경로로 반환

  * 실행파일, 소스, 메뉴얼 페이지 파일까지 검색하여 결과를 출력

  * 명령어 형식

    ```bash
    $ whereis 명령어
    ```

    ```bash
    $ whereis mv
    mv: /bin/mv /usr/share/man/man1/mv.1.gz
    ```

* `which`

  * `$PATH` 환경변수에 지정된 디렉토리를 검색하여 명령어의 위치를 찾아 절대경로로 반환

  * 명령어 형식

    ```bash
    $ which 명령어
    ```

    ```bash
    $ which mv
    /bin/mv
    ```

<u>(4)</u> 파일의 종류 출력 (`file` 명령어)

* 인자로 지정한 파일의 종류를 출력

* 시스템에서 생성된 파일 정보를 갖는 `/usr/share/file/magic.mgc` 파일을 검색하여 파일의 종류를 추출하여 사용자에게 반환

* 파일에 대해 읽기/실행 전, 해당 파일이 어떤 종류인지 알고 싶을 때 유용

* 명령어 형식

  ```bash
  $ file filename
  ```

<u>(5)</u> 문자/단어/문장의 수 출력 (`wc` 명령어)

* `wc`는 word count 의 약자로 파일 내에 존재하는 단어의 수를 출력

* 옵션을 통해 문자의 수나 문장의 수도 출력 가능

* 명령어 형식

  ```bash
  $ wc [option] filename
  ```

  > options
  >
  > * `-c`(character): 파일 내 문자의 수 출력
  > * `-w`(word): 파일 내 단어의 수 출력
  > * `-l`: 파일 내 문장의 수를 출력
  > * 옵션 미지정: 문장/단어/문자의 수 모두 출력

<u>(Exercise)</u>

1. `/etc/passwd` 파일에 `tmdgns` 이라는 단어를 포함한 문장과 그 아래 1문장을 함께 출력

   ```bash
   $ grep tmdgns /etc/passwd -A 1
   tmdgns1139-rsakey:x:1002:1003::/home/tmdgns1139-rsakey:/bin/bash
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
   kim:x:1004:1005:,,,:/home/kim:/bin/bash
   ```

2. `/etc/passwd` 파일에  `tmdgns` 이라는 단어를 포함한 문장과 그 위 2문장을 함께 출력

   ```bash
   $ grep tmdgns /etc/passwd -B 2
   ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
   aiteen-front-rsakey:x:1001:1002::/home/aiteen-front-rsakey:/bin/bash
   tmdgns1139-rsakey:x:1002:1003::/home/tmdgns1139-rsakey:/bin/bash
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
   ```

3. `/etc/passwd` 파일에  `tmdgns` 이라는 단어를 포함한 문장의 수를 출력

   ```bash
   $ grep tmdgns /etc/passwd -c
   2
   ```

4. `/etc/passwd` 파일에  `root` 이라는 단어를 포함한 문장에 문장 번호를 붙여 출력 (대소문자 구별 안함)

   ```bash
   $ grep root /etc/passwd -ni
   1:root:x:0:0:root:/root:/bin/bash
   ```

5. 현재 디렉토리에서 `my`라는 문자열을 포함한 문장만 자세히 출력

   ```bash
   $ ls -l | grep my
   -rwxr-xr-x 1 tmdgns1139 tmdgns1139   6 Feb 15 07:33 myfile2
   ```

6. `/dev` 디렉토리와 그 하위 디렉토리에서 파일명에 `sd`라는 문자열이 포함된 파일들을 출력

   ```bash
   $ find /dev -maxdepth 2 -name *sd*
   /dev/sda15
   /dev/sda14
   /dev/sda1
   /dev/sda
   ```

7. `/dev` 디렉토리와 그 하위 디렉토리에서 파일명에 파일 크기가 `3KB` 이상인 파일들을 출력

   ```bash
   $ find /dev -size +3k
   /dev
   /dev/char
   ```

8. 시스템 전체에서 소유자가 `kim` 인 파일들을 출력

   ```bash
   $ find / -user kim
   /home/kim
   /home/kim/.bashrc
   /home/kim/.profile
   /home/kim/.bash_history
   /home/kim/.bash_logout
   ```

9. 홈 디렉토리에서 하위의 모든 디렉토리를 검색하여 심볼릭 링크 파일들 출력

   ```bash
   $ find ~/ -type l
   /home/tmdgns1139/test/myfile2.sl
   /home/tmdgns1139/test/member_sl
   /home/tmdgns1139/frontend/.front/bin/python
   /home/tmdgns1139/frontend/.front/bin/python3
   /home/tmdgns1139/frontend/.front/lib64
   ```

10. 현재 디렉토리 및 하위 디렉토리에서 크기가 `500KB` 이상인 파일을 검색한 결과를 `result` 파일에 저장

    ```bash
    $ find ./ -size +500k 1> result
    ```

    ```bash
    $ find ./ -size +500k -fprint result
    ```

11. 현재 디렉토리 및 하위 디렉토리에서 `member` 단어가 포함된 파일을 제거

    ```bash
    $ find ./ -name *member* -exec rm {} \;
    ```

12. 아래와 같이 `test.jpg` 파일을 생성하고 종류를 조사

    ```bash
    $ cat > test.jpg
    this is a text file, although it has .jpg extension.
    $ file test.jpg 
    test.jpg: ASCII text
    ```

13. `/etc/passwd` 파일에서 문장의 수를 출력

    ```bash
    $ wc /etc/passwd -l
    35 /etc/passwd
    ```

14. `/etc/passwd` 파일에서 문장/단어/문자의 수를 출력

    ```bash
    $ wc /etc/passwd
    35   44 1855 /etc/passwd
    ```
