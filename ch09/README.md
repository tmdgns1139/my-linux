## <u>Chapter 9.</u> 프로세스 관리

* 리눅스에서의 프로세스 이해 및 관리 방법 숙지
* 네트워크 서비스 수행을 위한 데몬 프로세스 이해
* 작업 예약 방법 숙지

### <u>9.1.</u> 프로세스의 개요

##### <u>9.1.1.</u> 리눅스에서의 프로세스

<u>(1)</u> 프로세스의 개념

*프로세스*

* 프로세스(Process): 실행 중인 프로그램(Program in Execution)
  * 사용자 프로세스: 사용자 프로그램 수행을 위해 생성되는 프로세스
  * 시스템 프로세스: 시스템 운영을 위해 OS가 생성한 프로세스
  * 이 외에 데몬 프로세스, 고아 프로세스, 좀비 프로세스 등이 있음
* 저장장치에 있는 프로그램은 메모리에 적재되어야 실행될 수 있음
* 장기 스케줄러는 저장장치에 있는 프로그램을 메모리에 적재해 CPU가 실행하도록함
* 메모리에는 다수의 프로세스가 존재하여 CPU에 의해 동시에 수행됨
  * 멀티 프로그래밍(multi programming): 다수의 프로세스를 메모리에 적재하는 것
  * 멀티프로그래밍 차수: 메모리에 동시에 존재할 수 있는 프로세스의 개수
* 리눅스는 멀티 태스킹 환경을 지원함 
  * 멀티 태스킹(multi tasking): 메모리에 존재하는 다수의 프로세스를 동시에 실행시키는 것
  * 동시에 다수의 프로세스가 메모리에 적재되어 (시스템의 공유 자원을 통해) 동시에 실행됨
  * 특정 프로세스의 자원 과다 사용/점유 등에 의해 시스템 성능 저하가 발생할 수 있음
* 프로세스를 잘 관리하는 것도 시스템 관리자의 중요 업무임

*부모 프로세스와 자식 프로세스*

* 프로세스는 필요에 따라 1개 이상의 다른 프로세스를 생성할 수 있음
  * 자식 프로세스(child process): 프로세스에 의해 생성된 프로세스
  * 부모 프로세스(parent process): 자식 프로세스를 생성한 프로세스
* 시스템에 생성된 다수의 프로세스들은 부모-자식 관계를 유지함
  * 보통 부모 프로세스는 자식 프로세스의 실행 결과를 전달받아 수행됨
* 시스템 부팅 시, 스케줄러는 `systemd` 프로세스 및 `kthread` 스레드를 생성함
  * `systemd` 프로세스: 시스템에 생성되는 모든 프로세스의 부모 프로세스
  * `kthread` 스레드: 모든 스레드의 부모 스레드

*PID 및 PCB*

* 프로세스 아이디(PID, Process ID)
  * 프로세스 생성 시, 커널이 프로세스에 부여하는 고유 번호
  * OS는 프로세스의 PID 를 통해 프로세스를 식별/구별함
    (프로세스 관리를 위해 프로세스 PID 를 알아야함)
  * 시스템 부팅 이후, 프로세스의 생성 순서에 따라 일련의 PID 를 할당
    (같은 프로세스여도 시스템 부팅 때마다 PID는 달라짐)
  * `systemd` 프로세스는 PID `1`번을, `kthread` 스레드는 PID `2`번을 할당 받음
* 프로세스 제어 블록(PCB, Process Control Block)
  * 프로세스 생성 시, 해당 프로세스 수행 관련 정보 저장을 위해 할당받은 메모리의 일부 공간
  * PID, 프로세스 상태 정보, 우선순위 정보, 할당받은 자원 리스트 정보 등을 저장
  * 프로세스가 수행되어가면서 PCB 내의 정보는 갱신됨

<u>(2)</u> 프로세스의 수행

*프로세스의 상태 변화*

* 메모리에 존재하는 프로세스의 상태는 3가지로 구분함
  * 준비 상태(ready state): CPU에 의해 실행 가능한 상태
  * 실행 상태(execute state): CPU에 의해 실행 중인 상태
  * 대기 상태(wait state): 입출력 등의 이유로 CPU의 수행이 중지된 상태
* 준비 상태에서 실행 상태로 천이(transition)
  * 준비 상태의 프로세스는 준비 큐(ready queue)에서 CPU에 의해 실행되기를 기다림
  * 시스템의 스케줄링 알고리즘에 의해 큐에서 프로세스를 선택해 실행하여 실행 상태로 변경시킴
* 실행 상태에서 대기 상태로 전환
  * 입출력 등의 이유로 프로세스의 실행이 중단되는 경우가 있음
  * 그런 경우, OS는 해당 프로세스를 실행 상태에서 대기 상태로 전환시키고 대기 큐(queue)에 보냄
  * 그리고 OS는 프로세스 중단의 원인이 되는 입출력 등을 수행함
* 프로세스는 여러 상태를 거쳐 실행된 후 종료됨

*문맥 교환 (Context Switching)*

* 멀티 태스킹 환경에서 다수의 프로세스가 동시에 수행됨
* 특정 시점에 CPU에 의해 실행되는 프로세스는 1개뿐임 (실행 상태의 프로세스는 1개뿐임)
* 다수의 프로세스를 번갈아 수행시켜 동시에 실행되는 것처럼 보이게함
* 프로세스 실행 도중 중단될 경우, 커널은 중단 시점 이전에 실행된 결과를 PCB에 저장
* 시간이 지나 중단되었던 프로세스를 재실행할 때, PCB 정보를 읽어들여 이어서 실행함
* **예.** 프로세스 `A` 실행 중, 입출력이 필요한 상황
  * 커널은 `A` 에 대한 현재까지의 실행 결과를 PCB 에 저장
  * 프로세스 `B` 의 PCB 정보를 읽어 `B` 를 실행
  * `A` 의 입출력이 완료되면 `B` 의 실행 정보를 PCB 에 저장
  * `A` 의 PCB 로부터 이전 실행 결과를 읽어 실행을 이어나감
* 위 예시와 같은 과정을 문맥 교환이라고함
* 시스템은 다수의 프로세스들은 문맥 교환을 통해 실행시켜 멀티 태스킹 환경을 구현

##### <u>9.2.1.</u> 프로세스의 종류

*데몬(daemon) 프로세스*

