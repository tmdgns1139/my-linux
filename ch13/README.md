## <u>Chapter 13.</u> 파일의 전송과 공유

* FTP 서버 설치 및 운영 (원격지 호스트 간 파일의 전송)
* NFS 서버 설치 및 운영 (같은 네트워크 내의 리눅스 시스템들 사이의 파일 공유)
* Samba 서버 설치 및 운영 (같은 네트워크 내의 리눅스와 윈도우 사이의 파일 공유)

### <u>13.1.</u> FTP 서버

<u>(1)</u> FTP 개요

* 네트워크 상의 컴퓨터들 사이에서 파일을 전송하기 위한 네트워크 서비스 (File Transfer Protocol)
* 최근에는 HTTP 를 통해 파일 전송이 가능하여 파일 전송에 있어 FTP 와 HTTP 를 혼용하기도함
* 파일 전송 성능의 관점에서 FTP 가 아주 우수함 (FTP 목적이 파일 전송이기 때문에)
* 리눅스에서는 주로 vsFTPD 를 사용함
  * Cris Evans 에 의해 개발된 **v**ery **s**ecure **FTP** **D**aemon
  * GPL 기반 FTP 서버로 안정성, 속도, 보안성이 우수함
* FTP 서비스는 일반적으로 `xinetd` 방식으로 사용됨 (`standalone` 방식도 가능함)
* 익명(anonymous) 접속을 통해 서버 접속이 가능함

<u>(2)</u> FTP 서버 설치 과정

1. `vsftpd` 패키지 설치

   ```bash
   $ aptitude show vsftpd
   Package: vsftpd
   Version: 3.0.3-12
   State: not installed
   ...
   ```

   ```bash
   $ sudo apt-get install vsftpd
   ```

2. 서버 접속 환경 설정

   * `/etc/vsftpd.conf`: vsFTPD 서버 환경 설정 관련 파일

   ```bash
   $ sudo vim /etc/vsftpd.conf
   ```

   |           항목            | 설 명                                                        |
   | :-----------------------: | ------------------------------------------------------------ |
   |         `listen`          | FTP 데몬 동작 방식 지정<br />(`yes`: standalone, `no`: xinetd) |
   |    `anonymous_enable`     | 익명 계정의 접속 가능 여부<br />(`yes`: 가능, `no`: 불가능)  |
   |   `anon_upload_enable`    | 익명 접속 사용자의 파일 업로드 허용 여부<br />(`yes`: 업로드 가능, `no`: 다운로드만 가능) |
   | `anon_mkdir_write_enable` | 익명 접속 사용자의 디렉토리 생성 허용 여부<br />(`yes`: 디렉토리 생성 가능, `no`: 불가능) |
   |       `local_umask`       | 파일 생성 시, 기본 umask 값 지정<br />(`022`: 권한을 `755` 로 지정하여 파일 생성) |
   |      `local_enable`       | 계정 접속 가능 여부<br />(`yes`: 가능, `no`: 불가능)         |
   |      `write_enable`       | 계정 접속 사용자게에 쓰기 권한 허용 여부 (익명은 해당 안됨)<br />(`yes`: 데이터 업로드, 디렉토리 추가/삭제, 기존 파일 수정/삭제 허용) |
   |    `dirmessage_enable`    | 접속한 디렉토리의 파일 리스트 공개 여부<br />(`yes`: 공개, `no`: 비공개) |
   |  `connect_from_port_20`   | 데이터를 전송하는 포트 지정<br />(`yes`: 20번 포트, `no`: 1024번 이상의 임의의 포트) |
   |     `xferlog_enable`      | 업로드/다운로드 내용의 로그 파일 저장 여부<br />(`yes`: 저장, `no`: 저장 안함) |
   |      `xferlog_file`       | 업로드/다운로드 상황을 저장할 로그 파일 지정                 |
   |  `idle_session_timeout`   | FTP 실행 후, 지정한 시간(초) 내에 <br />FTP 명령이 실행되지 않으면 접속 종료 |
   | `data_connection_timeout` | 데이터 업로드/다운로드 후, 지정한 시간(초) 내에<br />또 다른 파일의 전송이 이루어지지 않으면 접속 종료 |
   |       `fptd_banner`       | FTP 접속시, 보여주는 문구 지정                               |
   |    `chroot_local_user`    | `yes` 인 경우, 계정 홈 디렉토리를 루트로 인식 <br />(다른 계정 홈 디렉토리로 이동 못함) |
   | `allow_writeable_chroot`  | `/home` 디렉토리로 접근한 사용자들에게 쓰기 권한 부여 여부<br />(`yes`: 부여, `no`: 미부여) |

   > `chroot`: change root

