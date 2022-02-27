## <u>Chapter 5.</u> 리눅스의 사용자와 그룹 관리

* 사용자와 그룹을 생성하는 방법
* 시스템 내부 사용자와 그룹의 정보를 수정 및 관리하는 shell 명령어 체득

### <u>5.1.</u> 사용자와 그룹의 생성

##### <u>5.1.1.</u> 사용자 생성

* 'root' 계정 전환 혹은 sudo 명령을 통해 사용자 계정을 생성할 수 있음
* 리눅스에서 `useradd`, `adduser` 명령어를 사용하여 사용자 계정을 생성함
* 그러나, 페도라에서는 두 명령어가 같지만, 우분투에서는 다름
* 이처럼 리눅스 배포판마다 명령어의 동작이 다를 수 있음

<u>(1)</u> `useradd` 명령어를 이용한 계정 생성

* 해당 명령어로 계정을 생성하면, 홈 디렉토리 지정이 안되고 로그인 shell 이 Bourne Shell 로 지정됨

* `useradd` 명령어로 계정을 생성하면, 홈 디렉토리와 로그인 shell 을 따로 지정해줘야함

* 명령어 형식

  ```bash
  $ sudo useradd [option] USER_ACCOUNT
  ```

  > options
  >
  > * `-c`(comment): 사용자에 대한 설명 입력
  > * `-u`(User ID): 사용자 UID 값을 지정
  > * `-d`(Directory): 인자값을 사용자 홈 디렉토리로 지정
  > * `-s`(Shell): 사용자 로그인 shell 지정
  > * `-m`: 사용자 홈 디렉토리 따로 지정하겠다고 선언
  > * `-e`(end): 사용자 계정의 만료일 지정 (지정 형식은 YYYY-MM-DD)
  > * `-g`(Group ID): 그룹의 GID 값을 지정
  > * `-G`(Group): 사용자가 속할 그룹들 지정
  > * `-D`: 사용자 계정 생성 시, 지정된 기본 설정 내용 출력

`/etc/default/useradd` 파일과 `-D` 옵션

* `/etc/default/useradd`: `useradd` 명령어로 생성된 계정들에 지정된 기본 설정 사항들을 모아둔 파일

  ```bash
  $ cat /etc/default/useradd
  # Default values for useradd(8)
  #
  # The SHELL variable specifies the default login shell on your
  # system.
  # Similar to DHSELL in adduser. However, we use "sh" here because
  # useradd is a low level utility and should be as general
  # as possible
  SHELL=/bin/sh
  #
  # The default group for users
  ...
  # CREATE_MAIL_SPOOL=yes
  ```

* `useradd -D`: `/etc/default/useradd` 파일의 주요 설정 내용 확인

  ```bash
  $ useradd -D
  GROUP=100            # 사용자 기본 그룹인 'users'의 GID
  HOME=/home           # 사용자의 홈 디렉토리의 생성 위치
  INACTIVE=-1          # 계정 활성화 지정 ('-1': INACTIVE 사용 않함을 의미)
  EXPIRE=              # 계정 만료일 지정 (비어있으면 만료일 지정 안함을 의미)
  SHELL=/bin/sh        # 사용자 로그인 shell
  SKEL=/etc/skel       # 홈 디렉토리에 생성할 기본 환경설정 파일 저장 위치
  CREATE_MAIL_SPOOL=no # 메일 디렉토리 생성 여부
  ```

* 우분투의 모든 사용자는 기본적으로 `users` 라는 그룹에 속함

* `INACTIVE`: 패스워드 최대 사용일 이후 계정을 사용할 수 있는 날의 수
  ( '-1'인 경우, 패스워드 사용일 이후 계정이 바로 닫힘 )

* `SKEL`: 홈 디렉토리 내에 생성할 환경설정 파일들이 존재하는 위치 지정

  ```bash
  $ ll /etc/skel
  total 20
  drwxr-xr-x  2 root root 4096 Dec 14 17:08 ./
  drwxr-xr-x 99 root root 4096 Feb 11 14:06 ../
  -rw-r--r--  1 root root  220 Apr  4  2018 .bash_logout
  -rw-r--r--  1 root root 3771 Apr  4  2018 .bashrc
  -rw-r--r--  1 root root  807 Apr  4  2018 .profile
  ```

  > 사용자 계정 생성 시, 위 파일들이 복사되어 사용자 홈 디렉토리에 복사됨

* `useradd -D` 명령어를 통한 기존 환경설정 내용 변경

  ```bash
  $ useradd -D [option] [details]
  ```

  > options
  >
  > * `-g`: 기본 그룹 변경
  > * `-d`: 사용자 홈 디렉토리 생성 위치 변경
  > * `-f`: INACTIVE 값 변경 (패스워드 만료 후 계정 사용가능일 변경)
  > * `-s`: 로그인 shell 변경

`passwd` 명령어를 통한 사용자 계정의 패스워드 지정

* 관리자는 사용자 계정 생성 직후, 패스워드를 부여해야함

* `passwd` 명령어 형식

  ```bash
  $ passwd [option] [USER_ACCOUNT]
  ```

  > options
  >
  > * `-l`(lock): 사용자 패스워드 일지 잠금 (일시적 로그인 막기)
  > * `-u`(unlock): 사용자 패스워드 잠금 해제
  > * `-d`(delete): 사용자 패스워드 삭제
  > * 명령어 단독 사용: 사용자 계정의 패스워드 변경

<u>(Exercise)</u>

1. 사용자 기본 로그인 shell 을 Bash Shell 로 변경하고 변경내용 확인

   ```bash
   $ sudo useradd -D -s /bin/bash
   $ sudo cat /etc/default/useradd | grep SHELL
   # The SHELL variable specifies the default login shell on your
   SHELL=/bin/bash
   ```

2. 사용자 기본 로그인 shell 을 Bourne Shell 로 변경하고 변경내용 확인

   ```bash
   $ sudo useradd -D -s /bin/sh
   $ sudo cat /etc/default/useradd | grep SHELL
   # The SHELL variable specifies the default login shell on your
   SHELL=/bin/sh
   ```

3. `kim1` 닉네임을 갖는 사용자 계정 생성

   ```bash
   $ sudo useradd kim1
   ```

4. 생성된 계정에 임시 비밀번호 부여

   ```bash
   $ sudo passwd kim1
   Enter new UNIX password: 
   Retype new UNIX password: 
   passwd: password updated successfully
   ```

