## <u>Chapter 8.</u> 파일 시스템과 디스크 관리

* 우분투의 파일 시스템 및 파일 시스템을 마운트하는 방법 숙지
* 하드 디스크 관리 방법 및 (쿼터를 통해) 사용자별 하드 디스크 공간 제한 방법 숙지
* 파일 시스템과 하디 디스크 관리를 위한 명령어 숙지

### <u>8.1.</u> 파일 시스템 관리

##### <u>8.1.1.</u> 리눅스 파일 시스템 개요

<u>(1)</u> 리눅스 파일 시스템의 발전

* 파일 시스템(File System): 디스크에 존재하는 파일과 디스크를 관리하는 체계
* 모든 OS는 자신의 파일/디렉토리를 관리할 수 있는 파일 시스템을 가짐
* 윈도우의 파일 시스템
  * FAT(File Allocation Table) 에서 FAT32 으로 발전
* NT 기반 OS의 파일 시스템
  * NTFS(New Technology File System) 을 사용
* 리눅스 OS의 파일 시스템
  * ext, ext2, ext3, ext4 파일 시스템으로 점진적으로 발전

* `ext` 파일 시스템
  * 레미 카드(Remi Card)에 의해 개발된 리눅스 최초의 파일 시스템
  * 공식이름은 `extfs`(EXTended File System) 이지만 `ext`, `ext1` 로 부름
  * MFS(Minix File System) 을 확장한 파일 시스템
    * 기존 파일 시스템의 크기 확장: 64MB 에서 2GB 로
    * 파일 이름 길이 확장: 14byte 에서 255byte 로
  * 문제점
    * 블록과 `i-node` 트랙을 다루기 위해 연결 리스트 사용
    * 이로 인해, 파일 시스템을 사용할수록 링크 구조가 복잡해지고 파편화 발생
* `ext2` 파일 시스템
  * 레미 카드(Remi Card)가 `ext` 를 보완하여 다시 개발한 파일 시스템
  * 파일 시스템 크기를 2TB 로 확장 (기존 2GB)
  * 최근에도 휴대용 USB 장치와 같은 일부 기기에서 사용 중인 파일 시스템
  * 파편화는 여전히 해결되지 않았음
* `ext3` 파일 시스템
  * 스티븐 트위디(Stephen Tweedie)가 `ext2` 기반으로 개발한 파일 시스템
  * `ext2`에서 생성된 파일은 별도의 변경없이 `ext3`에 이식해 사용 가능
  * 저널링(journaling)을 도입한 파일 시스템
    * 시스템에 전원이 끊기더라도 이전 데이터를 복원할 수 있는 것
    * 데이터를 디스크에 저장하기 전, 저널에 주요사항 기록
    * 전원에 문제 발생 시, 저널을 참조하여 데이터를 복구함
  * 파일 시스템 크기를 32TB 까지 확장 (기존 2TB)
* `ext4` 파일 시스템
  * 2006년, 시어도어 츠오(Theodore Tso)가 발표한 파일 시스템
  * 2008년, 리눅스 커널 버전 2.6.28에 추가된 파일 시스템
  * 현재, 리눅스의 표준 파일 시스템임
  * 파일 시스템 크기를 확장(1EB) 및 최대 16TB 의 파일 지원
  * 서브 디렉토리의 개수도 6만 4천개로 증가 (기존 3만 2천개)
  * `ext3` 파일 시스템을 `ext4`로 업그레이드 가능 (`ext3`와의 호환성)
  * 파일 시스템의 파편화 문제 해결

<u>(2)</u> 리눅스가 제공하는 기타 파일 시스템

* 리눅스는 다른 OS 와 호환 및 외부 장치와의 연결을 위한 파일 시스템을 제공

  | 파일 시스템 | 설 명                                            |
  | :---------: | ------------------------------------------------ |
  |   `msdos`   | MS-DOS 파티션 사용을 위한 파일 시스템            |
  |  `iso9690`  | CD-ROM, DVD 등 읽기 전용 파일 시스템             |
  |    `ufs`    | 유닉스 표준 파일 시스템                          |
  |   `sysv`    | 유닉스 시스템V 를 지원하기 위한 파일 시스템      |
  |   `vfat`    | 윈도우 95, 98 등을 지원하기 위한 파일 시스템     |
  |   `ntfs`    | 윈도우 NTFS 를 지원하기 위한 파일 시스템         |
  |    `hfs`    | 맥의 hfs 파일 시스템을 지원하기 위한 파일 시스템 |

* 리눅스가 제공하는 주요 가상 파일 시스템
  (가상 파일 시스템: 디스크가 아닌 메모리에 생성되는 가상 파일 시스템)

  | 파일 시스템 | 설 명                                                        |
  | :---------: | ------------------------------------------------------------ |
  |   `swap`    | 스왑 영역을 관리하기 위한 파일 시스템<br />(우분투 17버전부터 스왑 파일 시스템 대신 스왑 파일 사용) |
  |   `tmpfs`   | 메모리에 임시 파일을 저장하기 위한 파일 시스템               |
  |   `proc`    | 커널의 현재 상태를 나타내기 위한 파일 시스템                 |
  |  `rootfs`   | 시스템 초기화 및 관리를 위한 파일 시스템                     |

* 시스템이 지원하는 파일 시스템 확인 (`/proc/filesystems` 파일)

  ```bash
  $ cat /proc/filesystems
  nodev   sysfs
  nodev   tmpfs
  nodev   bdev
  nodev   proc
  nodev   cgroup
  nodev   cgroup2
  nodev   cpuset
  nodev   devtmpfs
  nodev   configfs
  nodev   debugfs
  nodev   tracefs
  nodev   securityfs
  nodev   sockfs
  nodev   bpf
  nodev   pipefs
  nodev   ramfs
  nodev   hugetlbfs
  nodev   devpts
          ext3
          ext2
          ext4
          squashfs
          vfat
  nodev   ecryptfs
          fuseblk
  nodev   fuse
  nodev   fusectl
  nodev   efivarfs
  nodev   mqueue
  nodev   pstore
          btrfs
  nodev   autofs
          xfs
          jfs
          msdos
          ntfs
          minix
          hfs
          hfsplus
          qnx4
          ufs
  nodev   binfmt_misc
  ```

  > `nodev`: 디바이스가 존재하지 않음을 의미 (가상 파일 시스템을 나타냄)

##### <u>8.1.2.</u> 파일 시스템 마운트

<u>(1)</u> 장치의 인식/해제 (`mount`/`umount` 명령어)

* 리눅스 시스템에서 하드 디스크, CD-ROM, USB 등 외부 저장장치도 파일 시스템으로 인식하여 사용 가능

* 마운트(mount): 리눅스 시스템에 외부 저장장치를 인식시키고 특정 디렉토리에 연결하는 것

* 마운트 포인트(mount point): `mount` 명령어에 의해 장치가 마운트되는 리눅스 시스템의 디렉토리

* 우분투에서의 마운트 포인트: `/media/사용자계정/외부저장장치_이름` (배포판, 버전마다 다를 수 있음)

