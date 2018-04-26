# Ubuntu 16.04.04 LTS 설치
* 기존 윈도우 시스템의 안정적인 운용을 위해 별도의 하드에 우분투를 설치하는 것이 안정적인 운용이 가능하다.
* 데스크탑용 우분투를 다운로드한다.<br />https://www.ubuntu.com/download/desktop
* 우분투 설치를 위해 USB를 부팅가능하도록 만든다.<br />https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows?_ga=2.252068618.2004108082.1514558137-364398367.1514558137#0
* 설치할 때 'Swap' 영역은 16GB, '/' 영역은 20GB, '/home' 영역은 HDD의 나머지를 할당한다.
  * MATLAB과 같이 무거운 프로그램을 설치할 예정이라면 '/'영역의 크기를 여유롭게 할당해야 함.
  * 이때 '/boot' 파티션을 할당하게 되면 컴퓨터 부팅 시에 부트로더가 실행되어 기존의 OS와 Ubuntu를 선택하는 화면이 나오게 할 수 있고, 그렇지 않으면 부팅 시에 'F12(메인보드 제조사마다 다름)' 등을 눌러 부팅장치 우선순위를 변경해서 부팅할 OS를 선택할 수 있도록 할 수 있다. 필자는 후자를 선택하였다.
* Ubuntu에서 한글입력이 가능하도록 설정한다.<br />http://hochulshin.com/ubuntu-1604-hangul/

# Chrome 설치

방법1.  
아래 명령어를 터미널 창에서 수행한다. https://brunch.co.kr/@hancoma/90
```
~$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
~$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```
방법2.  
```
~$ sudo apt-get install chrome
```

# virtualenv 설치하기 [[참고](https://gist.github.com/Geoyi/d9fab4f609e9f75941946be45000632b)]
* pip 설치
```
sudo apt-get install python3-pip
```

* pip3를 통해 virtualenv 설치하기
```
pip3 intstall virtualenv
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

# git 설치
* GitHub, Unreal Engine, AirSim 등을 활용하기 위하여 우분투에 git을 설치한다.
```
~$ sudo apt-get update
~$ sudo apt-get install git
```

# Pycharm 설치
* Python IDE로 유명한 Pycharm을 설치한다. (markdown도 지원하는 plugin이 공개되어 github 사용에도 유리하다.)
* 터미널 창에서 아래와 같이 입력하여 설치한다. [참고사이트](https://www.jetbrains.com/help/pycharm/install-and-set-up-pycharm.html#linux)
```
~$ sudo snap install pycharm-community --classic
~$ pycharm-community
```