* 네트워크 서비스 수행을 위한 시스템 프로세스의 일종
* 모든 네트워크 서비스는 서비스 수행을 위한 고유의 데몬 프로세스를 가짐
* **예.** 웹 서비스를 수행하는 `httpd`
  * `httpd` 데몬은 시스템에 생성되어 대기하고 있음
  * 클라이언트로부터 웹 서비스 요청이 발생하면 즉각 응답함

*고아(orphan) 프로세스*

* 자식 프로세스 종료 전, 부모 프로세스가 소멸된 자식 프로세스
* 고아 프로세스 발생 시, `systemd` 프로세스가 고아 프로세스의 부모 프로세스가 됨
* `systemd` 프로세스난 고아 프로세스가 정상 종료될 수 있도록함

*좀비(zombie) 프로세스*

* 소멸했지만 여전히 시스템의 자원을 점유하는 프로세스
* 부모 프로세스는 자식 프로세스의 종료 코드를 받아들여 자식 프로세스를 종료시킴
* 부모 프로세스가 종료코드를 받지 않아 자식 프로세스가 종료되지 않아 좀비 프로세스가 되기도함
* 좀비 프로세스는 자원을 OS에 돌려주지 않아 다른 프로세스의 수행에 방해가 되니 관리는 필수
* 좀비 프로세스의 제거
  * 좀비 프로세스는 `kill` 명령어 대신 `SIGCHD` 시그널을 부모 프로세스에 전송해 제거함
  * 부모 프로세스를 제거하여 `systemd` 프로세스가 좀비 프로세스를 제거

##### <u>9.1.2.</u> 프로세스의 종류

### <u>9.2.</u> foreground 와 background

##### <u>9.2.1.</u> foreground 실행과 background 실행

<u>(1)</u> foreground 와 background 의 개요

* 터미널에서 수행 시간이 긴 명령어를 background 에서 실행시켜 해당 명령어 수행 중 다른 명령어를 수행함

*Foreground 실행*

* 대화식으로 명령어를 수행하는 방식
  (터미널에서 명령어 수행 완료 이후, 프롬프트가 생겼을 때 다른 명령어를 실행시킴)
* Foreground 에서 수행되는 프로세스는 1개임

```bash
$ command
```

*Background 실행*

* 한 명령어가 background 에서 실행되는 동안, 다른 명령어를 foreground 에서 실행 가능
* background 에서 실행되는 명령어의 경우, 터미널에 실행 과정의 출력이 없음
* 명령어 실행이 끝나지 않아도 터미널에 프롬프트가 있음
* 보통 항상 실행되어야 하는 명령어(데몬 프로세스 등), 수행시간이 긴 명령어를 background 에서 실행시킴

```bash
$ command &
```

<u>(Exercise)</u>

1. `/dev` 디렉토리에서 `std`로 시작하는 모든 파일을 검색하는 명령을 foreground 에서 실행

   ```bash
   $ find /dev -name std*
   /dev/stderr
   /dev/stdout
   /dev/stdin
   ```

2. `sleep` 명령어로 1시간동안 수행을 중지하는 명령을 background 에서 실행

   ```bash
   $ sleep 3600 &
   [1] 3324
   ```

3. `sleep` 명령어로 2시간동안 수행을 중지하는 명령을 background 에서 실행

   ```bash
   $ sleep 7200 &
   [2] 3368
   ```

<u>(2)</u> background 작업 목록 출력 (`jobs` 명령어)

* 현재 background 에서 수행 중인 작업의 목록을 출력

* 명령어 형식

  ```bash
  $ jobs [%작업번호]
  ```

  > * `%작업번호`: 해당 작업 순서번호의 프로세스 정보 출력
  > * `%+`: 작업 순서가 `+`인 프로세스 정보 출력
  > * `%-`: 작업 순서가 `-`인 프로세스 정보 출력

* **예.** 현재 background 에서 수행 중인 프로세스 정보 출력

  ```bash
  $ jobs
  [1]-  Running                 sleep 3600 &
  [2]+  Running                 sleep 7200 &
  ```

  |   항 목   | 설 명                                                        |
  | :-------: | :----------------------------------------------------------- |
  | 작업 번호 | Background 에서 수행된 프로세스의 작업 번호<br />프로세스가 추가될 때마다 1씩 증가함 |
  | 작업 순서 | `+`: 가장 최근에 접근한 작업<br />`-`: `+` 작업 직전에 접근한 작업<br />기호 없음: 그 외의 작업 |
  | 실행 상태 | `Running`: 현재 실행 중인 상태<br />`Done`: 정상 완료 상태<br />`Terminated`: 비정상 종료 상태<br />`Stopped`: 일시 정지 상태 |
  | 실행 명령 | 현재 background 에서 실행 중인 명령어                        |

* **예.** 현재 background 에서 작업 번호 1번으로 수행중인 프로세스 정보 출력

  ```bash
  $ jobs %1
  [1]   Running                 sleep 3600 &
  ```

##### <u>9.2.2.</u> 실행의 전환과 종료

<u>(1)</u> 실행 환경의 전환 (`fg`, `bg` 명령어)

* `bg` 명령어: 프로세스 실행을 Foreground 에서 Background 로 전환

  * foreground 에서 실행 중인 프로세스 중단을 위해 `CTRL+Z` 입력 후, `bg` 명령어 입력

  ```bash
  CTRL + Z
  $ bg
  ```

* `fg` 명령어: 프로세스 실행을 Background 에서 Foreground 로 전환

  ```bash
  $ fg %작업번호
  ```

  > `jobs` 명령어로 작업번호를 알아냄

<u>(Exercise)</u>

1. 현재 background 에서 수행 중인 프로세스 출력

   ```bash
   $ jobs
   [1]   Running                 sleep 3600 &
   [2]-  Running                 sleep 7200 &
   [3]+  Running                 sleep 10000 &
   ```

2. 작업 번호가 1번인 프로세스를 foreground 로 전환

   ```bash
   $ fg %1
   sleep 3600
   ```

3. 현재 foreground 에서 수행되고 있는 프로세스를 background 로 전환

   ```bash
   CTRL + Z
   [1]+  Stopped                 sleep 3600
   $ bg
   [1]+ sleep 3600 &
   ```

   ```bash
   $ jobs
   [1]   Running                 sleep 3600 &
   [2]-  Running                 sleep 7200 &
   [3]+  Running                 sleep 10000 &
   ```

<u>(2)</u> 작업 종료

