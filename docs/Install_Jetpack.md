# Jetpack 설치
* NVIDIA 드라이버 설치
  ```
  sudo apt-get install nvidia-384
  sudo reboot
  ```
* cuDNN 설치
  * 설치된 CUDA 버전(v9.0)에 맞는 cuDNN(v7.0.5) 다운로드 [링크](https://developer.nvidia.com/cudnn)
    > cuDNN v7.0.5 Runtime Library for Ubuntu16.04 (Deb) <br>
    cuDNN v7.0.5 Developer Library for Ubuntu16.04 (Deb) <br>
    cuDNN v7.0.5 Code Samples and User Guide for Ubuntu16.04 (Deb)
  * cuDNN 설치
    ```
    sudo dpkg -i libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
    sudo dpkg -i libcudnn7-dev_7.0.5.15-1+cuda9.0_amd64.deb 
    sudo dpkg -i libcudnn7-doc_7.0.5.15-1+cuda9.0_amd64.deb
    ```
  * cuDNN을 먼저 설치하지 않으면 `sudo apt-get update`를 수행할 때 `Failed to fetch...`와 같은 오류가 발생하는 문제가 발생하였다. 현재(2018.05.03.) 정확한 이유는 파악하지 못하고 있으며, cuDNN을 JetPack보다 먼저 설치하면 이런 문제는 발생하지 않음을 확인하였다.
* 최신 버전(3.2 L4T)을 다운로드 받는다. [링크](https://developer.nvidia.com/embedded/jetpack)
  * JetPaqck 3.2 L4T 기준 설명
  * HOST PC 설치 항목
    * Tegra Graphics Debugger v2.5
    * NVIDIA System Profiler v4.0
    * JetPack Documentation 3.2
    * DevTools Documentation 3.2
    * OpenCV 3.3.1
    * Vision Works 1.6
    * CUDA Toolkit 9.0
  * Jetson TX2 설치 항목
    * VisionWorks 1.6
    * CUDA Toolkit 9.0
    * Compile CUDA Samples 9.0
    * cuDNN Package 7.0
    * TensorRT 3.0
    * OpenCV 3.3.1
    * Multimedia API package 28.2
* 설치 안내에 따라 설치한다. [링크](https://docs.nvidia.com/jetpack-l4t/index.html#developertools/mobile/jetpack/l4t/3.2/jetpack_l4t_install.htm)

# Host PC 환경구축
* [참고사이트](https://github.com/dusty-nv/jetson-inference)
  * Jetson TX2를 활용하기 위해 Jetpack 설치방법 부터 Host 개발환경까지 모두 설명이 되어 있다.
  * 2018.4.23. 현재 아래 환경까지 검증이 완료되었다.
  * Jetson TX2 - JetPack 3.2 / L4T R28.2 aarch64 (Ubuntu 16.04 LTS) inc. TensorRT 3.0 RC2

* NVcaffe 설치 [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildCaffe.md)
  * Dependencies
    ```
    sudo apt-get install --no-install-recommends build-essential cmake git gfortran libatlas-base-dev libboost-filesystem-dev libboost-python-dev libboost-system-dev libboost-thread-dev libgflags-dev libgoogle-glog-dev libhdf5-serial-dev libleveldb-dev liblmdb-dev libopencv-dev libsnappy-dev python-all-dev python-dev python-h5py python-matplotlib python-numpy python-opencv python-pil python-pip python-pydot python-scipy python-skimage python-sklearn 
    ```
  * 빌드 Protobuf [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildProtobuf.md)
    * Dependencies
      ```
      sudo apt-get install autoconf automake libtool curl make g++ git python-dev python-setuptools unzip
      ```
    * Protobuf 소스파일 다운로드
      ```
      # example location - can be customized
      export PROTOBUF_ROOT=~/protobuf
      git clone https://github.com/google/protobuf.git $PROTOBUF_ROOT -b '3.2.x'
      ```
    * Protobuf 빌드
      ```
      cd $PROTOBUF_ROOT
      ./autogen.sh
      ./configure
      sudo make "-j$(nproc)"
      sudo make install
      sudo ldconfig
      cd python
      python setup.py install --cpp_implementation
      ```
  * NVcaffe 소스파일 다운로드 (caffe 0.15 기준)
    ```
    # example location - can be customized
    export CAFFE_ROOT=~/caffe
    git clone https://github.com/NVIDIA/caffe.git $CAFFE_ROOT -b 'caffe-0.15'
    ```
  * 파이썬 패키지 설치
    ```
    pip install -r $CAFFE_ROOT/python/requirements.txt
    ```
  * 빌드 NVcaffe
    ```
    cd $CAFFE_ROOT
    mkdir build
    cd build
    cmake ..
    make all -j"$(nproc)"
    make install
    make runtest
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
    # For Ubuntu 16.04
    CUDA_REPO_PKG=http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
    ML_REPO_PKG=http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
    # Install repo packages
    wget "$CUDA_REPO_PKG" -O /tmp/cuda-repo.deb && sudo dpkg -i /tmp/cuda-repo.deb && rm -f /tmp/cuda-repo.deb
    wget "$ML_REPO_PKG" -O /tmp/ml-repo.deb && sudo dpkg -i /tmp/ml-repo.deb && rm -f /tmp/ml-repo.deb
    # Download new list of packages
    sudo apt-get update
    ```
  * Dependencies
    ```
    sudo apt-get install --no-install-recommends git graphviz python-dev python-flask python-flaskext.wtf python-gevent python-h5py python-numpy python-pil python-pip python-scipy python-tk
    ```
  * DIGITS 소프파일 다운로드
    ```
    # example location - can be customized
    DIGITS_ROOT=~/digits
    git clone https://github.com/NVIDIA/DIGITS.git $DIGITS_ROOT
    ```
  * 파이썬 패키지 설치
    ```
    pip install -r $DIGITS_ROOT/requirements.txt
    ```
  * 플러그인 지원 허용
    ```
    pip install -e $DIGITS_ROOT
    ```
  * 서버 시작
    ```
    ./digits-devserver
    ```
    제대로 실행된 것을 확인하려면 웹에서 0.0.0.0:5000 으로 접속
  * Remark
    * 'CAFFE_ROOT does not point to a valid installation of Caffe.' 등의 오류가 나오면 `~/.bashrc`에서 caffe 디렉토리가 path로 잘 지정되었는지 확인하고, 이상이 없다면 caffe 디렉토리에서 `make clean`하고 다시 빌드
    * `ImportError: No module named XXXX`라는 오류가 발생하면 `XXXX`라는 패키지를 수동으로 설치했다. `sudo pip install XXXX`
      ```
      pip install request numpy Pillow scipy
      ```  
    * 현재 위 상태면 virtualenv를 활성하지 않고 `digits-devserver`를 실행해야 함.