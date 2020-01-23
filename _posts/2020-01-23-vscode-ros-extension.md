---
layout: post
title: "vscode ros 환경 구축하기"
description: Jekyll을 이용한 github 블로그 만들기
tags: ROS
date: 2020-01-23 21:31:58
comments: true
---
# vscode ros 환경 구축하기

vscode는 마이크로소프트에서 배포한 윈도우, 맥, 리눅스에서 무료로 사용가능한 source code편집기입니다. 또 github에 source code가 올라와 있습니다. 그래서 사용자들이 개발한 extension들이 많다는 것이 큰 장점입니다. 이중 자율주행 분야, 특히 ROS개발 환경을 구축하는 방법에 대해 간략히 설명하겠습니다.

## ROS Extension 설치

먼저 vscode에서 `ctrl + shift + x`를 눌러 extensions tab을 켜줍니다. 여기서 `ROS`를 검색한후 아래의 extension을 설치합니다.

![](/../image/vscode_ros_commandline.png)

먼저 기본 설정을 해줍니다. `ctrl + ,`를 눌러 setting탭으로 이동후 `Extensions` > `ROS`를 클릭한 뒤 자신의 환경에 맞는 distro를 설정해 줍니다. 저는 kinetic을 사용하고 있기 때문에 kinetic을 적어 주었습니다.

![](/../image/ros_distro.png)

그 뒤 ROS workspace를 열어 줍니다. 여기선 ROS tutorial에서 처음 만들게되는 `catkin_ws`를 예로 들겠습니다.  
폴더를 열었다면 `ctrl + shift + p`를 눌러 command pallet를 열어줍니다.

![](/../image/update_path.png)

## ROS Command Line

아래는 각 명령어에 대한 설명입니다.

- `ROS:Preview URDF` : URDF파일을 Rviz에 띄우지 않고 미리보기를 할 수 있습니다. urdf파일에서 명령어를 실행하면 아래와 같이 미리보기 화면을 제공합니다.
![](/../image/ros_urdf_preview_in_vscode.png)
- `ROS:Update Python Path` : ROS python에 대한 Path를 설정합니다. `~/.bashrc`에 export되어 있는 ROS Path뿐만 아니라 각 패키지 별 `CMakeList.txt`의 dependency 설정에 따라 Path를 설정해 줍니다. Path를 설정하게 되면 ROS Python의 자동완성기능을 사용할 수 있습니다.
- `ROS:Update C++ Path` : 마친가지로 cpp의 Path를 설정해 줍니다.
- `ROS:Create Terminal` : Terminal이 생성됩니다. 하지만 workspace별로 `source devel/setup.sh`는 다시 해주어야 합니다.
- `ROS:Create Catkin Package` : workspace에서 `catkin_create_pkg <package_name> [depend1] [depend2] [depend3]`을 한 결과와 같습니다. catkin pkg을 생성합니다.
- `ROS:Run a ROS executable (rosrun)` : workspace 별로 `source devel/setup.sh`을 하지 않아도 바로 패키지를 실행할 수 있습니다. 기능은 `rosrun <pkg> <node>` 와 같습니다. 저는 이 명령어에 단축키를 할당했습니다.
- `ROS:Start Core` : roscore와 동일합니다.
- `ROS:Run a ROS launch file (roslaunch)` : roslaunch 와 동일합니다. 마찬가지로 단축키를 할당했습니다.
- `ROS:Show Core Status` : roscore의 status를 보여 줍니다. topic과 service 뿐만아니라 실행중인 node, node에서 publish, subscribe하는 topic까지 보여주기 때문에 가장 강력한 기능이라고 생각됩니다.
- `ROS:Stop Core` : roscore를 종료합니다.

## Attach Debug

- break point를 이용한 debug기능을 제공합니다. [설명](https://github.com/ms-iot/vscode-ros/blob/master/doc/debug-support.md#attach)

---
참고문서
<details>
<summary>참고문서</summary>
<div markdown="1">
- [Extensions:ROS](https://marketplace.visualstudio.com/items?itemName=ms-iot.vscode-ros)
</div>
</details>