* Foreground 에서 수행 중인 프로세스 종료 (`CTRL + C`)
* Background 에서 수행 중인 프로세스 종료
  * `fg` 명령어를 통해 프로세스를 foreground 로 전환한 후, `CTLR + C` 로 종료
  * `kill` 명령어를 통해 background 에서 수행 중인 프로세스 종료

### <u>9.3.</u> 프로세스 관리 명령

* 프로세스 관리를 통해 시스템의 성능을 최적의 상태로 유지해야함

##### <u>9.3.1.</u> 프로세스 정보 출력

<u>(1)</u> 프로세스 상태 정보 출력 (`ps` 명령어)

* 현재 수행 중인 프로세스의 정보 출력
  (프로세스 실행, CPU 사용, 메모리 사용 등의 정보)

* `ps` 명령어 옵션의 종류

  * BSD 옵션: 아무 기호 없이 옵션 추가 (예: `ax`)
  * UNIX 옵션: `-` 기호 하나를 사용하여 옵션 추가 (예: `-rf`)

* 명령어 형식

  ```bash
  $ ps [option]
  ```

  > options (UNIX)
  >
  > * `-e`: 수행 중인 모든 프로세스의 PID, 회선정보(TTY), 수행시간, 이름을 출력 (`-A` 와 동일)
  > * `-f`: 사용자의 UID, 부모 프로세스의 PID(PPID) 를 포함한 프로세스 정보 출력
  > * `-u UID`: 지정한 UID 에 해당하는 사용자에 관련된 프로세스 정보 출력
  > * `-p PID`: 지정한 PID 에 해당하는 프로세스의 정보 출력
  >
  > options(BSD)
  >
  > * `a`: 현재 터미널에서 수행 중인 프로세스의 PID, 회선정보, 상태, 수행시간, 이름을 출력
  > * `u`: 현재 터미널에서 수행 중인 프로세스의 사용자 계정, 메모리/CPU/시간 정보를 추가 출력
  > * `x`: 시스템에서 수행 중인 모든 프로세스의 PID, 회선정보, 상태, 수행시간, 이름을 출력

<u>(Exercise)</u>

1. 프로세스 상태 출력 (PID, TTY, TIME, CMD)

   ```bash
   $ ps
     PID TTY          TIME CMD
    4223 pts/0    00:00:00 bash
    4599 pts/0    00:00:00 sleep
    4603 pts/0    00:00:00 sleep
    4604 pts/0    00:00:00 sleep
    4606 pts/0    00:00:00 ps
   ```

   > `TTY`: 터미널 번호

2. 프로세스 상태 출력 (PID, TTY, TIME, CMD, UID, PPID, C, STIME)

   ```bash
   $ ps -f
   UID        PID  PPID  C STIME TTY          TIME CMD
   tmdgns1+  4223  4222  0 03:51 pts/0    00:00:00 -bash
   tmdgns1+  4599  4223  0 04:16 pts/0    00:00:00 sleep 3600
   tmdgns1+  4603  4223  0 04:17 pts/0    00:00:00 sleep 7200
   tmdgns1+  4604  4223  0 04:17 pts/0    00:00:00 sleep 10800
   tmdgns1+  4607  4223  0 04:17 pts/0    00:00:00 ps -f
   ```

   > `C`: CPU 사용률
   >
   > `STIME`: 프로세스가 시작된 시각

3. `tmdgns1139` 계정 사용자가 생성한 프로세스의 상태 출력 (PID, TTY, TIME, CMD)

   ```bash
   $ ps -u tmdgns1139
     PID TTY          TIME CMD
    4222 ?        00:00:00 sshd
    4223 pts/0    00:00:00 bash
    4224 ?        00:00:00 sftp-server
    4599 pts/0    00:00:00 sleep
    4603 pts/0    00:00:00 sleep
    4604 pts/0    00:00:00 sleep
    4651 ?        00:00:00 systemd
    4652 ?        00:00:00 (sd-pam)
   32116 ?        02:30:26 streamlit
   ```

4. bash 프로세스 상태 출력 (PID, TTY, TIME, CMD, UID, PPID, C, STIME)

   ```bash
   $ ps -fp 4223
   UID        PID  PPID  C STIME TTY          TIME CMD
   tmdgns1+  4223  4222  0 03:51 pts/0    00:00:00 -bash
   ```

5. 시스템에 생성된 모든 프로세스의 정보를 출력 (PID, TTY, TIME, CMD)

   ```bash
   $ ps -e
     PID TTY          TIME CMD
       1 ?        00:02:29 systemd
       2 ?        00:00:01 kthreadd
       ...
   32116 ?        02:30:26 streamlit
   ```

6. 현재 터미널에서 수행 중인 프로세스의 상태 출력 (PID, TTY, STAT, TIME CMD)

   ```bash
   $ ps a
     PID TTY      STAT   TIME COMMAND
    1754 ttyS0    Ss+    0:00 /sbin/agetty -o -p -- \u --keep-baud 115200,38400,9600 ttyS0 vt220
    1762 tty1     Ss+    0:00 /sbin/agetty -o -p -- \u --noclear tty1 linux
    4223 pts/0    Ss     0:00 -bash
    4599 pts/0    S      0:00 sleep 3600
    4603 pts/0    S      0:00 sleep 7200
    4604 pts/0    S      0:00 sleep 10800
    4703 pts/0    R+     0:00 ps a
   ```

   > STAT 의 첫번째 문자
   >
   > * `D`: 중지 불가능한 sleep 상태 (I/O 수행 등의 이유로 sleep)
   > * `R`: 현재 실행 중인 상태
   > * `S`: 중지 가능한 sleep 인 상태 (특정 이벤트 종료 대기 등의 이유로 sleep)
   > * `T`: 작업 제어 시그널로 정지되어 있거나 추적중인 상태
   > * `X`: 소멸된 상태
   > * `D`: 좀비 프로세스를 의미
   >
   > STAT 의 두번째 이후 문자
   >
   > * `<`: 프로세스의 우선순위가 높은 상태
   > * `N`: 프로세스의 우선순위가 낮은 상태
   > * `l`: 멀티 스레드
   > * `s`: 세션 리더
   > * `+`: foreground 에서 수행 중인 상태

