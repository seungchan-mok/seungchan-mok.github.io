---
layout: post
title: "ROS launch파일 작성하기"
description: ROS launch파일 작성하기
tags: ROS
date: 2020-05-27 10:38:08
comments: true
---

# ROS launch?

ROS는 node단위로 파일을 실행시키거나 rosparam과 같이 개별적으로 실행 시킬 수 있다는 것에서 장점을 가지고 있습니다. 하지만 프로젝트의 크기가 커질 경우 node를 각각 실행시키기 번거로울 때가 있습니다. 이때 roslaunch가 한번에 여러 node를 실행시키거나 같은 node를 여러개 쓰는 작업을 쉽게 해줄 수 있습니다.
<!-- 런치파일이 무엇인지 -->
<!-- 런치파일의 장점 -->
## Multiple Node

rosrun 으로 실행시키는 노드들을 한번에 launch파일로 작성할때는 다음과 같이 작성합니다.  
launch파일이 pkg에 포함되는것이 일반적이지만 굳이 포함할 필요는 없습니다. 이때 실행되는 node들은 `devel/setup.sh`에서 package의 path가 source되어야 합니다.(한 터미널에서 모두 실행시킬 수 있는 상태여야 합니다.)

```xml
<launch>
  <node name="turtlesim_node" pkg="turtlesim"  type="turtlesim_node" respawn="true" />
  <node name="turtle_teleop_key_node" pkg="turtlesim" type="turtle_teleop_key" output="screen" />
</launch>
```

`<node name="NODE_NAME" pkg="PACKAGE" type="NODE" />`의 형식으로 작성합니다. 만약 패키지를 실행시키는 rosrun command가 `rosrun turtlesim turtlesim_node`라면 `<node name="NODE_NAME" pkg="turtlesim" type="turtlesim_node" />` 같이 작성합니다. node name은 이미 실행되고 있는 이름이 아니라면 무관합니다.  

또한 다른 옵션들을 줄 수 있습니다.
- `respawn="true/false"` : node가 종료 되었을때 다시 실행시키는 옵션입니다.
- `output="screen"` : 기본적으로 launch를 실행하면 rosrun과는 달리 출력이 보이지 않습니다. 이 출력을 활성화하는 옵션입니다.
- `args="x y z ...."` : node가 argument를 필요로 하는 경우 작성합니다.

## Group nodes

```xml
<launch>
    <group ns="group1">
        <node name="turtlesim_node" pkg="turtlesim"  type="turtlesim_node" respawn="true" />
        <node name="turtle_teleop_key_node" pkg="turtlesim" type="turtle_teleop_key" output="screen" />
    </group>
    <group ns="group2">
        <node name="turtlesim_node" pkg="turtlesim"  type="turtlesim_node" respawn="true" />
        <node name="turtle_teleop_key_node" pkg="turtlesim" type="turtle_teleop_key" output="screen" />
    </group>
</launch>
```

`<group>`태그는 node를 그룹으로 실행시킬때 사용하는 태그입니다.  
위와 같은 launch를 실행하면 그룹으로 묶여 node가 실행됩니다. 이때 node의 이름은 /group_name/node_name 으로 생성됩니다. topic도 마찬가지로 /group_name/topic으로 생성됩니다. 아래는 rqt_graph로 node를 확인한 결과입니다.

![Imgur](https://i.imgur.com/F0ebMdP.png)

## Include other launch, arguments, param

launch에서 다른 launch file을 포함하거나, arguments,param등도 이용가능합니다.

```xml
<launch>
<!-- 다른 launch 포함 -->
<include file="$(find velodyne_pointcloud)/launch/VLP16_points.launch">
  </include>
<!-- arguments -->
<arg name="arg" default="10" />
<arg name="model" default="RobotA" doc="type your robot[RobotA, RobotB, RobotC]"/>
<!-- 직접 rosparam setting -->
<param name="use_sim_time" value="true" />
<!-- yaml 파일로 부터 rosparam 불러오기 -->
<rosparam file="$(find local_map_builder)/config/config.yaml" />
</launch>
```

여기서 `$(find package)`는 현재 source되어 있는 `devel/setup.sh`에서 실행가능한 package의 경로를 찾아줍니다. `arg`는 launch를 실행시킬때 뒤에 `arg:=12`와 같이 arguments를 설정할 수 있습니다.  
`param`은 rosparam을 직접 설정하는 명령어이고, yaml file로 여러개의 rosparam을 한번에 설정할 수도 있습니다.

<!-- node실행시키기 - respawn screen 등등 -->
<!-- launch 포함시키기 -->
<!-- param, arg -->



---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [Roslaunch tips for large projects](http://wiki.ros.org/ROS/Tutorials/Roslaunch%20tips%20for%20larger%20projects)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

