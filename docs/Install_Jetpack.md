# Jetpack 설치
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
   * 아래 코드를 실행하여, 설치가 제대로 되었는지 확인한다. (Test passed!가 출력되면 정상 설치된 것이다.)
     ```
     cp -r /usr/src/cudnn_samples_v7/ $HOME
     cd $HOME/cudnn_samples_v7/mnistCUDNN
     make clean && make
     ./mnistCUDNN
     ```

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
* [설치안내 링크](https://docs.nvidia.com/jetpack-l4t/index.html#developertools/mobile/jetpack/l4t/3.2/jetpack_l4t_install.htm)에 따라 설치한다.
  * Jetpack을 설치하고 나면 `sudo apt-get update` 수행 시에 오류가 발생한다. 이는 JetPack이 Jetson TX2를 지원하기 위해서 arm64를 지원하도록 ppa를 지정했기 때문으로 보인다. 아직 이 문제는 해결되지 않았으나 NVIDIA에서는 기능상 문제는 없다고 얘기하고 있다. [참고](https://devtalk.nvidia.com/default/topic/1002140/jetson-tx2/apt-get-update-errors/post/5163362/#5163362) 

# Host PC 환경구축
* [참고사이트](https://github.com/dusty-nv/jetson-inference)
  * Jetson TX2를 활용하기 위해 Jetpack 설치방법 부터 Host 개발환경까지 모두 설명이 되어 있다.
  * 2018.4.23. 현재 아래 환경까지 검증이 완료되었다.
  * Jetson TX2 - JetPack 3.2 / L4T R28.2 aarch64 (Ubuntu 16.04 LTS) inc. TensorRT 3.0 RC2
  
* 빌드 Protobuf [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildProtobuf.md)
  * NVcaffe를 사용하기 위해서는 Protobuf3가 빌드되어 있어야 한다.
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
    sudo python setup.py install --cpp_implementation
    ```

* NVcaffe 설치 [참고](https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildCaffe.md)
  * Dependencies
    ```
    sudo apt-get install --no-install-recommends build-essential cmake git gfortran libatlas-base-dev libboost-filesystem-dev libboost-python-dev libboost-system-dev libboost-thread-dev libgflags-dev libgoogle-glog-dev libhdf5-serial-dev libleveldb-dev liblmdb-dev libopencv-dev libsnappy-dev python-all-dev python-dev python-h5py python-matplotlib python-numpy python-opencv python-pil python-pip python-pydot python-scipy python-skimage python-sklearn 
    ```  
  * NVcaffe 소스파일 다운로드 (caffe 0.15 기준)
    ```
    # example location - can be customized
    export CAFFE_ROOT=~/caffe
    git clone https://github.com/NVIDIA/caffe.git $CAFFE_ROOT -b 'caffe-0.15'
    ```
  * 파이썬 패키지 설치
    ```
    sudo pip install -r $CAFFE_ROOT/python/requirements.txt
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
    sudo pip install -r $DIGITS_ROOT/requirements.txt
    ```
  * 플러그인 지원 허용
    ```
    sudo pip install -e $DIGITS_ROOT
    ```
  * 서버 시작
    ```
    ./digits-devserver
    ```
    제대로 실행된 것을 확인하려면 웹에서 0.0.0.0:5000 으로 접속