# Ubuntu 기본 명령어 (http://yahweh0.blog.me/)
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

# Ubuntu apt-get 명령어 (https://blog.outsider.ne.kr/346)
* apt를 이용해서 설치된 deb패키지는 /var/cache/apt/archive/ 에 설치된다.

* 설치된 패키지 정보 확인
```
~$ dpkg -l
~$ dpkg -l | grep PACKAGESUBNAME
```

* 패키지 인덱스 정보 업데이트: apt-get은 인덱스를 가지고 있는데 이 인덱스는 /etc/apt/sources.list에 있다. 이곳에 저장된 저장소에서 사용할 패키지의 정보를 얻는다.
```
~$ sudo apt-get update
```

* 설치된 패키지 업그레이드: 설치되어 있는 패키지를 모두 새버전으로 업그레이드 한다.
```
~$ sudo apt-get upgrade
~$ sudo apt-get dist-upgrage (의존성 검사하며 설치하기)
```

* 패키지 설치
```
~$ sudo apt-get install PACKAGENAME
```

* 패키지 재설치
```
~$ sudo apt-get --reinstall install PACKAGENAME
```

* 패키지 삭제
```
~$ sudo apt-get remove PACKAGENAME
~$ sudo apt-get --purge remove PACKGENAME (설정파일까지 모두 지움)
```

* 패키지 소스코드 다운로드
```
~$ sudo apt-get source PACKAGENAME
~$ sudo apt-get build-dep PACKGENAME (다운로드한 소스코드를 의존성에 맞춰(?) 빌드)
```

* 패키지 검색
```
~$ sudo apt-cache search PACKAGENAME
```

* 패키지 정보 보기
```
~$ sudo apt-cache show PACKAGENAME
```
<br/><br/><br/>

# .deb 파일 설치하기
* .deb 파일이 있는 디렉토리로 이동하여 아래 명령어 실행
```
~$ sudo dpkg -i FILENAME.deb
```
* 파일명이 복잡할 경우에는 파일명을 대충 입력하고 Tab키를 누르면 자동완성기능을 활용할 수 있다.