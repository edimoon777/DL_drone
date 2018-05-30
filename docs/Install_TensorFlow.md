# NVIDIA Driver / CUDA / cuDNN 설치 확인
* NVIDIA Driver v3.84이상 설치여부 확인하기
  ```
  nvidia -smi
  ```
  * 버전이 낮다면 [OS 및 기본 SW 설치](/docs/Install_OS_SW.md) 편 참고
* CUDA v9.0 설치여부 확인하기
  ```
  nvcc --version
  ```
  * 버전이 낮다면 [Jetson TX2 개발을 위한 Jetpack / NVcaffe / DIGITS 설치](/docs/Install_Jetpack.md) 편 참고
* cuDNN v7.0.5 설치여부 확인하기
  ```
  cat /usr/include/cudnn.h | grep CUDNN_MAJOR -A 2
  ```
  * 버전이 낮다면 [Jetson TX2 개발을 위한 Jetpack / NVcaffe / DIGITS 설치](/docs/Install_Jetpack.md) 편 참고
* cuDNN 버전을 v7.0.5로 고정하기
  ```
  sudo apt-mark hold libcudnn7 libcudnn7-dev libcudnn7-doc
  ```

# TensorFlow v1.8 설치 (v1.7부터 tensorRT와 통합)
* 개요
  * 참고 사이트: https://www.tensorflow.org/install/install_linux
  * 안정적인 개발환경을 구축을 위해 Virtualenv 기반 설치  
  * GPU를 사용하는 버전을 중심으로 설명

* CUDA Profiler Tools Interface 설치
  * 설치
    ```
    sudo apt-get install cuda-command-line-tools-9-0

    ```
  * `sudo gedit ~/.bashrc`를 통해 경로 추가
    ```
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH:+${LD_LIBRARY_PATH}:}/usr/local/cuda/extras/CUPTI/lib64
    ```
* 가상환경 생성
  ```
  virtualenv --system-site-packages ENVNAME # for Python 2.7
  virtualenv --system-site-packages -p python3 ENVNAME # for Python 3.n  
  ```     
* 가상환경 활성화
  ```
  source ~/ENVNAME/bin/activate
  ```
* pip v8.1 이상인지 확인
  ```
  easy_install -U pip
  ```
* 가상환경에서 GPU용 tensorflow 설치
  ```
  pip install --upgrade tensorflow-gpu  # for Python 2.7 and GPU
  pip3 install --upgrade tensorflow-gpu # for Python 3.n and GPU
  ```    
* 설치여부 확인
  * 가산환경 활성화 여부 확인 후 `python` 실행
  * 아래 명령어 코딩
    ```
    import tensorflow as tf
    hello = tf.constant('Hello, TensorFlow!')
    sess = tf.Session()
    print(sess.run(hello))
    ```
  * `Hello, TensorFlow!` 출력되는 지 확인