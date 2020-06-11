---
layout: post
title: "Iterative Closest Point(ICP) 구현하기"
description: Iterative Closest Point(ICP) 구현하기
#tags: ROS
date: 2020-05-27 10:38:08
comments: true
---

<!-- icp매칭이란? -->
Iterative Closest Point(ICP)는 두 point cloud집합의 차이를 최소화 하는 알고리즘입니다. 
  
![Imgur](https://i.imgur.com/JVSCQ7P.png)  

[wiki](https://en.wikipedia.org/wiki/Iterative_closest_point)에 서술되어 있는 알고리즘은 다음과 같습니다.  

1. source pointcloud와 reference pointcloud의 점 쌍 선택
2. 선택한 점들의 쌍이 일치하게 변환 행렬 추정
3. 선택한 쌍의 일치율로 가중치 부여
4. source pointcloud를 추정한 행렬로 변환
5. 반복

<!-- 구현코드 -->

<!-- 장단점 -->
<!-- 비슷한알고리즘 -->

---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [ICP( Iterative Closest Point )](https://m.blog.naver.com/tlaja/220666876033)
- [Iterative_closest_point - wiki](https://en.wikipedia.org/wiki/Iterative_closest_point)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

