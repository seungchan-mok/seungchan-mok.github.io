---
layout: post
title: "tf broad cast 예제"
description: tf broad cast 예제
# tags: ROS
date: 2020-02-26 15:13:13
comments: true
---

# [ROS] tf broad casting 사용하기

[지난 포스트](https://msc9533.github.io/2020/02/ros_tf_libaray/)에서 tf라이브러리를 사용하는 방법에 대해 간략히 알아보았었습니다. 이번 포스트에서는 [ROS wiki](http://wiki.ros.org/tf/Tutorials) 에 있는 내용을 기반으로 실제 tf broad casting이 어떻게 사용되는지 예제와 함께 알아 보도록 하겠습니다.

## Overview

## Simple odometry publisher

먼저 키보드로 입력을 받아 Control Input으로 부터 위치를 추정하고, 이 odometry를 publish 하는 node를 만들겠습니다. 키보드로부터의 input은 제 github에 있는 [serial_contorol](https://github.com/msc9533/serial_control)에 `cmd_vel`topic을 이용하겠습니다.

```bash
cd {work space}/src
git clone https://github.com/msc9533/serial_control.git
cd .. && catkin_make
source devel/setup.sh
```

이 키보드 입력을 받아 `odometry`를 publish 하는 node를 만들어 보겠습니다.

```bash
catkin_create_pkg odom_pub rospy roscpp nav_msgs tf tf2 geometry_msgs move_base_msgsasd

cd odom_pub/src && touch odom_pub.py && chmod u+x odom_pub.py
```

이 node의 source는 아래와 같습니다.

<!-- 단위 맞춰야 함 -->

```py
#!/usr/bin/env python
import rospy
import numpy
import sys, select, os
from std_msgs.msg import String
from nav_msgs.msg import Odometry
from geometry_msgs.msg import Twist
from math import cos, sin, tan
import tf
V,W = 0.0,0.0
x,y,heading = 0.0,0.0,0.0
previousTime = 0
def control_input(msg):
    global V,W
    print('callback!')
    V = msg.linear.x * 0.0001
    W = msg.angular.z * 0.01 #deg

def odom_pub():
    global V,W,x,y,heading
    rospy.init_node('odom_pub_node', anonymous=True)
    print('odom_pub_node!')
    sub = rospy.Subscriber('cmd_vel', Twist, control_input)
    rate = rospy.Rate(10) # 10hz
    odom_publisher = rospy.Publisher('odom_msg', Odometry,queue_size=1)
    previousTime = rospy.Time.now()
    while not rospy.is_shutdown():
        delta_time = (rospy.Time.now() - previousTime).to_sec()
        delta_transltation = V*delta_time
        delta_angle = W*delta_time
        heading = heading + delta_angle
        x = x + delta_transltation*cos(heading)
        y = y + delta_transltation*sin(heading)
        pubMsg = Odometry()
        pubMsg.header.stamp = rospy.Time.now()
        pubMsg.header.frame_id = '/map'
        pubMsg.pose.pose.position.x = x
        pubMsg.pose.pose.position.y = y
        quat = tf.transformations.quaternion_from_euler(0.0,0.0,numpy.deg2rad(heading))
        pubMsg.pose.pose.orientation.x = quat[0]
        pubMsg.pose.pose.orientation.y = quat[1]
        pubMsg.pose.pose.orientation.z = quat[2]
        pubMsg.pose.pose.orientation.w = quat[3]
        odom_publisher.publish(pubMsg)
        print('delta_time ',delta_time)
        print('xy : ' + str(x) + ','+ str(y))
        rate.sleep()

if __name__ == '__main__':
    try:
        odom_pub()
    except rospy.ROSInterruptException:
        pass
```

<!-- 키보드 입력 노드, 오도메트리 퍼블리시 따로 -->
<!-- tf broad cast node -->

<!-- 키보드로 오도메트리 퍼블리시하는 노드 -->
<!-- 여기에 tf 퍼블리시 -->
<!-- 라이다? -->

<!-- 변형예제?? -->

---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [ROS WIKI - Writing a tf broadcaster (C++)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28C%2B%2B%29)
- [ROS WIKI - writing a tf listener (C++)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28C%2B%2B%29)
- [ROS WIKI - Adding a frame(C++)](http://wiki.ros.org/tf/Tutorials/Adding%20a%20frame%20%28C%2B%2B%29)
- [ROS WIKI - Writing a tf broadcaster (Python)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28Python%29)
- [ROS WIKI - writing a tf listener (Python)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28Python%29)
- [ROS WIKI - Adding a frame(Python)](http://wiki.ros.org/tf/Tutorials/Adding%20a%20frame%20%28Python%29)
- [Publishing Odometry Information over ROS](http://wiki.ros.org/navigation/Tutorials/RobotSetup/Odom)


</div>
asd
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