7. 현재 터미널에서 수행 중인 프로세스의 상태 출력 
   (PID, TTY, STAT, TIME COMMAND, USER, %CPU, %MEM, VSZ, RSS)

   ```bash
   $ ps au
   USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
   root      1754  0.0  0.0  16416   152 ttyS0    Ss+   2021   0:00 /sbin/agetty -o -p -- \u --keep-baud 115200,38400,9600 ttyS0 vt220
   root      1762  0.0  0.0  14892   132 tty1     Ss+   2021   0:00 /sbin/agetty -o -p -- \u --noclear tty1 linux
   tmdgns1+  4223  0.0  0.2  23216  5156 pts/0    Ss   03:51   0:00 -bash
   tmdgns1+  4599  0.0  0.0   7928   772 pts/0    S    04:16   0:00 sleep 3600
   tmdgns1+  4603  0.0  0.0   7928   724 pts/0    S    04:17   0:00 sleep 7200
   tmdgns1+  4604  0.0  0.0   7928   724 pts/0    S    04:17   0:00 sleep 10800
   tmdgns1+  4805  0.0  0.1  37800  3348 pts/0    R+   04:34   0:00 ps au
   ```

8. 현재 시스템에서 수행 중인 프로세스의 상태 출력
   (PID, TTY, STAT, TIME COMMAND, USER, %CPU, %MEM, VSZ, RSS)

   ```bash
   $ ps aux
   USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
   root         1  0.0  0.4 160576  9392 ?        Ss    2021   2:29 /lib/systemd/systemd --system --deserialize 20
   root         2  0.0  0.0      0     0 ?        S     2021   0:01 [kthreadd]
   ...
   systemd+ 13625  0.0  0.2  70496  4092 ?        Ss   Feb16   0:04 /lib/systemd/systemd-resolved
   ...
   syslog   15394  0.0  0.1 267268  3816 ?        Ssl  Feb16   0:02 /usr/sbin/rsyslogd -n
   ...
   tmdgns1+ 32116  0.1 13.6 1439740 277504 ?      Sl    2021 150:27 /home/tmdgns1139/frontend/.front/bin/python3 /home/tmdgns1139/frontend/.front/bin/streamlit run front.py --server.port=8501
   ```

   > COMMAND 항목의 값이 스레드인 경우 대괄호안에 표현됨

<u>(2)</u> 프로세스 실행 정보 출력 (`top` 명령어)

* 프로세스의 실행 상태를 실시간으로 확인 (상태 갱신 간격을 옵션으로 지정 가능)

* 명령어 형식

  ```bash
  $ top [option]
  ```

  > options
  >
  > * `-d 시간(초 단위)`: 프로세스 실행 현황을 갱신하는 시간 지정
  > * `-p PID`: 지정된 PID 를 갖는 프로세스 정보만 확인
  > * `-i`: zombie, idle 상태의 프로세스는 확인하지 않음
  > * `-n 횟수`: 지정된 횟수만큼만 프로세스 실행 현황을 갱신
  > * `-u 사용자`: 지정된 사용자 계정이 수행하는 프로세스 정보만 확인

* 내부 명령

  |      내부 명령       | 설 명                             |
  | :------------------: | --------------------------------- |
  | `ENTER`, `SPACE_BAR` | 프로세스 실행 정보 갱신           |
  |       `h`, `?`       | `top` 명령어에 대한 도움말 출력   |
  |       `k PID`        | 수행 중인 프로세스 종료           |
  |         `u`          | 사용자 기준 정렬                  |
  |         `M`          | 메모리 점유율 기준, 내림차순 정렬 |
  |         `P`          | CPU 점유율 기준, 내림차순 정렬    |
  |         `q`          | `top` 명령 종료                   |

<u>(Exercise)</u>

1. 현재 시스템에서 수행 중인 프로세스 실행 정보를 실시간으로 확인

   ```bash
   $ top
   top - 04:50:12 up 62 days, 23:59,  1 user,  load average: 0.06, 0.02, 0.00
   Tasks: 123 total,   1 running,  72 sleeping,   0 stopped,   0 zombie
   %Cpu(s):  0.0 us,  0.2 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
   KiB Mem :  2031964 total,   258952 free,   456304 used,  1316708 buff/cache
   KiB Swap:        0 total,        0 free,        0 used.  1391840 avail Mem 
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
       1 root      20   0  160576   9392   6148 S   0.0  0.5   2:29.58 systemd                                                                  
       2 root      20   0       0      0      0 S   0.0  0.0   0:01.10 kthreadd 
   ```

   > * `PR`(priority): 프로세스의 우선순위
   > * `NI`(nice): 프로세스 우선순위의 기준값
   > * `VIRT`: 프로세스가 사용하는 가상 메모리 크기
   > * `RES`: 프로세스가 사용하는 실제 메모리 크기
   > * `SHR`: 프로세스가 사용하는 공유 메모리 크기
   > * `TIME+`: 프로세스의 CPU 누적 사용 시간

2. 현재 시스템에서 수행 중인 프로세스 실행 정보를 실시간으로 확인 (3초에 1번 갱신)

   ```bash
   $ top -d 3
   ```

3. 현재 streamlit 프로세스 실행 정보를 실시간으로 확인

   ```bash
   $ ps -e | grep streamlit
   32116 ?        02:30:29 streamlit
   ```

   ```bash
   $ top -p 32116
   top - 04:57:07 up 63 days, 5 min,  1 user,  load average: 0.01, 0.01, 0.00
   Tasks:   1 total,   0 running,   1 sleeping,   0 stopped,   0 zombie
   %Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
   KiB Mem :  2031964 total,   258956 free,   456224 used,  1316784 buff/cache
   KiB Swap:        0 total,        0 free,        0 used.  1391936 avail Mem 
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
   32116 tmdgns1+  20   0 1439740 277504   9800 S   0.0 13.7 150:29.44 streamlit
   ```

4. 수행 중인 프로세스 현황 실시간 확인 (10초에 1번 갱신, 총 5번 갱신)

   ```bash
   $ top -d 10 -n 5
   ```

