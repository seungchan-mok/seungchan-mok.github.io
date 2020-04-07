---
layout: post
title: "tf broad cast 예제"
description: tf broad cast 예제
tags: ROS
date: 2020-04-08 00:16:00
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
from math import cos, sin, tan,pi
import tf
V,W = 0.0,0.0
x,y,heading = 0.0,0.0,0.0
previousTime = 0
def control_input(msg):
    global V,W
    print('callback!')
    V = msg.linear.x * 0.0001
    W = -msg.angular.z *(pi/180) * 0.01 
    # subscribe되는 단위는 arduino에서 쓰기 편하게 맞춘 단위이므로 
    # scale을 낮춰 주었습니다.

def odom_pub():
    global V,W,x,y,heading
    rospy.init_node('odom_pub_node', anonymous=True)
    print('odom_pub_node!')
    sub = rospy.Subscriber('cmd_vel', Twist, control_input)
    rate = rospy.Rate(10) # 10hz
    odom_publisher = rospy.Publisher('odom_msg', Odometry,queue_size=1)
    previousTime = rospy.Time.now()
    while not rospy.is_shutdown():
        # 시간의 변화량
        delta_time = (rospy.Time.now() - previousTime).to_sec()
        # 이동한 거리입니다.
        delta_transltation = V*delta_time
        delta_angle = W*delta_time
        # 각도의 변화량
        heading = heading + delta_angle
        # heading방향으로 delta_translation 만큼 이동했을때
        # 위치를 x,y좌표로 나타내면 아래와 같이 계산할 수 있습니다.
        x = x + delta_transltation*cos(heading)
        y = y + delta_transltation*sin(heading)

        # 계산된 위치로 Odometry를 publish해줍니다.
        pubMsg = Odometry()
        pubMsg.header.stamp = rospy.Time.now()
        pubMsg.header.frame_id = '/map'
        pubMsg.pose.pose.position.x = x
        pubMsg.pose.pose.position.y = y
        quat = tf.transformations.quaternion_from_euler(0.0,0.0,heading)
        pubMsg.pose.pose.orientation.x = quat[0]
        pubMsg.pose.pose.orientation.y = quat[1]
        pubMsg.pose.pose.orientation.z = quat[2]
        pubMsg.pose.pose.orientation.w = quat[3]
        odom_publisher.publish(pubMsg)
        rate.sleep()

if __name__ == '__main__':
    try:
        odom_pub()
    except rospy.ROSInterruptException:
        pass
```

여기까지의 결과는 다음과 같습니다.  

![](https://github.com/msc9533/msc9533.github.io/raw/master/_files/keyboard_odom.gif)

## tf broadcaster

이제 이 odometry를 이용해 tf broadcasting node를 만들겠습니다. 이 예제는 [cpp](###-tf-broadcaster---cpp)와 [python](###-tf-broadcaster---python)으로 나누어 진행하겠습니다.

### tf broadcaster - cpp

```cpp
#include <ros/ros.h>
#include <tf/transform_broadcaster.h>
#include <nav_msgs/Odometry.h>

void poseCallback(const nav_msgs::Odometry::ConstPtr &msg){
    static tf::TransformBroadcaster br;
    // tf 를 broad cast할 행렬
    tf::Transform transform;

    // msg 의 xyz로 translation matrix를 설정
    transform.setOrigin(tf::Vector3(msg->pose.pose.position.x,
                                    msg->pose.pose.position.y,
                                    msg->pose.pose.position.z));
    tf::Quaternion q;

    // rotation matrix의 역할을 할 quaternion 값을 설정
    q.setX(msg->pose.pose.orientation.x);
    q.setY(msg->pose.pose.orientation.y);
    q.setZ(msg->pose.pose.orientation.z);
    q.setW(msg->pose.pose.orientation.w);
    // set rotation
    transform.setRotation(q);
    // map - base_link의 tf를 broadcast
    br.sendTransform(tf::StampedTransform(transform, ros::Time::now(), "map", "base_link"));
}   

int main(int argc, char** argv){
  ros::init(argc, argv, "tf_broadcaster_cpp");
  ros::NodeHandle node;
  ros::Subscriber sub = node.subscribe("/odom_msg", 10, &poseCallback);

  ros::spin();
  return 0;
};
```

위와 같은 코드로 `src/tf_broadcast.cpp`를 만듭니다. 코드에 대한 설명은 주석으로 대체합니다.


### tf broadcaster - python

```py
#!/usr/bin/env python  
import rospy
import tf
from nav_msgs.msg import Odometry

def tf_broad(msg):
    br = tf.TransformBroadcaster()
    # xyz, quaternion값을 직접 sendTransform
    # map - base_link
    br.sendTransform((msg.pose.pose.position.x, 
                    msg.pose.pose.position.y, 
                    msg.pose.pose.position.z),
                    [msg.pose.pose.orientation.x,
                    msg.pose.pose.orientation.y,
                    msg.pose.pose.orientation.z,
                    msg.pose.pose.orientation.w],
                    rospy.Time.now(),"base_link","map")

if __name__ == '__main__':
    rospy.init_node('tf_broadcast_py')
    rospy.Subscriber('odom_msg',Odometry,tf_broad)
    rospy.spin()
```

위의 cpp 예제보다 간단한 모습입니다. ros python에서 많은 msg가 리스트 형식으로 대체되기 때문에 직접 sendTransform 함수에 arguments로 사용하는 것이 더 간단합니다.

### Result

위의 결과 화면은 다음과 같습니다.  

![](https://github.com/msc9533/msc9533.github.io/raw/master/_files/tf_broad_demo.gif)  

이렇게 위치에 대한 값을 구할 수 있을때 tf를 설정해주었습니다. ROS에서 rviz에 좌표계를 일치시켜 message를 visualize한 결과를 보거나 tf listener로 변환행렬을 쉽게 구하는 등 여러 가지로 쓰일 수 있습니다.

### + static_transform_publisher

차량에 고정된 센서같이 변환행렬의 값이 변하지 않는 경우, 위의 방법으로 일일이 tf를 만들기는 번거로운 부분이 있습니다. 이경우 tf 패키지의 static_transform_publisher를 사용합니다. 간단한 arguments로 실행할 수 있습니다.

```bash
rosrun tf static_transform_publisher x y z R P Y /frame_id /child_frame_id periods
```

위의 예제에 base_link에 z 축으로 1m만큼 위에 하나의 좌표계를 추가하는 예시를 들겠습니다.

```bash
rosrun tf static_transform_publisher 0 0 1 0 0 0 /base_link /sensor 10
```

실행하면 다음과 같은 결과를 얻을 수 있습니다.

![](https://github.com/msc9533/msc9533.github.io/raw/master/_files/static_tf.gif) 

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
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>


