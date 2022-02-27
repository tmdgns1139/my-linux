## <u>Chapter 11.</u> 웹서버 관리

* 웹서버에 대해 알아보고 웹서버를 운영하는 과정 숙지
* HTTP 웹서버(리눅스에서 수행됨), PHP(웹 프로그래밍에 사용되는 언어), MariaDB(데이터베이스)

### <u>11.1.</u> 아파치 웹서버 설치

* 네트워크 서비스는 클라이언트-서버 모델을 통해 이루어짐
* 웹 서비스는 네트워크 서비스의 대표적인 예임
  * 사용자는 브라우저를 통해 서버에 접속하여 웹 페이지를 요구
  * 서버는 사용자가 요구한 웹 페이지를 서비스해줌
  * 클라이언트(웹 브라우저)-서버(웹서버)
* 웹 서비스를 위한 SW 구성
  * 웹서버: 웹 문서의 요청을 처리함 (`Apache` 웹서버)
  * 인터넷 프로그래밍 언어: 사용자의 요구를 처리하는 프로그램 개발 (`PHP`)
  * 데이터베이스: 서비스에 필요한 각종 데이터를 저장 (`MariaDB`)

##### <u>11.1.1.</u> Apache2 설치

<u>(1)</u> `apache2` 설치

* `apache2` 설치 버전 확인 및 설치

  ```bash
  $ sudo aptitude show apache2
  Package: apache2
  Version: 2.4.41-4ubuntu3.9
  State: not installed
  ...
  ```

  ```bash
  $ sudo apt-get install apache2
  ```