3. 서버 재가동

   ```bash
   $ sudo systemctl restart vsftpd.service
   ```

   ```bash
   $ systemctl status vsftpd.service
   ● vsftpd.service - vsftpd FTP server
        Loaded: loaded (/lib/systemd/system/vsftpd.service; enabled; vendor preset: enabled)
        Active: active (running) since Wed 2022-03-02 09:36:57 KST; 1min 49s ago
   ...
   ```

4. 방화벽 설정

   * FTP 가 사용하는 21번 포트를 허용

   ```bash
   $ sudo ufw allow 21/tcp
   ```

5. FTP 클라이언트를 통해 접속

   * CLI

     ```bash
     $ ftp 서버_이름(주소)
     ```

     ```bash
     $ ftp
     ftp> open 서버_이름(주소)
     ```

   * GUI

     ```bash
     $ gftp
     ```

6. FTP 내부 명령

   |     내부 명령     | 설 명                                                        |
   | :---------------: | ------------------------------------------------------------ |
   |       `pwd`       | 원격 서버 상의 현재 디렉토리 출력                            |
   |    `ls`, `dir`    | 원격 서버의 디렉토리 내용 출력                               |
   |   `get 파일명`    | 지정한 파일 1개를 서버에서 클라이언트로 전송                 |
   | `mget 파일리스트` | 지정한 파일 여러개를 서버에서 클라이언트로 전송              |
   |   `put 파일명`    | 지정한 파일 1개를 클라이언트에서 서버로 전송                 |
   | `mput 파일리스트` | 지정한 파일 여러개를 클라이언트에서 서버로 전송              |
   | `![ShellCommand]` | FTP 연결 해제 업이 지정한 shell 명령어를 클라이언트에서 실행 |
   |  `delete 파일명`  | 서버 상의 파일을 제거                                        |

<u>(Exercise)</u>

1. 아래와 같이 임시 파일 생성

   ```bash
   $ ll ../ > file1.txt
   $ ls -l > file2.txt
   $ cat ../README.md > file3.txt
   ```

   ```bash
   $ ls -l file*
   -rw-r--r--  1 seunghun  staff  1064  3  2 11:10 file1.txt
   -rw-r--r--  1 seunghun  staff   124  3  2 11:10 file2.txt
   -rw-r--r--  1 seunghun  staff  8721  3  2 11:10 file3.txt
   ```

2. FTP 서버에 `tel-user` 계정으로 접속

   ```bash
   $ ftp tel-user@www.tmdgns1139.com
   Connected to www.tmdgns1139.com.
   220 Welcome to blah FTP service.
   331 Please specify the password.
   Password:
   230 Login successful.
   ftp>
   ```

   > 맥에서 터미널 FTP 클라이언트 프로그램 설치: `$ brew install inetutils`

3. 접속한 서버 내의 현재 디렉토리 출력

   ```bash
   ftp> pwd
   257 "/" is the current directory
   ```

4. 현재 디렉토리의 내용 출력

   ```bash
   ftp> ls
   200 PORT command successful. Consider using PASV.
   150 Here comes the directory listing.
   drwxrwxr-x    2 1001     1001         4096 Mar 02 02:18 tmpftp
   -rw-rw-r--    1 1001     1001            0 Mar 02 02:18 tmpftp.txt
   226 Directory send OK.
   ```