5. `tmdgns1139` 계정 사용자와 관련된 프로세스 현황 실시간 확인

   ```bash
   $ top -u tmdgns1139
   top - 05:01:30 up 63 days, 10 min,  1 user,  load average: 0.00, 0.00, 0.00
   Tasks: 120 total,   1 running,  72 sleeping,   0 stopped,   0 zombie
   %Cpu(s):  0.2 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
   KiB Mem :  2031964 total,   259052 free,   456128 used,  1316784 buff/cache
   KiB Swap:        0 total,        0 free,        0 used.  1392032 avail Mem 
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    4222 tmdgns1+  20   0  107996   4232   3216 S   0.0  0.2   0:00.21 sshd                                                                     
    4223 tmdgns1+  20   0   23216   5156   3416 S   0.0  0.3   0:00.17 bash                                                                     
    4224 tmdgns1+  20   0   13064   1900   1780 S   0.0  0.1   0:00.00 sftp-server                                                              
    4599 tmdgns1+  20   0    7928    772    708 S   0.0  0.0   0:00.00 sleep                                                                    
    4603 tmdgns1+  20   0    7928    724    664 S   0.0  0.0   0:00.00 sleep                                                                    
    4604 tmdgns1+  20   0    7928    724    660 S   0.0  0.0   0:00.00 sleep                                                                    
    4651 tmdgns1+  20   0   76640   3416   2360 S   0.0  0.2   0:00.33 systemd                                                                  
    4652 tmdgns1+  20   0  193896   2676      0 S   0.0  0.1   0:00.00 (sd-pam)                                                                 
    5054 tmdgns1+  20   0   40152   3796   3152 R   0.0  0.2   0:00.00 top                                                                      
   32116 tmdgns1+  20   0 1439740 277504   9800 S   0.0 13.7 150:29.89 streamlit
   ```

##### <u>9.3.2.</u> 프로세스 우선순위 변경

* 프로세스 생성 시점에 OS 로부터 부여받은 우선순위에 따라 실행 순서가 결정됨
* 우선순위 값이 낮을수록 우선순위가 높다는 것을 의미함
  * 우선순위를 낮추는 것은 일반 사용자도 가능
  * 우선순위를 높이는 것은 root 계정만 가능

<u>(1)</u> 프로세스 생성 시 우선순위 변경 (`nice` 명령어)

* 프로세스 생성 시, `20`이라는 기본 우선순위를 부여받음

* 명령어 형식

  ```bash
  $ nice [option] 프로세스
  ```

  > option (UNIX)
  >
  > * `-n`: 프로세스 생성 시, 부여할 우선순위의 값 (기본값: 10, `-20~19` 범위 내에서 지정)

<u>(2)</u> 실행 중인 프로세스의 우선순위 변경 (`renice` 명령어)

* 이미 생성되어 있는 프로세스의 우선순위를 재지정 (프로세스의 PID 를 사용)

* 명령어 형식

  ```bash
  $ renice option PID
  ```

  > option (BSD)
  >
  > * `NI`: 새롭게 지정할 nice 값

<u>(Exercise)</u>

1. background 에서 `sleep 1200` 명령어를 실행한 후, 우선순위 값과 nice 값 확인

   ```bash
   $ sleep 1200 &
   [1] 5448
   $ top -p 5448
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    5448 tmdgns1+  20   0    7928    724    660 S   0.0  0.0   0:00.00 sleep  
   ```

2. nice 값으로 `5`를 갖는 프로세스를 background 에서 실행하고 우선순위 값과 nice 값 확인

   ```bash
   $ nice -5 sleep 1200 &
   [2] 5451
   $ top -p 5451
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    5451 tmdgns1+  25   5    7928    800    736 S   0.0  0.0   0:00.00 sleep  
   ```

3. nice 값으로 `-7`을 갖는 프로세스를 background 에서 실행하고 우선순위 값과 nice 값 확인

   ```bash
   $ nice --7 sleep 1200 &
   [3] 5453
   nice: cannot set niceness: Permission denied
   $ top -p 5453
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    5453 tmdgns1+  20   0    7928    728    664 S   0.0  0.0   0:00.00 sleep  
   ```

   > 우선순위 값을 줄여 우선순위를 높이는 것은 일반 사용자가 할 수 없음

4. nice 값으로 `-7`을 갖는 프로세스를 background 에서 실행하고 우선순위 값과 nice 값 확인 (`sudo` 사용)

   ```bash
   $ sudo nice --7 sleep 3600 &
   [5] 5510
   $ top -p 5510
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    5510 root      20   0   68300   4380   3856 S   0.0  0.2   0:00.00 sudo 
   ```

5. nice 값으로 `-7`을 갖는 프로세스를 background 에서 실행하고 우선순위 값과 nice 값 확인 (root 계정 전환)

   ```bash
   $ su
   Password:
   # nice --7 sleep 3600 &
   [1] 5565
   # top -p 5565
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    5565 root      13  -7    7928    832    772 S   0.0  0.0   0:00.00 sleep  
   ```

   > root 계정 비밀번호를 지정해야 접속 가능 (`$ sudo passwd root`)

6. nice 값으로 `5`를 갖는 프로세스 background 로 수행하고 우선순위를 `27`로 재지정

   ```bash
   $ nice -5 sleep 1200 &
   [1] 5699
   $ renice 7 5699
   5699 (process ID) old priority 5, new priority 7
   ```

7. PID 가 `5699`인 프로세스의 우선순위를 `23`으로 재지정

   ```bash
   $ renice 3 5699
   renice: failed to set priority for 5699 (process ID): Permission denied
   ```

8. PID 가 `5699`인 프로세스의 우선순위를 `23`으로 재지정 (`sudo` 사용)

   ```bash
   $ sudo renice 3 5699
   5699 (process ID) old priority 7, new priority 3
   $ top -p 5699 
     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                  
    5699 tmdgns1+  23   3    7928    724    660 S   0.0  0.0   0:00.00 sleep 
   ```

##### <u>9.3.3.</u> 프로세스 종료

##### <u>9.3.3.</u> 프로세스 종료

* 좀비 프로세스는 실행을 모두 마쳤으나 여전히 시스템의 자원을 점유하여 시스템 성능을 저하시킴

* 위와 같은 프로세스들은 직접 찾아서 `kill` 명령어로 강제로 종료시켜야함

* `kill` 명령어는 프로세스에 시그널을 보내는 명령어임

* 명령어 형식

  ```bash
  $ kill [signal] PID
  ```

  > `signal` 기본값: 15 (`SIGTERM`)

* 시그널 종류

  ```bash
  $ kill -l
   1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
   6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
  11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
  ...
  63) SIGRTMAX-1  64) SIGRTMAX
  ```

* 대부분의 프로세스들은 `(15) SIGTERM` 시그널을 전송받으면 자멸함

* `(15) SIGTERM` 시그널로 자멸하지 않는 프로세스들은 `(9) SIGKILL` 시그널을 전송하여 제거

### <u>9.4.</u> systemd 와 daemon 프로세스

##### <u>9.4.1.</u> init 프로세스

<u>(1)</u> `init` 프로세스

* 시스템 부팅 시, 시스템 운영에 필요한 각종 프로세스를 생성하는 최상단 부모 프로세스

