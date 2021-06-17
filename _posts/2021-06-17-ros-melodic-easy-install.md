---
layout: post
title: "ROS Noetic 한 줄 설치"
description: ROS Noetic 한 줄 설치
tags: ROS
date: 2021-06-17 10:34:27
comments: true
---

# ROS melodic 한 줄 설치

지난 [포스트](https://msc9533.github.io/2021/02/ros-noetic-easy-install)와 비슷한 한 줄 설치 스크립트입니다.
터미널을 열고 아래 스크립트를 입력하면 됩니다.

```
sudo apt-get install -y wget && wget https://gist.githubusercontent.com/msc9533/13c4be14db5fd35d83ea8e91ff96eec3/raw/0fc596be617792c76248f5c1ff4cd8bbf35264b1/ros-melodic-install.sh && chmod +x ros-melodic-install.sh && ./ros-melodic-install.sh && rm ros-melodic-install.sh
```

아주 간단한 패키지 [hello ros packge](https://github.com/msc9533/hello-ros-pkg) 를 `catkin_ws`에 만들어 빌드 테스트 하는 것 까지 되어 있습니다.  
아래와 같은 결과 화면이 나오면 정상적으로 설치가 완료된 것입니다.

![img](https://i.imgur.com/1mdhJhF.png)

<details>
<summary>참고문서</summary>
<div markdown="1">

- [http://wiki.ros.org/noetic/Installation/Ubuntu](http://wiki.ros.org/noetic/Installation/Ubuntu)
- [http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment)
- [hello ros packge](https://github.com/msc9533/hello-ros-pkg)

</div>
</details>

---

<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