5. 클라이언트에서 만들었던 임시 파일 1개를 서버로 전송

   ```bash
   ftp> !ls file*
   file1.txt	file2.txt	file3.txt
   ```

   ```bash
   ftp> put file1.txt
   200 PORT command successful. Consider using PASV.
   150 Ok to send data.
   226 Transfer complete.
   1084 bytes sent in 0.00139 seconds (760 kbytes/s)
   ```

   ```bash
   ftp> ls
   200 PORT command successful. Consider using PASV.
   150 Here comes the directory listing.
   -rw-r--r--    1 1001     1001         1084 Mar 02 02:22 file1.txt
   drwxrwxr-x    2 1001     1001         4096 Mar 02 02:18 tmpftp
   -rw-rw-r--    1 1001     1001            0 Mar 02 02:18 tmpftp.txt
   226 Directory send OK.
   ```

6. 일시적으로 클라이언트로 전환하여 `file1.txt` 파일을 삭제 후, FTP 로 복귀

   ```bash
   ftp> !
   $ rm file1.txt
   $ ls file*.txt
   file2.txt	file3.txt
   $ exit
   ftp> 
   ```

7. 서버로 전송했던 `file1.txt` 파일을 클라이언트로 다운로드

   ```bash
   ftp> get file1.txt
   200 PORT command successful. Consider using PASV.
   150 Opening BINARY mode data connection for file1.txt (1084 bytes).
   226 Transfer complete.
   1084 bytes received in 0.000859 seconds (1.2 Mbytes/s)
   ```

   ```bash
   ftp> !ls file*.txt
   file1.txt	file2.txt	file3.txt
   ```

8. 클라이언트에서 만들었던 임시 파일 3개를 서버로 전송

   ```bash
   ftp> mput file?.txt
   mput file1.txt? y
   200 PORT command successful. Consider using PASV.
   150 Ok to send data.
   226 Transfer complete.
   1084 bytes sent in 0.000484 seconds (2.14 Mbytes/s)
   mput file2.txt? y
   200 PORT command successful. Consider using PASV.
   150 Ok to send data.
   226 Transfer complete.
   127 bytes sent in 0.00103 seconds (120 kbytes/s)
   mput file3.txt? y
   200 PORT command successful. Consider using PASV.
   150 Ok to send data.
   226 Transfer complete.
   8884 bytes sent in 0.00203 seconds (4.17 Mbytes/s)
   ftp> ls
   200 PORT command successful. Consider using PASV.
   150 Here comes the directory listing.
   -rw-r--r--    1 1001     1001         1084 Mar 02 02:38 file1.txt
   -rw-r--r--    1 1001     1001          127 Mar 02 02:38 file2.txt
   -rw-r--r--    1 1001     1001         8884 Mar 02 02:38 file3.txt
   drwxrwxr-x    2 1001     1001         4096 Mar 02 02:18 tmpftp
   -rw-rw-r--    1 1001     1001            0 Mar 02 02:18 tmpftp.txt
   226 Directory send OK.
   ```

9. 클라이언트 쪽의 임시 파일을 삭제하고 서버에 있는 임시 파일 다운로드

   ```bash
   ftp> !rm -f file?.txt
   ftp> !ls
   ftp> mget file*.txt
   mget file1.txt? y
   200 PORT command successful. Consider using PASV.
   150 Opening BINARY mode data connection for file1.txt (1084 bytes).
   226 Transfer complete.
   1084 bytes received in 0.000398 seconds (2.6 Mbytes/s)
   mget file2.txt? y
   200 PORT command successful. Consider using PASV.
   150 Opening BINARY mode data connection for file2.txt (127 bytes).
   226 Transfer complete.
   127 bytes received in 0.000262 seconds (473 kbytes/s)
   mget file3.txt? y
   200 PORT command successful. Consider using PASV.
   150 Opening BINARY mode data connection for file3.txt (8884 bytes).
   226 Transfer complete.
   8884 bytes received in 0.00251 seconds (3.37 Mbytes/s)
   ftp> !ls
   file1.txt	file2.txt	file3.txt
   ```

##### <u>13.1.3.</u> 익명 접속

* 사용자 계정이 아닌 `anonymous` 계정으로 접속 (시스템 내부적으로는 `ftp`라는 계정을 사용)

* 서버 계정 없이 FTP 서버에 접속 가능, 일반적으로 파일 다운로드만 가능

* `/etc/passwd` 파일에 `ftp` 계정의 설정 및 권한에 따라 서버에 접속하게됨

  ```bash
  $ cat /etc/passwd | grep ftp
  ftp:x:113:119:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
  ```

