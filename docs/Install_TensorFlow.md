# CUDA 8.0 GA2 설치
* 다운로드 링크: https://developer.nvidia.com/cuda-80-ga2-download-archive
* 설치 예제: http://gusrb.tistory.com/6

# cuDNN v6.0 설치
* 참고 사이트: https://developer.nvidia.com/rdp/cudnn-download
* 아래 코드를 참고하여 설치한다.
```
~$ CUDNN_TAR_FILE="cudnn-8.0-linux-x64-v6.0.tgz"
~$ wget http://developer.download.nvidia.com/compute/redist/cudnn/v6.0/${CUDNN_TAR_FILE}
~$ tar -xzvf ${CUDNN_TAR_FILE}
~$ sudo cp -P cuda/include/cudnn.h /usr/local/cuda-8.0/include
~$ sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-8.0/lib64/
~$ sudo chmod a+r /usr/local/cuda-8.0/lib64/libcudnn*
```

# TensorFlow 1.4.0 설치
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
