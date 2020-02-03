---
layout: post
title: "ROS tf 라이브러리 사용하기"
description: ROS tf 라이브러리 사용하기
# tags: ROS
date: 2020-01-27 16:37:00
comments: true
---
# ROS tf 라이브러리 사용하기

contents
- tf란?
- 기본 튜토리얼
- tf소요시간?

## tf란?

[Introduction to tf](http://wiki.ros.org/tf/Tutorials/Introduction%20to%20tf)  
tf는 프레임을 추적할 수 있게 해주는 ros 라이브러리 입니다. tf를 이용해서 두 프레임간의 변환 행렬을 얻거나 서로 다른 프레임의 정보를 Rviz에 보여주기 쉽게 하기도 합니다. 또한 시간이 지나도 과거의 프레임을 추적할 수 있게 해줍니다.

## Tutorials

tf를 활용하는 좋은 예는 로봇 좌표계로부터 월드 좌표계로 변환하는 예제입니다.  
다음과 같은 정보가 주어져 있는 상황을 가정 해보겠습니다.

> 월드 좌표계로부터 로봇의 좌표, 로봇 좌표로부터 검출된 물체의 좌표값이 주어질때, 월드 좌표계 일정 범위 안에 검출된 물체의 개수?

![car](https://github.com/msc9533/msc9533.github.io/blob/master/image/tf_tree.png?raw=true)

[bag file](https://github.com/msc9533/msc9533.github.io/raw/master/_files/2020-01-30-13-12-20.bag)  

첨부된 bag file에는 frame_id가 /odom인 /odom topic과 frame_id가 /base_scan인 /scan과 /tf가 포함 되어 있습니다. 다시 정리 해보면 `odom`-`base_footprint`-`base_link`-`base_scan`에서 좌표 변환을 계산하면 되는 예제입니다.

[-0.032, 0.000, 0.182] base_footprint to base_scan
다음은 tf를 이용해 변환하는 예제입니다.

이때 변환행렬을 사용해 쉽게 좌표계간의 변환을 할수도 있습니다.

문제 변환행렬 식

하지만 다음과 같은 경우 - 좌표계가 많은 경우 - tf를 이용하면 간단히 변환행렬을 구할수 있습니다.

## tf 시간?

- 시간있는이유?



---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [TePRA2013_Foote.pdf](http://wiki.ros.org/Papers/TePRA2013_Foote?action=AttachFile&do=view&target=TePRA2013_Foote.pdf)
- [Transformation matrix](https://en.wikipedia.org/wiki/Transformation_matrix)
- [ROS wiki - tf tutorials](http://wiki.ros.org/tf/Tutorials)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>