* 익명 접속 허용을 위해 `/etc/vsftpd.conf` 파일을 수정

  ```bash
  anonymous_enable=YES
  #anon_upload_enable=YES
  #anon_mkdir_write_enable=YES
  ```

<u>(Exercise)</u>

1. `/src/ftp` 디렉토리에 임의의 파일 생성

   ```bash
   $ cd /src/ftp
   $ ll ~ > ~/q2452_ll.txt
   $ sudo mv ~/q2452_ll.txt ./
   [sudo] password for seunghun:
   $ ls
   q2452_ll.txt
   ```

2. `anonymous` 계정으로 FTP 서버 접속

   ```bash
   $ ftp anonymous@www.tmdgns1139.com
   Connected to www.tmdgns1139.com.
   220 Welcome to blah FTP service.
   331 Please specify the password.
   Password:
   230 Login successful.
   ftp>
   ```

   > 비밀번호는 입력하지 않거나 아무거나 입력하면됨

3. 디렉토리의 현재 위치, 내용물 출력, 파일 다운로드 수행

   ```bash
   ftp> pwd
   257 "/" is the current directory
   ```

   ```bash
   ftp> ls
   200 PORT command successful. Consider using PASV.
   150 Here comes the directory listing.
   -rw-rw-r--    1 1000     1000          723 Mar 02 02:52 q2452_ll.txt
   226 Directory send OK.
   ```

   ```bash
   ftp> get q2452_ll.txt
   200 PORT command successful. Consider using PASV.
   150 Opening BINARY mode data connection for q2452_ll.txt (723 bytes).
   WARNING! 13 bare linefeeds received in ASCII mode
   File may not have transferred correctly.
   226 Transfer complete.
   723 bytes received in 0.000638 seconds (1.08 Mbytes/s)
   ftp> !ls
   q2452_ll.txt
   ```

### <u>13.2.</u> NFS 서버

<u>(1)</u> NFS 서버의 개요

* Network File System 의 약자로 썬 마이크로프트시스템에서 개발한 파일 공유 네트워크 서비스

* 같은 네트워크의 리눅스 시스템 사이의 파일 시스템을 서로 마운트하여 사용

* 서버의 공유 디렉토리에 있는 데이터를 네트워크 내에 있는 지정된 클라이언트들과 공유

* RPC(Remote Procedure Call) 사용을 통한 서비스

  *  exporting: 서버가 공유 디렉토리를 띄워 놓고 클라이언트의 마운트 요청을 기다림
  * 클라이언트의 RPC 를 통해 공유 디렉토리 마운트 요청 및 서버의 마운트 요청 허가

