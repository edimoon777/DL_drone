# CUDA 9.0 설치
* 다운로드 링크: https://developer.nvidia.com/cuda-90-download-archive
* Linux > x86_64 > Ubuntu > 16.04 > deb (network) 순으로 선택하고, Base Install를 다운로드한다.
* 다운로드 디렉토리로 이동하여 아래 명령을 실행한다.
```
~$ sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
~$ sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
~$ sudo apt-get update
~$ sudo apt-get install cuda-9-0
```

# cuDNN v7.0.5 설치
* 다운로드 링크: https://developer.nvidia.com/rdp/cudnn-download
* Runtime Lib.(deb), Developer Lib.(deb), Code Sampes...(deb) 다운로드하고 HOME 디렉토리로 이동한 후, 아래코드 실행한다.
```
~$ sudo dpkg -i libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
~$ sudo dpkg -i libcudnn7-dev_7.0.5.15-1+cuda9.0_amd64.deb
~$ sudo dpkg -i libcudnn7-doc_7.0.5.15-1+cuda9.0_amd64.deb
```
* 아래 코드를 실행하여, 설치가 제대로 되었는지 확인한다.
```
~$ cp -r /usr/src/cudnn_samples_v7/ $HOME
~$ cd $HOME/cudnn_samples_v7/mnistCUDNN
~$ make clean && make
~$  ./mnistCUDNN
```
(Test passed!가 출력되면 정상 설치된 것이다.)

# TensorFlow 1.4.0 설치 (Python 3.n 기준)
* 참고 사이트: https://www.tensorflow.org/install/install_linux#InstallingVirtualenv
* 아래 코드 참고하여 설치한다. (ENVNAME은 사용자가 정하는 임의의 environment 명)
```
~$ sudo apt-get install python3-pip python3-dev python-virtualenv
~$ virtualenv --system-site-packages -p python3 ENVNAME
~$ source ~/ENVNAME/bin/activate
(ENVNAME) ~$ easy_install -U pip
(ENVNAME) ~$ pip3 install --upgrade tensorflow-gpu
```
* 아래 코드를 참고하여 정상설치 여부를 확인한다.
```
(ENVNAME) ~$ python
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
```

# TensorFlow 1.4.0 설치 (삭제예정)
※ Anaconda 기반의 개발 환경은 삭제 예정
* 참고 사이트: https://www.tensorflow.org/install/install_linux
* 아래 코드 참고하여 설치한다. (ENVNAME은 사용자가 정하는 임의의 environment 명)
```
~$ conda create -n ENVNAME python=3.6
~$ source activate ENVNAME
~$ pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.4.0-cp36-cp36m-linux_x86_64.whl
```
* Anaconda를 사용할 경우 위에서 언급했듯이 `~$ conda create -n ENVNAME python=3.6`과 같이 반드시 별도의 environment를 생성해서 Tensorflow를 설치하는 것이 바람직하다. 그렇지 않으면 추후 다른 프로그램 설치 과정에서 일부 패키지를 업데이트하여 고둥에 문제 발생할 수 있기 때문이다.

## spyder를 통한 TensorFlow 활용 방법
* 위와 같이 설치하고 나서 spyder를 통해서 Tensorflow를 구동하고자 할 때에는 아래와 같이 구동한다.
  * `~$ source activate ENVNAME`
  * `~$ anaconda-navigator` 실행 후 GUI 환경에서 'spyder' 설치
    * 단, 설치가 안 되어 있는 경우에만 수행한다.
    * navigator GUI를 통해서 spyder를 설치하면 관련 패키지들이 모두 설치되어 편리하다.
  * navigator 종료 후에 `(ENVNAME)~$ spyder` 실행
* ENVNAME과 같이 지정된 environment를 활성화하지 않고 spyder를 구동하면 tensorflow가 설치된 지정 environment에서 구동되지 않기 때문에 tensorflow가 import되지 않음.

# git 설치
* GitHub, Unreal Engine, AirSim 등을 활용하기 위하여 우분투에 git을 설치한다.
```
~$ sudo apt-get update
~$ sudo apt-get install git
```
