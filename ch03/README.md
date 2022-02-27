## <u>Chapter 3.</u> 리눅스 shell 활용

* 사용자와 리눅스 커널의 인터페이스 역할을 하는 "리눅스 shell" 에 대해 학습
* bash shell 의 환경변수와 프롬프트를 학습
* history, alias 학습을 통해 shell 명령어를 효율적으로 사용하기

### <u>3.1.</u> 리눅스의 shell

##### <u>3.1.1.</u> shell 의 역할과 종류

<u>(1)</u> 리눅스에서 shell 의 역할

* 사용자와 리눅스 커널 사이의 인터페이스 역할
* UNIX shell 을 기반으로 만들어진 Linux shell
* 사용자가 시스템에 접속하면 자동으로 default/login shell 이 실행됨
  (`/etc/passwd`: 사용자의 login shell 에 대한 정보를 저장한 파일)
* 명령어 해석
  * 사용자 명령어의 유효성 검사 후, 해당 명령어를 리눅스 커널에 전송
  * 커널은 해당 명령어에 부합하게 HW 를 직접 제어하고 shell 이 실행 결과를 받아 사용자에게 전송
* 시스템 사용 환경 설정
  * 초기화 파일을 통한 초기 환경설정 수행
    (초기화 파일: 파일 검색 경로, 파일 생성시 부여되는 기본 권한, 기타 환경 변수 등 포함)
  * 사용자가 시스템에 접속하면 shell 은 해당 사용자의 초기화 파일을 실행시켜 환경 설정 수행
* shell script 수행
  * shell script: 다수의 개별 shell command 을 통해 shell 이 특정 동작을 수행하도록 하는 프로그램
  * 자주 사용되는 shell 명령어를 프로그램으로 작성하여 한번에 다수의 명령어를 실행시킬 수 있음

<u>(2)</u> shell 의 종류

* UNIX 개발 초기 단계에서 shell 은 파일 시스템의 일부였음 (Ken Thompson 에 의해 최초 개발)
* 1979년, Bourne Shell 개발 (AT&T 의 Steven Bourne)
* 1981년, C Shell 개발 (C언와 유사한 명령어 구문, Billy Joy)
* 1986년, Korn Shell (Bourne Shell 과 C Shell 을 조합, David Korn)
* 1989년, Bash Shell (FSF에서 무료 공개, Brain Fox)
* Shell 은 Bourne Shell 계열과 C Shell 계열로 나뉨

1. Bourne Shell
   * 가장 오래된 shell, 최초로 대중화된 shell, UNIX 기본 shell
   * UNIX 표준 구성 요소
   * 프로그램 이름: `sh`
   * 프롬프트 기호: `$`
   * 환경설정 파일: `.profile`
   * Bourne Shell 에 alias 등의 편의 기능을 추가한 Shell 을 사용함
2. C Shell
   * Bourne Shell 을 확장한 Shell, C언어와 유사한 구문의 명령어를 갖는 Shell
   * 프로그램 이름: `csh`
   * 프롬프트 기호: `%`
   * 환경설정 파일: `.cshrc`
   * Bourne Shell 에 비해 무겁고 명령어 처리 속도가 느리나 사용자 편의성 개선됨
   * 업그레이드된 Bourne Shell 등장 이후, 거의 사용되지 않음
3. Korn Shell
   * Bourne Shell 과 C Shell 의 장점을 조합하여 개발된 Shell
   * 프로그래밍 인터페이스가 우수하고 Bourne Shell 과 호환성을 유지
   * 사용자 편의 기능 제공 및 빠른 속도 제공
   * 프로그램 이름: `ksh`
   * 프롬프트 기호: `$`
   * 환경설정 파일: `.kshrc`
4. Bash Shell
   * Bourne Shell 과 호환성을 유지 (Bourne Again Shell)
   * C Shell 과 Korn Shell 의 사용자 편의성까지 제공
   * 우분투의 default/login Shell 로 사용됨
   * 프로그램 이름: `bash`
   * 프롬프트 기호: `$`
   * 환경설정 파일: `.bashrc`

##### <u>3.1.2.</u> shell 의 전환

