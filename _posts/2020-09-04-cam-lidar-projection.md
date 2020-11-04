---
layout: post
title: "Camera-LiDAR Projection 구현하기"
description: Camera-LiDAR Projection 구현하기
# tags: Algorithm
date: 2020-08-21 11:04:25
comments: true
---

LiDAR와 카메라는 인지 분야에서 널리 활용됩니다. 하지만 각각의 센서가 갖는 특성과 단점이 명확하기에 각각의 센서만으로는 완벽한 인지를 할 수 없습니다.  
카메라는 LiDAR에 비해 얻을 수 있는 정보의 해상도가 뛰어납니다.
LiDAR는 카메라에 비해 정확한 거리정보를 얻을 수 있습니다.
3D 객체를 검출할때는 이 두센서의 정보를 융합하는 것으로 많은 단점을 해결 할 수 있습니다.
핀홀 카메라 모델에서 3차원 공간상의 점들은 이미지 평면에 투사되어 이미지로 나타나게 됩니다.

## Camera Projection Model

카메라센서는 픽셀단위의 센서값입니다. 카메라 렌즈에 의해 맺히는 상의 모델은 pinhole 모델로 간단하게 나타낼수 있습니다.
이 pinhole 모델에서 pinhole은 렌즈의 중심이며 이 중심점을 통해 센서에 상이 거꾸로 맺히게 됩니다.

>![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/Pinhole-camera.svg/1280px-Pinhole-camera.svg.png)
>pin hole camera model. 출처 : (https://en.wikipedia.org/wiki/Pinhole_camera_model)

하지만 이 [pinhole모델](https://en.wikipedia.org/wiki/Pinhole_camera_model)은 이상적인 카메라 모델입니다. 실제로는 렌즈에 의한 왜곡이 생기며 이 왜곡모델은 다음과 같습니다.

$$
\begin{bmatrix} u \\ v \\ 1 \end{bmatrix}= \begin{bmatrix} f_x && s_x && c_x \\ 0 && f_y && c_y \\ 0 && 0 && 1 \end{bmatrix}  \begin{bmatrix} x \\ y \\ z  \end{bmatrix}
$$
> u,v = 이미지 픽셀 좌표계, x,y,z = 3차원 좌표계  
> $f_x, f_y$ = 초점 거리  
> $c_x, c_y$ = 주점  
> $s_x$ = 비대칭계수

여기서 주목해야할 점은 3차원의 점이 이 모델에 의해 픽셀좌표계로 변환된다는 점입니다. 즉, 카메라 왜곡 모델을 알고 있다면 3차원의 점(Pointcloud)을 픽셀좌표계로 변환할 수 있습니다.

## Camera intrinsic parameter

그렇다면 어떻게 이 왜곡모델을 구할수 있을까요?  
이미 제조사에서 제공한 parameter가 존재할 수도 있지만 모르는 경우는 
체스보드와 같은 **이미 알고있는 3차원 물체**를 이용합니다. 
체스보드를 이용하는 한 예를 들면, [A4 체스보드](https://github.com/artoolkit/ARToolKit5/blob/master/doc/patterns/Calibration%20chessboard%20(A4).pdf)에서 검은색 사각형의 크기를 알고 있고, 이 검은색 사각형이 맞닿는 지점을 구합니다. 이 기능은 OpenCV에 `findChessboardCorners`로 구현이 되어있습니다. 또한 이 구한 chess board corners로 parameter를 구하는 기능또한 구현이 되어있습니다.

```cpp
Mat img = imread(argv[1], IMREAD_GRAYSCALE);
bool found = findChessboardCorners( img, boardSize, ptvec, CALIB_CB_ADAPTIVE_THRESH );
FileStorage fs( filename, FileStorage::READ );
Mat intrinsics, distortion;
fs["camera_matrix"] >> intrinsics;
fs["distortion_coefficients"] >> distortion;
```

## Extrinsic parameter
<!-- pinhole model -->
<!-- 3d lidar point to camera pixel -->

## Camera-LiDAR extrinsic calibration

 같

---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [Camera-Lidar Projection: Navigating between 2D and 3D](https://medium.com/swlh/camera-lidar-projection-navigating-between-2d-and-3d-911c78167a94)
- [카메라 캘리브레이션 - 다크프로그래머](https://darkpgmr.tistory.com/32)
- [How project Velodyne point clouds on image? (KITTI Dataset)](https://stackoverflow.com/questions/39104666/how-project-velodyne-point-clouds-on-image-kitti-dataset)
- [opencv camera calibration tutorial](https://docs.opencv.org/master/d4/d94/tutorial_camera_calibration.html)
- [opencv findChessboardCorners](https://docs.opencv.org/master/d9/d0c/group__calib3d.html#ga93efa9b0aa890de240ca32b11253dd4a)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