* 동작 방식

  ![image](https://user-images.githubusercontent.com/87659486/156289040-72e46106-17c3-4faf-81f3-2ce999296c04.png)

<u>(2)</u> NFS 서버 설치

* 실습을 위해서는 2대의 우분투 시스템이 필요함 (NFS 클라이언트/서버 각각 1대씩)

1. NFS 서버 설치 (`nfs-kernel-server` 패키지)

   ```bash
   $ aptitude show nfs-kernel-server
   Package: nfs-kernel-server
   Version: 1:1.3.4-2.5ubuntu3.4
   State: not installed
   ...
   ```

   ```bash
   $ sudo apt-get install nfs-kernel-server
   ```

2. 공유 디렉토리 및 공유 파일 생성

   ```bash
   $ sudo mkdir /home/nfs-share
   ```

   ```bash
   $ sudo chmod 777 /home/nfs-share
   ```

   > RPC 를 사용한 클라이언트는 외부 사용자이므로 디렉토리 허가권한을 재설정해줌

   ```bash
   $ sudo ls -l > /home/nfs-share/testfile.txt
   ```

3. NFS 서버의 공유 디렉토리에 접속 가능한 클라이언트 지정

   * `/etc/exports` 파일에 NFS 서버 접속 가능한 클라이언트를 지정

   * 클라이언트 지정 형식

     ```plain text
     공유_디렉토리_이름 클라이언트_주소 [option]
     ```

     |     옵션 항목      | 설 명                                                        |
     | :----------------: | ------------------------------------------------------------ |
     |  `ro` (read only)  | 서버의 공유 디렉토리를 읽기 전용으로 마운트                  |
     | `rw` (read write)  | 서버의 공유 디렉토리를 읽기 쓰기 가능 모드로 마운트          |
     |       `sync`       | 클라이언트의 마운트 디렉토리와 서버의 공유 디렉토리 동기화   |
     | `no_subtree_check` | 클라이언트가 요청한 파일의 위치 확인을 위한 <br />subtree_checking 을 실행하지 않음 |
     |  `no_root_squash`  | 클라이언트가 root 계정으로 서버에 접근할 경우, <br />서버에서도 root 권한을 부여 |

   * 설정 변경 및 변경된 설정 확인

     ```bash
     /home/nfs-share 192.168.35.2(rw)
     ...
     ```

     ```bash
     $ sudo exportfs
     /home/nfs-share
     		192.168.35.2
     ```

4. 설정 변경 후, 서버의 `NFS`, `rpcbind` 데몬 재시작 및 방화벽 설정

   ```bash
   $ sudo systemctl restart nfs-kernel-server
   $ sudo systemctl restart rpcbind
   ```

   ```bash
   $ sudo ufw disable
   ```

   > NFS 관련 다양한 데몬들 각각마다 고정 포트를 지정하고 접근을 허용해야함

5. NFS 클라이언트 설치 (`nfs-common` 패키지)

   ```bash
   $ sudo apt-get install nfs-common
   ```

6. NFS 서버의 공유 디렉토리 확인

   * 클라이언트는 서버의 IP 주소 및 공유 디렉토리 이름을 가지고 해당 공유 디렉토리를 마운트함

   * 서버에 생성되어 있는 공유 디렉토리 이름 확인

     ```bash
     $ shomount -e www.tmdgns1139.com
     Export list for www.tmdgns1139.com
     /home/nfs-share www.tmdgns1139.client
     ```

7. 공유 디렉토리 마운트

   ```bash
   $ sudo mount -t nfs www.tmdgns1139.com:/home/nfs-share /mnt
   ```

8. 공유 디렉토리 확인

   ```bash
   $ ls /mnt
   testfile.txt
   ```

9. 클라이언트에서 생성한 파일을 서버에서 확인하기

   ```bash
   $ cd /mnt # client system
   $ touch testfile_from_client.txt
   ```

   ```bash
   $ ls # server system
   testfile_from_client.txt  testfile.txt
   ```

### <u>13.3.</u> Samba 서버

##### <u>13.3.1.</u> Samba 서버의 개요

<u>(1)</u> Samba 서버의 역할

* 다른 OS 와 리눅스 사이의 데이터 공유를 위해 사용되는 서버
  * 윈도우의 공유 폴더를 리눅스에서 마운트하여 사용 가능
  * 리눅스의 공유 디렉토리를 윈도우의 드라이브에 마운트하여 사용 가능
* HW 공유
  * 데이터뿐만 아니라 HW 까지 공유
  * 다른 OS 에 연결된 프린터, CD-ROM 등의 장치 공유 가능
* 백업 시스템으로 활용
  * 윈도우와 리눅스 사이의 데이터 공유를 통해 리눅스 시스템을 윈도우 시스템의 백업 서버로 활용 가능

<u>(2)</u> Samba 서버를 이용한 공유

* 윈도우/리눅스는 Samba 클라이언트/서버로 동작함

##### <u>13.3.2.</u> 리눅스에서 윈도우의 공유 폴더 사용

<u>(1)</u> 자원 공유 과정 및 방법

* 윈도우(Samba 서버): 자원 공유 및 공유 대상자 등록만 수행 (Samba 서버 프로그램은 따로 필요 없음)
* 리눅스(Samba 클라이언트): Samba 클라이언트 프로그램이 설치되어야함

* 윈도우에서
  1. 공유 폴더 생성 및 지정: Samba 를 통해 공유 및 리눅스에서 마운트할 폴더 생성 및 지정
  2. 사용자 등록: 윈도우 서버에 접속할 Samba 클라이언트 사용자의 이름 및 패스워드 생성/지정
* 리눅스에서
  1. Samba 클라이언트 설치 (`smbclient` 패키지)
  2. 계정 접속 및 공유 확인 (`smbclient` 명령어): 윈도우 IP 주소 필요
  3. 마운트 포인트 생성 및 마운트 (`mount` 명령어)

<u>(2)</u> Samba 서버 설정 (윈도우 시스템)

* 윈도우의 공유 폴더 생성
  1. 폴더를 생성 (예시로 C드라이브에 생성)
  2. 해당 폴더에 대해 [우클릭]-[속성]-[공유]-[고급 공유] 선택
  3. 대화상자에서 '선택한 폴더 공유' 버튼 체크 후, [확인] 버튼 클릭
  4. [Samba-Share 속성] 대화상자에서 [공유] 버튼 클릭
  5. [네트워크 엑세스] 대화상자에서 `Everyone` 선택 후, [추가] 버튼 클릭
  6. `Everyone`의 사용 권한 수준을 "읽기/쓰기"로 변경 후, [공유] 버튼 클릭
  7. [네트워크 엑세스] 대화상자에서 공유되는 정보 확인 후, [완료] 버튼 클릭
  8. 공유 폴더에 임시 파일을 하나 생성
* 공유 사용자 계정 생성
  1. 윈도우의 [설정]-[계정] 선택, [가족 및 다른 사용자] 선택, '이 PC에 다른 사용자 추가' 선택
  2. [Microsoft 계정] 대화상자에서 '이 사람의 로그인 정보를 가지고 있지 않습니다.' 클릭
  3. 'Microsoft 계정없이 사용자 추가' 클릭
  4. '내 PC용 계정 만들기'에서 사용자 계정 이름은 `root`로 지정하고 패스워드 입력 후, [다음] 버튼 클릭

<u>(3)</u> Samba 클라이언트 설치 (`smbclient` 패키지, 리눅스 시스템)

1. Samba 클라이언트 설치

   ```bash
   $ aptitude show smbclient
   Package: smbclient
   Version: 2:4.13.17~dfsg-0ubuntu0.21.04.1
   State: not installed
   ...
   ```

   ```bash
   $ sudo apt-get install smbclient
   ```

2. 윈도우 서버로부터 공유 관련 정보 출력

   ```powershell
   PS > ipconfig
   ...
   IPv4 주소 . . . . . . : 192.168.???.???
   ...
   ```

   ```bash
   $ smbclient -L www.t495s.com
   Enter WORKGROUP\seunghun's password:
   
   	Sharename       Type      Comment
   	---------       ----      -------
   	ADMIN$          Disk      원격 관리
   	C$              Disk      기본 공유
   	IPC$            IPC       원격 IPC
   	samba-share     Disk
   	Users           Disk
   SMB1 disabled -- no workgroup available
   ```

   > * 윈도우에서 IP 주소 알아내기: cmd, power shell 등에서 `ipconfig` 명령어 입력하여 확인
   > * 비밀번호는 윈도우를 등록한 MS 계정의 비밀번호를 입력

3. 마운트 포인트 생성

   ```bash
   $ sudo mkdir /mnt/samba-share
   ```

4. 공유 폴더 마운트

   * 윈도우의 공유 폴더를 마운트 포인트에 마운트

   * 파일 시스템은 윈도우, 유닉스/리눅스를 모두 지원하는 CIFS 를 사용함
     (Common Internet File System: 네트워크를 위한 파일 공유 프로토콜의 확장판)

   * 리눅스에 `cifs-utils` 패키지 설치

     ```bash
     $ sudo apt-get install cifs-utils
     ```

   * 마운트 수행

     ```bash
     $ sudo mount -t cifs //www.t495s.com/samba-share
     ```

     > 윈도우에서 root 계정을 만들면서 지정했던 패스워드 입력

     ```bash
     $ ls -l
     total 1
     -rwxr-xr-x 1 root root 39 Mar  2 07:02 samba-window.txt
     ```

##### <u>13.3.3</u> 윈도우에서 리눅스의 공유 디렉토리 사용

<u>(1)</u> 자원 공유 과정 및 방법

* 윈도우(Samba 클라이언트), 리눅스(Samba 서버)
* 리눅스: Samba 서버 패키지가 설치되어있고 Samba 서비스 제공 데몬이 동작하고 있어야함
* 윈도우: 네트워크 드라이브를 통해 리눅스에 접근

* 윈도우에서
  * 네트워크 드라이브 생성 (리눅스에 접근 가능)
  * Samba 서버 동작 확인: 네트워크 드라이브로 리눅스의 공유 디렉토리에 정상 접근이 가능한지 확인
* 리눅스에서
  * Samba 서버 패키지 설치 (`samba` 패키지)
  * 공유 디렉토리 생성 및 허가 권한 설정
  * 환경 설정 (`/etc/samba/smb.conf`)

<u>(2)</u> Samba 서버 설치 (리눅스 시스템)

* `samba` 패키지 설치 및 설정 파일 확인

  ```bash
  $ aptitude show samba
  Package: samba
  Version: 2:4.13.17~dfsg-0ubuntu0.21.04.1
  State: not installed
  ...
  ```

  ```bash
  $ sudo apt-get install samba
  ```

  ```bash
  $ ls -l /etc/samba/smb.conf
  -rw-r--r-- 1 root root 8942 Mar  2 06:50 /etc/samba/smb.conf
  ```

* 공유 디렉토리 생성 및 공유 파일 생성

  ```bash
  $ mkdir /home/seunghun/samba-share-ubuntu
  $ ll / >> /home/seunghun/samba-share-ubuntu/testfile.txt
  ```

* Samba 사용자 계정 생성

  ```bash
  $ sudo adduser mysmb
  ```

* Samba 서버의 환경 설정

  ```bash
  $ cd /etc/samba
  $ sudo cp smb.conf smb.conf.org # backup 
  ```

  ```bash
  $ sudo vim smb.conf
  ...
  	workgroup = WORKGROUP
  ...
  [smbshare]
  path = /home/seunghun/samba-share-ubuntu
  read only = no
  valid users = mysmb
  create mask = 0644
  directory mask = 0755
  ```

  > `workgroup` 에 윈도우가 속한 `WORKGROUP` 을 지정

  |       항목       | 설 명                                     |
  | :--------------: | ----------------------------------------- |
  |   `[smbshare]`   | 윈도우의 네트워크에서 접근할 때 쓰는 이름 |
  |      `path`      | 공유 디렉토리를 절대 경로로 지정          |
  |   `read only`    | `yes`: 읽기만 가능, `no`: 읽기/쓰기 가능  |
  |  `valid users`   | Samba 서버에 접속 가능한 계정의 이름      |
  |  `create mask`   | 생성되는 파일의 기본 권한 설정            |
  | `directory mask` | 생성되는 디렉토리의 기본 권한 설정        |

* Samba 사용자의 패스워드 지정

  ```bash
  $ sudo smbpasswd -a mysmb
  ```

  > 리눅스 시스템에 접속할 때 쓰는 비밀번호와는 다름

* 설정 적용을 위한 서버 재시작

  ```bash
  $ sudo systemctl restart smbd.service
  $ sudo systemctl restart nmbd.service
  ```

<u>(3)</u> Samba 클라이언트 (윈도우 시스템)

* 파일 탐색기의 [네트워크] 우클릭, [네트워크 드라이브 연결] 클릭
* 대화상자의 [폴더] 칸에 IP 주소 및 리눅스의 공유 디렉토리 이름을 지정
  * 정확하게 `\\IP주소\smbshare` 라고 적어줌
  * 오류: Windows Creators Update 이후 발생하는 오류로 로컬 그룹 정책을 변경하여 해결
  * cmd 에서 PS > `gpedit.mcs` 를 실행하여 로컬 그룹 정책기 대화상자 열기
  * [컴퓨터 구성]-[관리 템플릿]-[네트워크]-[Lanman 워크스테이션] 선택
  * 대화상자 우측 창의 '보안되지 않은 게스트 로그온 사용'을 더블 클릭 
  * '사용'에 체크하고 [확인] 버튼 클릭
* 네트워크 드라이브 연결 다시 실행
  * 아이디: 리눅스 사용자 계정 이름 입력
  * 비밀번호: `smbpasswd` 로 지정했던 패스워드 입력
* 공유된 디렉토리 확인