* 사용자는 login shell 에서 다른 shell 로 전환할 수 있음

1. 일시적인 shell 전환

   * 쓰고 싶은 shell(sub-shell) 을 직접 실행하여 shell 전환 수행

   ```bash
   tmdgns1139@aiteen-front:~/frontend$ sh
   $ ps
     PID TTY          TIME CMD
   14789 pts/0    00:00:00 bash # login shell
   16677 pts/0    00:00:00 sh   # sub shell
   16678 pts/0    00:00:00 ps
   ```

   ```bash
   $ exit
   tmdgns1139@aiteen-front:~/frontend$ ps
     PID TTY          TIME CMD
   14789 pts/0    00:00:00 bash
   16681 pts/0    00:00:00 ps
   ```

   > `/etc/passwd` 파일은 그대로 유지됨

2. login shell 변경

   * 기본 shell 지정 및 변경은 관리자 계정을 통해 이루어짐

     * `/etc/passwd` 파일 수정 혹은 `usermod` 명령어

   * `chsh` 명령어를 통해 본인 계정의 login shell 변경 가능

   * 변경하려는 shell 이 시스템에 설치되어있어야함
     (`/etc/shells` 파일에서 시스템에 설치된 shell 확인)

     ```bash
     $ cat /etc/shells
     # /etc/shells: valid login shells
     /bin/sh
     /bin/bash
     /bin/rbash
     /bin/dash
     /usr/bin/tmux
     /usr/bin/screen
     ```

<u>(Exercise)</u>

1. 로그인 shell 에서 Bourne Shell 로 전환

   ```bash
   tmdgns1139@aiteen-front:~/frontend$ sh
   $
   ```

2. `/etc/passwd` 파일에서 자기 계정의 login shell 정보 확인

   ```bash
   $ cat /etc/passwd | grep tmdgns1139
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
   ```

3. 다시 로그인 shell 로 전환

   ```bash
   $ exit
   tmdgns1139@aiteen-front:~/frontend$ 
   ```

4. 전환 가능 shell 출력

   ```bash
   $ cat /etc/shells
   # /etc/shells: valid login shells
   /bin/sh
   /bin/bash
   /bin/rbash
   /bin/dash
   /usr/bin/tmux
   /usr/bin/screen
   ```

5. 자기 계정의 login shell 을 Bourne Shell 로 변경 후, 변경 사항 확인

   ```bash
   $ chsh -s /bin/sh tmdgns1139
   password:
   $ cat /etc/passwd | grep tmdgns1139
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/sh
   ```

6. 자기 계정의 login shell 을 Bash Shell 로 변경 후, 변경 사항 확인

   ```bash
   $ chsh -s /bin/bash tmdgns1139
   password:
   $ cat /etc/passwd | grep tmdgns1139
   tmdgns1139:x:1003:1004::/home/tmdgns1139:/bin/bash
   ```

### <u>3.2.</u> bash shell 의 환경 설정

##### <u>3.2.1.</u> shell 변수와 환경 변수

<u>(1)</u> shell 변수와 환경 변수 개요

* 시스템의 환경 설정을 위해 shell 은 shell 변수 및 환경 변수를 사용한다.

  * shell 변수: 현재 사용하는 shell 에서만 사용됨 (주로 대문자)
  * 환경 변수: 현재 shell 과 sub-shell 모두에서 사용됨 (주로 대문자)

* 사용자 명령어 검색 경로 지정, 프롬프트 모양/색상 지정 등 다양한 환경을 설정

* `set` 명령어

  * 모든 shell/환경 변수의 이름(name)과 값(value)을 출력

  ```bash
  $ set
  BASH=/bin/bash
  ...
  LOGNAME=tmdgns1139
  ...
  OSTYPE=linux-gnu
  PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
  ...
  ```

* `env` 명령어

  * 모든 환경 변수의 이름과 값을 출력

  ```bash
  $ env
  ...
  USER=tmdgns1139
  ...
  PWD=/home/tmdgns1139
  HOME=/home/tmdgns1139
  ...
  SHELL=/bin/bash
  ...
  LOGNAME=tmdgns1139
  ...
  PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
  ...
  ```

