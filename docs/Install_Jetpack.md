# Jetpack 설치
* 최신 버전(3.2 L4T)을 다운로드 받는다. [링크](https://developer.nvidia.com/embedded/jetpack)
  * JetPaqck 3.2 L4T 기준 설명
  * TensorRT 3.0 GA, cuDNN 7.0.5, VisionWorks 1.6, CUDA 9.0, OpenCV 3.3.1, L4T Driver Package 등이 포함
* 설치 안내에 따라 설치한다. [링크](https://docs.nvidia.com/jetpack-l4t/index.html#developertools/mobile/jetpack/l4t/3.2/jetpack_l4t_install.htm)

# Host PC 환경구축
* [참고사이트](https://github.com/dusty-nv/jetson-inference)
  * Jetson TX2를 활용하기 위해 Jetpack 설치방법 부터 Host 개발환경까지 모두 설명이 되어 있다.
  * 2018.4.23. 현재 아래 환경까지 검증이 완료되었다.
  * Jetson TX2 - JetPack 3.2 / L4T R28.2 aarch64 (Ubuntu 16.04 LTS) inc. TensorRT 3.0 RC2

* NVIDIA 드라이버 설치
  ```
  ~$ sudo apt-get install nvidia-384
  ~$ sudo reboot
  ```
* cuDNN 설치
  * 설치된 CUDA 버전(v9.0)에 맞는 cuDNN(v7.1.3) 다운로드 [링크](https://developer.nvidia.com/cudnn)
    > cuDNN v7.1.3 Runtime Library for Ubuntu16.04 (Deb) <br>
    cuDNN v7.1.3 Developer Library for Ubuntu16.04 (Deb) <br>
    cuDNN v7.1.3 Code Samples and User Guide for Ubuntu16.04 (Deb)
  * cuDNN 설치
    ```
    ~$ sudo dpkg -i libcudnn7_7.1.3.16-1+cuda9.0_amd64.deb
    ~$ sudo dpkg -i libcudnn7-dev_7.1.3.16-1+cuda9.0_amd64.deb 
    ~$ sudo dpkg -i libcudnn7-doc_7.1.3.16-1+cuda9.0_amd64.deb
    ```
* 가상환경 신규생성 또는 활성화
  * NVcaffe와 DIGITS 설치에 영향을 주지는 않을 것으로 보임.
  * 단, NVcaffe와 DIGITS를 위해 설치되는 패키지들이 다른 개발환경에 영향을 줄 것임.
  * 이에 가상환경을 설정하는 것을 추천함.
    * 신규생성
      ```
      ~$ virtualenv -p /usr/bin/python ENVNAME
      ```
    * 또는 활성화
      ```
      ~$ source ENVNAME/bin/activate
      ```
* NVcaffe 설치 [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildCaffe.md)
  * 빌드 Protobuf [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildProtobuf.md)
    * Dependencies
      ```
      ~$ sudo apt-get install autoconf automake libtool curl make g++ git python-dev python-setuptools unzip
      ```
    * Protobuf 소스파일 다운로드
      ```
      ~$ # example location - can be customized
      ~$ export PROTOBUF_ROOT=~/protobuf
      ~$ git clone https://github.com/google/protobuf.git $PROTOBUF_ROOT -b '3.2.x'
      ```
    * Protobuf 빌드
      ```
      ~$ cd $PROTOBUF_ROOT
      ~$ ./autogen.sh
      ~$ ./configure
      ~$ sudo make "-j$(nproc)"
      ~$ sudo make install
      ~$ sudo ldconfig
      ~$ cd python
      ~$ python setup.py install --cpp_implementation
      ```
  * Dependencies
    ```
    ~$ sudo apt-get install --no-install-recommends build-essential cmake git gfortran libatlas-base-dev libboost-filesystem-dev libboost-python-dev libboost-system-dev libboost-thread-dev libgflags-dev libgoogle-glog-dev libhdf5-serial-dev libleveldb-dev liblmdb-dev libopencv-dev libsnappy-dev python-all-dev python-dev python-h5py python-matplotlib python-numpy python-opencv python-pil python-pip python-pydot python-scipy python-skimage python-sklearn
    ```
  * NVcaffe 소스파일 다운로드 (caffe 0.15 기준)
    ```
    ~$ # example location - can be customized
    ~$ export CAFFE_ROOT=~/caffe
    ~$ git clone https://github.com/NVIDIA/caffe.git $CAFFE_ROOT -b 'caffe-0.15'
    ```
  * 파이썬 패키지 설치
    ```
    pip3 install -r $CAFFE_ROOT/python/requirements.txt
    ```
  * 빌드 NVcaffe
    ```
    ~$ cd $CAFFE_ROOT
    ~$ mkdir build
    ~$ cd build
    ~$ cmake ..
    ~$ make -j"$(nproc)"
    ~$ make install
    ```
  * ~/.bashrc에 경로설정
    ```
    sudo gedit ~/.bashrc
    ```
    아래 경로 추가
    > export CAFFE_ROOT=/home/USER/caffe <br>
    export PYTHONPATH=/home/USER/caffe/python:$PYTHONPATH 


* DIGITS 설치 [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildDigits.md)
  * NVIDIA 드라이버 설치
    ```
    ~$ # For Ubuntu 16.04
    ~$ CUDA_REPO_PKG=http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
    ~$ ML_REPO_PKG=http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
    ~$ # Install repo packages
    ~$ wget "$CUDA_REPO_PKG" -O /tmp/cuda-repo.deb && sudo dpkg -i /tmp/cuda-repo.deb && rm -f /tmp/cuda-repo.deb
    ~$ wget "$ML_REPO_PKG" -O /tmp/ml-repo.deb && sudo dpkg -i /tmp/ml-repo.deb && rm -f /tmp/ml-repo.deb
    ~$ # Download new list of packages
    ~$ sudo apt-get update
    ```
  * Dependencies
    ```
    sudo apt-get install --no-install-recommends git graphviz python-dev python-flask python-flaskext.wtf python-gevent python3-h5py python3-numpy python3-pil python3-pip python3-scipy python-tk
    ```
  * DIGITS 소프파일 다운로드
    ```
    ~$ # example location - can be customized
    ~$ DIGITS_ROOT=~/digits
    ~$ git clone https://github.com/NVIDIA/DIGITS.git $DIGITS_ROOT
    ```
  * 파이썬 패키지 설치
    ```
    pip3 install -r $DIGITS_ROOT/requirements.txt
    ```
  * 플러그인 지원 허용
    ```
    pip3 install -e $DIGITS_ROOT
    ```
  * 서버 시작
    ```
    ./digits-devserver
    ```
    제대로 실행된 것을 확인하려면 웹에서 0.0.0.0:5000 으로 접속해본다.