* 시스템 부팅 시, 커널이 시스템의 초기화를 위해 생성하는 유일한 프로세스

  * `init` 프로세스: UNIX 운영체제 및 초기 우분투에서 사용
  * `upstart`: 14.10 버전까지 사용된 우분투 자체 제작 프로세스
  * `systemd`: 최근 우분투 및 대부분의 배포판에서 사용되는 프로세스 

* 프로세스 계층 출력

  ```bash
  $ pstree
  systemd─┬─accounts-daemon───2*[{accounts-daemon}]
          ...
          ├─sshd───sshd───sshd─┬─bash───pstree
          │                    └─sftp-server
          ...
          ├─streamlit───6*[{streamlit}]
          ...
          └─uuidd
  ```

  > 최상위 프로세스는 `systemd` 임

* `ps aux` 명령어 결과

  ```bash
  $ ps aux
  USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
  root         1  0.0  0.4 160576  9392 ?        Ss    2021   2:29 /lib/systemd/systemd --system --deserialize 20
  ```

  > 책에서는 `/sbin/init splash` 가 COMMAND 로 나와있음

* `/sbin/init`, `/lib/systemd/systemd` 정보 출력

  ```bash
  $ ls -l /sbin/init
  lrwxrwxrwx 1 root root 20 Dec 10 09:15 /sbin/init -> /lib/systemd/systemd
  $ ls -l /lib/systemd/systemd
  -rwxr-xr-x 1 root root 1616248 Dec 10 09:15 /lib/systemd/systemd
  ```

  > 우분투에서는 `init` 대신 `systemd` 프로세스를 사용함

* `init` 스크립트(`/etc/init.d`): `init` 프로세스가 `run level`을 통해 수행할 일련의 작업들

  ```bash
  $ ls -l /etc/init.d
  total 180
  -rwxr-xr-x 1 root root 2269 Apr 22  2017 acpid
  ...
  -rwxr-xr-x 1 root root 2757 Jan 20  2017 x11-common
  ```

* 대부분의 리눅스 배포판은 `run level`을 사용함 ( `init` 스크립트 파일은 아직 남아있음)

<u>(2)</u> `init` 프로세스와 `run level`

* `init` 프로세스는 시스템을 7단계로 구분하고 각 단계별로 정의된 스크립트를 실행함
  (각 단계별로 정의된 스크립트를 모아둔 디렉토리: `run level`)

* 우분투에서 사용하는 `run level`

  | 런레벨 | 관련 스크립트 | 설 명                        |
  | :----: | :-----------: | ---------------------------- |
  |   0    | `/etc/rc0.d/` | 시스템 종료                  |
  |   1    | `/etc/rc1.d/` | 단일 사용자 모드             |
  |   2    | `/etc/rc2.d/` | 다중 사용자 모드             |
  |   3    | `/etc/rc3.d/` | 다중 사용자 모드             |
  |   4    | `/etc/rc4.d/` | 다중 사용자 모드             |
  |   5    | `/etc/rc5.d/` | 그래픽 환경 다중 사용자 모드 |
  |   6    | `/etc/rc6.d/` | 시스템 재부팅                |

* 현재의 `run level` 출력

  ```bash
  $ sunlevel
  N 5
  ```

  ```bash
  $ ls -l /etc/rc5.d
  total 0
  lrwxrwxrwx 1 root root 15 Dec 14 17:09 S01acpid -> ../init.d/acpid
  ...
  lrwxrwxrwx 1 root root 15 Dec 14 17:09 S01uuidd -> ../init.d/uuidd
  ```

##### <u>9.4.2.</u> systemd 프로세스

<u>(1)</u> `systemd` 개요

* `systemd` 프로세스 이전의 상황
  * `init` 프로세스는 각 `run level`에 있는 스크립트를 순차적으로 실행시킴
  * 이러한 `init` 프로세스 기본 구조에 기능을 추가하는 방향으로 발전
  * 기능 추가는 각 `run level`의 스크립트를 보완/추가하는 방식
  * 기능이 추가될수록 실행시켜야할 스크립스가 늘어나 속도 저하 발생
* `init` 프로세스의 속도 저하 등의 문제 해결을 위해 레드햇 개발자들이 `sytemd` 개발
  * 성능을 크게 개선시켰고 의존성을 해치지 않는 범위 내에서 병렬로 프로세스를 실행시켜 속도 증가
  * 시스템 관리를 위해 유닛(unit)이라는 구성요소 이용
    (service, socket, device, target 등 다양한 유닛이 있음)
  * 유닛의 형식: `서비스이름.유닛종류` (atd.service: 작업을 예약하는 atd 데몬 관리를 위한 서비스 유닛)

<u>(2)</u> `systemd` 기반 서비스 관리

* `systemctl` 명령어: `systemd` 프로세스의 유닛 관리 명령어

  * 서비스를 시작/종료, 서비스의 상태를 출력하는 등의 작업 수행

* 명령어 형식

  ```bash
  $ systemctl 옵션 내부명령 유닛
  ```

  > 옵션
  >
  > * `-a`: 모든 유닛 전체를 출력
  > * `-t 유닛_종류`: 지정한 종류의 유닛만 출력
  >
  > 내부명령
  >
  > * `start`: 지정한 유닛 시작
  > * `stop`: 지정한 유닛 종료
  > * `restart`: 지정한 유닛 재시작
  > * `status`: 지정한 유닛 상태 출력
  > * `reload`: 지정한 유닛 설정 파일 다시 읽기
  > * `enable`: 부팅 시, 지정한 유닛 동작
  > * `disable`: 부팅 시, 지정한 유닛 동작 안함
  > * `is-active`: 지정한 유닛이 동작하는지 확인
  >
  > 단독사용: 현재 시스템에 동작 중인 유닛만 출력

* 유닛 서비스 시작

  ```bash
  $ sudo systemctl start systemd-networkd.service
  ```

  ```bash
  $ systemctl is-active systemd-networkd.service
  active
  ```

* 유닛의 상태 출력

  ```bash
  $ systemctl status systemd-networkd.service
  ● systemd-networkd.service - Network Service
     Loaded: loaded (/lib/systemd/system/systemd-networkd.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2022-02-23 10:53:20 UTC; 31s ago
     ...
  ```

* 유닛 서비스 시작 여부 확인

  ```bash
  $ systemctl is-active systemd-networkd.service
  ```

* 유닛 서비스 중지

  ```bash
  $ sudo systemctl stop systemd-networkd.service
  Warning: Stopping systemd-networkd.service, but it can still be activated by:
    systemd-networkd.socket
  ```

  ```bash
  $ systemctl is-active systemd-networkd.service
  inactive
  ```