* `echo` 명령어

  * 개별 shell/환경 변수의 값을 출력

  ```bash
  $ echo $SHELL
  /bin/bash
  $ echo $HOME
  /home/tmdgns1139
  $ 
  ```

<u>(2)</u> shell 변수와 환경 변수 설정

* shell 변수 설정

  * 별도의 명령어는 없고 프롬프트에서 변수명과 값을 지정해줌

  ```bash
  $ BIRTHDAY=1003
  $ set | grep BIRTHDAY
  BIRTHDAY=1003
  $ env | grep BIRTHDAY
  ```

* 환경 변수 설정

  * `export` 명령어 사용

  ```bash
  $ export [option] ENV_VAR=VALUE
  $ export BIRTHDAY=1003
  ```

  > `-n`: 환경 변수를 shell 변수로 전환

  * From 환경 변수 To shell 변수

    ```bash
    $ export -n BIRTHDAY
    $ set | grep BIRTHDAY
    BIRTHDAY=1003
    $ env | grep BIRTHDAY
    ```

  * From shell 변수 To 환경 변수

    ```bash
    $ BIRTHDAY=1003
    $ export BIRTHDAY
    $ set | grep BIRTHDAY
    BIRTHDAY=1003
    $ env | grep BIRTHDAY
    BIRTHDAY=1003
    ```

<u>(3)</u> shell 변수와 환경 변수 제거

* `unset` 명령어를 통해 shell/환경 변수를 제거

  ```bash
  $ unset BIRTHDAY
  $ set | grep BIRTHDAY
  $ env | grep BIRTHDAY
  ```

##### <u>3.2.2.</u> bash shell 프롬프트 설정

<u>(1)</u> 프롬프트 구조 및 설정

1. 프롬프트 기본 구조

   * 프롬프트 등장은 shell 이 명령어를 입력받을 준비가 되었음을 의미

   * 이 외에도 로그인한 사용자와 서버의 이름, 현재 위치한 디렉토리 등 정보를 프롬프트에 포함 가능

     ```bash
     tmdgns1139@aiteen-front:~$
     ```

     > * tmdgns1139 라는 사용자가 aiteen-front 라는 이름을 갖는 서버에 접속함
     > * 현재 위치한 디렉토리는 홈 디렉토리임
     > * 위는 bash shell 의 기본 구조임

