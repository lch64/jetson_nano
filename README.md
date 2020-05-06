# Jetson Nano에 Darknet Yolo 메뉴얼



1.  [Darknet YoloV3 설치법](#설치하기)
2.  [Darknet YoloV3 사용법](#how-to-use-on-the-command-line)


## 설치하기

### 1. OpenCV 설치
#### 설치준비
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

* 비디오 코덱 설치
```
sudo apt install -y libavcodec-dev libavformat-dev libswscale-dev libavresample-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev libfaac-dev libmp3lame-dev libvorbis-dev
```
* 카메라 프로그래밍 라이브러리 설치
```
sudo apt-get install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils
cd /usr/include/linux
sudo ln -s -f ../libv4l1-videodev.h videodev.h
cd ~
```
