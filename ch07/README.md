## <u>Chapter 7.</u> 패키지와 소프트웨어 관리

* 우분투 SW 설치 방법 숙지
* 패키지 저장소를 사용한 패키지 설치 방법 숙지
* 압축 파일 관리 방법 숙지
* 사용자가 작성한 프로그램 컴파일 및 실행 방법 숙지

### <u>7.1.</u> 우분투의 패키지

##### <u>7.1.1.</u> 우분투 패키지 개요

<u>(1)</u> 우분투 패키지

* 리눅스 시스템에 다양한 SW 를 추가로 설치 가능함

* 압축 파일 형태로 제공된 소스 코드를 내려받아 소프트웨어 설치

  * 관리자가 설치 환경을 설정
  * 컴파일러를 사용해 직접 컴파일해야함
  * 현 시스템에 최적화된 SW 설치가 가능하지만 설치 과정이 복잡하고 어려움

* 패키지 형태로 배포된 SW 를 설치

  * 간단한 명령어를 사용해 네트워크를 통해 설치
  * 컴파일 과정 없이 쉽게 설치 가능하지만 패키지의 의존성 문제가 방생할 수 있음

* 리눅스 배포판에 따라 서로 다른 패키지 형식을 이용함

  * 레드햇 리눅스 기반: `RPM` 패키지 형식 (CentOS, Fedora 등)
  * Debian GNU/Linux: `deb` 패키지 형식 (Ubuntu 등)

* 우분투 패키지의 4가지 분류 (캐노니컬 사의 공식 지원 여부 및 라이센스 차이)

  |   카테고리   | 설 명                                    |
  | :----------: | ---------------------------------------- |
  |    `main`    | 우분투 정식 지원 및 완전한 자유 SW       |
  | `restricted` | 우분투 정식 지원 및 부분적 자유 SW       |
  |  `universe`  | 우분투 정식 미지원 및 완전한 자유 SW     |
  | `multiverse` | 우분투 정식 미지원 및 라이센스가 있는 SW |

* 우분투에서 사용되는 deb 패키지는 `.deb` 확장자를 갖고 아래와 같은 이름 구조를 가짐

  ```bash
  파일명_버전-리비전_시스템구조.deb
  ```

* 패키지 이름의 의미

  | 구성 요소  | 설 명                                                        |
  | :--------: | ------------------------------------------------------------ |
  |   파일명   | 패키지의 이름                                                |
  |    버전    | 패키지의 버전                                                |
  |   리비전   | 버전 내, 세부 버전                                           |
  | 시스템구조 | 설치할 수 있는 시스템 플랫폼 의미<br />(`i386`: 인텔 플랫폼, `all`: 모든 플랫폼) |

  > `i386` vs `amd64`
  >
  > * https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ohdoking&logNo=140195618269
  > * `i386` vs `x86_64` 차이: https://superuser.com/questions/74351/what-is-the-difference-between-x86-64-and-i386

<u>(2)</u> 패키지 저장소 (package repository)

* 우분투에서 패키지를 관리하는 방식