2. 이스케이프 문자를 활용한 프롬프트 변경

   * 이스케이프 문자

     * `\` 로 시작하는 문자
     * `\n`과 같은 문자열을 하나의 문자로 처리
     * shell 이 문자의 의미를 해석하여 실행함
     * https://sharkmino.tistory.com/1504

     | 이스케이프 문자 |                           설   명                            |
     | :-------------: | :----------------------------------------------------------: |
     |      `\d`       |           "요일 월 일" 과 같은 날짜 형식으로 표시            |
     |      `\h`       |               호스트 이름 (abc.com 을 abc 로)                |
     |      `\H`       |          호스트 전체 이름 (abc.com 을 abc.com 으로)          |
     |      `\u`       |                       사용자 계정 이름                       |
     |      `\e`       |                      색상 등 효과 지정                       |
     |      `\s`       |                 사용하는 shell 의 이름 표시                  |
     |      `\t`       |           현재시각을 24시간제 표시 (HH:MM:DD 형식)           |
     |      `\T`       |           현재시각을 12시간제 표시 (HH:MM:DD 형식)           |
     |      `\@`       |          현재시각을 12시간제 표시 (오전/오후 형식)           |
     |      `\w`       |               현재 위치한 디렉토리의 절대경로                |
     |      `\W`       |   현재 위치한 디렉토리의 절대경로 중 제일 마지막 디렉토리    |
     |      `\!`       |                      현재 history 번호                       |
     |      `\[`       | 프롬프트 내에 출력하지 않는 문자열의 시작 지정 <br />(색상 지정 등의 효과에 사용) |
     |      `\]`       | 프롬프트 내에 출력하지 않는 문자열의 끝 지정<br />(색상 지정 등의 효과에 사용) |

   * 프롬프트가 포함하는 내용은 `PS1` 이름을 갖는 환경변수에 저장됨

     ```bash
     $ echo $PS1
     \[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$
     ```

     ```bash
     $ vim ~/.bashrc
     ...
     if [ "$color_prompt" = yes ]; then
         PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
     else
         PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
     fi
     ...
     ```

     > `\u@\h:\w\$` 의미
     >
     > * `\u`: 사용자 계정
     > * `\h`: 호스트 이름
     > * `\w`: 현재 디렉토리의 절대 경로
     > * `\$`: "$" 를 문자로 인식하라고 하는 것 (이스케이프 문자 아님)

<u>(Exercise)</u>

1. 현재 `PS1` 환경변수에 저장된 값 출력

   ```bash
   $ echo $PS1
   \[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$
   ```

2. `PS1` 에 저장된 값을 임시 shell 변수에 담기

   ```bash
   $ MYPROMPT=$PS1
   $ echo $MYPROMPT
   \[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$
   ```

3. 프롬프트 형식을 '[계정@디렉토리 현재시간] $ ' 으로 지정
   (현재 디렉토리 중, 제일 마지막 디렉토리만 출력, 시간은 24시간제로 표현)

   ```bash
   tmdgns1139@aiteen-front:~$ PS1='[\u@\W \t] $ '
   [tmdgns1139@~ 07:47:38] $ 
   ```

4. 원래 프롬프트로 복원

   ```bash
   [tmdgns1139@~ 07:47:38] $ PS1=$MYPROMPT
   tmdgns1139@aiteen-front:~$ 
   ```

<u>(2)</u> 프롬프트 색상 지정

* 가독성을 위해 프롬프트의 각 요소마다 색상 부여 가능

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

> `\033[01;32m\]` 의미
>
> * `\033`: 프롬프트 색상 지정을 위한 이스케이프 문자
> * `01;32m`: "01"은 진하게, "32"은 초록색 의미
>
> `\033[00m\]` 의미
>
> * `\033`: 프롬프트 색상 지정을 위한 이스케이프 문자
> * `00m`: "00"은 기본 문자(진하지 않은 흰 문자)

* 프롬프트에서 쓸 수 있는 색상번호

| 글자 번호 | 색 상  | 글자 번호 | 색 상  |
| :-------: | :----- | :-------: | :----- |
|    30     | 검정색 |    31     | 빨간색 |
|    32     | 초록색 |    33     | 갈색   |
|    34     | 파란색 |    35     | 보라색 |
|    36     | 청록색 |    37     | 흰색   |

* 프롬프트 색 지정을 위한 `PS1` 환경변수 지정 형식

  ```bash
  $ PS1='\[\e[글자색:효과m\] 이스케이프 문자'
  ```

  > `\e`: 색상 지정을 위한 이스케이프 문자

* 지정된 글자색 및 효과는 이어서 등장하는 문자들에 적용됨

* 각 글자에 다른 색과 효과를 주려면 문자마다 별도의 색과 효과를 지정해야함

  ```bash
  $ PS1='\[\e[37m\]\u\[\e[31m\]\h \[\e[00m\]$ '
  ```

<u>(Exercise)</u>

1. 현재 환경변수 `PS1` 에 저장된 값을 백업

   ```bash
   $ MYPROMPT=$PS1
   ```

2. 아래 형식을 갖춘 프롬프트로 변경

   * 형식: `사용자계정@호스트이름:디렉토리 $ `
     * 사용자 계정: 흰색, 진하게 표시
     * 호스트 이름: 빨간색, 진하게 표시
     * 디렉토리: 파란색, 진하게 표시 (절대경로 모두 표시)
     * 프롬프트 기호($): 흰색 기본 문자 

   ```bash
   $ PS1='\[\e[01;33m\]\u\[\e[00m\]@\[\e[01;31m\]\h\[\e[01;34m\]\w \[\e[00m\]\$ '
   ```

3. 원래 프롬프트로 복원

   ```bash
   $ PS1=$MYPROMPT
   ```

### <u>3.3.</u> history 와 alias

##### <u>3.3.1.</u> history

<u>(1)</u> history 명령어

* 많은 shell 명령어를 실행하며 시스템을 관리하다보면 같은 명령어를 반복하는 경우 많음

* 그래서, 시스템은 사용자 로그인 이후 실행한 shell 명령어를 저장해두고 빠른 재실행을 도움

* 그것이 바로 C Shell 에서 처음 제공했던 `history` 명령어

* 시스템이 유저의 shell 명령어를 저장하는 개수는 환경 변수로 지정되있음

  ```bash
  $ echo $HISTSIZE
  1000
  ```

  > 최대 1,000개의 사용자의 이전의 shell 명령어를 시간순으로 저장해둔다.

* 명령어 형식

  ```bash
  $ history [option]
  $ history
      1  sudo apt update
      2  sudo apt install python
      3  sudo apt install python3
      4  sudo apt install python3-venv
      5  git
      6  git clone
      7  git clone https://github.com/boostcampaitech2/final-project-level3-cv-18.git
     ...
    268  PS1=$MYPROMPT
    269  echo $HISTSIZE
    270  history
  ```

<u>(2)</u> history 사용

| 사용법  | 설 명                                                   |
| :-----: | ------------------------------------------------------- |
|  !번호  | history 번호에 해당하는 명령어 다시 수행                |
| !문자열 | history 명령어 중 문자열이 포함된 최근 명령어 다시 수행 |

```bash
$ history | grep apt-get
    1  sudo apt update
    2  sudo apt install python
    3  sudo apt install python3
    4  sudo apt install python3-venv
   15  sudo apt update
  271  history | grep apt
$ !1
$ !15
$ !pw
pwd
/home/tmdgns1139/frontend/__pycache__
$ !sudo
sudo apt update
Hit:1 http://asia-northeast3.gce.archive.ubuntu.com/ubuntu bionic InRelease
Hit:2 http://asia-northeast3.gce.archive.ubuntu.com/ubuntu bionic-updates InRelease                  
Hit:3 http://asia-northeast3.gce.archive.ubuntu.com/ubuntu bionic-backports InRelease                
Hit:4 http://security.ubuntu.com/ubuntu bionic-security InRelease                                    
Reading package lists... Done                      
Building dependency tree       
Reading state information... Done
10 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

<u>(3)</u> history 저장 및 삭제

* 사용자의 명령어 이력은 각 사용자의 홈 디렉토리의 `.bash_history` 파일에 저장됨

  ```bash
  $ cat ~/.bash_history 
  sudo apt update
  ...
  exit
  ```

* 접속 중 수행했던 명령어들은 메모리에 존재하다가 로그아웃을 하면 해당 명령어 이력들을 `~/.bash_history` 파일에 저장함

* 명령어 이력 삭제

  ```bash
  $ history -c
  $ history
      1  history 
  ```

  > `history -c`: 메모리에 있던 사용자의 명령어 이력을 지움

##### <u>3.3.2.</u> alias 

<u>(1)</u> alias 용도

* 특정 명령어에 별명을 부여하는 것 (긴 명령어를 짧게 표현 가능)

* C Shell 이 최초 제공했고 Bash Shell 에서도 사용 가능

* 시스템에 정의된 alias 출력해보기

  ```bash
  $ alias
  alias egrep='egrep --color=auto'
  alias fgrep='fgrep --color=auto'
  alias grep='grep --color=auto'
  alias l='ls -CF'
  alias la='ls -A'
  alias ll='ls -alF'
  alias ls='ls --color=auto'
  ```

<u>(2)</u> alias 설정과 제거

* alias 추가을 위한 명령어 형식

  ```bash
  $ alias ALIAS_NAME='command [option] [;command [option]]'
  ```

  > 기존의 명령어 이름과 동일한 alias 지정이 가능함 (`alias rm='rm -i'`)

* alias 제거를 위한 명령어 형식

  ```bash
  $ unalias ALIAS_NAME
  ```

<u>(Exercise)</u>

1. `touch` 명령어를 이용해 파일 2개를 생성

   ```bash
   $ touch test1.txt
   $ touch test2.txt
   $ ll
   total 48
   ...
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   64 Dec 23 01:00 requirements.txt
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139    0 Feb 10 09:45 test1.txt
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139    0 Feb 10 09:44 test2.txt
   ```

2. `rm` 명령어를 이용해 파일 1개를 삭제

   ```bash
   $ rm test1.txt
   $ ll
   total 48
   ...
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   64 Dec 23 01:00 requirements.txt
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139    0 Feb 10 09:44 test2.txt
   ```

3. `-i` 옵션을 포함한 `rm` 명령어를 alias 로 지정

   ```bash
   $ alias rm='rm -i'
   ```

4. 위에서 생성한 명령어로 나머지 파일을 삭제

   ```bash
   $ rm test2.txt
   rm: remove regular empty file 'test2.txt'? y
   $ ll
   total 48
   ...
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139   64 Dec 23 01:00 requirements.txt
   ```

5. 사용자의 홈 디렉토리로 이동하는 명령어와 현재 디렉토리 위치를 반환하는 명령어를 합친 alias 를 생성

   ```bash
   $ alias mypwd='cd;pwd'
   ```

6. `/etc` 디렉토리로 이동하여 위에서 만든 alias 실행

   ```bash
   ~ $ cd /etc
   /etc $ mypwd
   /home/tmdgns1139
   ~ $
   ```

7. 생성했던 alias 들을 제거

   ```bash
   $ unalias mypwd
   $ unalias rm
   ```

<u>(3)</u> 환경설정 파일을 이용한 alias 지정

* 시스템 실행 시 혹은 사용자 로그인 시, 실행되는 환경설정 파일들이 있음

* 그래서 alias 나 프롬프트에 대한 설정을 환경설정 파일에 저장하지 않으면 다음 로그인때 원래대로 초기화됨

* 환경설정 파일 종류

  * *시스템* 환경설정 파일: 시스템을 사용하는 모든 사용자에게 적용되는 파일

    * Bash Shell 에서는 `/etc/profile`, `/etc/bash.bashrc` 등이 있음

  * *사용자* 환경설정 파일: 사용자 계정마다 적용되는 파일로 사용자 로그인 시 실행됨

    * Bash Shell 에서는 `.profile`, `.bashrc`, `.bash_history`, `bash_logout` 등이 있음

    | 사용자 환경설정 파일 | 설 명                                           |
    | :------------------: | ----------------------------------------------- |
    |      `.profile`      | 경로 추가 등의 환경 지정 및 `.bashrc` 파일 실행 |
    |      `.bashrc`       | history 크기, alias 지정, 함수 등을 지정        |
    |   `.bash_history`    | history 목록을 저장한 파일                      |
    |    `.bash_logout`    | 사용자 로그아웃 발생 시, 실행될 함수 등을 지정  |

* 사용자 환경설정 파일 중, `.bashrc`에 alias 를 지정하면 로그인할 때마다 지정한 alias 사용 가능

* `.bashrc` 를 고친 직후, 해당 설정을 재로그인 없이 적용하려면 `source ~/.bashrc` 를 수행

<u>(Exercise)</u>

1. `~/.bashrc` 파일 열어서 `ls -al` 을 `files` 라는 이름의 alias 를 추가

   ```bash
   $ vim ~/.bashrc
   alias files='ls -al'
   $ files
   Command 'files' not found
   ```

2. `source` 명령어를 통해 `.bashrc` 파일을 적용

   ```bash
   $ source ~/.bashrc
   $ files
   total 60
   drwxr-xr-x 6 tmdgns1139 tmdgns1139  4096 Feb 10 10:18 .
   ...
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139    17 Feb 10 08:48 .vimrc
   ```

3. 재로그인하여 추가했던 alias 를 실행

   ```bash
   $ exit
   $ files
   total 60
   drwxr-xr-x 6 tmdgns1139 tmdgns1139  4096 Feb 10 10:18 .
   ...
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139    17 Feb 10 08:48 .vimrc
   ```

4. 추가했던 alias 삭제

   ```bash
   $ vim ~/.bashrc
   alias files='ls -al' # 해당 줄 삭제
   ```

