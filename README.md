# Jetson Nano에 Darknet Yolo 메뉴얼



1.  [Darknet YoloV3 설치법](#설치법)
2.  [Darknet YoloV3 사용법](#사용법)


## 설치법

Jetson Nano에서 Yolo를 사용하기 위해서는 opencv 와 CUDA가 필요합니다.
Cuda는 Jetson Nano에 이미 설치돼 있기 때문에, opencv만 설치하면 됩니다.

### 1. OpenCV 설치
* OpenCV 유무 확인
```
sudo apt-get install -y pkg-config
pkg-config --modversion opencv
```
`No package 'opencv' found` 라고 나올 경우 OpenCV 설치 필요

구버전의 opencv 삭제를 원하는 경우 다음과 같이 진행
```
sudo apt purge libopencv*
sudo apt autoremove
sudo apt update && sudo apt upgrade
```

* 필요 도구 설치
```
sudo apt install -y build-essential cmake pkg-config unzip yasm git checkinstall
```

* 이미지 파일 설치
```
sudo apt install -y libjpeg-dev libpng-dev libtiff-dev
```

* 비디오 코덱 
```
sudo apt install -y libavcodec-dev libavformat-dev libswscale-dev libavresample-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev libfaac-dev libmp3lame-dev libvorbis-dev
```
* 카메라 프로그래밍 도구 
```
sudo apt-get install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils
cd /usr/include/linux
sudo ln -s -f ../libv4l1-videodev.h videodev.h
cd ~
```

* 파이썬 도구
```
sudo apt-get install python3-dev python3-pip
sudo -H pip3 install -U pip numpy
sudo apt install python3-testresources
```

* 최적화 도구
```
sudo apt-get install libatlas-base-dev gfortran
```

* 그 외 필요 도구
```
sudo apt-get install libprotobuf-dev protobuf-compiler libgoogle-glog-dev libgflags-dev libgphoto2-dev libeigen3-dev libhdf5-dev doxygen libgtk-3-dev libtbb-dev 
```

* OpenCV 파일 다운로드
```
cd ~
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip
unzip opencv.zip
unzip opencv_contrib.zip
```


## 사용법
