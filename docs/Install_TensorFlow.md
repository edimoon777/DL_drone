# CUDA 9.0 설치
* 다운로드 링크: https://developer.nvidia.com/cuda-90-download-archive
* Linux > x86_64 > Ubuntu > 16.04 > deb (network) 순으로 선택하고, Base Install를 다운로드한다.
* 다운로드 디렉토리로 이동하여 아래 명령을 실행한다.
```
sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda-9-0
```
* 재부팅한다.
* 아래와 같이 .bashrc에 path를 추가한다.
```
export PATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

# cuDNN v7.0.5 설치
* 다운로드 링크: https://developer.nvidia.com/rdp/cudnn-download
* Runtime Lib.(deb), Developer Lib.(deb), Code Sampes...(deb) 다운로드하고 HOME 디렉토리로 이동한 후, 아래코드 실행한다.
```
sudo dpkg -i libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
sudo dpkg -i libcudnn7-dev_7.0.5.15-1+cuda9.0_amd64.deb
sudo dpkg -i libcudnn7-doc_7.0.5.15-1+cuda9.0_amd64.deb
```
* 아래 코드를 실행하여, 설치가 제대로 되었는지 확인한다.
```
cp -r /usr/src/cudnn_samples_v7/ $HOME
cd $HOME/cudnn_samples_v7/mnistCUDNN
make clean && make
 ./mnistCUDNN
```
(Test passed!가 출력되면 정상 설치된 것이다.)

# TensorFlow 1.6 설치 (Python 3.n 기준)
* 본 과정부터는 가상환경을 적용하는 것이 안정적인 개발환경을 구축하는데 도움이 된다.
* 참고 사이트: https://www.tensorflow.org/install/install_linux#InstallingVirtualenv
* 아래 코드 참고하여 설치한다. (ENVNAME은 사용자가 정하는 임의의 environment 명)

1. PIP와 virtualEnvironment를 설치한다.
```
sudo apt-get install python3-pip python3-dev python-virtualenv
virtualenv --system-site-packages -p python3 ENVNAME
source ~/ENVNAME/bin/activate
(ENVNAME) easy_install -U pip
(ENVNAME) pip3 install --upgrade tensorflow-gpu
```

* 아래 코드를 참고하여 정상설치 여부를 확인한다.
```
(ENVNAME) python
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
```
