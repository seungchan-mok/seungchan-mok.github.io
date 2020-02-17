---
layout: post
title: "ROS usb cam 2개이상의 카메라에서 사용하기"
description: ROS usb cam 2개이상의 카메라에서 사용하기
tags: ROS
date: 2020-02-17 12:27:29
comments: true
---

# ROS usb cam 2개이상의 카메라에서 사용하기

ROS 상에서 usb cam을 사용할때 여러개의 카메라를 사용할때 애를 먹은적이 있어 도움이 되는 분들이 있을까 하고 포스팅을 남깁니다.  
## ROS usb_cam pkg 가져오기

[github - ros-drivers/usb_cam](https://github.com/ros-drivers/usb_cam)  
작업하실 워크스페이스에서 git clone과 catkin_make을 먼저 진행 해줍시다.

```
$ cd {workspace/src}
$ git clone https://github.com/ros-drivers/usb_cam.git
$ cd .. && catkin_make && source devel/setup.sh
```

연결된 카메라를 확인 합니다. 이때 카메라에 할당되는 번호는 0번부터 차례대로 할당됩니다. 카메라가 확인되었으면 카메라에 대한 권한을 줍니다.

```
$ ls -l /dev/video*
```

권한 설정, `{number}` 는 위에서 확인된 사용할 카메라입니다. 그냥 `/dev/video*`로 모든 카메라에 대해 설정하셔도 무방합니다.

```
$ sudo chmod 777 /dev/video{number}
```

## usb_cam launch file수정하기

<!-- 그룹 노드 -->
<!-- 재귀 런치파일? -->

---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [github - ros-drivers/usb_cam](https://github.com/ros-drivers/usb_cam)
- [ROS Wiki - How do I create dynamic launch files?](https://answers.ros.org/question/229489/how-do-i-create-dynamic-launch-files/)
- [ROS:: usb캠 사용하기, fps 향상 (usb_cam, logitech c920)](https://m.blog.naver.com/PostView.nhn?blogId=nswve&logNo=221483691234&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>