##### <u>9.4.3.</u> 데몬 프로세스

* 데몬(daemon)
  * 사전적 의미는 "수호신"
  * 리눅스 서버 부팅 시, 메모리에 실행 혹은 sleep 상태에 있는 시스템 프로세스
  * 클라이언트에서 요구되는 네트워크 서비스를 수행해주는 시스템 프로세스
* 모든 네트워크 서비스는 각각의 데몬이 존재
* 데몬이 정상 실행되어야 해당 네트워크 서비스 수행 가능
* **예.** httpd 데몬
  * 클라이언트로부터 웹 서비스 요청이 발생하면 즉각 서비스를 수행함
* 데몬은 동작 방식에 따라 2가지로 구분 가능
  * 서비스의 특성과 성격에 맞는 방식을 골라 데몬을 운영해야함

<u>(1)</u> `standalone` 방식 데몬

* 데몬이 메모리에서 항상 실행되고 있음
* 클라이언트로부터 해당 서비스 요청을 받으면 즉각 처리 가능
* 장점: 서비스 속도가 빠름
* 단점: 메모리 일정 공간을 계속 차지함 (경우에 따라 자원 낭비가 되기도함)
* `/etc/init.d/`: standalone 방식의 데몬들을 모아둔 디렉토리
  * 대표적으로 웹 서버 데몬(httpd)이 있음

<u>(2)</u> `xinetd` 방식 데몬

* 데몬이 기본적으로 메모리에서 sleep 상태를 유지함

* 서비스 요청을 받으면 실행 상태가 되어 서비스를 제공하고 다시 sleep 상태로 전환함

* 장점: 적은 시스템 자원으로 여러 서비스 제공 가능

* 단점: 서비스 속도가 느림

* `xinetd` 데몬 (슈퍼 데몬)

  * sleep 상태인 데몬을 깨우는 데몬으로 standalone 방식으로 동작함

* `/etc/xinetd.d/`: xinetd 방식의 데몬들을 모아둔 디렉토리

  * 대표적으로 FTP, telnet 등이 있음

