# ubuntu 16.04 with 1080ti installation guide
- install ubuntu-16.04.4-server-amd64.iso
- update os
  ```sh
  apt-get -y update
  apt-get -y upgrade
  apt-get -y dist-upgrade
  apt-get -y autoremove
  apt-get -y install build-essential libopencv-dev python-opencv git
  ```
- install nvidia driver
  1. install
  ```sh
  apt-get purge nvidia*
  reboot
  sh cuda_9.0.176_384.81_linux.run # nvidia driver + cuda 9.0 (tf18.0 compatable)
  sh cuda_9.0.176.1_linux.run # cuda patch
  sh cuda_9.0.176.2_linux.run
  sh cuda_9.0.176.3_linux.run
  ```
  2. uninstall
  ```sh
  To uninstall the CUDA Toolkit, run the uninstall script in /usr/local/cuda-9.2/bin
  To uninstall the NVIDIA Driver, run nvidia-uninstall
  ```
  3. vi /etc/environment: add /usr/local/cuda/bin at the beginning
  4. vi /etc/ld.so.conf.d/cuda.conf: add /usr/local/cuda/lib64
  5. ldconfig (log out then log back in)
  6. nvcc -V
- install cudnn
  ```sh
  dpkg -i libcudnn7_7.1.4.18-1+cuda9.0_amd64.deb # cuda 9.0
  dpkg -i libcudnn7-dev_7.1.4.18-1+cuda9.0_amd64.deb
  dpkg -i libcudnn7-doc_7.1.4.18-1+cuda9.0_amd64.deb
  cp -r /usr/src/cudnn_samples_v7/ .
  cd ./cudnn_samples_v7/mnistCUDNN
  make clean && make
  ./mnistCUDNN
  ```
  
