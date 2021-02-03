---
layout: post
title: "ROS Noetic 한 줄 설치"
description: ROS Noetic 한 줄 설치
tags: ROS
date: 2021-02-03 13:59:14
comments: true
---

# ROS Noetic 한 줄 설치

지난 2020년 5월 23일에 Realease 된 ROS Noetic 입니다. Ubuntu 20.04 기준 설치 방법을 스크립트로 모아서 실행하게 만들어 두었습니다.  

터미널을 열고 아래 스크립트를 입력하면 됩니다.

```
sudo apt-get install -y wget && wget https://gist.githubusercontent.com/msc9533/25e32865c1e26b54aa845a94b83055da/raw/67350d7c4bdc69b6f81dc4500957053143b7bc34/ros-noetic-install.sh && chmod +x ros-noetic-install.sh && ./ros-noetic-install.sh
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

