# Ubuntu 16.04.03 LTS 설치
* 기존 윈도우 시스템의 안정적인 운용을 위해 별도의 하드에 우분투를 설치하는 것이 안정적인 운용이 가능하다.
* 데스크탑용 우분투를 다운로드한다.<br />https://www.ubuntu.com/download/desktop
* 우분투 설치를 위해 USB를 부팅가능하도록 만든다.<br />https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows?_ga=2.252068618.2004108082.1514558137-364398367.1514558137#0
* 설치할 때 'Swap' 영역은 16GB, '/' 영역은 20GB, '/home' 영역은 HDD의 나머지를 할당한다.
  * MATLAB과 같이 무거운 프로그램을 설치할 예정이라면 '/'영역의 크기를 여유롭게 할당해야 함.
  * 이때 '/boot' 파티션을 할당하게 되면 컴퓨터 부팅 시에 부트로더가 실행되어 기존의 OS와 Ubuntu를 선택하는 화면이 나오게 할 수 있고, 그렇지 않으면 부팅 시에 'F12(메인보드 제조사마다 다름)' 등을 눌러 부팅장치 우선순위를 변경해서 부팅할 OS를 선택할 수 있도록 할 수 있다. 필자는 후자를 선택하였다.
* Ubuntu에서 한글입력이 가능하도록 설정한다.<br />http://hochulshin.com/ubuntu-1604-hangul/

# Chrome 설치
아래 명령어를 터미널 창에서 수행한다. https://brunch.co.kr/@hancoma/90
```
~$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
~$ sudo apt-get install libxss1 libgconf2-4 libappindicator1 libindacator7
~$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```

# Anaconda 설치
※ Tensorflow 1.4.0에서는 Python 3.6을 지원한다. [참고사이트](https://www.tensorflow.org/install/install_linux#python_36)
* 다운로드한다. https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
* 터미널 창에서 아래와 같이 입력하여 설치한다.
```
~$ bash ~/Downloads/Anaconda3-5.0.1-Linux-x86_64.sh
```
  * 최신 버전 다운로드 링크: https://www.anaconda.com/download/#linux
  * 설치방법 링크: https://docs.anaconda.com/anaconda/install/linux
* .bashrc에 'PATH'를 설정한다.
```
~$ cd ~
~$ gedit .bashrc
~$ export PATH="/<path to anaconda>/bin:$PATH"
~$ source .bashrc
```
* 'Anaconda Navigator'를 실행한다.
```
~$ anaconda-navigator
```
