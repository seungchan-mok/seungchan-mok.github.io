---
layout: post
title: "Camera-LiDAR Projection 구현하기"
description: Camera-LiDAR Projection 구현하기
# tags: Algorithm
date: 2020-08-21 11:04:25
comments: true
---

## Pinhole Camera Model

LiDAR와 카메라는 인지 분야에서 널리 활용됩니다. 하지만 각각의 센서가 갖는 특성과 단점이 명확하기에 각각의 센서만으로는 완벽한 인지를 할 수 없습니다.  
카메라는 LiDAR에 비해 얻을 수 있는 정보의 해상도가 뛰어납니다.
LiDAR는 카메라에 비해 정확한 거리정보를 얻을 수 있습니다.
3D 객체를 검출할때는 이 두센서의 정보를 융합하는 것으로 많은 단점을 해결 할 수 있습니다.
핀홀 카메라 모델에서 3차원 공간상의 점들은 이미지 평면에 투사되어 이미지로 나타나게 됩니다.

$$
\begin{bmatrix} u \\ v \\ 1 \end{bmatrix}= \begin{bmatrix} f_x && s_x && c_x \\ 0 && f_y && c_y \\ 0 && 0 && 1 \end{bmatrix} \begin{bmatrix} R | t \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}
$$

<!-- pinhole model -->
<!-- 3d lidar point to camera pixel -->

## 3D points to pixel

 같

---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [Camera-Lidar Projection: Navigating between 2D and 3D](https://medium.com/swlh/camera-lidar-projection-navigating-between-2d-and-3d-911c78167a94)
- [카메라 캘리브레이션 - 다크프로그래머](https://darkpgmr.tistory.com/32)
- [How project Velodyne point clouds on image? (KITTI Dataset)](https://stackoverflow.com/questions/39104666/how-project-velodyne-point-clouds-on-image-kitti-dataset)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