* **예.** telnet 동작 방식

  ![image](https://user-images.githubusercontent.com/87659486/155312435-f1500472-f33a-489c-a442-49a55651c0fe.png)

### <u>9.5.</u> 작업의 예약

* 사용자가 지정한 시각에 시스템이 작업을 자동으로 실행하면 좋은 상황
  * 특정 시간에 지정된 작업 수행해야하는 경우 (1시간 뒤 웹 서비스를 개시하는 작업 등)
  * 일정 주기마다 작업을 반복 수행해야하는 경우 (1달에 1번 시스템을 점검하는 작업 등)

##### 9.5.1. 일시적 작업 예약

<u>(1)</u> 일시적 작업 예약 (`at` 명령어)

* `at` 명령어: 특정 명령어를 지정된 시간에 1번만 실행시키는 명령어

* 명령어 형식

  ```bash
  $ at [option] (시간 날짜) | (+증가시간)
  ```

  > options
  >
  > * `-q 큐_이름`: 예약된 작업이 저장될 큐의 이름 지정 (기본값: a)
  > * `-f 파일명`: 예약 작업을 지정된 파일에서 읽어들임
  > * `-l`: 예약된 작업의 목록 출력
  > * `-d`: 예약된 작업 삭제
  >
  > 시간
  >
  > * `hh:mm`: 작업 예약 시간을 "시간:분" 형태로 지정 (예. 16:25)
  > * `am/pm`: 12시간제로 오전/오후를 표현 (생략 시, 24시간제임) (예. 4:25pm)
  > * `midnight`: 자정(00:00)을 나타냄
  > * `moon`: 정오(12:00)을 나타냄
  > * `now`: 현재 시간을 나타냄
  >
  > 날짜
  >
  > * `YYYY-MM-DD`: 작업 예약 날짜를 "연-월-일" 형태로 지정 (예. 2022-2-22)
  > * `month day`: 월(month)은 영문으로, 일(day)는 숫자로 지정 (예. April 20)
  > * `today`: 오늘 날짜를 나타냄
  >
  > `+증가시간`: 지정한 시간과 날짜를 기준으로 몇 시간 후인지를 표현함

* `at` 패키지 설치 확인 및 설치

  ```bash
  $ aptitude show at
  Package: at                       
  Version: 3.1.20-3.1ubuntu2
  State: installed
  ...
  ```

  ```bash
  $ sudo apt-get install at
  ```

* **예.** 오후 12시 16분에 작업을 예약

  ```bash
  $ at 12:16
  warning: commands will be executed using /bin/sh
  at> ls -l > my_data.txt
  at> mkdir ./my_data
  at> <EOT>
  job 2 at Wed Feb 23 12:16:00 2022
  ```

  ```bash
  $ ls -l
  total 16
  ...
  drwxrwxr-x 2 tmdgns1139 tmdgns1139 4096 Feb 23 12:16 my_data
  -rw-rw-r-- 1 tmdgns1139 tmdgns1139  193 Feb 23 12:16 my_data.txt
  ...
  ```

* `at` 명령어의 작업 파일

  * `/var/spool/cron/atjobs/`: `at` 명령어로 생성된 작업 파일을 일련의 순서로 보관하는 디렉토리
  * 예약된 명령어 수행 후, 제거되는 파일들임

<u>(Exercise)</u>

1. `2022.2.23. 12시 30분`에 파일 1개, 디렉토리 1개를 생성하는 작업을 예약

   ```bash
   $ at 12:30 2022-2-23
   warning: commands will be executed using /bin/sh
   at> touch test12_30.txt
   at> mkdir test12_30
   at> <EOT>
   job 3 at Wed Feb 23 12:30:00 2022
   ```

2. `2022.2.23. 12시 31분`에 파일 1개, 디렉토리 1개를 생성하는 작업을 예약

   ```bash
   $ at 12:31 2022-2-23
   warning: commands will be executed using /bin/sh
   at> touch test12_31.txt
   at> mkdir test12_31
   at> <EOT>
   job 4 at Wed Feb 23 12:31:00 2022
   ```

3. 지금으로부터 6시간 후에 디렉토리 1개를 생성하는 작업을 예약

   ```bash
   $ at now +6 hours
   warning: commands will be executed using /bin/sh
   at> mkdir test_after_6h 
   at> <EOT>
   job 5 at Wed Feb 23 18:26:00 2022
   ```

4. 현재 예약된 모든 작업 목록을 출력

   ```bash
   $ at -l
   4       Wed Feb 23 12:31:00 2022 a tmdgns1139
   6       Wed Feb 23 12:32:00 2022 a tmdgns1139
   3       Wed Feb 23 12:30:00 2022 a tmdgns1139
   1       Wed Feb 23 21:15:00 2022 a tmdgns1139
   5       Wed Feb 23 18:26:00 2022 a tmdgns1139
   ```

5. 예약된 작업 중, 4번 작업의 예약을 취소 및 취소 내용 확인

   ```bash
   $ at -d 4
   $ at -l
   6       Wed Feb 23 12:32:00 2022 a tmdgns1139
   3       Wed Feb 23 12:30:00 2022 a tmdgns1139
   1       Wed Feb 23 21:15:00 2022 a tmdgns1139
   5       Wed Feb 23 18:26:00 2022 a tmdgns1139
   ```

<u>(2)</u> 작업 예약 목록 출력 (`atq` 명령어)

* 예약 큐에 존재하는 작업 목록을 출력

* 예약된 작업과 예약 번호를 출력 (예약 번호는 예약 작업 생성 순서에 따라 부여됨)

* 명령어 형식

  ```bash
  $ atq [option]
  ```

  > option
  >
  > * `-q 큐_이름`: 지정된 큐에 있는 예약 작업 목록을 출력 (`$ at -l -q 큐_이름` 명령어와 동일)
  >
  > 단독 사용시: `$ at -l` 명령어와 동일

<u>(3)</u> 예약 작업 취소 (`atrm`)

* `at` 명령어로 예약한 작업을 취소하는 기능

* `$ at -d 작업번호` 명령어와 같은 기능

* 명령어 형식

  ```bash
  $ atrm 작업번호
  ```

<u>(Exercise)</u>

1. 현재 예약된 작업 목록 출력 및 1번, 5번 작업 삭제 후 확인

   ```bash
   $ atq
   1       Wed Feb 23 21:15:00 2022 a tmdgns1139
   5       Wed Feb 23 18:26:00 2022 a tmdgns1139
   $ atrm 1 5
   ```

   ```bash
   $ atq
   $
   ```

<u>(4)</u> `at` 명령 사용의 제한

* 리눅스 시스템은 사용자마다 `at` 명령어 사용의 허용/제한하는 방법을 제공함

* 허용

  * `/etc/at.allow` 파일에 사용자 계정을 줄마다 입력
  * 해당 파일은 관리자가 직접 생성하여 작성해야함

* 제한

  * `/etc/at.deny` 파일에 사용자 계정을 줄마다 입력
  * 해당 파일은 빈 파일로 기본 제공됨

* 위 두 파일 적용 기준

  |           경 우           | `at` 명령어 사용 가능한 사람                                 |
  | :-----------------------: | ------------------------------------------------------------ |
  | `/etc/at.allow` 파일 있음 | `/etc/at.allow` 파일에 등록되거나 <br />`/etc/at.deny` 파일에 등록되지 않은 사용자 |
  | `/etc/at.allow` 파일 없음 | `/etc/at.deny` 파일에 등록되지 않은 모든 사용자              |
  |     두 파일 모두 없음     | 오직 관리자만                                                |

  ```bash
  $ sudo mv at.deny at.deny.org
  $ at now +6 min
  You do not have permission to use at.
  ```

##### <u>9.5.2.</u> 주기적 작업 예약 (`crontab`)

* 일정 시간마다 반복되는 작업을 예약하기 위한 명령어

* 명령어 형식

  ```bash
  $ crontab option
  ```

  > options
  >
  > * `-l`: 현재 `crontab` 명령으로 지정된 내용 출력
  > * `-r`: 현재 `crontab` 명령으로 지정된 내용 취소
  > * `-e`: 주기적으로 실행할 작업을 추가/수정

* `-e` 옵션은 주기적인 작업 지정/수정하기 위해 사용함

  ```bash
  $ crontab -e
  no crontab for tmdgns1139 - using an empty one
  Select an editor.  To change later, run 'select-editor'.
    1. /bin/nano        <---- easiest
    2. /usr/bin/vim.basic
    3. /usr/bin/vim.tiny
    4. /bin/ed
  Choose 1-4 [1]: 2
  No modification made
  ```

  > 처음 `-e` 옵션 명령어를 수행하면 기본 편집기를 선택한 후, 주기적인 작업 지정/수정을 수행

* `crontab` 명령어는 환경변수(`EDITOR`)에 지정된 편집기를 기본 편집기로 사용함

  ```bash
  $ EDITOR=nano
  $ export EDITOR
  ```

  > 환경변수를 통해 기본 편집기를 `nano`로 바꿈

* `crontab` 파일 구조

  * `crontab` 명령어 수행 후, 아래 형식으로 주기적인 작업을 예약함
  * `분(0~59) 시(0~23) 일(1~31) 월(1~12) 요일(0~6) 작업내용`
  * 요일: 일요일(`0`), 월요일~토요일(`1~6`)
  * `*`: 해당 분, 시, 일, 월, 요일은 무관함를 의미

* `/var/spool/cron/crontabs/`: `crontab` 명령어로 지정한 작업 파일을 모아둔 디렉토리

* `crontab` 명령 사용 제한은 `at` 명령 사용 제한과 유사함

<u>(Exercise)</u>

1. 매주 토요일 오후 2시에 `/var/log` 디렉토리에 존재하는 모든 파일을 제거하도록 예약

   ```bash
   $ crontab -e
   ```

   ```bash
   14 00 * * 6 rm -rf /var/log/*
   ```

   > 위 내용을 편집기에 입력

   ```bash
   crontab: installing new crontab
   $
   ```

2. 매월 1일 자정에 모든 백업 파일을 제거도록 예약

   ```bash
   $ crontab -e
   ```

   ```bash
   0 0 1 * * rm *.bak
   ```

   > 위 내용을 편집기에 입력

   ```bash
   crontab: installing new crontab
   $
   ```

3. 주기적으로 실행되도록 예약된 작업 내용 출력

   ```bash
   $ crontab -l
   14 00 * * 6 rm -rf /var/log/*
   0 0 1 * * rm *.bak
   ```

4. 주기적으로 실행되도록 예약된 작업을 모두 제거 후 확인

   ```bash
   $ crontab -r
   $ crontab -l
   no crontab for tmdgns1139
   ```