5. `kim1` 계정으로 전환하여 사용자의 홈 디렉토리가 생성되었는지 확인하고 본 계정으로 복귀

   * `$ su kim2` 입력 후, 설정 확인
   * 로그인 shell: Bourne Shell
   * 홈 디렉토리: 생성 안됨

6. 아래 설정대로 지정된 계정 `kim2` 를 생성하고 임의의 비밀번호 지정

   * 로그인 shell 은 `Bash Shell`
   * 홈 디렉토리는 `/home/kim2` 
   * UID 는 `2000`

   ```bash
   $ sudo useradd kim2 -s /bin/bash -m -d /home/kim2 -u 2000
   $ sudo passwd kim2
   Enter new UNIX password: 
   Retype new UNIX password: 
   passwd: password updated successfully
   ```

7. `kim2` 로 전환하여 설정이 제대로 적용되었는지 확인

   ```bash
   tmdgns1139@aiteen-front:~$ su kim2
   Password: 
   kim2@aiteen-front:/home/tmdgns1139$ ll /home
   total 28
   drwxr-xr-x  7 root        root        4096 Feb 11 14:40 ./
   drwxr-xr-x 23 root        root        4096 Feb  8 06:31 ../
   drwxr-xr-x  2 kim2        kim2        4096 Feb 11 14:40 kim2/
   drwxr-xr-x  7 tmdgns1139  tmdgns1139  4096 Feb 11 08:57 tmdgns1139/
   kim2@aiteen-front:/home/tmdgns1139$ ps
     PID TTY          TIME CMD
    7078 pts/0    00:00:00 bash
    7088 pts/0    00:00:00 ps
   kim2@aiteen-front:/home/tmdgns1139$ cat /etc/passwd | grep kim2
   kim2:x:2000:2000::/home/kim2:/bin/bash
   ```

<u>(2)</u> adduser 명령어를 통한 계정 생성

* 우분투에서는 `useradd` 와 달리 `adduser` 명령어 단독 사용으로도 계정의 정보들 지정 가능
  (로그인 shell, 기본 그룹, 홈 디렉토리, 패스워드 초기화 등을 지정할 수 있고 이름 등 부가 정보 입력 가능)

* 그래서 우분투 공식적으로, `useradd` 보다는 `adduser` 명령어 사용을 권고함