* GCP VM 인스턴스에 하드 디스크 추가 (https://kibua20.tistory.com/97)

  * 특정 인스턴스에 새 디스크 추가 
    (https://cloud.google.com/compute/docs/disks/add-persistent-disk?hl=ko#console)

    ```bash
    $ lsblk
    NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    ...
    sdb       8:16   0    10G  0 disk 
    ```

    > 방금 추가한 `sdb` 이름을 갖는 `disk` 타입의 디스크의 마운트 포인트 지정이 안된 모습

  * 추가한 디스크(HDD)를 특정 파일 시스템(ext4)으로 포맷

    ```bash
    $ sudo mkfs.ext4 -m 0 -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb
    mke2fs 1.44.1 (24-Mar-2018)
    Discarding device blocks: done                            
    Creating filesystem with 2621440 4k blocks and 655360 inodes
    Filesystem UUID: 3d9f7d93-d207-4a2e-8ca8-*
    Superblock backups stored on blocks: 
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632
    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (16384 blocks): done
    Writing superblocks and filesystem accounting information: done 
    $ ls -l /dev/sdb
    brw-rw---- 1 root disk 8, 16 Feb 21 18:14 /dev/sdb
    ```

    > * 구글 권장 flag 사용
    > * 마운트된 장치는 블록 단위 입출력을 수행하는 블록 장치임

  * 추가한 디스크를 원하는 마운트 포인트로 마운트

    ```bash
    $ sudo mkdir -p /mnt/tmdgns1139/user-information
    $ sudo mount -o discard,defaults /dev/sdb /mnt/tmdgns1139/user-information
    $ lsblk
    NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    ...
    sdb       8:16   0    10G  0 disk /mnt/tmdgns1139/user-information
    $ ls -l /mnt
    total 4
    drwxr-xr-x 3 root root 4096 Feb 21 18:15 tmdgns1139
    ```

    > 마운트 포인트의 소유 권한은 `root:root`, 허가 권한은 `755` 로 외부 사용자는 마운트 포인트 내에서 수정 불가

  * 디스크를 마운트 포인트에 권한 부여/변경

    ```bash
    $ sudo chmod 777 /mnt/tmdgns1139
    $ sudo chmod a+w /mnt/tmdgns1139
    ```

    혹은

    ```bash
    $ sudo chown -R tmdgns1139.tmdgns1139 /mnt/tmdgns1139
    ```

  * 디스크의 UUID 확인 및 `/etc/fstab` 작성

    ```bash
    $ sudo blkid /dev/sdb
    /dev/sdb: UUID="3d9f7d93-d207-4a2e-8ca8-*" TYPE="ext4"
    ```

    ```bash
    $ sudo vi /etc/fstab
    ...
    UUID=3d9f7d93-d207-4a2e-8ca8-* /mnt/disks/sdb ext4 discard,defaults,nobootwait 0 2
    ...
    ```

  * 디스크의 마운트 확인

    ```bash
    $ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    ...
    /dev/sdb        9.8G   37M  9.8G   1% /mnt/tmdgns1139/user-information
    ```

    ```bash
    $ mount
    ...
    /dev/sdb on /mnt/tmdgns1139/user-information type ext4 (rw,relatime,discard)
    ```

    ```bash
    $ sudo fdisk -l
    ...
    Disk /dev/sdb: 10 GiB, 10737418240 bytes, 20971520 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    ```

* 명령어 형식

  ```bash
  $ mount -t 파일_시스템 장치_이름 마운트_포인트
  ```

  > 옵션 없이 단독 사용: 현재 시스템에 마운트된 모든 장치의 정보 출력

  ```bash
  $ sudo mount -t iso9660 /dev/cdrom /mnt/cdrom
  ```

  > 파일 시스템
  >
  > * `ext4`: 하드 디스크
  > * `iso9660`: CD, DVD 미디어 
  > * `vfat`, `ntfs`: 윈도우 기반 파일 시스템
  >
  > 장치 이름
  >
  > * `/dev/cdrom`: CD, DVD 드라이브로 `/dev/sr0` 파일에 대한 심볼릭 링크임

* 시스템에 마운트된 장치의 정보는 `/etc/mtab` 파일에 저장됨

* 언마운트(unmount): 시스템에 마운트된 장치의 연결을 해제하는 것

* 명령어 형식

  ```bash
  $ umount 장치_이름 | 마운트_포인트
  ```

  > 위 명령어는 마운트 포인트가 아닌 다른 디렉토리에서 수행되어야함

<u>(Exercise)</u>

1. 시스템에 연결된 장치 확인

   ```bash
   $ sudo fdisk -l
   ...
   Disk /dev/sdb: 10 GiB, 10737418240 bytes, 20971520 sectors
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 4096 bytes
   ```

2. 연결된 장치를 `/mnt/tmdgns1139/user-information` 디렉토리에 마운트

   ```bash
   $ sudo mount -t ext4 /dev/sdb /mnt/tmdgns1139/user-information
   ```

3. 마운트 포인트의 내용 출력

   ```bash
   $ ls -l /mnt/tmdgns1139/user-information/
   total 16
   drwx------ 2 tmdgns1139 tmdgns1139 16384 Feb 21 18:14 lost+found
   ```

4. 연결 해제

   ```bash
   $ sudo umount /mnt/tmdgns1139/user-information
   $ ls -l /mnt/tmdgns1139/user-information/
   total 0
   ```

   ```bash
   $ sudo umount /dev/sdb
   $ ls -l /mnt/tmdgns1139/user-information/
   total 0
   ```

<u>(2)</u> 파일 시스템 자동 마운트 (`/etc/fstab`)

* 모든 장치들이 시스템에 자동으로 마운트되지 않음

  * 시스템에 새로운 하드 디스크를 추가할 때마다 마운트 해줘야함

* 시스템이 종료 시, 마운트된 장치들은 자동으로 마운트 해제됨

  * 시스템 재시작 시, 사용하고 싶은 장치들을 일일이 마운트해줘야함

* 위 불편함 해소를 위해 파일 시스템 마운트 테이블 정보를 저장하는 `/etc/fstab` 파일을 사용함

* 시스템 부팅 시, `/etc/fstab` 파일을 읽어 테이블 정보대로 장치를 마운트함

* `/etc/fstab` 파일의 구조

  | 장치 이름 | 마운트 포인트 | 파일 시스템 종류 | 옵션 | 덤프(dump) 여부 | 파일 점검 여부 |
  | :-------: | :-----------: | :--------------: | :--: | :-------------: | :------------: |

  * 장치 이름

    * 파일 시스템의 이름

    * UUID(Universally Unique Identifier): 우분투 시스템이 파티션에 부여하는 ID

    * UUID 는 시스템의 HW 정보와 시간 정보를 조합하여 임의로 생성됨

    * 장치의 UUID 는 `/dev/disk/by-uuid` 디렉토리에서 확인 가능

      ```bash
      $ ls -l /dev/disk/by-uuid/
      total 0
      lrwxrwxrwx 1 root root 11 Dec 22 04:51 *-* -> ../../sda15
      lrwxrwxrwx 1 root root  9 Feb 21 18:14 *-d207-4a2e-8ca8-* -> ../../sdb
      lrwxrwxrwx 1 root root 10 Dec 22 04:51 *-1fab-4afd-8a03-* -> ../../sda1
      ```

  * 옵션

    * 파일 시스템의 속성들 `,` 로 구분하여 지정

    * 주요 속성

      |    옵션    | 설 명                                                        |
      | :--------: | ------------------------------------------------------------ |
      | `default`  | 시스템의 기본 설정 (`rw`, `nouser`, `auto`, `exec` 속성 포함) |
      |   `auto`   | 시스템 부팅 시, 자동으로 마운트                              |
      |  `noauto`  | 시스템 부팅 시, 자동으로 마운트되지 않음                     |
      |    `ro`    | 읽기 전용으로 마운트                                         |
      |    `rw`    | 읽기/쓰기 전용으로 마운트                                    |
      |   `user`   | 일반 사용자도 마운트 가능하도록 설정                         |
      |  `nouser`  | 시스템 관리자만 마운트 가능하도록 설정                       |
      |   `exec`   | 마운트 되는 장치 내 실행파일의 실행을 허용                   |
      |  `noexec`  | 마운트 되는 장치 내 실행파일의 실행을 허용 않함              |
      | `usrquota` | 사용자 별로 디스크 쿼터 가능                                 |
      | `grpquota` | 소유그룹 별로 디스크 쿼터 가능                               |

      > * 디스크 쿼터: 사용자/소유그룹 별로 사용 가능한 디스크/파일의 용량을 제한하는 것
      > * 8.3. 절 확인

  * 덤프 여부

    * `dump` 명령어를 통해 파일 시스템의 덤프 가능 여부 지정
      * dump: 주 기억장치 혹은 저장장치의 일부를 화면이나 프린터로 출력하는 것
    * `1`: 덤프 가능 / `0`: 덤프 불가능

  * 파일 시스템 점검 여부

    * 시스템 부팅 시, 파일 시스템의 점검 여부 지정
    * `0`: 파일 시스템 점검 안함
    * `1`: `fsck` 명령을 통해 파일 시스템을 점검 (주로 루트 파일 시스템에 지정)
    * `2`: `/etc/fstab` 파일 내 순서대로 `fsck` 명령을 통해 파일 시스템 점검 (주로 루트를 제외한 파일 시스템 지정)

### <u>8.2.</u> 디스크 관리

##### <u>8.2.1.</u> 하드 디스크 추가 및 설치

* 다른 하드 디스크를 시스템에 설치하는 방법

<u>(1)</u> 하드 디스크 파티션의 개요

* 새로운 하드 디스크 설치 과정

  |        순서         | 설 명                                                        |
  | :-----------------: | ------------------------------------------------------------ |
  | 새 하드 디스크 장착 | 가상 머신 사용 시, VMWare 를 통한 하드 디스크 추가 지정<br />가상 머신 미사용 시, 하드 디스크를 물리적으로 장착 |
  | 디스크 파티션 생성  | `fdisk` 명령을 통한 파티션 생성                              |
  |  파일 시스템 생성   | `mkfs` 명령을 통한 파일 시스템 생성                          |
  | 하드 디스크 마운트  | `mount` 명령을 통한 하드 디스크 마운트                       |

* 개인 컴퓨터에는 4개의 하드 디스크 장착 가능

  |    연결 방식     | IDE 방식 | S-ATA 방식 |
  | :--------------: | :------: | :--------: |
  |  Primary master  | /dev/hda |  /dev/sda  |
  |  Primary Slave   | /dev/hdb |  /dev/sdb  |
  | Secondary master | /dev/hdc |  /dev/sdc  |
  | Secondary Slave  | /dev/hdd |  /dev/sdd  |

  > * 최근에는 거의 S-ATA 방식의 하드 디스크를 사용함
  > * IDE vs. S-ATA vs. SCSI 
  >   (https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dlrhkdals11&logNo=220605147783)
  >   (http://www.enuri.com/knowcom/detail.jsp?kbno=91617)

* 하나의 디스크를 여러 파티션으로 나누어 사용 가능

* 여러 파티션으로 나뉜 디스크의 표현

  * 순서에 따라 디스크의 이름 다음에 일련의 숫자를 추가하여 파티션을 구분함
  * `/dev/sda1`: 1번째 하드 디스크의 1번째 파티션
  * `/dev/sda2`: 1번째 하드 디스크의 2번째 파티션
  * `/dev/sda3`: 1번째 하드 디스크의 3번째 파티션

* 파티션 구분

  |       종류       | 설 명                                                        |
  | :--------------: | ------------------------------------------------------------ |
  | 네이티브(Native) | 데이터들이 저장될 수 있는 파티션<br />(주, 확장, 논리 파티션으로 구분됨) |
  |    스왑(Swap)    | 시스템의 메모리 공간 부족 시, 가상 메모리로 사용되는 파티션<br />(크기는 일반적으로 실제 메모리의 2배) |

  > 리눅스 배포판 및 버전에 따라 스왑 파티션 없이 스왑 파일을 사용하기도 함

* 네이티브 파이션 구분

  |      종류       | 설 명                                                        |
  | :-------------: | ------------------------------------------------------------ |
  |  주 (Primary)   | 데이터가 저장되는 기본 파티션<br />(디스크 1개 당, 최대 4개) |
  | 확장 (Extended) | 오직 논리 파티션을 담는 그릇 역할 <br />(디스크 1개 당, 최대 1개) |
  | 논리 (Logical)  | 확장 파티션을 여러 논리 파티션으로 나누어 데이터 저장        |

  > * 주 파티션의 개수와 확장 파티션의 개수의 합은 최대 `4`임
  > * https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=haejoon90&logNo=220749495797

  * 예. 1개의 하드 디스크에 6개의 파티션을 생성
    * 주 파티션 3개, 확장 파티션 1개, 논리 파티션 3개로 총 7개의 파티션 생성
    * 주 파티션 2개, 확장 파티션 1개, 논리 파티션 4개로 총 7개의 파티션 생성
    * 주 파티션 1개, 확장 파티션 1개, 논리 파티션 5개로 총 7개의 파티션 생성
    * 주 파티션 0개, 확장 파티션 1개, 논리 파티션 6개로 총 7개의 파티션 생성

* `fdisk`: 하드 디스크의 파티션 생성을 위한 명령어

<u>(2)</u> 하드 디스크 추가

* VMware 가상 머신에서
  * VM 재실행 및 VM 선택 후, "Edit virtual machine settings" 선택
  * 선택 후, 대화 상자의 [Hardware] 탭의 'Hard Disk(SCSI)' 선택하고 [Add...] 버튼 클릭
  * 클릭 후, 대화 상자에서 'Hard Disk' 선택하고 [Next] 버튼 클릭
  * 클릭 후, 대화 상자에서 'SCSI' 선택하고 [Next] 버튼 클릭
  * 클릭 후, 대화 상자에서 'Create a new virtual disk' 선택하고 [Next] 버튼 클릭
  * 클릭 후, 대화 상자에서 디스크 용량 선택 및 'Allocate all disk space now' 와 'Store virtual disk as a single file' 활성화하고 [Next] 버튼 클릭
  * 클릭 후, 대화 상자에서 새로 생성한 가상 디스크의 파일명 입력하고 [Finish] 버튼 클릭
  * 추가된 하드 디스크 확인

<u>(3)</u> 파티션 생성 (`fdisk` 명령어)

* 하드 디스크 장착 후, `fdisk` 명령어를 통해 파티션 생성

* 명령어 형식

  ```bash
  $ fdisk option 장치명
  ```

  > options
  >
  > * `-b 크기`: 섹터의 크기 지정 [512(기본), 1024, 2048, 4096]
  > * `-l`: 시스템의 파티션 정보를 출력
  >
  > 관리자만 수행 가능한 명령어

* 내부 명령

  ```bash
  $ sudo fdisk -m
  fdisk: invalid option -- 'm'
  Try 'fdisk --help' for more information.
  $ date
  Mon Feb 21 20:51:41 UTC 2022
  ```

  | 내부 명령 | 설 명                               |
  | :-------: | ----------------------------------- |
  |    `d`    | 파티션 제거(delete)                 |
  |    `l`    | 시스템의 파티션 정보 출력(listing)  |
  |    `m`    | 내부 명령어의 상세 내용 출력        |
  |    `n`    | 새로운 파티션 생성(new)             |
  |    `p`    | 파티션 테이블의 내용 출력(print)    |
  |    `q`    | 파티션 작업 내용 저장하지 않고 종료 |
  |    `v`    | 파티션 테이블 검사(verbose)         |
  |    `w`    | 파티션 설정 정보를 저장하고 종료    |

<u>(Exercise)</u>

1. 아래 내용대로 `/dev/sdb` 파티션 지정

   * 하드 디스크 1개를 주 파티션 2개로 나눔
   * 각 파티션은 `5GB` 용량을 가짐
   * 섹터의 크기는 `2048` 로 지정

   ```bash
   $ sudo fdisk /dev/sdb
   
   Welcome to fdisk (util-linux 2.31.1).
   Changes will remain in memory only, until you decide to write them.
   Be careful before using the write command.
   
   The old ext4 signature will be removed by a write command.
   
   Device does not contain a recognized partition table.
   Created a new DOS disklabel with disk identifier 0x445dc4c6.
   
   Command (m for help): n
   Partition type
      p   primary (0 primary, 0 extended, 4 free)
      e   extended (container for logical partitions)
   Select (default p): p
   Partition number (1-4, default 1): 1
   First sector (2048-20971519, default 2048): 2048
   Last sector, +sectors or +size{K,M,G,T,P} (2048-20971519, default 20971519): +5G
   
   Created a new partition 1 of type 'Linux' and of size 5 GiB.
   
   Command (m for help): n
   Partition type
      p   primary (1 primary, 0 extended, 3 free)
      e   extended (container for logical partitions)
   Select (default p): p
   Partition number (2-4, default 2): 2
   First sector (10487808-20971519, default 10487808):  
   Last sector, +sectors or +size{K,M,G,T,P} (10487808-20971519, default 20971519): 
   
   Created a new partition 2 of type 'Linux' and of size 5 GiB.
   
   Command (m for help):
   ```

2. 설정대로 되었는지 확인 및 파티션 정보 저장 후, `fdisk` 명령 종료

   ```bash
   Command (m for help): p
   Disk /dev/sdb: 10 GiB, 10737418240 bytes, 20971520 sectors
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 4096 bytes
   Disklabel type: dos
   Disk identifier: 0x445dc4c6
   
   Device     Boot    Start      End  Sectors Size Id Type
   /dev/sdb1           2048 10487807 10485760   5G 83 Linux
   /dev/sdb2       10487808 20971519 10483712   5G 83 Linux
   
   Command (m for help): w
   The partition table has been altered.
   Calling ioctl() to re-read partition table.
   Syncing disks.
   
   $
   ```

3. 시스템의 파티션 정보 출력

   ```bash
   $ sudo fdisk -l
   Disk /dev/sdb: 10 GiB, 10737418240 bytes, 20971520 sectors
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 4096 bytes
   Disklabel type: dos
   Disk identifier: 0x445dc4c6
   
   Device     Boot    Start      End  Sectors Size Id Type
   /dev/sdb1           2048 10487807 10485760   5G 83 Linux
   /dev/sdb2       10487808 20971519 10483712   5G 83 Linux
   ```

<u>(4)</u> 파일 시스템 생성 (`mkfs` 명령어)

* 파티션에 파일 시스템이 생성되어야 데이터를 저장할 수 있음

  * 파티션 생성: 시스템이 하드 디스크를 인식하게 하는 것
  * 파일 시스템 생성: 시스템이 하드 디스크에 파일을 저장 및 관리할 수 있는 체계를 갖추는 것

* `mkfs`(make file system) 명령어를 통해 장치(파티션)에 파일 시스템 생성

* 명령어 형식

  ```bash
  $ mkfs option 장치명
  ```

  > option
  >
  > * `-t 파일_시스템`: 파일 시스템을 지정 (기본값: ext2)

  `$ mkfs.ext4` 명령어는 `$ mkfs -t ext4` 와 동일

<u>(Exercise)</u>

1. 생성한 2개의 파티션에 `ext4` 파일 시스템 생성

   ```bash
   $ sudo mkfs -t ext4 /dev/sdb1
   mke2fs 1.44.1 (24-Mar-2018)
   Discarding device blocks: done                            
   Creating filesystem with 1310720 4k blocks and 327680 inodes
   Filesystem UUID: 1cf81ac9-254c-46c3-ab46-*
   Superblock backups stored on blocks: 
           32768, 98304, 163840, 229376, 294912, 819200, 884736
   
   Allocating group tables: done                            
   Writing inode tables: done                            
   Creating journal (16384 blocks): done
   Writing superblocks and filesystem accounting information: done 
   
   $ sudo mkfs -t ext4 /dev/sdb2
   mke2fs 1.44.1 (24-Mar-2018)
   Discarding device blocks: done                            
   Creating filesystem with 1310464 4k blocks and 327680 inodes
   Filesystem UUID: 89aaf319-4972-4ae9-9df7-*
   Superblock backups stored on blocks: 
           32768, 98304, 163840, 229376, 294912, 819200, 884736
   
   Allocating group tables: done                            
   Writing inode tables: done                            
   Creating journal (16384 blocks): done
   Writing superblocks and filesystem accounting information: done 
   ```

<u>(5)</u> 디스크 마운트 

* 파일 시스템 생성 후, 시스템에 마운트하면 장착한 하드 디스크를 사용할 수 있음

<u>(Exercise)</u>

1. 마운트 포인트로 사용할 디렉토리 생성

   ```bash
   $ sudo mkdir -p /mnt/tmdgns1139/user-information1
   $ sudo mkdir -p /mnt/tmdgns1139/user-information2
   $ l /mnt/tmdgns1139
   m_sdb1/  m_sdb2/
   ```

2. `/dev/sdb1` 은 `/mnt/tmdgns1139/m_sdb1` 로, `/dev/sdb2` 는 `/mnt/tmdgns1139/m_sdb2` 로 마운트

   ```bash
   $ sudo mount -t ext4 /dev/sdb1 /mnt/tmdgns1139/m_sdb1
   $ sudo mount -t ext4 /dev/sdb2 /mnt/tmdgns1139/m_sdb2
   ```

3. `/dev/sdb1`, `/dev/sdb2` 파티션의 마운트 정보 확인

   ```bash
   $ mount | grep sdb
   /dev/sdb1 on /mnt/tmdgns1139/m_sdb1 type ext4 (rw,relatime)
   /dev/sdb2 on /mnt/tmdgns1139/m_sdb2 type ext4 (rw,relatime)
   ```

4. `/mnt/tmdgns1139/m_sdb1`,`/mnt/tmdgns1139/m_sdb2`  디렉토리에 파일 생성

   ```bash
   $ ll /home/tmdgns1139 > ~/test1.txt
   $ sudo mv ~/test1.txt /mnt/tmdgns1139/m_sdb1/test1.txt
   $ ls -l m_sdb1/
   total 20
   drwx------ 2 root       root       16384 Feb 21 21:25 lost+found
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139  1015 Feb 21 21:39 test1.txt
   ```

   ```bash
   $ ll / > ~/test2.txt
   $ sudo mv ~/test2.txt /mnt/tmdgns1139/m_sdb2/test2.txt
   $ ls -l m_sdb2/
   total 20
   drwx------ 2 root       root       16384 Feb 21 21:25 lost+found
   -rw-rw-r-- 1 tmdgns1139 tmdgns1139  1503 Feb 21 21:39 test2.txt
   ```

5. `/etc/fstab` 파일에 마운트 정보 입력

   ```bash
   $ sudo vim /etc/fstab
   ...
   /dev/sdb1 /mnt/tmdgns1139/m_sdb1 ext4 defaults 0 2
   /dev/sdb2 /mnt/tmdgns1139/m_sdb2 ext4 defaults 0 2
   ...
   ```

##### <u>8.2.2.</u> 디스크 쿼터 지정

<u>(1)</u> 디스크 쿼터의 개요

* 웹 메일, 웹 하드 등 다수 사용자 대상 서비스는 사용자마다 일정 공간을 할당함
* 특정 사용자의 과한 디스크 사용을 방지하고 모든 사용자가 같은 크기의 공간을 사용하도록 설정 가능
* 디스크 쿼터(Disk Quota)
  * 사용자마다 일정 크기의 디스크 공간 할당
  * 생성 가능한 파일 수에 제한을 두는 것
  * 사용자나 그룹을 대상으로 지정 가능
* 디스크 쿼터를 지정하는 과정
  * 쿼터 속성 설정: 디스크 쿼터를 지정할 파일 시스템의 옵션에 쿼터 속성을 지정 (`/etc/fstab` 파일 수정)
  * 쿼터 비활성화: 쿼터가 이미 활성화된 경우, 비활성화 수행 (`quotaoff`)
  * 쿼터 데이터베이스 파일 생성 (`quotacheck`)
  * 쿼터 활성화 (`quotaon`)
  * 사용자/그룹에 쿼터 지정 (`edquota`)
* 쿼터 정보
  * 쿼터 정보 확인 (`quota`)
  * 다른 사용자에 쿼터 복사 (`edquota -p`)
  * 모든 사용자/그룹의 쿼터 정보 출력 (`requota`)

<u>(2)</u> 사용자 디스크 쿼터 설정 (`quota` 패키지)

* `quota` 패키지 설치 유무 파악 및 설치

  ```bash
  $ aptitude show quota
  Package: quota                    
  Version: 4.04-2ubuntu0.1
  State: not installed
  ...
  ```

  ```bash
  $ sudo apt-get install quota
  ```

쿼터 속성 설정

* `/etc/fstab` 파일에서 (디스크 쿼터를 사용할)파일 시스템에 쿼터 속성을 지정

* `/etc/fstab` 파일에 지정된 파일 시스템의 옵션 항목에 쿼터 속성을 추가로 지정

* 사용자에 대한 쿼터와 그룹에 대한 쿼터는 별도로 지정

* `ext2` 파일 시스템 (저널링 미지원)

  * 사용자 쿼터: `usrquota`
  * 그룹 쿼터: `grpquota`

* `ext3`, `ext4` 파일 시스템 (저널링 지원)

  * 사용자 쿼터만 지정: `usrjquota=aquota.user,jqfmt=vfsv0`
  * 그룹 쿼터만 지정: `grpjquota=aquota.group,jqfmt=vfsv0`
  * 모두 지정: `usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0`

* **예.** `/etc/fstab` 파일의 루트 파일 시스템에 사용자 쿼터만 지정

  ```bash
  UUID=*-*-*-* / ext4 erros=remount-ro usrjquota=aquota.user,jqfmt=vfsv0 0 1
  ```

  > 위의 UUID 를 갖는 파티션의 파일 시스템은 사용자에 대해 디스크 쿼터를 지정할 것이라고 선언

쿼터 비활성화 (`quotaoff`)

* 쿼터 정보 지정 혹은 기존 정보 변경 시, 쿼터 비활성화 상태여야함

* 명령어 형식

  ```bash
  $ quotaoff option
  ```

  > options
  >
  > * `-a`: `/etc/fstab` 파일에 지정된 모든 파일 시스템의 쿼터를 비활성화
  > * `-u`: 사용자 쿼터 비활성화
  > * `-g`: 그룹 쿼터 비활성화
  > * `-v`: 쿼터 비활성화 과정에서 발생하는 메세지 출력

쿼터 데이터베이스 파일 생성 (`quotacheck`)

* `/etc/fstab` 파일에 사용자 쿼터 설정 정보 추가 후, `quotacheck` 명령어를 통해 쿼터 정보를 저장하는 DB 파일을 생성

* 사용자 쿼터 DB 파일: `aquota.user`, 그룹 쿼터 DB 파일: `aquota.grp`
  (두 파일의 허가 권한 기본값은 `600(rw-------)`임)

* 명령어 형식

  ```bash
  $ quotacheck option
  ```

  > options
  >
  > * `-a`: `/etc/fstab` 파일에 지정된 모든 파일 시스템을 스캔
  > * `-u`: 사용자 쿼터 정보 확인(check)
  > * `-g`: 그룹 쿼터 정보 확인(check)
  > * `-v`: DB 파일 생성 과정에서 발생하는 메세지 출력
  > * `-m`: 파일 시스템을 다시 마운트 하지 않음

쿼터 활성화 (`quotaon`)

* 디스크 쿼터가 시스템에 적용될 수 있도록 쿼터를 활성화함

* 명령어 형식

  ```bash
  $ quotaon option 파일_시스템이름
  ```

  > options
  >
  > * `-a`: 전체 파일 시스템의 디스크 쿼터를 활성화
  > * `-u`: 사용자 쿼터를 활성화
  > * `-g`: 그룹 쿼터 활성화
  > * `-v`: 쿼터 활성화 과정에서 발생하는 메세지 출력

사용자에 쿼터 지정 (`edquota`)

* 사용자/그룹 계정에 생성한 디스크 쿼터 적용

* 명령어 형식

  ```bash
  $ edquota option 사용자/그룹_계정
  ```

  > options
  >
  > * `-u`: 사용자 쿼터 지정
  > * `-g`: 그룹 쿼터 지정
  > * `-t`: 디스크 쿼터의 유예 기간 지정
  > * `-p`: 쿼터 설정 복사

* 명령어 수행으로 파악/수정할 수 있는 정보

  | File System | blocks | soft | hard | inodes | soft | hard |
  | :---------: | :----: | :--: | :--: | :----: | :--: | ---- |

  * File System: 디스크 쿼터가 설정된 파일 시스템의 이름
  * blocks: 현재 사용자가 사용 중인 하드 디스크의 용량(KB) ***[수정 금지]***
    * soft: 지정한 크기를 초과할 경우 경고 메세지 출력
    * hard: 지정한 크기만큼의 공간만 사용 가능
  * inodes: 현재 사용자가 생성한 파일의 수 ***[수정 금지]***
    * soft: 지정한 파일 수를 초과할 경우 경고 메세지 출력
    * hard: 지정한 수만큼의 파일만 생성 가능

* 유예 기간(grace period)

  * soft blocks 이상의 디스크 공간을 쓰거나 soft inodes 이상의 파일을 생성한 시점 이후로 시스템을 사용할 수 있는 기간

  * 설정: `$ sudo edquota -t`

  * 항목

    |        구분        | 설 명                                       |
    | :----------------: | ------------------------------------------- |
    |     Filesystem     | 디스크 쿼터가 설정된 파일 시스템            |
    | Block grace period | 지정된 디스크 용량에 대한 유예 기간         |
    | Inode grade period | 지정된 생성 가능 파일의 수에 대한 유예 기간 |

쿼터 정보 확인 (`quota`)

* 현재 지정된 쿼터 정보 확인

* 명령어 형식

  ```bash
  $ quota option 사용자/그룹_계정
  ```

  > options
  >
  > * `-u`: 사용자에 지정된 쿼터 출력
  > * `-g`: 그룹에 지정된 쿼터 출력

다른 사용자에 쿼터 복사 (`edquota -p`)

* 한 사용자에게 지정한 쿼터 정보를 다른 사용자에게 복사

* **예.** `kim` 사용자 계정에 지정된 쿼터 정보를 `lee` 사용자 계정에 복사

  ```bash
  $ sudo edquota -p kim lee
  ```

* **예.** `kim` 사용자 계정에 지정된 쿼터 정보를 `lee`, `park` 사용자 계정에 복사

  ```bash
  $ sudo edquota -p kim lee park
  ```

모든 사용자의 쿼터 정보 출력 (`repquota`)

* 사용자/그룹에 지정된 쿼터의 내용 출력

* 명령어 형식

  ```bash
  $ repquota option 사용자/그룹_계정
  ```

  > options
  >
  > * `-a`: 모든 사용자에 지정된 쿼터 출력
  > * `-u`: 사용자에 지정된 쿼터 출력
  > * `-g`: 그룹에 지정된 쿼터 출력
  > * `-v`: 사용량이 없는 쿼터의 정보도 출력

<u>(Exercise)</u>

1. `/dev/sdb`의 파티션에 사용자 쿼터를 지정하기 위한 설정 정보 `/etc/fstab` 파일에 입력

   ```bash
   $ sudo vim /etc/fstab
   ...
   UUID=*-*-*-*-*   /mnt/tmdgns1139/user-information    ext4 discard,defaults,nobootwait,usrjquota=aquota.user,jqfmt=vfsv0   0   2
   ...
   ```

   ```bash
   $ mount | grep sdb
   /dev/sdb on /mnt/tmdgns1139/user-information type ext4 (rw,relatime,discard,jqfmt=vfsv0,usrjquota=aquota.user)
   ```

2. 쿼터 비활성화

   ```bash
   $ sudo quotaoff -augv
   ```

3. 편집기 없이 `aquota.user` 데이터베이스 파일 생성 후 생성 파일 확인

   ```bash
   $ sudo quotacheck -augvm 
   quotacheck: Scanning /dev/sdb [/mnt/tmdgns1139/user-information] done
   quotacheck: Cannot stat old user quota file /mnt/tmdgns1139/user-information/aquota.user: No such file or directory. Usage will 
   not be subtracted.
   quotacheck: Old group file name could not been determined. Usage will not be subtracted.
   quotacheck: Checked 3 directories and 0 files
   quotacheck: Old file not found.
   ```

   ```bash
   $ ls -l /mnt/tmdgns1139/user-information/
   total 24
   -rw------- 1 root       root        6144 Feb 22 00:39 aquota.user
   drwx------ 2 tmdgns1139 tmdgns1139 16384 Feb 21 21:51 lost+found
   ```

4. 디스크 쿼터 활성화

   ```bash
   $ sudo quotaon -uv /mnt/tmdgns1139/user-information
   /dev/sdb [/mnt/tmdgns1139/user-information]: user quotas turned on
   ```

5. 사용자 쿼터 지정을 위해 `user-01`, `user-02` 사용자 계정 생성

   ```bash
   $ sudo adduser user-01
   $ sudo adduser user-02
   ```

6. `user-01` 사용자에 대해 아래와 같이 쿼터 지정

   * 디스크 사용 용량 제한: `100MB` / 최대 디스크 사용 용량: `110MB`
   * 생성 가능 파일 개수: `100개` / 최대 생성 가능 파일 개수: `110개`

   ```bash
   $ sudo edquota -u user-01
   Disk quotas for user user-01 (uid 1005):
     Filesystem                   blocks       soft       hard     inodes     soft     hard
     /dev/sdb                          0     102400     112640          0      100      100
   ```

7. 시스템의 유예 기간 확인

   ```bash
   $ sudo edquota -t
   Grace period before enforcing soft limits for users:
   Time units may be: days, hours, minutes, or seconds
     Filesystem             Block grace period     Inode grace period
     /dev/sdb                      7days                  7days
   ```

8. `user-01`, `user-02` 사용자의 쿼터 정보 확인

   ```bash
   $ sudo quota -u user-01
   Disk quotas for user user-01 (uid 1005): 
        Filesystem  blocks   quota   limit   grace   files   quota   limit   grace
          /dev/sdb       4  102400  112640               1     100     100
   ```

   ```bash
   $ sudo quota -u user-02
   Disk quotas for user user-02 (uid 1006): none
   ```

9. `user-01` 계정의 쿼터 정보를 `user-02` 계정에 복사 후 결과 확인

   ```bash
   $ sudo edquota -p user-01 user-02
   ```

   ```bash
   $ sudo quota -u user-02
   Disk quotas for user user-02 (uid 1006): none
   ```

   ```bash
   $ sudo edquota -u user-02
   Disk quotas for user user-02 (uid 1006): 
        Filesystem  blocks   quota   limit   grace   files   quota   limit   grace
          /dev/sdb       4  102400  112640               1     100     100 
   ```

10. `/dev/sdb` 에 적용된 사용자의 모든 쿼터 정보 출력

    ```bash
    $ sudo repquota /dev/sdb
    *** Report for user quotas on device /dev/sdb
    Block grace time: 7days; Inode grace time: 7days
                            Block limits                File limits
    User            used    soft    hard  grace    used  soft  hard  grace
    ----------------------------------------------------------------------
    tmdgns1139 --      24       0       0              3     0     0       
    user-01   --       4  102400  112640              1   100   100       
    user-02   --       4  102400  112640              1   100   100 
    ```

<u>(3)</u> 그룹 디스크 쿼터 설정

* 사용자 디스크 쿼터 지정과 유사

<u>(Exercise)</u>

1. 그룹 쿼터 지정을 위한 디스크 쿼터 비활성화

   ```bash
   $ sudo quotaoff -augv
   ```

2. `/dev/sdb` 파티션에 그룹 쿼터 지정을 위한 설정 정보를 `/etc/fstab` 에 입력

   ```bash
   $ sudo vim /etc/fstab
   ...
   UUID=*-*-*-*-*   /mnt/tmdgns1139/user-information    ext4 discard,defaults,nobootwait,usrjquota=aquota.user,jqfmt=vfsv0,grpjquota=aquota.group   0   2
   ...
   ```

3. `/deb/sdb` 파티션 리마운트

   ```bash
   $ sudo umount /mnt/tmdgns1139/user-information
   $ sudo mount -o discard,defaults,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0
    /dev/sdb /mnt/tmdgns1139/user-information
   ```

   ```bash
   $ mount | grep sdb
   /dev/sdb on /mnt/tmdgns1139/user-information type ext4 (rw,relatime,discard,jqfmt=vfsv0,usrjquota=aquota.user,grpjquota=aquota.g
   roup)
   ```

4. 편집기 없이 그룹 쿼터 정보 DB 파일(`aquota.grp`) 생성 후 파일 확인

   ```bash
   $ sudo quotacheck -augvm 
   quotacheck: Scanning /dev/sdb [/mnt/tmdgns1139/user-information] done
   quotacheck: Cannot stat old group quota file /mnt/tmdgns1139/user-information/aquota.group: No such file or directory. Usage wil
   l not be subtracted.
   quotacheck: Cannot stat old group quota file /mnt/tmdgns1139/user-information/aquota.group: No such file or directory. Usage wil
   l not be subtracted.
   quotacheck: Checked 3 directories and 4 files
   quotacheck: Old file not found.
   ```

   ```bash
   $ ls -l aquota.*
   -rw------- 1 root root 6144 Feb 22 01:33 aquota.group
   -rw------- 1 root root 6144 Feb 22 01:33 aquota.user
   ```

5. 그룹 쿼터 활성화

   ```bash
   $ sudo quotaon -gv /dev/sdb
   /dev/sdb [/mnt/tmdgns1139/user-information]: group quotas turned on
   ```

6. `member-01` 그룹 생성

   ```bash
   $ sudo addgroup member-01
   Adding group `member-01' (GID 1005) ...
   Done.
   ```

7. `member-01` 그룹에 아래와 같이 쿼터 지정

   * 디스크 사용 용량: `300MB` / 최대 디스크 사용 용량: `310MB`
   * 생성 가능 파일 개수: `1000개` / 최대 생성 가능 파일 개수: `1100개`

   ```bash
   $ sudo edquota -g member-01
   ```

8. 디스크에 `member-01` 그룹의 파일을 생성한 후, 그룹의 쿼터 정보 확인

   ```bash
   $ sudo chown .member-01 user-01_note.txt user-02_note.txt
   $ ls -l user*.txt
   -rw-rw-r-- 1 user-01 member-01 44 Feb 22 01:13 user-01_note.txt
   -rw-rw-r-- 1 user-02 member-01 44 Feb 22 01:13 user-02_note.txt
   ```

   ```bash
   $ sudo quota -g member-01
   Disk quotas for group member-01 (gid 1005): 
        Filesystem  blocks   quota   limit   grace   files   quota   limit   grace
          /dev/sdb       8     300     310               2    1000    1100
   ```

9. 디스크에 적용된 모든 그룹의 쿼터 정보 출력

   ```bash
   $ sudo repquota -g /dev/sdb
   *** Report for group quotas on device /dev/sdb
   Block grace time: 7days; Inode grace time: 7days
                           Block limits                File limits
   Group           used    soft    hard  grace    used  soft  hard  grace
   ----------------------------------------------------------------------
   tmdgns1139 --      24       0       0              3     0     0     
   member-01 --       8     300     310              2  1000  1100 
   ```

### <u>8.3.</u> 기타 디스크 관리 명령어

##### <u>8.3.1.</u> 디스크 용량 확인

<u>(1)</u> 파일 시스템의 공간 정보 확인 (`df` 명령어)

* `df`: disk free 의 약자로 사용 가능한 하드 디스크의 용량 출력이 주 목적

* 파일 시스템의 사용 현황 및 가용 현황 정보 출력 (출력 용량 기본 단위: `KB`)

* 명령어 형식

  ```bash
  $ df [option] [파일_시스템]
  ```

  > options
  >
  > * `-k`: 용량을 KB 단위로 출력
  > * `-m`: 용량을 MB 단위로 출력
  > * `-h`: KB, MB, GB 적절하게 사용하여 용량을 보기 쉬운 단위로 출력
  > * `-T`: 파일 시스템의 종류를 함께 출력
  > * `-t 파일_시스템`: 지정된 파일 시스템의 용량 출력

<u>(2)</u> 디렉토리 공간 정보 확인 (`du` 명령어)

* `du`: disk usage 의 약자로 현재 하드 디스크의 사용 공간을 출력이 주 목적

* 특정 디렉토리가 얼마의 공간을 차지하는지 확인할 때 사용 (출력 용량 기본 단위: `KB`)

* 명령어 형식

  ```bash
  $ du [opiton] [디렉토리]
  ```

  > options
  >
  > * `-a 디렉토리`: 디렉토리와 그 하위 디렉토리의 사용 용량 정보 출력
  > * `-s`: 디렉토리의 내용 출력 없이 디렉토리 전체 용량만을 출력
  > * `-k`: 용량을 KB 단위로 출력
  > * `-m`: 용량을 MB 단위로 출력
  > * `-h`: KB, MB, GB 적절하게 사용하여 용량을 보기 쉬운 단위로 출력

<u>(Exercise)</u>

1. 모든 파일 시스템에 대한 사용 및 가용 공간 정보 출력

   ```bash
   $ df
   ```

2. 모든 파일 시스템에 대한 사용 및 가용 공간 정보 출력 (보기 쉬운 용량 단위)

   ```bash
   $ df -h
   ```

3. 파일 시스템의 종류가 `ext4`인 파티션의 사용 및 가용 공간 정보 출력 (보기 쉬운 용량 단위)

   ```bash
   $ df -h -t ext4
   Filesystem      Size  Used Avail Use% Mounted on
   /dev/sda1       9.6G  8.7G  905M  91% /
   /dev/sdb        9.8G   37M  9.8G   1% /mnt/tmdgns1139/user-information
   ```

4. 현재 디렉토리의 용량을 KB 단위로 출력

   ```bash
   $ du
   48      .
   ```

5. `/mnt/tmdgns1139/user-information` 디렉토리의 용량 출력 (보기 쉬운 용량 단위)

   ```bash
   $ du -h -s /mnt/tmdgns1139/user-information
   52K     /mnt/tmdgns1139/user-information
   ```

##### <u>8.3.2.</u> 파일 시스템 검사

<u>(1)</u> 시스템 부팅 정보 출력 (`dmesg`)

* 시스템 부팅 시, 리눅스는 시스템에 장착된 각종 장치 인식, 오류 검사, 파일 시스템 마운트 등 많은 작업을 수행
* 시스템이 위 작업 과정을 메세지로 출력하고 문제가 없으면 `OK`를 출력하고 문제가 있으면 `FAIL`을 출력
  (부팅 시, 메세지를 출력하려면 `/etc/default/grub` 파일을 수정하고 `$ sudo update-grub` 실행)
* 가장 최근 부팅 과정에서 발생한 메세지를 `/var/log/boot.log` 파일에 저장함
* `dmesg`: `/var/log/boot.log` 파일의 내용을 출력하는 명령어

<u>(2)</u> 파일 시스템 검사를 통한 파일 시스템 복구 (`fsck`, `e2fsck` 명령어)

* 파일 시스템에 문제 발생 시, 파일 손상을 수반하는 경우가 많음

* 심각한 파일 손상으로 파일 시스템 접근조차 불가능한 경우도 있음

* 부적절한 시스템 종료, 전원 차단, HW/SW 오류 등 다양한 원인으로 파일 시스템의 손상이 발생함

* 이러한 문제에 대비하여 주기적으로 파일 시스템을 검사하여 문제 발생 시, 시스템 복구를 수행

* `fsck`, `e2fsck` 명령어를 사용하여 파일 시스템 점검 시, 해당 파일 시스템은 언마운트 상태여야함

  ```bash
  $ sudo e2fsck /dev/sdb
  e2fsck 1.44.1 (24-Mar-2018)
  /dev/sdb is mounted.
  e2fsck: Cannot continue, aborting.
  ```

`fsck` 명령어와 `e2fsck` 명령어

* `fsck`: file system check 의 약자로 `/etc/fstab` 에 적힌 모든 파일 시스템을 검사하는 명령어

* 손상된 디렉토리/파일 수정 시, 임시로 `/lost+found` 디렉토리에서 작업을 수행함

* 명령어 형식

  ```bash
  $ fsck option 디바이스_이름
  ```

  > options
  >
  > * `-a`: 파일 시스템 검사 도중에 문제 발견 시, 복구도 진행
  > * `-y`: 모든 질문에 yes로 응답
  > * `-c`: 배드 블록 검사 (배드 블록 발견 시, 배드 블록 리스트에 추가)
  > * `-f`: 파일 시스템 강제 검사
  > * `-v`: 파일 시스템 검사 과정 출력

* `e2fsck`: `fsck` 명령어를 확장한 명령어로 ext2, ext3, ext4 파일 시스템을 검사함

* `i-node`, 블록의 크기, 디렉토리 구조와 연결성, 파일의 링크, 전체 블록 중 사용 중인 블록 등을 점검함

* 명령어 형식

  ```bash
  $ e2fsck option 디바이스_이름
  ```

  > option
  >
  > * `-p`: 파일 시스템을 검사 도중에 문제 발견 시, 복구도 진행
  > * `-y`: 모든 질문에 yes로 응답
  > * `-c`: 배드 블록 검사 (배드 블록 발견 시, 배드 블록 리스트에 추가)
  > * `-j ext3 | ext4`: ext3/ext4 저널링 파일 시스템을 지정
  > * `-f`: 파일 시스템 강제 검사
  > * `-v`: 파일 시스템 검사 과정 출력

`badblocks` 명령어

* 배드 블록(bad block): 디스크에서 발생하는 심각한 오류 중 하나 (데이터 유실 가능성이 매우 높은 오류)

* 배드 블록에 대비한 주기적인 백업과 파일 시스템 검사는 필수임

* `fsck`, `e2fsck`에 비해 배드 블록 검사 결과를 자세히 출력

* 배드 블록 발생 시, 블록의 목록을 파일로 출력할 수 있게 해줌

* 명령어 형식

  ```bash
  $ badblocks [option] 디바이스_이름
  ```

  > options
  >
  > * `-o 파일명`: 검출된 배드 블록의 번호를 지정한 파일에 저장
  > * `-v`: 배드 블록 검사 과정 출력

<u>(Exercise)</u>

1. `/dev/sdb` 파일 시스템 언마운트

   ```bash
   $ sudo e2fsck /dev/sdb
   e2fsck 1.44.1 (24-Mar-2018)
   /dev/sdb is mounted.
   e2fsck: Cannot continue, aborting.
   $ sudo umount /dev/sdb
   $ ls -l /mnt/tmdgns1139/user-information/
   total 0
   ```

2. `/dev/sdb` 파일 시스템 점검

   ```bash
   $ sudo e2fsck /dev/sdb
   e2fsck 1.44.1 (24-Mar-2018)
   /dev/sdb: clean, 17/655360 files, 66761/2621440 blocks
   ```

3. `/dev/sdb` 파일 시스템 강제 점검

   ```bash
   $ sudo e2fsck -f /dev/sdb
   e2fsck 1.44.1 (24-Mar-2018)
   Pass 1: Checking inodes, blocks, and sizes
   Pass 2: Checking directory structure
   Pass 3: Checking directory connectivity
   Pass 4: Checking reference counts
   Pass 5: Checking group summary information
   /dev/sdb: 17/655360 files (0.0% non-contiguous), 66761/2621440 blocks
   ```

4. `/dev/sdb` 파일 시스템에 배드 블록이 있는지 점검

   ```bash
   $ sudo e2fsck -c /dev/sdb
   e2fsck 1.44.1 (24-Mar-2018)
   Checking for bad blocks (read-only test): done                                                 
   /dev/sdb: Updating bad block inode.
   Pass 1: Checking inodes, blocks, and sizes
   Pass 2: Checking directory structure
   Pass 3: Checking directory connectivity
   Pass 4: Checking reference counts
   Pass 5: Checking group summary information
   
   /dev/sdb: ***** FILE SYSTEM WAS MODIFIED *****
   /dev/sdb: 17/655360 files (0.0% non-contiguous), 66761/2621440 blocks
   ```

   ```bash
   $ sudo badblocks -v /dev/sdb
   Checking blocks 0 to 10485759
   Checking for bad blocks (read-only test): done                                      
   Pass completed, 0 bad blocks found. (0/0/0 errors)
   ```

5. `/dev/sdb` 파일 시스템에 배드 블록이 있는지 점검하고 배드 블록 번호를 `badblocklist` 파일에 저장

   ```bash
   $ sudo badblocks -v -o badblocklist /dev/sdb
   Checking blocks 0 to 10485759
   Checking for bad blocks (read-only test): done
   Pass completed, 0 bad blocks found. (0/0/0 errors)
   $ cat badblocklist
   $
   ```

<u>(3)</u> 백업 슈퍼블록을 이용한 파일 시스템 복구 (`dumpe2fs`, `dd` 명령어)

기본 슈퍼블록과 백업 슈퍼블록 (`dumpe2fs`)

* 기본 슈퍼블록(Primary super block)

  * 리눅스의 파일 시스템에 대한 전체적인 정보를 갖는 블록
  * 기본 슈퍼블록이 손상되면 파일 시스템 사용 불가

* 백업 슈퍼블록

  * 기본 슈퍼블록의 손상을 대비하여 백업해둔 블록
  * 백업 슈퍼블록을 사용하려면 해당 블록의 번호를 알아야함

* `mke2fs` 명령어: 파일 시스템을 생성하면서 백업 슈퍼블록도 생성하는 명령어

* `dumpe2fs` 명령어: 백업 슈퍼블록의 번호를 확인하는 명령어 (파일 시스템의 정보를 출력함)

  ```bash
  $ sudo dumpe2fs /dev/sdb | grep superblock
  dumpe2fs 1.44.1 (24-Mar-2018)
    Primary superblock at 0, Group descriptors at 1-2
    Backup superblock at 32768, Group descriptors at 32769-32770
    ...
    Backup superblock at 1605632, Group descriptors at 1605633-1605634
  ```

  > 백업 슈퍼블록의 번호는 파일 시스템을 구성하는 정보임

* 파일 시스템을 복구

  ```bash
  $ sudo e2fsck -b 백업_슈퍼블록_위치 디바이스명
  ```

  > 파일 시스템을 복구하면서 기본 슈퍼블록을 새롭게 갱신

`dd` 명령어

* data duplicator 의 약자로 데이터를 복사하는데 주로 사용되는 명령어임

* 명령어 형식

  ```bash
  $ dd if=파일명 of=파일명 bs=바이트_수 [count=블록_수]
  ```

  > 인자
  >
  > * `if=파일명`: 복사할 파일명 지정
  > * `of=파일명`: 읽은 파일의 내용을 출력할 파일명 지정
  > * `bs=바이트_수`: 1번에 읽어 들일 바이트의 수 지정 (1번에 1블록을 읽어들임)
  > * `count=블록_수`: 복사할 블록의 개수를 지정

* **예.** 기본 슈퍼블록 제거

  ```bash
  $ dd if=/dev/zero of=/dev/sdb bs=4k count=8
  ```

  > * `/dev/zero`: `null`문자를 제공하는 특수 디바이스 파일
  > * `/dev/sdb` 의 8개 블록 전체에 `0`이 저장되면서 기본 슈퍼 블록이 제거됨

* **예.** 장치 내용 전체 복사

  ```bash
  $ dd if=/dev/sdb1 of=/dev/sdb2 bs=1024k
  ```

  > * `/dev/sdb1` 파일 시스템의 모든 내용을 `/dev/sdb2` 파일 시스템에 덮어씌움
  > * 두 파일 시스템의 크기는 동일해야하고 `/dev/sdb2` 파일 시스템은 언마운트 상태여야함

<u>(Exercise)</u>

1. `/dev/sdb` 파일 시스템의 백업 슈퍼블록의 번호 출력

   ```bash
   $ sudo dumpe2fs /dev/sdb | grep superblock
   dumpe2fs 1.44.1 (24-Mar-2018)
     Primary superblock at 0, Group descriptors at 1-2
     Backup superblock at 32768, Group descriptors at 32769-32770
     ...
     Backup superblock at 1605632, Group descriptors at 1605633-1605634
   ```

2. `/mnt/tmdgns1139/user-information` 에 마운트 되어 있는 `/dev/sdb`의 내용을 출력

   ```bash
   $ ls -l /mnt/tmdgns1139/user-information
   total 48
   -rw------- 1 root       root        6144 Feb 22 05:11 aquota.group
   -rw------- 1 root       root        6144 Feb 22 05:11 aquota.user
   ...
   -rw-rw-r-- 1 user-02    member-01     44 Feb 22 01:13 user-02_note.txt
   ```

3. `/dev/sdb` 파일 시스템의 기본 슈퍼블록을 인위적으로 제거 (기본 슈퍼블록에 `0`값을 덮어씀)

   ```bash
   $ sudo dd if=/dev/zero of=/dev/sdb bs=4096 count=8
   8+0 records in
   8+0 records out
   32768 bytes (33 kB, 32 KiB) copied, 0.000107821 s, 304 MB/s
   ```

   > 기본 슈퍼블록의 크기(4096KB)를 갖는 블록 8개에 `0`값을 입력하여 기본 슈퍼블록 제거

4. `/mnt/tmdgns1139/user-information` 에 마운트 되어 있는 `/dev/sdb`의 내용을 출력

   ```bash
   $ ls -l /mnt/tmdgns1139/user-information
   total 0
   ```

   > 기본 슈퍼블록 제거로 인해 파일 시스템에 접근 불가 상태임

5. `/dev/sdb` 파일 시스템을 언마운트/마운트를 수행

   ```bash
   $ sudo umount /dev/sdb
   $ mount | grep sdb
   $
   ```

   ```bash
   $ sudo mount -o discard,defaults,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 /dev/sdb /mnt/tmdgns1139/user-information
   NTFS signature is missing.
   Failed to mount '/dev/sdb': Invalid argument
   The device '/dev/sdb' doesn't seem to have a valid NTFS.
   Maybe the wrong device is used? Or the whole disk instead of a
   partition (e.g. /dev/sda, not /dev/sda1)? Or the other way around?
   ```

   ```bash
   $ sudo mount -t ext4 /dev/sdb /mnt/tmdgns1139/user-information/ 
   mount: /mnt/tmdgns1139/user-information: wrong fs type, bad option, bad superblock on /dev/sdb, missing codepage or helper program, or other error.
   ```

6. 첫번째 백업 슈퍼블록을 사용해 `/dev/sdb` 파일 시스템 복구

   ```bash
   $ sudo e2fsck -b 32768 -y /dev/sdb
   e2fsck 1.44.1 (24-Mar-2018)
   /dev/sdb was not cleanly unmounted, check forced.
   Resize inode not valid.  Recreate? yes
   
   Pass 1: Checking inodes, blocks, and sizes
   Pass 2: Checking directory structure
   Pass 3: Checking directory connectivity
   Pass 4: Checking reference counts
   Pass 5: Checking group summary information
   Block bitmap differences:  +(98304--99330) +(163840--164866) +(229376--230402) +(294912--295938) +(819200--820226) +(884736--885762) +
   (1605632--1606658)
   Fix? yes
   
   Free blocks count wrong for group #0 (23510, counted=23511).
   Fix? yes
   
   Free blocks count wrong for group #1 (31741, counted=31733).
   Fix? yes
   
   Free blocks count wrong (2554686, counted=2554679).
   Fix? yes
   
   Free inodes count wrong for group #0 (8181, counted=8175).
   Fix? yes
   
   Free inodes count wrong (655349, counted=655343).
   Fix? yes
   
   Inode bitmap differences: Group 0 inode bitmap does not match checksum.
   FIXED.
   
   /dev/sdb: ***** FILE SYSTEM WAS MODIFIED *****
   /dev/sdb: 17/655360 files (0.0% non-contiguous), 66761/2621440 blocks
   ```

7. `/dev/sdb` 파일 시스템을 마운트 포인트에 마운트

   ```bash
   $ sudo mount -t ext4 /dev/sdb /mnt/tmdgns1139/user-information
   ```

   혹은

   ```bash
   $ sudo mount -o discard,defaults /dev/sdb /mnt/tmdgns1139/user-informati
   on
   ```

8. 마운트 포인트 디렉토리의 내용 출력

   ```bash
   $ ls -l /mnt/tmdgns1139/user-information
   total 48
   -rw------- 1 root       root        6144 Feb 22 05:11 aquota.group
   -rw------- 1 root       root        6144 Feb 22 05:11 aquota.user
   ...
   -rw-rw-r-- 1 user-02    member-01     44 Feb 22 01:13 user-02_note.txt
   ```