* 관리자는 패키지를 사용하여 SW 를 설치/제거 및 보안/기능 향상을 위한 SW 업데이트를 수시로 수행

  ![package-repo](https://user-images.githubusercontent.com/87659486/154210888-7e6aff88-2820-41cb-a8da-06ddc6b1233f.png)

* 패키지와 패키지의 정보를 저장하는 서버를 패키지 저장소(package repository) 라고 부름

* 우분투는 패키지 저장소를 두고 네트워크를 통해 패키지를 관리

* 패키지 저장소 사용으로 패키지의 업데이트 상황의 추적/관리를 쉽게하고 시스템을 최적의 상태로 유지 가능

* 우분투의 버전마다 다른 패키지 저장소를 제공할 수 있음

  * KDE 기반의 Kbuntu 와 Xface 기반의 xubuntu 사이의 SW 구성이 다를 수 있음
  * 데스크톱 환경이 다르면 패키지의 구성도 달라짐

* 패키지 저장소 설정을 통해 패키지 저장소를 사용할 수 있음

  * 패키지 저장소 설정은 `/etc/apt/sources.list` 파일을 통해 이루어짐

    ```bash
    $ cat /etc/apt/sources.list
    ...
    ## This software is not part of Ubuntu, but is offered by Canonical and the
    ## respective vendors as a service to Ubuntu users.
    # deb http://archive.canonical.com/ubuntu bionic partner
    # deb-src http://archive.canonical.com/ubuntu bionic partner
    deb http://security.ubuntu.com/ubuntu bionic-security main restricted
    # deb-src http://security.ubuntu.com/ubuntu bionic-security main restricted
    deb http://security.ubuntu.com/ubuntu bionic-security universe
    # deb-src http://security.ubuntu.com/ubuntu bionic-security universe
    deb http://security.ubuntu.com/ubuntu bionic-security multiverse
    # deb-src http://security.ubuntu.com/ubuntu bionic-security multiverse
    ```

    ```bash
    deb http://security.ubuntu.com/ubuntu bionic-security main restricted
    # deb-src http://security.ubuntu.com/ubuntu bionic-security main restricted
    ```

    > `deb`: 컴파일된 바이너리 패키지를 관리하는 패키지 저장소
    >
    > `deb-src`: 컴파일이 안된 소스 패키지를 관리하는 패키지 저장소
    >
    > `http://...`: 패키지 저장소의 URI
    >
    > `bionic`: 배포판의 이름 (`bionic`은 18.04 LTS 버전을 의미)
    >
    > `main restricted`: 패키지의 카테고리

  * 주석 제거 등을 통해, 다른 카테고리의 패키지를 사용할 수 있음

  * 우분투가 공식 지원하는 국가별 미러 사이트 리스트에서 URI 변경으로 다른 패키지 저장소를 지정 가능

    * 국가별 미러 사이트 리스트: https://launchpad.net/ubuntu/+archivemirrors
    * 카카오의 우분투 미러: https://launchpad.net/ubuntu/+mirror/mirror.kakao.com-archive
    * 미러 사이트의 상세 내용에서 `/etc/apt/sources.list` 파일에 새로운 URI 지정 방법이 있음

  * 패키지 저장소 변경 후, 아래 명령을 통해 업데이트를 하여 변경된 URI 에서 패키지 다운 가능

    ```bash
    $ sudo apt-get update
    ```

<u>(Exercise)</u>

1. 패키지 저장소 설정 파일에서 main, restricted 카테고리에 대한 패키지 저장소 URI 를 카카오 미러 사이트 URI 로 변경 후, 시스템에 반영

   ![kakao-corp-sources.list](https://user-images.githubusercontent.com/87659486/154209431-56799f51-7b46-4bb6-87bf-819d9780ff25.png)

   ```bash
   $ sudo vim /etc/apt/sources.list
   deb https://mirror.kakao.com/ubuntu/ bionic main restricted
   # deb-src https://mirror.kakao.com/ubuntu/ bionic main 
   $ sudo apt-get update
   Get:1 https://mirror.kakao.com/ubuntu bionic InRelease [242 kB]
   ...
   Get:5 https://mirror.kakao.com/ubuntu bionic/main amd64 Packages [1019 kB]
   Get:6 https://mirror.kakao.com/ubuntu bionic/main Translation-en [516 kB]
   ...
   Get:8 https://mirror.kakao.com/ubuntu bionic/restricted amd64 Packages [9184 B]
   ...
   Get:10 https://mirror.kakao.com/ubuntu bionic/restricted Translation-en [3584 B] ...
   Fetched 6242 kB in 2s (3738 kB/s)             
   Reading package lists... Done
   ```

##### <u>7.1.2.</u> 우분투 패키지 관리

* 1개의 SW 는 여러 패키지로 나뉘어 제공될 수 있음
* 이 경우, 패키지 설치 순서가 중요해지기도함
  (예를 들어, `A` 라는 패키지가 설치되어야 `AB` 라는 패키지를 설치할 수 있음 등등)
* 위와 같은 것을 패키지의 의존성이라고함
* 이러한 패키지 의존성은 설치 혹은 삭제 과정에서 발생할 수 있음
* 리눅스 패포판마다 패키지 의존성 해결 방법을 제공해줌
  * 레드햇 계열: `yum`, `dnf` 명령어는 패키지 설치/제거 순서 자동 인식을 통해 패키지 의존성 해결
  * 우분투: `APT`(Advanced Package Tool) 와 `aptitude` 명령어, `dpkg` 명령어를 통해 패키지 의존성 해결
* `APT`(Advanced Package Tool) 구성
  * `apt-cache` 명령어: 패키지의 정보 검색 및 출력
  * `apt-get` 명령어: 패키지의 설치/업데이트/제거 등의 작업 수행
  * `apt` 명령어: `apt-cache` 명령어와 `apt` 명령어를 결합한 명령어
* `aptitude` 명령어: `apt-get` 명령어와 거의 유사하나 비주얼 모드로 패키지를 관리
* `dpkg` 명령어: 패키지 관리 명령어로 레드햇 계열의 `RPM`(Redhat Package Manager) 과 비슷한 명령어

<u>(1)</u> `apt-cache` 명령어

* 우분투에서 패키지 정보 검색 및 출력

* 명령어 형식

  ```bash
  $ apt-cache 세부명령어
  ```

  > 세부명령어
  >
  > * `pkgnames`: 사용 가능한 패키지의 이름 출력
  > * `show 패키지명`: 지정한 패키지의 간단한 정보 출력
  > * `search 키워드`: 지정한 키워드를 포함하는 패키지를 검색
  > * `depends 패키지명`: 설치를 수행하지 않고 지정된 패키지의 의존성 정보만 출력
  > * `stats`: 현재 설치된 패키지들의 요약 정보 출력

<u>(2)</u> `apt-get` 명령어

* 패키지 설치/업데이트/삭제 기능 수행

* 패키지 관리에 있어 가장 많이 사용되는 명령어

* 명령어 형식

  ```bash
  $ apt-get 세부명령어
  ```

  > 세부명령어
  >
  > * `update`: 패키지 저장소의 정보를 업데이트
  > * `upgrade`: 현재 시스템에 설치된 모든 패키지들을 업그레이드
  > * `install 패키지명`: 지정된 패키지를 설치
  > * `--reinstall install 패키지명`: 이미 설치된 패키지를 다시 설치
  > * `remove 패키지명`: 지정한 패키지 제거 (설정 정보는 유지)
  > * `purge 패키지명`: 지정한 패키지 제거 (설정 정보는 제거)
  > * `autoremove`: 설치되어 있으나 사용되지 않는 패키지를 자동 제거
  > * `download 패키지명`: 지정한 패키지를 내려받기만 수행 (설치는 안함)
  > * `source 패키지명`: 지정한 패키지의 소스를 내려받기 혹은 설치 수행
  > * `clean`: 패키지 캐시 디렉토리에 존재하는 모든 패키지 파일 제거

* `update` 세부 명령어

  * `/etc/apt/sources.list` 파일에 지정된 내용을 업데이트하여 시스템에 반영
  * 해당 파일에서 패키지 저장소 URI 를 수정/추가 시, 반드시 `update` 세부 명령어로 변경 정보를 반영해야함

  ```bash
  $ sudo apt-get update
  ```

* `upgrade` 세부 명령어

  * 많은 패키지가 설치된 시스템에서 패키지 업데이트를 하나씩 하는 것은 말도 안되는 일임
  * `upgrade` 세부 명령어를 통해, 현재 시스템에 설치된 패키지들에 대한 업데이트 수행
  * 시스템 내에 설치된 패키지를 조사하여 업그레이드가 필요한 사항을 출력

  ```bash
  $ sudo apt-get upgrade
  ```

* `install` 세부 명령어

  * 인자로 지정한 패키지를 시스템에 설치
  * 지정한 패키지 설치 과정에 필요한 정보를 출력

  ```bash
  $ sudo apt-get install 패키지명1, 패키지명2, ... ,패키지명n
  ```

  ```bash
  $ sudo apt-get --reinstall install 패키지명
  ```

  > 이미 설치된 패키지를 다시 설치

* `remove`, `purge` 세부 명령어

  * 설치된 패키지 제거

  ```bash
  $ sudo apt-get remove 패키지명
  ```

  > 패키지의 설정 정보는 유지

  ```bash
  $ sudo apt-get --purge remove 패키지명
  $ sudo apt-get purge 패키지명
  ```

  > 패키지의 설정 정보까지 삭제

* `autoremove` 세부 명령어

  * 시스템에 불필요한 패키지를 인식하여 해당 패키지들을 삭제
    (의존성때문에 설치된 패키지가 시간이 흘러 사용되지 않는 등 여러 기준이 있음)

  ```bash
  $ sudo apt-get autoremove
  ```

* `download` 세부 명령어

  * 패키지(`.deb`)를 설치하지 않고 내려받기만 수행

  ```bash
  $ sudo apt-get download 패키지명
  ```

* `source` 세부 명령어

  * 지정한 패키지의 소스파일을 내려받음

  ```bash
  $ sudo apt-get --download-only source 패키지명
  ```

  > 지정한 패키지의 소스파일을 압축 파일 형태로 내려받기만 수행

  ```bash
  $ sudo apt-get source 패키지명
  ```

  > 지정한 패키지의 소스파일을 압축 파일 형태로 내려받고 압축 해제까지 수행

  ```bash
  $ sudo apt-get --compile source 패키지명
  ```

  > 지정한 패키지의 소스파일을 압축 파일 형태로 내려받고 압축 해제 및 컴파일까지 수행

* `clean` 세부 명령어

  * `apt-get` 명령어를 통해 검색/설치 시, 내려받은 패키지 파일은 `/var/cache/apt/archive` 에 저장됨
  * 패키지 캐시 디렉토리의 내용을 `clean` 명령어를 통해 제거할 수 있음

  ```bash
  $ sudo apt-get clean
  ```

<u>(3)</u> `apt` 명령어

* 사용자 편의성에 맞추어 `apt-cache` 명령어와 `apt-get` 명령어를 부분적으로 통합한 명령어

* 서버 관리 등의 목적으로 세밀한 패키지 관리가 목적이라면 `apt-get` 명령어를 사용

* 명령어 형식

  ```bash
  $ sudo apt 세부명령어
  ```

  > 세부명령어
  >
  > * `install 패키지명`: 지정한 패키지를 설치
  > * `remove 패키지명`: 지정한 패키지 제거 (설정 정보 유지)
  > * `purge 패키지명`: 지정한 패키지 제거 (설정 정보 제거)
  > * `search 키워드`: 키워드를 포함하는 설치 가능한 패키지를 검색
  > * `show 패키지명`: 패키지의 정보 출력
  > * `list`: 패키지 저장소로부터 설치 가능한 패키지 목록 출력
  > * `--installed list`: 시스템에 설치된 패키지들 출력

<u>(4)</u> `aptitude` 명령어

* 비주얼 모드로 실행되는 `apt` 명령어라고 생각하면 편함

* 터미널에서 실행되는 그래픽 라이브러리인 `curses`를 사용한 비주얼 모드 제공

* 사용자는 마우스를 사용해 메뉴를 선택할 수 있음

* 명령어 형식

  ```bash
  $ aptitude 세부명령어
  ```

  > 세부명령어
  >
  > * `update`: 패키지 저장소의 정보 업데이트
  > * `upgrade`: 현재 시스템에 설치된 패키지 업그레이드
  > * `install 패키지명`: 지정한 패키지 설치
  > * `download 패키지명`: 지정한 패키지(`.deb`) 다운로드
  > * `remove 패키지명`: 지정한 패키지 제거 (패키지 설정 정보 유지)
  > * `purge 패키지명`: 지정한 패키지 제거 (패키지 설정 정보 삭제)
  > * `clean`: 패키지 캐시 디렉토리의 패키지 파일 제거
  > * `show 패키지명`: 지정한 패키지 정보 출력
  > * `search 키워드`: 지정한 키워드를 포함한 패키지를 검색하여 출력

* 기본으로 제공되는 명령어가 아니므로 따로 설치해야함

  ```bash
  $ sudo apt-get install aptitude
  ```

<u>(5)</u> `dpkg` 명령어

* Debian Linux 에서 패키지를 관리하기 위한 명령어

* `apt-get` 과 유사하나 주로 시스템에 현재 설치된 패키지 관리 목적용 명령어임

* 명령어 형식

  ```bash
  $ dpkg option 패키지명/파일명
  ```

  > options
  >
  > * `-l`: 시스템에 설치된 패키지 목록 출력
  > * `-s 패키지명`: 설치된 패키지 중 인자로 지정한 패키지의 상세정보 출력
  > * `-S 파일/명령어(절대경로)`: 설치된 패키지 중 특정 파일/명령어가 포함된 패키지를 출력
  > * `-L 패키지명`: 시스템에 설치된 패키지로 인해 설치된 파일의 목록 출력
  > * `-c 파일명(.deb)`: 인자로 지정한 파일(`.deb`)이 가진 내용 출력
  > * `-i 파일명(.deb)`: 인자로 지정한 파일(`.deb`)을 설치
  > * `-r 패키지명`: 인자로 지정한 패키지 제거 (설정 정보 유지)
  > * `-P 패키지명`: 인자로 지정한 패키지 제거 (설정 정보 제거)

<u>(Exercise)</u>

1. 현재 시스템에서 사용 가능한 패키지 정보 출력

   ```bash
   $ apt-cache pkgnames
   ...
   zathura-cb
   libghc-ansi-wl-pprint-doc
   linux-modules-extra-4.18.0-1025-azure
   ```

2. `vsftpd` 패키지의 정보 출력

   ```bash
   $ apt-cache pkgnames | grep vsftpd
   vsftpd
   vsftpd-dbg
   $ apt-cache show vsftpd
   Package: vsftpd
   Architecture: amd64
   Version: 3.0.3-9build1
   Priority: extra
   Section: net
   Origin: Ubuntu
   ...
   ```

3. 키워드 `ttd` 가 포함된 패키지 검색

   ```bash
   $ apt-cache search ttd
   catcodec - tool to decode/encode the sample catalogue for OpenTTD
   grfcodec - suite of programs to modify Transport Tycoon Deluxe's GRF files
   nml - newgrf meta language compiler
   openttd - reimplementation of Transport Tycoon Deluxe with enhancements
   openttd-data - common data files for the OpenTTD game
   openttd-opengfx - free graphics set for use with the OpenTTD game
   openttd-openmsx - free music set for use with the OpenTTD game
   muttdown - Compiles annotated text mail into html using the Markdown standard
   openttd-opensfx - sound set for use with the OpenTTD game
   ```

   ```bash
   $ apt-cache pkgnaems | grep ttd
   openttd
   openttd-data
   openttd-opengfx
   openttd-openmsx
   openttd-opensfx
   muttdown
   ```

4. `vsftpd` 패키지의 의존성 정보 출력

   ```bash
   $ apt-cache depends vsftpd
   vsftpd
    |Depends: debconf
     Depends: <debconf-2.0>
       cdebconf
       debconf
     Depends: libc6
     ...
     Depends: netbase
     Conflicts: <ftp-server>
       ftpd
       ...
       twoftpd-run
     Recommends: logrotate
     Recommends: ssl-cert
     Replaces: <ftp-server>
       ftpd
       ...
       twoftpd-run
   ```

5. 현재 시스템에 설치된 모든 패키지 업그레이드 수행

   ```bash
   $ sudo apt-get upgrade
   Reading package lists... Done
   Building dependency tree       
   Reading state information... Done
   Calculating upgrade... Done
   The following packages have been kept back:
     snapd
   0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
   ```

6. `openttd` 패키지 설치

   ```bash
   $ sudo apt-get install openttd
   ```

7. 이미 설치된 `openttd` 패키지 재설치

   ```bash
   $ sudo apt-get --reinstall install openttd
   ```

8. `openttd` 패키지 제거 (패키지 설정 정보도 제거)

   ```bash
   $ sudo apt-get --purge remove openttd
   $ sudo apt-get purge openttd
   ```

9. `openttd` 패키지 내려받기만 수행

   ```bash
   $ sudo apt-get download openttd
   Get:1 http://asia-northeast3.gce.archive.ubuntu.com/ubuntu bionic/universe amd64 openttd amd64 1.7.1-1build1 [1950 kB]
   Fetched 1950 kB in 1s (3164 kB/s)
   W: Download is performed unsandboxed as root as file '/home/tmdgns1139/openttd_1.7.1-1build1_amd64.deb' couldn't be accessed by user '_ap
   t'. - pkgAcquire::Run (13: Permission denied)
   $ ls -l openttd*
   -rw-r--r-- 1 root root 1949768 Mar 22  2018 openttd_1.7.1-1build1_amd64.deb
   ```

10. 현 시스템에 설치된 패키지 목록 출력

    ```bash
    $ dpkg -l
    ...
    ii  zerofree                     1.0.4-1             amd64               zero free blocks from ext2, ext3 and ext4 file-systems
    ii  zlib1g:amd64                 1:1.2.11.dfsg-0ubun amd64               compression library - runtime
    ```

11. `adduser` 패키지에 대한 자세한 정보 출력

    ```bash
    $ dpkg -s adduser
    Package: adduser
    Status: install ok installed
    Priority: important
    Section: admin
    Installed-Size: 624
    Maintainer: Ubuntu Core Developers <ubuntu-devel-discuss@lists.ubuntu.com>
    Architecture: all
    Multi-Arch: foreign
    Version: 3.116ubuntu1
    Depends: passwd, debconf (>= 0.5) | debconf-2.0
    Suggests: liblocale-gettext-perl, perl, ecryptfs-utils (>= 67-1)
    Conffiles:
     /etc/deluser.conf 773fb95e98a27947de4a95abb3d3f2a2
    Description: add and remove users and groups
     This package includes the 'adduser' and 'deluser' commands for creating
     and removing users.
     ...
      Development mailing list:
        http://lists.alioth.debian.org/mailman/listinfo/adduser-devel/
    Homepage: http://alioth.debian.org/projects/adduser/
    Original-Maintainer: Debian Adduser Developers <adduser-devel@lists.alioth.debian.org>
    ```

12. `adduser` 명령어가 포함된 패키지 검색

    ```bash
    $ dpkg -S /usr/sbin/adduser
    adduser: /usr/sbin/adduser
    ```

13. `adduser` 패키지가 설치한 파일 목록 출력

    ```bash
    $ dpkg -L adduser
    /.
    /etc
    /etc/deluser.conf
    /usr
    /usr/sbin
    /usr/sbin/adduser
    /usr/sbin/deluser
    ...
    /usr/share/man/sv/man8/addgroup.8.gz
    /usr/share/man/sv/man8/delgroup.8.gz
    ```

14. 다운받았던 `openttd_1.7.1-1build1_amd64.deb` 패키지의 내용 출력

    ```bash
    $ dpkg -c openttd_1.7.1-1build1_amd64.deb
    drwxr-xr-x root/root         0 2018-03-22 21:41 ./
    drwxr-xr-x root/root         0 2018-03-22 21:41 ./usr/
    drwxr-xr-x root/root         0 2018-03-22 21:41 ./usr/games/
    -rwxr-xr-x root/root   5979992 2018-03-22 21:41 ./usr/games/openttd
    ..
    drwxr-xr-x root/root         0 2018-03-22 21:41 ./usr/share/lintian/overrides/
    -rw-r--r-- root/root       242 2017-12-05 12:15 ./usr/share/lintian/overrides/openttd
    ```

15. 다운받았던 `openttd_1.7.1-1build1_amd64.deb` 패키지 설치

    ```bash
    $ sudo dpkg -i openttd_1.7.1-1build1_amd64.deb
    ```

### <u>7.2.</u> 압축 파일 관리

##### <u>7.2.1.</u> 아카이브 파일 생성

<u>(1)</u> 리눅스에서 파일 압축 방법

* 리눅스에서 여러 파일로 구성된 소스는 하나의 파일로 압축하여 패키지로 제공됨
* 먼저, 여러 파일을 하나의 파일로 수집하여 묶는 과정을 거처 아카이브 파일을 생성함
  (아카이브: 저장소, 보관소의 의미이나 리눅스에서는 압축/전달을 위해 여러 파일을 하나의 파일로 묶는 것)
* 아카이브 파일은 여러 파일을 단순히 묶어놓으 것에 불과함
* 압축 파일은 아카이브 파일을 압축하여 만들어지는 파일임

<u>(2)</u> 파일 아카이브 생성 (`tar` 명령어)

* `tar`: <u>t</u>ape <u>ar</u>chive

* 여러 파일이나 디렉토리들을 하나의 파일(`.tar` 확장자)로 묶어 마그네틱 데이프 등의 장치에 저장하였음

* `tar` 명령어를 통해 여러 파일들을 하나의 아카이브 파일로 묶을 수 있음

* `tar` 명령어를 통해 아카이브 파일을 여러 파일로 풀 수 있음

* 또, 아카이브 파일에 새로운 파일을 포함시키거나 구성 파일의 내용 변경도 가능

* 명령어 형식

  ```bash
  $ tar option 아카이브파일이름.tar 묶을_파일/디렉토리
  ```

  > options
  >
  > * `-c`(create): 아카이브 파일 생성
  > * `-t`: 아카이브 파일의 내용물 화면에 출력 (아카이브 파일 해제 안함)
  > * `-x`: 아카이브 파일을 해제하여 묶였던 파일 추출
  > * `-v`: 아카이브 파일 생성/해제하는 과정 출력
  > * `-f`: 사용할 tar 아카이브 파일 이름을 지정
  > * `-r`: 기존 아카이브 파일 안에 새로운 파일 추가
  > * `-u`: 기존 아카이브 파일 안의 파일을 수정
  > * `-z`: 아카이브 파일 생성과 동시에 gzip으로 압축하거나 해제
  > * `-j`: 아카이브 파일 생성과 동시에 bzip2으로 압축하거나 해제

* 아카이브 파일 생성/출력/해제

  * `-cvf`: 아카이브 파일 생성
  * `-tvf`: 아카이브 파일의 내용 확인
  * `-xvf`: 아카이브 파일 해제

* 아카이브 파일 수정

  * `-rvf`: 아카이브 파일 안에 새로운 파일 추가
  * `-uvf`: 아카이브 파일 안에 파일을 수정

<u>(Exercise)</u>

1. `ls` 명령어와 redirection 을 통해 아래와 같이 파일 3개를 생성

   ```bash
   $ ls -l ./ > sample_101
   $ ls -l /bin > sample_102
   $ ls -l /etc > sample_103
   $ ls -F sample*
   sample_101  sample_102  sample_103
   ```

2. `sample` 로 시작하는 모든 파일을 묶어 `sample.tar` 아카이브 파일 생성

   ```bash
   $ tar -cvf sample.tar sample*
   sample_101
   sample_102
   sample_103
   $ ls -F sample.tar
   sample.tar
   ```

3. `sample.tar` 아카이브 파일 내용물 출력

   ```bash
   $ tar -tvf sample.tar
   -rw-rw-r-- tmdgns1139/tmdgns1139 292 2022-02-16 10:50 sample_101
   -rw-rw-r-- tmdgns1139/tmdgns1139 9292 2022-02-16 10:50 sample_102
   -rw-rw-r-- tmdgns1139/tmdgns1139 10823 2022-02-16 10:50 sample_103
   ```

4. 아래와 같이 `sample_104` 파일을 생성하여 `sample.tar` 파일에 추가한 후, 내용물 확인

   ```bash
   $ ls -l /home > sample_104
   $ tar -rvf sample.tar sample_104
   sample_104
   $ tar -tvf sample.tar
   -rw-rw-r-- tmdgns1139/tmdgns1139 292 2022-02-16 10:50 sample_101
   -rw-rw-r-- tmdgns1139/tmdgns1139 9292 2022-02-16 10:50 sample_102
   -rw-rw-r-- tmdgns1139/tmdgns1139 10823 2022-02-16 10:50 sample_103
   -rw-rw-r-- tmdgns1139/tmdgns1139   424 2022-02-16 10:55 sample_104
   ```

5. 아래와 같이 `sample_102` 파일을 수정 후, `sample.tar` 내부의 파일을 업데이트한 후 확인

   ```bash
   $ ls -l /dev >> sample_102
   $ tar -uvf sample.tar sample_102
   sample_102
   -rw-rw-r-- tmdgns1139/tmdgns1139 292 2022-02-16 10:50 sample_101
   -rw-rw-r-- tmdgns1139/tmdgns1139 9292 2022-02-16 10:50 sample_102
   -rw-rw-r-- tmdgns1139/tmdgns1139 10823 2022-02-16 10:50 sample_103
   -rw-rw-r-- tmdgns1139/tmdgns1139   424 2022-02-16 10:55 sample_104
   -rw-rw-r-- tmdgns1139/tmdgns1139 20131 2022-02-16 10:56 sample_102
   ```

6. `sample.tar` 아카이브 파일 해제 후, 결과 확인

   ```bash
   $ tar -xvf sample.tar
   sample_101
   sample_102
   sample_103
   sample_104
   sample_102
   $ ll sample_*
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   292 Feb 16 10:50 sample_101
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139 20131 Feb 16 10:56 sample_102
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139 10823 Feb 16 10:50 sample_103
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   424 Feb 16 10:55 sample_104
   ```

##### <u>7.2.2.</u> 파일의 압축과 해제

* 아카이브 파일 압출을 위해 `gzip` 혹은 `bzip2` 명령어를 이용함
  * `gzip` 을 통한 압축 결과, `.gz` 확장자를 가진 압축 파일이 생성됨 (`tar -czvf`)
  * `bzip2` 을 통한 압축 결과, `.bz2` 확장자를 가진 압축 파일이 생성됨 (`tar -cjvf`)

<u>(1)</u> 파일의 압축과 해제 ( `gzip`/`gunzip` 명령어)

<u>gzip</u>

* 'Lempel-Ziv' 인코딩 방법을 통한 아카이브 파일 압축 수행

* 텍스트 파일 기준 60~70% 정도의 압축률 

* `.gz` 확장자를 갖는 압축 파일 생성

* 압축 성공 시, 원본 파일은 자동 삭제됨

* 명령어 형식

  ```bash
  $ gzip [option] 압축할_파일명
  ```

  > options
  >
  > * `-[1~9]`: 압축률 지정 (`1`에 가까움: 낮은압축률/빠른압축속도 & `9`에 가까움: 높은압축률/느린압축속도)
  > * `-d`: 압축 해제 (성공 시, 압축 파일 삭제)
  > * `-v`: 압축 수행 과정 출력
  > * `-l`: 압축된 파일 정보 출력

<u>gunzip</u>

* `gzip` 을 통해 압축된 파일을 압축 해제하는 명령어 (`gzip -d` 와 동일)

* 압축 해제 성공 시, 압축 파일은 삭제됨

* 명령어 형식

  ```bash
  $ gunzip 압축된_파일명
  ```

<u>zcat</u>

* `.gz` 압축 파일 내부에 있는 파일들의 내용을 확인

* 명령어 형식

  ```bash
  $ zcat 압축된_파일명(.gz)
  ```

<u>(2)</u> 파일의 압축과 해제 ( `bzip2`/`bunzip2` 명령어)

<u>bzip2</u>

* `gzip` 이후 등장

* 블록 정렬 텍스트 압축 알고리즘 및 허프만 코딩을 사용하여 `gzip` 보다 압축륙이 높음

* `.bz2` 확장자를 갖는 압축 파일 생성

* 압축 성공 시, 원본 파일은 삭제됨

* 명령어 형식

  ```bash
  $ bzip2 [option] 압축할_파일명
  ```

  > options
  >
  > * `-d`: 압축을 해제함
  > * `-v`: 압축 과정 정보 출력
  > * `-l`: 압축된 파일의 정보 출력

<u>bunzip2</u>

* `bzip2` 명령어를 통해 압축된 파일을 해제하는 명령어 (`bzip2 -d` 명령어와 동일)

* 압축 해제 성공 시, 원본 파일 삭제

* 명령어 형식

  ```bash
  $ bunzip2 압축된_파일명(.bz2)
  ```

<u>bzcat</u>

* `.bz` 압축 파일 내부에 있는 파일들의 내용을 확인

* 명령어 형식

  ```bash
  $ bzcat 압축된_파일명(.bz2)
  ```

<u>(Exercise)</u>

1. `sample.tar` 아카이브 파일을 `sample.tar.gz` 로 압축 및 압축된 파일 정보 출력

   ```bash
   $ gzip -v sample.tar 
   sample.tar:      88.1% -- replaced with sample.tar.gz
   $ ll sample.tar.gz
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139 6118 Feb 16 10:57 sample.tar.gz
   ```

   ```bash
   $ gzip -l sample.tar.gz
            compressed        uncompressed  ratio uncompressed_name
                  6118               51200  88.1% sample.tar
   ```

2. `sample.tar.gz` 파일을 압축 해제

   ```bash
   $ gunzip sample.tar.gz
   $ ll sample.tar*
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139 51200 Feb 16 10:57 sample.tar
   ```

3. `sample.tar.gz` 파일 내부에 있는 파일들의 내용을 확인

   ```bash
   $ zcat sample.tar.gz | more
   ```

4. `tar` 명령어를 통해 파일 4개(`sample101` ~ `sample104`) 압축

   ```bash
   $ tar -czvf sample.tar.gz sample_*
   ```

5. `tar` 명령어를 통해 `sample.tar.gz` 파일 압축 해제

   ```bash
   $ tar -xzvf sample.tar.gz
   sample_101
   sample_102
   sample_103
   sample_104
   $ ll sample_*
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   292 Feb 16 10:50 sample_101
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139 20131 Feb 16 10:56 sample_102
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139 10823 Feb 16 10:50 sample_103
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   424 Feb 16 10:55 sample_104
   ```

6. `sample_` 로 시작하는 파일을 묶어 `sample.tar` 파일 생성 후, `bzip2` 으로 압축

   ```bash
   $ tar -cvf sample.tar sample_*
   sample_101
   sample_102
   sample_103
   sample_104
   $ bzip2 -v sample.tar
    sample.tar:  7.688:1,  1.041 bits/byte, 86.99% saved, 40960 in, 5328 out.
    $ ls sample.*
   sample.tar.bz2
   ```

7. `sample.tar.bz2` 파일을 압축 해제

   ```bash
   $ bunzip2 sample.tar.bz2
   $ ls sample.*
   sample.tar
   ```

8. `tar` 명령어를 통해 `sample_` 로 시작하는 파일들을 `sample.tar.bz2` 로 압축

   ```bash
   $ tar -cjvf sample.tar.bz2 sample_*
   $ ls sample.*
   sample.tar.bz2
   ```

9. `tar` 명령어를 통해 `sample.tar.bz2` 파일 압축 해제

   ```bash
   $ rm sample_*
   $ tar -xjvf sample.tar.bz2 
   sample_101
   sample_102
   sample_103
   sample_104
   $ ls sample_*
   sample_101  sample_102  sample_103  sample_104
   ```

10. `bzcat` 명령어를 통해 `sample.tar.bz2` 압축 파일 내부의 파일들의 내용 출력

    ```bash
    $ bzcat sample.tar.bz2 | more
    ```

### <u>7.3.</u> 소프트웨어 컴파일

* 일반적으로 패키지 형태로 제공되는 SW는 `apt-get`, `aptitude`, `dpkg` 명령어로 쉽게 설치 가능
* 그러나, 소스 코드를 컴파일하고 실행하는 경우도 있음
  * 프로그램 소스 코드를 직접 내려받아 컴파일/설치하는 경우
  * 프로그램을 직접 작성하여 컴파일/실행하는 경우
* 소스를 컴파일/실행하는 2가지 방법
  * 컴파일하는 파일 수가 적은 경우: 컴파일러를 사용해 직접 컴파일하고 실행 파일을 생성
  * 컴파일하는 파일 수가 많은 경우: `Makefile` 파일 작성 및 `make` 명령어를 사용하는 방법

##### <u>7.3.1.</u> 컴파일을 사용한 소스 코드 실행

* 먼저 컴파일러를 통해 소스코드를 목적코드로 변환한 후, 목적 코드를 실행파일을 생성
* 경우에 따라, 소스코드에서 바로 실행파일을 생성하기도함

<u>(1)</u> 컴파일러 설치

* 컴파일러 설치 여부 확인 (`gcc` 컴파일러 사용)

  ```bash
  $ aptitude show gcc
  Package: gcc                      
  Version: 4:7.4.0-1ubuntu2.3
  State: not installed
  Priority: optional
  Section: devel
  ...
  $ dpkg -L gcc
  dpkg-query: package 'gcc' is not installed
  Use dpkg --info (= dpkg-deb --info) to examine archive files,
  and dpkg --contents (= dpkg-deb --contents) to list their contents.
  ```

* `gcc` 컴파일러 설치

  ```bash
  $ apt-get install gcc
  $ gcc --version
  gcc (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
  Copyright (C) 2017 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
  ```

<u>(2)</u> 프로그램 작성 및 실행

* C 프로그램 작성

  * 프로그램의 구조

    ![image](https://user-images.githubusercontent.com/87659486/154291293-d76146fa-627a-405f-8ac9-d8a16c9c2354.png)

  * 소스파일은 각각 컴파일되어 목적파일로 변환됨

  * 변환된 목적파일들은 링크를 통해 `helloworld` 라는 실행파일을 생성함

  * 소스파일 3개를 편집기를 이용하여 작성

* 컴파일과 프로그램 실행

  * `gcc` 컴파일러를 통한 소스파일 컴파일 (소스코드에서 목적파일로)

    ```bash
    $ gcc -c -o helloworld.o helloworld.c
    $ gcc -c -o sub_first.o sub_first.c
    $ gcc -c -o sub_second.o sub_second.c
    $ ls *.o
    helloworld.o  sub_first.o  sub_second.o
    ```

    > `-c`: 소스코드를 목적파일로 변경
    >
    > `-o 파일이름`: 출력파일의 이름을 설정

  * `gcc` 컴파일러를 통한 목적파일 링킹 (목적파일에서 실행파일로)

    ```bash
    $ gcc -o helloworld helloworld.o sub_first.o sub_second.o
    $ ls helloworld*
    helloworld  helloworld.c  helloworld.o
    ```

  * 실행파일 실행

    ```bash
    $ ./helloworld
    ```

##### <u>7.3.2.</u> `make` 명령어를 사용한 소스 코드 실행

<u>(1)</u> `make` 명령어 개요

* 소스 파일(`.c` 파일)이 많으면 일일이 목적파일로 변경시키고 링킹하는 것은 매우 번거로움
* 여러 소스코드들을 컴파일하는 일련의 순서를 `Makefile`에 저장하여 빠르게 컴파일 진행
* `Makefile` 사용을 통해 소스 파일이 많고 코드 사이의 관계가 복잡한 패키지 컴파일이 쉬워짐
* `Makefile` 은 `make` 명령어에 의해 실행됨

<u>(2)</u> `Makefile` 생성

* `Makefile` 파일의 구조

  ```makefile
  CC=gcc
  
  target1: dependency1 dependency2
  	command1
  	command2
  
  target2: dependency3 dependency4
  	command3
  	command4
  
  clean:
  	command5
  ```

* 매크로 (`CC`)

  * 코드 단순화를 위해 사용됨
  * `CC`: 일반적으로 컴파일러 지정을 위해 사용됨
  * `TARGET`: 일반적으로 목표 파일을 지정하기 위해 사용됨
  * `${CC}`, `${TARGET}`: `Makefile` 에서 매크로 사용법

* 목표 파일 (`target1`, `target2`)

  * 생성시킬 파일의 이름
  * 생성시킬 파일은 목적 파일이나 실행 파일임

* 의존성 (`dependency1`, ..., `depandency4`)

  * 목표 파일 생성을 위해 필요한 파일들을 지정
    * 생성시킬 파일이 실행 파일이면, 목적 파일 리스트를 지정해줌
    * 생성시킬 파일이 목적 파일이면, 소스 코드 파일을 지정해줌

* 명령 (`command1`, ..., `command4`)

  * 목표 파일 생성을 위해 필요한 명령들을 지정
  * `command1`, `command2` 은 `target1` 생성을 위해 필요한 명령어들임

* 더미 타겟 (`clean`)

  * 이 부분에서는 목표 파일을 생성하지 않음
  * 컴파일 과정에서 생성된 목적 파일 및 실행 파일을 제거하는 것이 주 사용처

<u>(3)</u> `make` 명령어를 이용한 컴파일과 실행

* `make` 패키지 설치

  ```bash
  $ make
  Command 'make' not found, but can be installed with:
  apt install make      
  apt install make-guile
  Ask your administrator to install one of them.
  ```

  ```bash
  $ aptitude show make
  Package: make                     
  Version: 4.1-9.1ubuntu1
  State: not installed
  ...
  ```

  ```bash
  $ sudo apt-get install make
  ```

* `Makefile` 파일 작성

  ```makefile
  CC=gcc
  TARGET=helloworld
  
  ${TARGET}: sub_first.o sub_second.o helloworld.o
  	${CC} -o helloworld sub_first.o sub_second.o helloworld.o
  
  sub_first.o: sub_first.c
  	${CC} -c -o sub_first.o sub_first.c
   
  sub_second.o: sub_second.c
  	${CC} -c -o sub_second.o sub_second.c
  
  helloworld.o: helloworld.c
  	${CC} -c -o helloworld.o helloworld.c
  
  clean:
  	rm *.o helloworld
  ```

* 컴파일

  ```bash
  $ make
  gcc -c -o sub_first.o sub_first.c
  gcc -c -o sub_second.o sub_second.c
  gcc -c -o helloworld.o helloworld.c
  gcc -o helloworld sub_first.o sub_second.o helloworld.o
  ```

  ```bash
  $ ls *.o
  helloworld.o  sub_first.o  sub_second.o
  ```

  ```bash
  $ ./helloworld
  hello world. 
  sub first!
  sub second!
  ```

