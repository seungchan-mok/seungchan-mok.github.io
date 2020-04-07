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
`rosrun`을 이용해 하나의 usbcam노드를 실행시키는 것은 아무런 문제가 없습니다. 하지만 2개 이상의 노드를 실행시키면 중복된 노드를 켤수 없다는 오류메시지가 뜨며 하나의 노드의 실행이 중지됩니다. 2개이상의 같은 노드를 실행시키기 위해서는 이 노드를 분리해서 실행 시켜야 합니다.
먼저 2개의 usb cam을 사용하는 launch파일은 다음과 같습니다.
```
<launch>
 <group ns="camera1">
  <node name="usb_cam1" pkg="usb_cam" type="usb_cam_node" output="screen" >
    <param name="video_device" value="/dev/video0" />
    <param name="image_width" value="640" />
    <param name="image_height" value="480" />
    <param name="pixel_format" value="mjpeg" />
    <param name="camera_frame_id" value="yuyv" />
    <param name="io_method" value="mmap"/>
  </node>
  <node name="image_view" pkg="image_view" type="image_view" respawn="false" output="screen">
    <remap from="image" to="/camera1/usb_cam1/image_raw"/>
    <param name="autosize" value="true" />
  </node>
 </group>
<group ns="camera2">
  <node name="usb_cam2" pkg="usb_cam" type="usb_cam_node" output="screen" >
    <param name="video_device" value="/dev/video1" />
    <param name="image_width" value="1280" />
    <param name="image_height" value="720" />
    <param name="pixel_format" value="mjpeg" />
    <param name="camera_frame_id" value="yuyv" />
    <param name="io_method" value="mmap"/>
  </node>
  <node name="image_view" pkg="image_view" type="image_view" respawn="false" output="screen">
    <remap from="image" to="/camera2/usb_cam2/image_raw"/>
    <param name="autosize" value="true" />
  </node>
 </group>
</launch>
```

`group`을 이용해 같은 노드가 실행될 수 있게합니다. 이때 publish되는 topic명은 camera1/usb_cam1/image_raw,camera2/usb_cam2/image_raw 와 같이 {group name}/{camera name}/image_raw로 publish 됩니다.
2개 이상의 카메라를 쓰는 경우 camera3,camera4...처럼 실행 시킬수도 있습니다.


---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [github - ros-drivers/usb_cam](https://github.com/ros-drivers/usb_cam)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>