* 브라우저를 통해 설치 확인

  ![image](https://user-images.githubusercontent.com/87659486/155879728-b843339e-9c75-419f-bd3a-8f1ba938da56.png)

<u>(2)</u> `apache2`를 이용한 브라우저 출력

* `/var/www/html`: `apache2` 웹서버가 호스팅하는 웹 문서들을 저장하는 디렉토리 (문서 루트)

* 문서 디렉토리 내용물 확인

  ```bash
  $ cd /var/www/html
  $ ls -aF
  ./  ../  index.html
  ```

* `html` 문법에 맞추어 `my1st.html` 파일 생성

  ```bash
  $ cat my1st.html
  <html>
  	<head>
  		<title>my 1st web doc</title>
  	</head>
  	<body>
  		<h1>Hello world in 1st web doc</h1>
  	</body>
  </html>
  ```

* 만든 페이지를 브라우저로 확인

  ![image](https://user-images.githubusercontent.com/87659486/155879938-66ce5ab6-6d07-4b38-ac7f-f56dabf6cf82.png)

##### <u>11.1.2.</u> 사용자 디렉토리의 사용

*사용자 디렉토리의 개요*

* 웹서버는 여러 웹사이트를 동시에 호스팅할 수 있음
  * 웹사이트마다 호스팅을 수행하는 웹서버가 별도로 존재하면 비효율적일 수 있음
* 웹서버가 1개 이상의 사이트를 호스팅하기 위해 사용자 디렉토리(User Directory)를 사용함
  * 웹 문서를 문서 루트 디렉토리가 아닌 사용자들의 홈 디렉토리에 유지하는 방법
  * 사용자의 홈 디렉토리마다 1개의 웹 사이트를 유지하여 사용자 계정 수만큼 웹 사이트 호스팅 가능

<u>(1)</u> `userdir.conf` 파일 수정

* 사용자 디렉토리 사용을 위해 `/etc/apache2/mods-available` 디렉토리의 `userdir.conf` 파일을 수정

* `userdir.conf` 파일 내용 확인

  ```bash
  $ cd /etc/apache2/mods-available
  $ cat userdir.conf
  <IfModule mod_userdir.c>
  	UserDir public_html
  	#UserDir disabled root
  
  	<Directory /home/*/public_html>
  		AllowOverride FileInfo AuthConfig Limit Indexes
  		Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
  		Require method GET POST OPTIONS
  	</Directory>
  </IfModule>
  
  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```

  > * `UserDir public_html` 
  >   * 사용자들이 제공하는 웹사이트의 웹 문서들이 저장되는 디렉토리 이름을 `public_html`로 지정
  >   * 즉, 웹 문서들이 사용자마다 `~/public_html` 디렉토리에 저장되어야함
  >   * 브라우저에서는 `http://IP주소/~사용자계정/웹문서이름` 으로 접근함
  > * `UserDir disabled root` 
  >   * 사용자 디렉토리를 사용하려면 주석 처리해야하는 부분

*`userdir.conf`에 대한 심볼릭 링크 생성*

* `/etc/apache2/mods-available`: 사용 가능한 `apache2` 모듈을 지정하는 디렉토리

* `/etc/apache2/mods-enabled`: 실제 사용할 모듈들이 심볼릭 링크 형태로 존재하는 디렉토리

* `mods-available/` 안의 파일의 심볼링 링크 파일을 `mods-enabled/` 안에 두면 모듈을 사용함

  ```bash
  $ cd /etc/apache2/mods-available
  $ sudo ln -s userdir.conf ../mods-enabled/userdir.conf
  $ sudo ln -s userdir.load ../mods-enabled/userdir.load
  $ ll | grep userdir
  lrwxrwxrwx 1 root root   30 Feb 27 11:42 userdir.conf -> ../mods-available/userdir.conf
  lrwxrwxrwx 1 root root   30 Feb 27 11:44 userdir.load -> ../mods-available/userdir.load
  ```

*기본 문자 집합 지정*

* 기본적으로 사용할 기본 문자 집합을 지정하여 웹 문서의 한글이 깨지는 현상을 방지

* `/etc/apache2/conf-available/` 안의 `charset.conf` 파일에서 기본 문자 집합을 지정 

* 해당 파일에서 `AddDefaultCharset UTF-8` 이 쓰여진 문장의 주석을 해제

  ```bash
  $ cd /etc/apache2/conf-available
  $ sudo vim charset.conf
  ```

*모든 설정을 끝마치고 웹서버 재구동*

* 웹서버 재구동

  ```bash
  $ sudo systemctl restart apache2.service
  ```

<u>(2)</u> 사용자 디렉토리를 이용한 웹 문서 실행

* `helloworld` 이름의 계정을 생성 및 `/home/helloworld/public_html/test.html` 파일을 작성

  ```bash
  $ sudo adduser helloworld
  ```

  ```bash
  $ su helloworld
  Password:
  ```

  ```bash
  $ mkdir ~/public_html
  $ vim ~/public_html/test.html
  ...
  ```

  ```bash
  $ cat ~/public_html/test.html
  <html>
  	<head>
  		<title>web doc in helloworld</title>
  	</head>
  	<body>
  		<h1>Hello world in helloworld user dir</h1>
  	</body>
  </html>
  ```

* 브라우저로 위에서 만든 파일의 웹 문서 확인

  ![image](https://user-images.githubusercontent.com/87659486/155881187-a7fa33e2-8113-42ec-b581-048afa9dd36f.png)

### <u>11.2.</u> MariaDB 설치

* GPL v2 라이선스의 `MySQL`이 오라클에 인수되면서 오픈소스 라이선스를 갖는 `MariaDB`가 개발됨
* `MariaDB`는 `MySQL` 개발자들이 개발했기 때문에 기본 구조 및 사용법이 `MySQL`과 아주 유사함

##### <u>11.2.1.</u> MariaDB 설치

* `MariaDB` 설치 확인 및 설치

  ```bash
  $ sudo aptitude show mariadb-server
  Package: mariadb-server
  Version: 1:10.3.32-0ubuntu0.20.04.1
  State: not installed
  ...
  ```

  ```bash
  $ sudo apt-get install mariadb-server
  ```

  ```bash
  $ systemctl status mariadb.service
  ...
  Active: active (running) since Sun 2022-02-27 11:59:30 UTC; 26s ago
  ...
  ```

##### <u>11.2.2.</u> MariaDB 사용

*데이터베이스 로그인*

* root 계정으로 접속 (`mysql` 명령어)

  ```bash
  $ sudo mysql -u root
  Welcome to the MariaDB monitor.  Commands end with ; or \g.
  Your MariaDB connection id is 50
  Server version: 10.3.32-MariaDB-0ubuntu0.20.04.1 Ubuntu 20.04
  
  Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
  
  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  
  MariaDB [(none)]>
  ```

* 데이터베이스 리스트 출력

  ```mariadb
  MariaDB [(none)]> show db;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  +--------------------+
  3 rows in set (0.001 sec)
  ```

* 데이터베이스 선택

  ```mariadb
  MariaDB [(none)]> use mysql;
  Reading table information for completion of table and column names
  You can turn off this feature to get a quicker startup with -A
  
  Database changed
  ```

* 선택한 데이터베이스에 존재하는 테이블 리스트 출력

  ```mariadb
  MariaDB [mysql]> show tables;
  +---------------------------+
  | Tables_in_mysql           |
  +---------------------------+
  | column_stats              |
  ...
  | user                      |
  +---------------------------+
  31 rows in set (0.001 sec)
  ```

*사용자 계정 생성*

* `MariaDB`에 접속 가능한 아이디와 패스워드를 생성

  ```mariadb
  CREATE USER '계정이름'@'서버이름' identified by '패스워드'
  ```

  ```mariadb
  MariaDB [mysql]> CREATE USER tmdgns@q2452 identified BY '1234';
  Query OK, 0 rows affected (0.001 sec)
  ```

*사용자 데이터베이스 생성과 권한 지정*

* 생성된 사용자가 이용할 데이터베이스를 생성

  ```mariadb
  CREATE DATABASE 데이터베이스명
  ```

* 사용자 자신에게 주어진 데이터베이스를 쓸 수 있도록 권한을 지정

  ```mariadb
  GRANT 명령리스트 ON 데이터베이스명.테이블명 TO '계정이름'@'서버이름'
  ```

  > 명령리스트는 `,`를 이용하여 여러 명령을 지정할 수 있음

  ```mariadb
  GRANT SELECT,INSERT ON seunghun_db.seunghun_table TO seunghun@seunghun
  ```

* 모든 권한을 지정

  ```mariadb
  GRANT ALL ON 데이터베이스명.테이블명 TO '계정이름'@'서버이름'
  ```

*권한 제거 및 사용자 제거*

* 사용자 계정에 부여된 권한 제거

  ```mariadb
  REVOKE 명령리스트 ON 데이터베이스명.테이블명 FROM '계정이름'@'서버이름'
  ```

* 사용자 계정 제거

  ```mariadb
  DROP USER '계정이름'@'서버이름'
  ```

*MariaDB 서비스 재시작*

* 사용자 계정 생성 및 권한 지정 후, `MariaDB` 서비스를 재시작해야 지정한 내용들이 반영됨

  ```bash
  $ sudo mysqladmin load
  ```

<u>(Exercise)</u>

1. `MariaDB`에 아래의 사용자 계정을 생성

   * 사용자 계정 이름: `tmdgns1139`
   * 서버 이름: `localhost`
   * 패스워드: `1234`

   ```mariadb
   MariaDB [(none)]> CREATE USER 'tmdgns1139'@'localhost' IDENTIFIED BY '1234';
   Query OK, 0 rows affected (0.001 sec)
   ```

2. 생성된 계정이 사용할 데이터베이스를 생성

   ```mariadb
   MariaDB [mysql]> CREATE DATABASE tmdgns1139_db;
   Query OK, 1 row affected (0.000 sec)
   ```

3. 생성된 계정이 생성된 데이터베이스의 모든 테이블에 대한 모든 권한을 갖도록 설정하고 반영

   ```mariadb
   MariaDB [mysql]> GRANT ALL ON tmdgns1139_db.* TO 'tmdgns1139'@'localhost';
   Query OK, 0 rows affected (0.001 sec)
   ```

   ```bash
   MariaDB [mysql]> exit
   $ sudo mysqladmin reload
   Password:
   ```

4. 생성된 계정으로 `MariaDB`에 접속하고 생성했던 데이터베이스를 선택

   ```bash
   $ mysql -u tmdgns1139 -p
   Enter Password:
   Welcome to the MariaDB monitor.  Commands end with ; or \g.
   Your MariaDB connection id is 58
   Server version: 10.3.32-MariaDB-0ubuntu0.20.04.1 Ubuntu 20.04
   
   Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   MariaDB [(none)]>
   ```

   ```mariadb
   MariaDB [(none)]> SHOW DATABASES;
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | tmdgns1139_db      |
   +--------------------+
   2 rows in set (0.001 sec)
   
   MariaDB [(none)]> USE tmdgns1139_db;
   Database changed
   MariaDB [tmdgns1139_db]>
   ```

5. `MariaDB`에 root 계정으로 접속 후, `tmdgns1139` 계정에 부여된 권한을 제거

   ```bash
   $ sudo mysql -u root 
   ```

   ```mariadb
   MariaDB [(none)]> REVOKE ALL ON tmdgns1139_db.* FROM 'tmdgns1139'@'localhost';
   Query OK, 0 rows affected (0.001 sec)
   ```

6. `MariaDB`에 root 계정으로 접속 후, `tmdgns1139` 계정 삭제

   ```bash
   $ sudo mysql -u root
   ```

   ```mariadb
   MariaDB [(none)]> DROP USER 'tmdgns1139'@'localhost';
   Query OK, 0 rows affected (0.001 sec)
   ```

### <u>11.3.</u> PHP 설치

* `PHP`: `apache` 웹서버, `MariaDB` 데이터베이스와 같이 사용되는 오픈소스 인터넷 프로그래밍 언어

##### <u>11.3.1.</u> PHP 설치

<u>(1)</u> `PHP` 패키지 설치

* `PHP` 패키지 설치 확인 및 설치

  ```bash
  $ aptitude show php
  Package: php
  Version: 2:7.4+75
  State: not installed
  ...
  ```

  ```bash
  $ sudo apt-get install php
  ...
  The following NEW packages will be installed:
    libapache2-mod-php7.4 php php-common php7.4 php7.4-cli php7.4-common php7.4-json php7.4-opcache php7.4-readline
  0 upgraded, 9 newly installed, 0 to remove and 1 not upgraded.
  Need to get 4,021 kB of archives.
  After this operation, 18.0 MB of additional disk space will be used.
  Do you want to continue? [Y/n] Y
  ...
  ```

* `libapache2-mod-php7.4` 패키지 설치 확인

  ```bash
  $ aptitude show libapache2-mod-php7.4
  Package: libapache2-mod-php7.4
  Version: 7.4.3-4ubuntu2.8
  State: installed
  ...
  ```

  > * `apache2` 와 `PHP` 연결을 위한 패키지로 설치되지 않으면 `PHP`가 수행되지 않음

<u>(2)</u> 추가 모듈 설치

* `PHP` 실행을 위한 추가 모듈을 설치

  ```bash
  $ sudo apt-get install php-mbstring php-gd php-mysql
  ```

  > * `php-mbstring`: 다국어 처리 모듈
  > * `php-gd`: 이미지와 그래픽 처리
  > * `php-mysql`: `PHP`와 `MySQL`/`MariaDB` 연동 모듈

##### <u>11.3.2.</u> PHP 실행

<u>(1)</u> `PHP` 동작 확인

* 루트 디렉토리 이동 후, `test.php` 문서를 생성하고 웹 브라우저로 확인

  ```bash
  $ cd /var/www/html
  $ sudo vim test.php
  ...
  $ cat test.php
  <?php 
  	phpinfo();
  ?>
  ```

  ![image](https://user-images.githubusercontent.com/87659486/155883246-d672db97-ed4f-4fd3-964b-b11b1259ae59.png)

<u>(2)</u> 사용자 디렉토리에서 `PHP` 동작 확인

* 사용자 웹 문서 디렉토리에 `test2.php` 파일 생성 후, 웹 브라우저로 확인

  ```bash
  $ vim test2.php
  ...
  $ cat test2.php
  <?php
  	phpinfo();
  ?>
  ```

  ![image](https://user-images.githubusercontent.com/87659486/155883321-a28a7daa-6c68-4342-a0dd-efccdc0e97df.png)

  * `apache2`는 기본적으로 사용자 디렉토리에서의 `PHP` 사용을 금지함
  * `/etc/apache2/mods-available` 디렉토리의 `php7.4.conf` 파일을 수정해야함

* `PHP`사용을 위해 `php7.4.conf` 파일 수정 후, `apache2` 서비스 재시작 및 브라우저로 결과 확인

  ```bash
  $ cd /etc/apache2/mods-available
  $ sudo vim php7.4.conf
  #<IfModule mod_userdir.c>
  #    <Directory /home/*/public_html>
  #        php_admin_flag engine Off
  #    </Directory>
  #</IfModule>
  ```

  > 위 문장들을 찾아 주석처리

  ```bash
  $ sudo systemctl restart apache2.sevice
  ```

  ![image](https://user-images.githubusercontent.com/87659486/155883517-3acc03a0-d9d1-429a-b2b1-a37b0606aa2d.png)