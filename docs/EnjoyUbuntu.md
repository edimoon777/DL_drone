# Ubuntu 기본 명령어 [[참고](http://yahweh0.blog.me/)]
* `ls` 디렉토리 검색
* `cd DIRECTORYNAME` 디렉토리 내로 진입
* `./FILENAME` 파일 실행
* `su` root 권한
* `sudo COMMAND` 부분 root 권한

* `pwd` 현재 작업하고 있는 디렉토리의 전체 경로
* `mkdir` 디렉토리 생성 명령어
* `rmdir` 디렉토리 삭제 명령어
* `cp` 파일 복사하기
* `rm` 파일 지우기
* `chmod` 파일 권한 바꾸기
* `cmp` 파일 비교하기
<br/><br/><br/>

# Ubuntu apt-get 명령어 [[참고](https://blog.outsider.ne.kr/346)]
* apt를 이용해서 설치된 deb패키지는 /var/cache/apt/archive/ 에 설치된다.

* 설치된 패키지 정보 확인
  ```
  dpkg -l
  dpkg -l | grep PACKAGESUBNAME
  ```

* 패키지 인덱스 정보 업데이트: apt-get은 인덱스를 가지고 있는데 이 인덱스는 /etc/apt/sources.list에 있다. 이곳에 저장된 저장소에서 사용할 패키지의 정보를 얻는다.
  ```
  sudo apt-get update
  ```

* 설치된 패키지 업그레이드: 설치되어 있는 패키지를 모두 새버전으로 업그레이드 한다.
  ```
  sudo apt-get upgrade
  sudo apt-get dist-upgrage (의존성 검사하며 설치하기)
  ```

* 패키지 설치
  ```
  sudo apt-get install PACKAGENAME
  ```

* 패키지 재설치
  ```
  sudo apt-get --reinstall install PACKAGENAME
  ```

* 패키지 삭제 (설정파일 유지)
  ```
  sudo apt-get remove PACKAGENAME
  ```

* 패키지 완전 삭제 
  ```
  sudo apt-get purge PACKGENAME (설정파일까지 모두 지움)
  ```

* 패키지 소스코드 다운로드
  ```
  sudo apt-get source PACKAGENAME
  sudo apt-get build-dep PACKGENAME (다운로드한 소스코드를 의존성에 맞춰(?) 빌드)
  ```

* 패키지 검색 (PACKAGENAME를 포함한 패키지)
  ```
  sudo apt-cache search PACKAGENAME
  ```

* 패키지 정보 보기
  ```
  sudo apt-cache show PACKAGENAME
  ```
<br/><br/><br/>

# .deb 파일 설치하기
* .deb 파일이 있는 디렉토리로 이동하여 아래 명령어 실행
  ```
  sudo dpkg -i FILENAME.deb
  ```
* 파일명이 복잡할 경우에는 파일명을 대충 입력하고 Tab키를 누르면 자동완성기능을 활용할 수 있다.
<br/><br/><br/>

# virtualenv 설치하기 [[참고](https://gist.github.com/Geoyi/d9fab4f609e9f75941946be45000632b)]
* pip 설치
  ```
  sudo apt-get install python-pip
  sudo apt-get install python3-pip
  ```

* pip를 통해 virtualenv 설치하기
  ```
  sudo pip intstall virtualenv
  sudo pip3 intstall virtualenv
  ```

*  가상환경(virtual environment) 생성하기
  ```
  virtualenv ENVNAME
  virtualenv -p /usr/bin/python3 ENVNAME
  ```

* 가상환경 활성화
  ```
  source ENVNAME/bin/activate
  ```

* 가상환경 비활성화
  ```
  deactivate
  ```
<br/><br/><br/>

# 하드 자동으로 마운트시키기
* 하드드라이브 설치 후 확인
  ```
  sudo fdisk -l
  ```
* uuid 확인
  ```
  sudo blkid
  ```
* 마운트할 디렉토리 생성
  ```
  sudo mkdir /home/USER/MNTNAME
  ```
* fstab 파일에 파티션 추가
  ```
  sudo gedit /etc/fstab
  UUID=.... /home/USER/MNTNAME ext4 defaults 0 0
  ```
* 마운트 확인
  ```
  sudo mount -a
  df -h
  ```