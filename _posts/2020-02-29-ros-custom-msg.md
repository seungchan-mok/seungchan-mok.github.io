---
layout: post
title: "ROS Custom msg 튜토리얼"
description: ROS Custom msg 튜토리얼
# tags: etc
date: 2020-02-26 15:13:13
comments: true
---

# ROS Custom msg 튜토리얼

ROS에서 노드에서 message를 topic으로 publish하는 것으로 통신합니다. 이때 message는 topic의 지정된 데이터 구조를 갖는 자료형을 말합니다.  
ROS에서는 Custom message를 지원합니다. ROS kinetic을 기준으로 간단한 튜토리얼을 해보겠습니다.  

## Custom msg definition

먼저 msg type에 대한 파일을 만들어 주어야 합니다. 이 파일의 형식은 다음과 같습니다.

```
fieldtype1 fieldname1
fieldtype2 fieldname2
fieldtype3 fieldname3
```

> 출처 : [ROS Wiki - msg](http://wiki.ros.org/msg)

이때 fieldtype은 표준 자료형 혹은 ROS msg type이어야 합니다.

`예시`
```
Header header
int64 num1
Pointcloud2 point_cloud
```

보통 이 msg파일은 package폴더내에 msg폴더에 위치하는것이 일반적입니다.  
msg파일을 패키지와 함께 build하기위해선 패키지의 CMakeList.txt에서 빌드 스크립트를 호출해야 합니다.  
<!-- 여기 내용확인! 빌드안됨.. -->
먼저 `package.xml`에서 다음 두줄을 추가해 줍니다.


---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [ROS - WIKI - msg](http://wiki.ros.org/msg)
- [ROS - WIKI - Messages](http://wiki.ros.org/Messages)
- [ROS - WIKI - Creating a ROS msg and srv](http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Creating_a_msg)
- [ROS - WIKI - Defining Custom Messages](http://wiki.ros.org/ROS/Tutorials/DefiningCustomMessages)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>