* `adduser` 명령어의 기본 설정 정보는 `/etc/adduser.conf` 파일에 저장되어있음
  (옵션 없이 사용자 계정 생성 시, 해당 파일에 정의된 설정대로 계정이 생성됨)

  ```bash
  $ cat /etc/adduser.conf
  # /etc/adduser.conf: `adduser' configuration.
  # See adduser(8) and adduser.conf(5) for full documentation.
  # The DSHELL variable specifies the default login shell on your
  # system.
  DSHELL=/bin/bash
  # The DHOME variable specifies the directory containing users' home
  # directories.
  DHOME=/home
  # If GROUPHOMES is "yes", then the home directories will be created as
  # /home/groupname/user.
  GROUPHOMES=no
  # If LETTERHOMES is "yes", then the created home directories will have
  # an extra directory - the first letter of the user name. For example:
  # /home/u/user.
  LETTERHOMES=no
  # The SKEL variable specifies the directory containing "skeletal" user
  # files; in other words, files such as a sample .profile that will be
  # copied to the new user's home directory when it is created.
  SKEL=/etc/skel
  # FIRST_SYSTEM_[GU]ID to LAST_SYSTEM_[GU]ID inclusive is the range for UIDs
  # for dynamically allocated administrative and system accounts/groups.
  # Please note that system software, such as the users allocated by the base-passwd
  # package, may assume that UIDs less than 100 are unallocated.
  FIRST_SYSTEM_UID=100
  LAST_SYSTEM_UID=999
  FIRST_SYSTEM_GID=100
  LAST_SYSTEM_GID=999
  # FIRST_[GU]ID to LAST_[GU]ID inclusive is the range of UIDs of dynamically
  # allocated user accounts/groups.
  FIRST_UID=1000
  LAST_UID=59999
  FIRST_GID=1000
  LAST_GID=59999
  # The USERGROUPS variable can be either "yes" or "no".  If "yes" each
  # created user will be given their own group to use as a default.  If
  # "no", each created user will be placed in the group whose gid is
  # USERS_GID (see below).
  USERGROUPS=yes
  # If USERGROUPS is "no", then USERS_GID should be the GID of the group
  # `users' (or the equivalent group) on your system.
  USERS_GID=100
  # If DIR_MODE is set, directories will be created with the specified
  # mode. Otherwise the default mode 0755 will be used.
  DIR_MODE=0755
  # If SETGID_HOME is "yes" home directories for users with their own
  # group the setgid bit will be set. This was the default for
  # versions << 3.13 of adduser. Because it has some bad side effects we
  # no longer do this per default. If you want it nevertheless you can
  # still set it here.
  SETGID_HOME=no
  # If QUOTAUSER is set, a default quota will be set from that user with
  # `edquota -p QUOTAUSER newuser'
  QUOTAUSER=""
  # If SKEL_IGNORE_REGEX is set, adduser will ignore files matching this
  # regular expression when creating a new home directory
  SKEL_IGNORE_REGEX="dpkg-(old|new|dist|save)"
  ...
  ```

* `useradd` 명령어 형식

  ```bash
  $ useradd [option] USER_ACCOUNT
  ```

  > options
  >
  > * `--uid`: 사용자 UID 지정
  > * `--gid`: 사용자의 GID 지정
  > * `--home`: 사용자의 홈 디렉토리 설정
  > * `--shell`: 사용자의 기본 로그인 shell 지정
  > * `--gecos`: 사용자의 이름 등 부가적인 정보 저장

(Exercise)

1. `adduser` 를 통해 `lee1` 계정 생성

   ```bash
   $ sudo adduser lee1
   Adding user `lee1' ...
   Adding new group `lee1' (1005) ...
   Adding new user `lee1' (1004) with group `lee1' ...
   Creating home directory `/home/lee1' ...
   Copying files from `/etc/skel' ...
   Enter new UNIX password: 
   Retype new UNIX password: 
   passwd: password updated successfully
   Changing the user information for lee1
   Enter the new value, or press ENTER for the default
           Full Name []: lee one
           Room Number []: 1
           Work Phone []: 011
           Home Phone []: 032
           Other []: lee is lee one
   Is the information correct? [Y/n] Y
   ```

2. 생성된 계정으로 전환해 홈 디렉토리 생성 여부 확인

   ```bash
   $ su lee1
   password:
   lee1@aiteen-front:/home/tmdgns1139$ cd;pwd
   /home/lee1
   lee1@aiteen-front:~$ ll
   total 20
   drwxr-xr-x 2 lee1 lee1 4096 Feb 11 15:13 ./
   drwxr-xr-x 8 root root 4096 Feb 11 15:13 ../
   -rw-r--r-- 1 lee1 lee1  220 Feb 11 15:13 .bash_logout
   -rw-r--r-- 1 lee1 lee1 3771 Feb 11 15:13 .bashrc
   -rw-r--r-- 1 lee1 lee1  807 Feb 11 15:13 .profile
   lee1@aiteen-front:~$ cat /etc/passwd | grep lee1
   lee1:x:1004:1005:lee one,1,011,032,lee is lee one:/home/lee1:/bin/bash
   ```

3. `adduser` 를 통해 아래 조건을 만족하는 `lee2` 계정 생성

   * 홈 디렉토리: `/home/mylee2`
   * UID: `2005`

   ```bash
   $ sudo adduser lee2 --home /home/mylee2 --uid 2005
   Adding user `lee2' ...
   Adding new group `lee2' (2005) ...
   Adding new user `lee2' (2005) with group `lee2' ...
   Creating home directory `/home/mylee2' ...
   Copying files from `/etc/skel' ...
   Enter new UNIX password: 
   Retype new UNIX password: 
   passwd: password updated successfully
   Changing the user information for lee2
   Enter the new value, or press ENTER for the default
           Full Name []: lee two
           Room Number []: 2
           Work Phone []: 032-1234-2222
           Home Phone []: 032-hhhh-2222
           Other []: leeeeeee is leeeeee
   Is the information correct? [Y/n] Y
   ```

4. 생성된 계정으로 전환해 설정 확인

   ```bash
   $ su lee2
   password:
   lee2@aiteen-front:/home/tmdgns1139$ cd;pwd
   /home/mylee2
   lee2@aiteen-front:~$ cat /etc/passwd | grep lee2
   lee2:x:2005:2005:lee two,2,032-1234-2222,032-hhhh-2222,leeeeeee is leeeeee:/home/mylee2:/bin/bash
   ```

##### <u>5.1.2.</u> 그룹 생성

* 리눅스 시스템 내 각 사용자는 의무적으로 하나의 1차 그룹에 속해야함

* 사용자 계정 생성 시, 계정과 같은 이름의 그룹이 자동 생성되고 그 그룹에 속하게됨
  (이러한 그룹을 기본 그룹 혹은 1차 그룹이라고 부름)

* 사용자는 기본 그룹 외 하나 이상의 2차 그룹에 속할 수 있음

* 보통 사용자 특성에 따라 추가 그룹에 속하게하여 사용자 계정을 효율적으로 관리함

* 생성되는 그룹은 반드시 GID 를 가져야함 (관리자 지정 혹은 시스템 기본 설정에 따라 지정됨)
  (시스템 기본 설정은 `/etc/adduser.conf` 파일에서 확인)

  ```bash
  $ cat /etc/adduser.conf
  ...
  # FIRST_SYSTEM_[GU]ID to LAST_SYSTEM_[GU]ID inclusive is the range for UIDs
  # for dynamically allocated administrative and system accounts/groups.
  # Please note that system software, such as the users allocated by the base-passwd
  # package, may assume that UIDs less than 100 are unallocated.
  FIRST_SYSTEM_UID=100
  LAST_SYSTEM_UID=999
  FIRST_SYSTEM_GID=100
  LAST_SYSTEM_GID=999
  # FIRST_[GU]ID to LAST_[GU]ID inclusive is the range of UIDs of dynamically
  # allocated user accounts/groups.
  FIRST_UID=1000
  LAST_UID=59999
  FIRST_GID=1000
  LAST_GID=59999
  ..
  ```

* 시스템 내의 그룹에 대한 정보는 `/etc/group` 파일에 저장되어 있음

  ```bash
  $ cat /etc/group
  root:x:0:
  ...
  sudo:x:27:ubuntu
  ...
  users:x:100:
  nogroup:x:65534:
  ...
  ssh:x:111:
  ...
  admin:x:113:
  ...
  tmdgns1139:x:1004:
  lee1:x:1005:
  lee2:x:2005:
  ```

* `groupadd` 혹은 `addgroup` 명령어를 쓸 수 있음 (두 명령어의 차이는 GID 지정 옵션)

<u>(1)</u> `groupadd` 를 통한 그룹 생성

* 명령어 형식

  ```bash
  $ sudo groupadd [option] GROUP_NAME
  ```

  > option
  >
  > * `-g` (GID): 그룹 GID 지정

* 그룹 GID 미저정 시, `max{GIDs in system} + 1` 을 GID 로 지정

<u>(2)</u> `addgroup` 을 통한 그룹 생성

* 명령어 형식

  ```bash
  $ sudo addgroup [option] GROUP_NAME
  ```

  > option
  >
  > * `--gid`: 그룹 GID 지정

* 그룹 GID 미지정 시, 시스템이 자동 부여하는 GID 보다 1만큼 더 큰 GID 로 지정

<u>(Exercise)</u>

1. `/etc/group` 파일의 내용 출력

   ```bash
   $ tail /etc/group -n 1
   tmdgns1139:x:1004:
   ```

2. 옵션 없이 `groupadd` 명령을 통해 새로운 그룹 `student1` 생성

   ```bash
   $ sudo groupadd student1
   ```

3. `groupadd` 명령을 통해 GID 값이 `2022` 인 그룹 `student2` 생성

   ```bash
   $ sudo groupadd student2 -g 2022
   ```

4. 옵션 없이 `addgroup` 명령을 통해 새로운 그룹 `student3` 생성

   ```bash
   $ sudo addgroup student3
   Adding group `student3' (GID 1006) ...
   Done.
   ```

