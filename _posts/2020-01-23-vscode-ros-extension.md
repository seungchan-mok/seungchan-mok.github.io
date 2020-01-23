---
layout: post
title: "vscode ros 환경 구축하기"
description: Jekyll을 이용한 github 블로그 만들기
tags: ROS
date: 2020-01-23 21:31:58
comments: true
---
**작성중**
# vscode ros 환경 구축하기

vscode는 마이크로소프트에서 배포한 윈도우, 맥, 리눅스에서 무료로 사용가능한 source code편집기입니다. 또 github에 source code가 올라와 있습니다. 그래서 사용자들이 개발한 extension들이 많다는 것이 큰 장점입니다. 이중 자율주행 분야, 특히 ROS개발 환경을 구축하는 방법에 대해 간략히 설명하겠습니다.

## ROS Extension 설치

먼저 vscode에서 `ctrl + shift + x`를 눌러 extensions tab을 켜줍니다. 여기서 `ROS`를 검색한후 아래의 extension을 설치합니다.

![](/../image/ros_extension.png)

### ROS Path 설정

그 뒤 ROS workspace를 열어 줍니다. 여기선 ROS tutorial에서 처음 만들게되는 `catkin_ws`를 예로 들겠습니다.  
폴더를 열었다면 `ctrl + shift + p`를 눌러 command pallet를 열어줍니다.

![](/../image/update_path.png)
