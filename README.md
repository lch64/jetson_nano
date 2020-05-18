# Jetson Nano 사용자를 위한 Darknet Yolo 메뉴얼



1.  [Darknet YoloV3 설치법](#설치법)
	* [OpenCV 설치](#OpenCV-설치)
	* [Darnet YoloV3 설치](#Darknet-Yolo-설치)
2.  [Darknet YoloV3 사용법](#사용법)


## 설치법

Jetson Nano에서 Yolo를 사용하기 위해서는 opencv 와 CUDA가 필요합니다.
Cuda는 Jetson Nano에 이미 설치돼 있기 때문에, opencv만 설치하면 됩니다.

### OpenCV 설치
* OpenCV 유무 확인
```
sudo apt install -y pkg-config
pkg-config --modversion opencv
```
`No package 'opencv' found` 라고 나올 경우 OpenCV 설치 필요
* OpenCV 유무 확인2
pkg 라이브러리 오류로 openCV가 인식이 안될 수도 있음
python을 실행해서 opencv설치 유무 확인
```
python3
>> import cv2
>> cv2.__version__
```
output 오류가 날 경우 opencv 설치 필요

* 구버전의 opencv 삭제를 원하는 경우 다음과 같이 진행
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
sudo apt install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils
cd /usr/include/linux
sudo ln -s -f ../libv4l1-videodev.h videodev.h
cd ~
```

* 파이썬 도구
```
sudo apt install python3-dev python3-pip
sudo -H pip3 install -U pip numpy
sudo apt install python3-testresources
```

* 최적화 도구
```
sudo apt install libatlas-base-dev gfortran
```

* 그 외 필요 도구
```
sudo apt install libprotobuf-dev protobuf-compiler libgoogle-glog-dev libgflags-dev libgphoto2-dev libeigen3-dev libhdf5-dev doxygen libgtk-3-dev libtbb-dev 
```

* OpenCV 파일 다운로드
```
cd ~
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip
unzip opencv.zip
unzip opencv_contrib.zip
```
* OpenCV 빌드 

```
cd opencv-4.2.0
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D WITH_CUDA=ON \
      -D CUDA_ARCH_PTX="" \
      -D CUDA_ARCH_BIN="5.3,6.2,7.2" \
      -D WITH_CUBLAS=ON \
      -D WITH_LIBV4L=ON \
      -D BUILD_opencv_python3=ON \
      -D BUILD_opencv_python2=OFF \
      -D BUILD_opencv_java=OFF \
      -D WITH_GSTREAMER=ON \
      -D WITH_GTK=ON \
      -D BUILD_TESTS=OFF \
      -D BUILD_PERF_TESTS=OFF \
      -D BUILD_EXAMPLES=OFF \
      -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.2.0/modules \
      ..
```

* 메모리 부족 현상 방지를 위해, swap 메모리를 늘립니다
```
sudo apt install zram-config
sudo gedit /usr/bin/init-zram-swapping
```
그러면 메모장이 하나 열리는데, 
`mem=$(((totalmem / 2 / ${NRDEVICES}) * 1024))` 룰 `mem=$(((totalmem / ${NRDEVICES}) * 1024))` 로 수정 뒤 저장하고 reboot 해줍니다.

* OpenCV 컴파일
```
make -j4
sudo make install
```

* OpenCV 설치 확인
```
pkg-config --modversion opencv
```
또는
```
python3
>> import cv2
>> cv2.__version__
```
opencv 4.2.0 이 나오면 설치 완료입니다.

### Darknet Yolo 설치
* 라이브러리 업데이트
```
sudo apt update
```

* CUDA 경로 설정
```
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

* Darknet Yolo 다운로드
```
cd ~
git clone https://github.com/AlexeyAB/darknet
cd darknet
wget https://pjreddie.com/media/files/yolov3.weights
wget https://pjreddie.com/media/files/yolov3-tiny.weights
```

* Darknet Yolo 빌드
```
sudo gedit Makefile
```
메모장이 열리는데, 
```
GPU=0
CUDNN=0
OPENCV=0
```
을
```
GPU=1
CUDNN=1
OPENCV=1
```
로 변경 후 저장

이후 터미널에 `make` 입력 

## 사용법

* 이미지 사용
```
cd ~/darknet
./darknet detector test cfg/coco.data cfg/yolov3.cfg yolov3.weights -ext_output dog.jpg
```

* 동영상
```
cd ~/darknet
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights -ext_output test.mp4
```

* 웹캠 (실시간)
```
cd ~/darknet
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights -c 0
```

### 실행 도중 컴퓨터가 멈출 경우
```
cd ~/darknet/cfg
sudo gedit yolov3.cfg
```
* `batch` 와 `subdivision`의 값을 1로 변경
* `width`와 `height`를 224로 변경