5. `addgroup` 명령을 통해 GID 값이 `2023` 인 그룹 `student4` 생성

   ```bash
   $ sudo addgroup student4 --gid 2023
   Adding group `student4' (GID 2023) ...
   Done.
   ```

6. `/etc/group` 파일의 내용 출력

   ```bash
   $ tail /etc/group -5
   tmdgns1139:x:1004:
   student1:x:1005:
   student2:x:2022:
   student3:x:1006:
   student4:x:2023:
   ```

### <u>5.2.</u> 사용자와 그룹 정보 관리

* 시스템 내의 많은 사용자 및 그룹을 관리할 방법이 필요
* 리눅스는 사용자 및 그룹 정보가 저장된 파일을 제공함

##### <u>5.2.1.</u> 사용자 계정 관련파일

<u>(1)</u> `/etc/passwd` 파일

* 시스템 내의 모든 사용자 계정 정보가 저장되는 파일

* UNIX 에서는 이 파일에 사용자 패스워드까지 저장하여 `passwd` 라는 이름을 갖던 것이 리눅스까지 이어짐

* 리눅스에서는 보안을 이유로 `/etc/passwd` 에는 사용자 계정 정보만 저장함

* 이 파일의 소유자인 `root` 만 수정 가능하고 일반 사용자는 읽기만 가능

* 그러나 관리자도 이 파일을 직접 수정하기보다 계정 정보를 변경하는 명령어를 사용함

* 파일 구조

  ```bash
  계정_이름:x(비식별화된 패스워드):UID:GID:추가정보:홈 디렉토리:로그인_Shell
  ```

* 계정 이름

  * 최대 32글자까지 가능하나 8글자 이하를 권고 (다른 운영체제와의 호환성)

* 패스워드

  * 실제 패스워드는 `/etc/passwd` 에 암호화되어 저장됨
  * `x` 의 의미는 패스워드가 존재한다는 것만 알려줌

* UID (User ID)

  * 리눅스 시스템이 사용자를 인식하기 위해 개별 사용자에게 부여되는 일련의 번호가 UID 임
    (사용자와 UID 는 1:1 로 매칭되어야함)
  * `0` 부터 `999` 까지는 시스템이 사용하는 계정에 부여되는 UID 임
  * `1000` 부터 `59999` 까지가 시스템으로부터 부여받을 수 있는 UID 임

* GID (Group ID)

  * 리눅스 시스템이 그룹을 인식하기 위해 개별 그룹에게 부여되는 일련의 번호가 GID 임
    (그룹과 GID 는 1:1 로 매칭되어야함)
  * `0` 부터 `999` 까지는 시스템이 사용하는 그룹에 부여되는 GID 임
  * `1000` 부터 `59999` 까지가 시스템으로부터 부여받을 수 있는 GID 임

* 추가정보

  * 사용자 이름, 전화번호 등으로 사용자 계정 생성 시점에 지정하거나 변경이 가능함
  * 지정하지 않으면 빈칸으로 출력됨

* 홈 디렉토리

  * 사용자 계정마다 지정된 홈 디렉토리의 절대경로
  * 기본값은 `/home/{USER_ACCOUNT}` 임
  * 사용자 계정 생성 시점에 다르게 지정이 가능하고 변경도 가능함

* 로그인 Shell

  * 사용자 계정이 시스템에 접속했을 때 사용하게 되는 기본 shell
  * 기본값은 Bash Shell
  * 사용자 계정 생성 시점에 다르게 지정이 가능하고 변경도 가능함

<u>(2)</u> `/etc/shadow` 파일

* 사용자의 패스워드 관련 정보를 저장

* 보안상 매우 중요한 파일로 관리자만 읽고 수정 가능

* 파일 구조

  ```bash
  사용자_계정이름:암호화_패스워드:최종변경일:MIN:MAX:WARNING:INACTIVE:EXPIRE:FLAG
  ```

  > * MIN: 패스워드 의무 사용 날짜
  > * MAX: 패스워드 최대 사용 날짜
  > * WARNING: 패스워드 최대 사용일 전, 경고를 출력하는 날짜
  > * INACTIVE: 변경된 패스워드의 최대 사용일 이후로도 사용 가능한 날짜
  > * EXPIRE: 패스워드 만료일
  > * FLAG: 부가정보

* 사용자 계정이름

  * `/etc/passwd` 파일에 있는 사용자 계정 이름을 의미함

* 패스워드

  * 암호화된 사용자 패스워드가 저장되며 복호화가 불가능함
  * `!` 는 패스워드 미지정을 의미

* 최종변경일

  * 패스워드가 마지막으로 변경된 날짜로 숫자로만 표현되어 있음
  * 1970년 1월 1일 기준, 패스워드를 마지막으로 변경한 날짜까지의 수를 의미
    (18036 은 패스워드 마지막 변경일은 1970년 1월 1일부터 18036번째 되는 날임을 의미)

* MIN

  * 패스워드 변경 후, 해당 패스워드를 사용해야하는 의무 기간
  * 기본값은 `0`으로 변경된 패스워드의 의무 사용 기간은 없음

* MAX

  * 패스워드를 사용할 수 있는 최대 기간
  * 보안상 일정 기간 이후, 패스워드를 변경하도록 유도함
  * `99999`: 패스워드 사용 기간을 지정하지 않음

* WARNING

  * 패스워드 변경 경고 보낼 날을 MAX 로 지정된 날로부터 몇일 전으로 할 것인지 지정
    (MAX=180, WARNING=10 인 경우, 패스워드 변경 후, 170일 이후에 경고를 보냄)

* INACTIVE

  * MAX 에서 지정한 날짜 이후, 해당 패스워드를 사용할 수 있는 일 수
    (MAX=180, INACTIVE=10 인 경우, 패스워드 변경 후, 190일 이후 패스워드가 유효함)

* EXPIRE

  * 패스워드 만기일 지정으로 계정의 접속 차단에 있어 강력한 조치임
  * MIN, MAX, WARNING, INACTIVE 와 달리, 패스워드 변경 유도가 없음
  * 최종 변경일과 마찬가지의 표현법을 통해 패스워드 만기일을 지정함

* FLAG

  * 입력된 값은 없고 향후 특정 용도를 위해 사용됨

##### <u>5.2.2.</u> 그룹 관련 파일

<u>(1)</u> `/etc/group` 파일

* 시스템에 생성된 그룹에 대한 정보 저장

* 사용자 계정의 기본 그룹은 `/etc/passwd` 파일에, 추가 그룹의 `/etc/group` 파일에 저장됨

* 파일 구조

  ```bash
  그룹이름:x(암호화된 그룹 패스워드):GID:그룹멤버
  ```

* 그룹 패스워드

  * 그룹에 지정된 패스워드를 `x` 로 표시
  * 실제 패스워드는 `/etc/gshadow` 에 암호화되어 저장됨
  * 그룹 패스워드는 `newgrp` 명령어를 통해 현재 세션의 사용자 그룹을 변경할 때 입력되어야함

* 그룹 멤버

  * 해당 그룹에 속한 사용자 계정들을 의미하고 `, ` 로 구분
  * 추가 그룹으로 해당 그룹에 속한 사용자 계정들이 저장됨

  ```bash
  $ cat /etc/group
  root:x:0:
  ...
  adm:x:4:syslog,ubuntu,aiteen-front-rsakey,tmdgns1139-rsakey,tmdgns1139
  ...
  dialout:x:20:ubuntu,aiteen-front-rsakey,tmdgns1139-rsakey,tmdgns1139
  ...
  video:x:44:ubuntu,aiteen-front-rsakey,tmdgns1139-rsakey,tmdgns1139
  ...
  users:x:100:
  nogroup:x:65534:
  ...
  ssh:x:111:
  ...
  netdev:x:114:ubuntu,aiteen-front-rsakey,tmdgns1139-rsakey,tmdgns1139
  ...
  tmdgns1139:x:1004:
  ```

<u>(2)</u> `/etc/gshadow` 파일

* 그룹의 패스워드를 저장하는 파일로 UNIX 에서는 없던 파일임

* 파일 구조

  ```bash
  그룹_이름:그룹_패스워드:관리자:그룹멤버
  ```

* 그룹 이름

  * 현재 시스템에 생성된 그룹 이름

* 그룹 패스워드

  * 암호화 되어 표현된 그룹 패스워드
  * `!` 는 패스워드 미지정을 의미

* 관리자

  * 그룹 관리 권한을 가진 사용자 계정을 의미하고 `,` 로 구분
    (그룹 관리: 그룹의 패스워드 번경, 그룹 멤버 변경 등)

### <u>5.3.</u> 사용자와 그룹의 정보 출력/수정/삭제

##### <u>5.3.1.</u> 사용자와 그룹의 정보 출력

<u>(1)</u> UID, GID 정보 출력 (`id` 명령어)

* 현재 시스템에 로그인한 사용자 본인의 UID 와 GID 정보를 출력

* 특정 사용자의 UID, GID 를 출력하려면 인자로 해당 사용자의 계정을 넘겨줘야함

* 명령어 형식

  ```bash
  $ id [option] [사용자 계정]
  ```

  > options
  >
  > * `-g`: 기본 그룹 GID 출력
  > * `-G`: 기본 그룹 GID, 추가 그룹의 GID 출력

<u>(2)</u> 사용자의 그룹 정보 출력 (`groups` 명령어)

* 현재 사용자 본인이 속해 있는 그룹의 이름 출력

* 특정 계정을 인자로 넣어 해당 계정이 속한 그룹의 이름 출력

* 명령어 형식

  ```bash
  $ groups [사용자_계정]
  ```

<u>(3)</u> 사용하는 계정의 이름 출력 (`whoami` 명령어)

* 현재 사용 중인 계정의 이름 출력

* 명령어 형식

  ```bash
  $ whoami
  ```

<u>(4)</u> 로그인된 사용자들의 계정 정보 출력 (`who` 명령어)

* 현재 시스템에 로그인해 있는 모든 사용자들의 계정, 접속 회선, 접속 시간, 접속한 위치 등의 정보 출력

* 명령어 형식

  ```bash
  $ who [option]
  ```

  > options
  >
  > * `-q`: 로그인한 계정의 이름과 계정의 수를 출력
  > * `-H`: 출력 정보의 헤더를 표시
  > * `-b`: 마지막으로 재시작한 날짜 정보 출력
  > * `-m`: 사용자의 로그인 계정 정보 출력 (EUID 출력)

  ```bash
  $ who -H
  NAME     LINE         TIME             COMMENT
  tmdgns1139 pts/0        2022-02-13 12:08 (35.235.243.2)
  ```

  > * NAME: 현재 접속한 계정의 이름
  > * LINE: 계정이 접속한 회선 (`:0` 은 콘솔 로그인 의미)
  > * TIME: 시스템에 로그인한 날짜와 시간
  > * COMMENT: 원격 접속을 한 경우, 원격지의 IP 주소 출력 (`:0` 은 콘솔 로그인 의미)

* 사용자 계정은 UID 혹은 EUID(Effective UID) 로 표현됨

  * UID: 계정 생성시 시스템 혹은 관리자로부터 부여받은 고유번호
  * EUID: 현재 명령어를 수행하고 있는 계정의 UID
  * UID 와 EUID 가 다른 값을 가지는 경우
    * `tmdgns1139` 계정으로 로그인하고 `su` 명령어를 통해 `kim1` 계정으로 전환한 경우
    * 이 경우, UID 는 `tmdgns1139` 의 UID 가 되고, EUID 는 `kim1` 의 UID 가 됨
    * 계정 전환 이후 명령어 수행 계정은 `kim1` 이기 때문에 EUID 는 `kim1` 의 UID 임

<u>(5)</u> 로그인된 사용자들의 계정 이름 출력 (`users` 명령어)

* 현재 시스템에 로그인된 사용자 계정들을 출력

* 명령어 형식

  ```bash
  $ users
  tmdgns1139
  ```

<u>(6)</u> 로그인된 사용자들의 작업 내용 출력 (`w` 명령어)

* `who` 명령어와 유사하지만, 사용자들의 현재 작업 내용까지 제공

* 명령어 형식

  ```bash
  $ w [option]
  ```

  > options
  >
  > * `-f`: 원격지 호스트의 이름은 출력하지 않음
  > * `-h`: 출력 정보의 헤더 표시 생략
  > * `-s`: CPU 정보를 제외한 나머지 정보를 출력

<u>(Exercise)</u>

1. 현재 로그인한 사용자 계정 본인의 UID, GID 를 출력

   ```bash
   $ id
   uid=1003(tmdgns1139) gid=1004(tmdgns1139) groups=1004(tmdgns1139),4(adm),20(dialout),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plug
   dev),108(lxd),114(netdev),1000(ubuntu),1001(google-sudoers)
   ```

2. 특정 사용자 계정이 속해있는 모든 그룹의 GID 를 출력

   ```bash
   $ id tmdgns1139 -G
   1004 4 20 24 25 29 30 44 46 108 114 1000 1001
   ```

3. 현재 로그인한 사용자 본인이 속한 그룹 이름을 출력

   ```bash
   $ groups
   tmdgns1139 adm dialout cdrom floppy audio dip video plugdev lxd netdev ubuntu google-sudoers
   ```

4. `kim1` 계정이 속한 그룹 이름을 출력

   ```bash
   $ groups kim1
   kim1 : kim1
   ```

5. 현재 사용 중인 계정의 이름을 출력

   ```bash
   $ whoami
   tmdgns1139
   ```

6. `kim1` 사용자 계정으로 이동 후, 해당 계정의 이름을 출력

   ```bash
   $ su kim1
   Password:
   kim1:$ whoami
   kim1
   ```

7. 현재 시스템에 로그인한 계정의 이름과 계정의 수를 출력

   ```bash
   $ who -q
   tmdgns1139
   # users=1
   ```

8. 현재 시스템에 로그인된 사용자의 게정 정보를 헤더와 함께 출력

   ```bash
   $ who -H
   NAME     LINE         TIME             COMMENT
   tmdgns1139 pts/0        2022-02-13 12:08 (35.235.243.2)
   ```

9. 현재 시스템에 로그인된 사용자 계정 출력

   ```bash
   $ users
   tmdgns1139
   ```

   ```bash
   $ who
   tmdgns1139 pts/0        2022-02-13 12:08 (35.235.243.2)
   $ who -q
   tmdgns1139
   # users=1
   ```

10. 현재 시스템에 로그인한 사용자의 CPU 사용 정보와 현재 수행 중인 작업 내용 출력

    ```bash
    $ w
     13:26:38 up 53 days,  8:35,  1 user,  load average: 0.00, 0.00, 0.00
    USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
    tmdgns11 pts/0    35.235.243.2     12:08    0.00s  0.13s  0.01s sshd: tmdgns1139 [priv]
    ```

11. 현재 시스템에 로그인한 사용자의 CPU 사용 정보 외, 현재 수행 중인 작업 내용 출력

    ```bash
    $ w -s
     13:27:47 up 53 days,  8:36,  1 user,  load average: 0.00, 0.00, 0.00
    USER     TTY      FROM              IDLE WHAT
    tmdgns11 pts/0    35.235.243.2      0.00s sshd: tmdgns1139 [priv]
    ```

    > * USER: 사용자 계정 이름
    > * TTY: 회선 종류
    > * FROM: 접속한 호스트 IP
    > * LOGIN@: 로그인한 시간
    > * IDLE: 계정이 시스템에 머무른 시간 ??
    > * JCPU: 접속 후, 수행한 모든 명령의 수행시간
    > * PCPU: 현재 실행 중인 명령의 수행시간
    > * WHAT: 사용자가 수행한 작업 메타 정보

##### <u>5.3.2.</u> 사용자 정보 수정

<u>(1)</u> 사용자 계정 정보 수정 (`usermod`)

* 사용자 계정의 홈 디렉토리, 그룹, 패스 만료일 등 사용자 정보 변경

* 명령어 형식

  ```bash
  $ usermod option 사용자_계정
  ```

  > options
  >
  > * `-c`(comment): 사용자 추가 정보 지정 혹은 수정
  > * `-d`: 홈 디렉토리 변경 후, 기존 홈 디렉토리 삭제(`-m` 옵션과 같이 사용)
  > * `-m`: 이전 홈 디렉토리의 파일을 변경된 홈 디렉토리로 복사 (`-d` 옵션과 같이 사용)
  > * `-g`(group): 사용자의 기본 그룹 지정 혹은 수정
  > * `-G`(Group): 사용자의 추가 그룹 지정 혹은 수정
  > * `-s`(shell): 사용자의 로그인 shell 변경
  > * `-l`: 사용자의 계정 변경
  > * `-p`(password): 사용자의 패스워드 변경
  > * `-u`(uid): 사용자의 UID 변경
  > * `-f`: 계정 비활성화(INACTIVE) 날수 지정
  > * `-e`(expire): 계정 만기일(EXPIRE)을 YYYY-MM-DD 형식으로 지정

<u>(2)</u> 패스워드 에이징 지정 및 변경 (`chage`)

* 패스워드 에이징: 패스워드 변경 유도를 목적으로 패스워드의 사용 만료일 관련 정보를 지정하는 것

* MIN, MAX, WARNING, INACTIVE, EXPIRE 등을 지정하여 일정 기간에만 로그인이 가능하게함

* 명령어 형식

  ```bash
  $ chage option 사용자_계정
  ```

  > options
  >
  > * `-m`: MIN 값 지정 (변경한 패스워드 최소 사용일)
  > * `-M`: MAX 값 지정 (변경한 패스워드 최대 사용일)
  > * `-W`: WARNING 지정 (MAX 일 몇일 전에 패스워드 변경 메세지를 보낼지 지정)
  > * `-I`: INACTIVE 지정 (MAX 이후, 몇일 동안 패스워드로 로그인이 가능한지 지정)
  > * `-E`: EXPIRE 지정 (계정 만료일)
  > * `-l`(log): 지정한 사용자에 대한 패스워드 에이징 정보 출력

<u>(Exercise)</u>

1. `kim1` 사용자 계정에 'kim minho' 라는 추가정보 지정 후, 변경 사항 확인

   ```bash
   $ grep kim1 /etc/passwd
   kim1:x:1004:1008:,,,:/home/kim1:/bin/bash
   $ sudo usermod kim1 -c 'kim minho'
   $ grep kim1 /etc/passwd
   kim1:x:1004:1008:kim minho:/home/kim1:/bin/bash
   ```

2. `kim1` 사용자 계정의 홈 디렉토리를 'kim1home' 으로 변경 후, 변경 사항 확인

   ```bash
   $ ll /home
   total 28
   ...
   drwxr-xr-x  2 kim1  kim1  4096 Feb 12 12:38 kim1/
   ...
   $ sudo usermod kim1 -m -d /home/kim1home
   $ su kim1
   Password
   kim1: $ cd;pwd
   /home/kim1home
   ```

3. `kim1` 사용자의 로그인 shell 을 Bourne Shell 로 변경

   ```bash
   $ grep kim1 /etc/passwd
   kim1:x:1004:1008:kim minho:/home/kim1home:/bin/bash
   $ sudo usermod kim1 -s /bin/sh
   $ grep kim1 /etc/passwd
   kim1:x:1004:1008:kim minho:/home/kim1home:/bin/sh
   ```

4. `kim1` 사용자 계정을 `mykim1` 로 변경

   ```bash
   $ grep kim1 /etc/passwd
   kim1:x:1004:1008:kim minho:/home/kim1home:/bin/sh
   $ sudo usermod kim1 -l mykim1
   $ grep mykim1 /etc/passwd
   mykim1:x:1004:1008:kim minho:/home/kim1home:/bin/sh
   ```

5. `mykim1` 사용자 UID 를 `2500`으로 변경

   ```bash
   $ grep mykim1 /etc/passwd
   mykim1:x:1004:1008:kim minho:/home/kim1home:/bin/sh
   $ sudo usermod mykim1 -u 2500
   $ grep mykim1 /etc/passwd
   mykim1:x:2500:1008:kim minho:/home/kim1home:/bin/sh
   ```

6. `mykim1` 의 기본 그룹을 `student1` 로 변경

   ```bash
   $ grep student1 /etc/group
   student1:x:1005:
   $ grep mykim1 /etc/passwd
   mykim1:x:2500:1008:kim minho:/home/kim1home:/bin/sh
   $ grep 1008 /etc/group
   kim1:x:1008:
   $ sudo usermod mykim1 -g student1
   $ grep mykim1 /etc/passwd
   mykim1:x:2500:1005:kim minho:/home/kim1home:/bin/sh
   ```

7. `mykim1` 의 추가 그룹으로 `student2`,`student3` 을 지정

   ```bash
   $ grep mykim1 /etc/group
   $ sudo usermod mykim1 -G student2,student3
   $ grep mykim1 /etc/group
   student2:x:2022:mykim1
   student3:x:1006:mykim1
   ```

8. `mykim1` 의 추가 그룹인 `student3` 에 속하지 않도록 수정 

   ```bash
   $ grep mykim1 /etc/group
   student2:x:2022:mykim1
   student3:x:1006:mykim1
   $ sudo usermod mykim1 -G student2
   $ grep mykim1 /etc/group
   student2:x:2022:mykim1
   ```

9. `mykim1` 계정의 MIN 값은 `0`으로, MAX 값은 `180`으로, WARNING 값은 `5`로 수정

   ```bash
   $ sudo grep mykim1 /etc/shadow
   mykim1:xxxxxx:19035:0:99999:7:::
   $ chage mykim1 -m 0 -M 180
   $ sudo grep mykim1 /etc/shadow
   mykim1:xxxxxx:19035:0:180:5:::
   ```

10. `mykim1` 계정의 INACTIVE 값을 `7`로 지정하고 패스워드 만기일을 `2022년 09월 30일`로 지정

    ```bash
    $ sudo grep mykim1 /etc/shadow
    mykim1:xxxxx:19035:0:180:5:::
    $ sudo chage mykim1 -I 7 -E 2022-09-30
    $ sudo grep mykim1 /etc/shadow
    mykim1:xxxxxx:19035:0:180:5:7:19265:
    ```

    ```bash
    $ sudo grep mykim1 /etc/shadow
    mykim1:xxxxxx:19035:0:180:5:7:19265:
    $ sudo mykim1 usermod -f 10 -e 2025-09-30
    $ sudo chage mykim1 -l
    Last password change                                    : Feb 12, 2022
    Password expires                                        : Aug 11, 2022
    Password inactive                                       : Aug 21, 2022
    Account expires                                         : Sep 30, 2025
    Minimum number of days between password change          : 0
    Maximum number of days between password change          : 180
    Number of days of warning before password expires       : 5
    ```

##### <u>5.3.3.</u> 그룹 정보 수정

<u>(1)</u> 그룹 계정 정보 수정 (`groupmod` 명령어)

* 그룹 이름, GID 등 그룹의 정보 변경

* 명령어 형식

  ```bash
  $ groupmod option 그룹_이름
  ```

  > options
  >
  > * `-n`(name): 그룹 이름 변경
  > * `-g`(gid): GID 변경 

<u>(2)</u> 그룹 패스워드 지정 (`gpasswd` 명령어)

* `gpasswd`: 그룹에 패스워드 부여/제거 및 그룹에 속할 사용자 추가/제거 가능

* 위 명령어의 수행 결과가 `/etc/gshadow` 에 저장됨

* 위 명령어는 UNIX 에는 없고 리눅스에만 있음

* 명령어 형식

  ```bash
  $ gpasswd [option] 그룹_이름
  ```

  > options
  >
  > * `-r`(remove): 그룹에 부여된 패스워드 제거
  > * `-a 사용자_계정`(add): 인자로 지정된 사용자 계정을 그룹에 추가
  > * `-d 사용자_계정`(delete): 인자로 지정된 사용자 계정을 그룹에 제거
  > * 단독 사용: 그룹의 패스워드 지정

<u>(3)</u> 자신의 기본 그룹 변경 (`newgrp` 명령어)

* 사용자 자신의 추가 그룹들 중 한 그룹을 기본 그룹으로 변경 시, 해당 그룹 패스워드 필요 없이 변경 가능

* 사용자 자신의 추가 그룹이 아닌 그룹 중 한 그룹을 기본 그룹으로 지정 시, 해당 그룹 패스워드 필요

* 기본 그룹이 변경된 세션을 새로 만듦

* 관리자 권한 없이 수행 가능

* 명령어 형식

  ```bash
  $ newgrp 그룹_이름
  ```

<u>(Exercise)</u>

1. `student2` 그룹 이름을 `student5` 로 변경

   ```bash
   $ grep student /etc/group
   student1:x:1005:
   student2:x:2022:mykim1
   student3:x:1006:
   student4:x:2023:
   $ sudo groupmod student2 -n student5
   $ grep student /etc/group
   student1:x:1005:
   student3:x:1006:
   student4:x:2023:
   student5:x:2022:mykim1
   ```

2. `student4` 그룹에 대해 이름은 `student6` 로, GID 는 `2030`으로 변경

   ```bash
   $ grep student /etc/group
   student1:x:1005:
   student3:x:1006:
   student4:x:2023:
   student5:x:2022:mykim1
   $ sudo groupmod student4 -n student6 -g 2030
   $ grep student /etc/group
   student1:x:1005:
   student3:x:1006:
   student5:x:2022:mykim1
   student6:x:2030:
   ```

3. `student7` 이라는 이름의 그룹을 생성하고 임의의 패스워드 부여

   ```bash
   $ sudo addgroup student7
   $ sudo gpasswd student7
   Changing the password for group student7
   New Password: 
   Re-enter new password: 
   ```

4. `student7` 그룹에 패스워드가 지정 여부 파악

   ```bash
   $ sudo grep student7 /etc/gshadow
   student7:$6$x2idD/bFPw$NQEQLIvoXDFuGI3FUNbjVBKHuxLXy89s/LOuf3.niqzwqojfHnw8gwHozlgxDvg3JA4lSbWUp/z37gj.7pHTn1::
   ```

5. `mykim1` 사용자가 소속된 그룹을 출력

   ```bash
   $ id mykim1 -G
   1005 2022
   ```

   ```bash
   $ groups mykim1
   mykim1 : student1 student5
   ```

6. `mykim1` 사용자가 `student7` 그룹에 추가 그룹으로 속하도록 지정

   ```bash
   $ sudo gpasswd student7 -a mykim1
   Adding user mykim1 to group student7
   $ groups mykim1
   mykim1 : student1 student5 student7
   ```

7. `mykim1` 사용자가 `student7` 그룹에 속하지 않도록 지정하고 `mykim1` 사용자가 소속된 그룹 출력

   ```bash
   $ sudo gpasswd student7 -d mykim1
   $ groups mykim1
   mykim1 : student1 student5
   ```

8. `student7` 그룹에 부여된 패스워드 제거 후 확인

   ```bash
   $ sudo gpasswd student7 -r
   $ sudo grep student7 /etc/gshadow
   student7:::
   ```

9. `student1`, `student5` 그룹에 각각 임의의 패스워드 지정

   ```bash
   $ sudo gpasswd student1
   Changing the password for group student1
   New Password: 
   Re-enter new password:
   $ sudo gpasswd student5
   Changing the password for group student5
   New Password: 
   Re-enter new password: 
   ```

10. `mykim1` 사용자 계정의 추가 그룹으로 `student1` 과 `student5` 를 지정 후, `mykim1` 사용자의 그룹 정보 확인

    ```bash
    $ sudo gpasswd student1 -a mykim1
    $ sudo gpasswd student5 -a mykim1
    ```

    ```bash
    $ sudo usermod mykim1 -G student1,student5
    ```

    ```bash
    $ id mykim1
    ```

11. `mykim1` 계정으로 전환 후, 해당 계정의 기본 그룹을 `student5` 로 변경

    ```bash
    $ su mykim1
    Password:
    mykim1: $ newgrp student5
    mykim1: $ id
    uid=2500(mykim1) gid=2022(student5) groups=2022(student5),1005(student1)
    ```

12. `mykim1` 계정의 기본 그룹을 `student7` 로 변경

    ```bash
    $ su mykim1
    Password:
    mykim1: $ newgrp student7
    Password:
    mykim1: $ id 
    uid=2500(mykim1) gid=1009(student7) groups=1009(student7),1005(student1),2022(student5)
    ```

##### <u>5.3.4.</u> 사용자와 그룹의 삭제

<u>(1)</u> 사용자 계정 삭제 (`userdel` 명령어)

* 시스템 내의 사용자 계정을 영구 삭제 (복구 불가능)

* `/etc/passwd` 파일에 저장된 계정 정보도 삭제됨

* 그러나, 기존의 홈 디렉토리는 삭제되지 않음 (삭제하려면 `-r` 옵션 사용)

* 명령어 형식

  ```bash
  $ userdel [option] 사용자_계정
  ```

  > option
  >
  > * `-r`: 해당 사용자 계정의 홈 디렉토리까지 삭제

<u>(2)</u> 그룹 계정 삭제 (`groupdel` 명령어)

* 시스템 내의 그룹을 영구 삭제

* 어떤 사용자의 기본 그룹으로 지정된 경우, 그룹 삭제가 불가능
  (이 경우, 사용자의 기본 그룹을 변경하거나 해당 사용자를 제거 등을 선수행)

* `/etc/group` 파일에 저장된 그룹 정보도 삭제됨

* 명령어 형식

  ```bash
  $ groupdel 그룹_이름
  ```

<u>(Exercise)</u>

1. 시스템에 예시로 만들었던 사용자 계정 및 홈 디렉토리 모두 삭제

   ```bash
   $ tail /etc/passwd
   ...
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
   mykim1:x:2500:1005:kim minho:/home/kim1home:/bin/bash
   $ sudo userdel mykim1 -r
   userdel: mykim1 mail spool (/var/mail/mykim1) not found
   ```

2. `/etc/passwd` 파일에 사용자 계정 정보가 삭제되었는지 확인

   ```bash
   $ tail /etc/passwd
   ...
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
   ```

3. `/home` 디렉토리에 사용자들의 홈 디렉토리가 없는지 확인

   ```bash
   $ ll /home
   total 24
   drwxr-xr-x  6 root        root        4096 Feb 13 16:17 ./
   drwxr-xr-x 23 root        root        4096 Feb  8 06:31 ../
   drwxr-xr-x  7 tmdgns1139  tmdgns1139  4096 Feb 13 14:07 tmdgns1139/
   ```

4. 시스템에 예시로 만들었던 그룹 제거

   ```bash
   $ tail /etc/group
   tmdgns1139:x:1004:
   student1:x:1005:
   student3:x:1006:
   stduent5:x:2024:
   stduent6:x:1007:
   kim1:x:1008:
   student5:x:2022:
   student6:x:2030:
   student7:x:1009:
   $ sudo groupdel kim1
   $ sudo groupdel student1
   $ sudo groupdel student3
   $ sudo groupdel stduent5
   $ sudo groupdel student5
   $ sudo groupdel student6
   $ sudo groupdel stduent6
   $ sudo groupdel student7
   $ tail /etc/group
   tmdgns1139:x:1004:
   ```
