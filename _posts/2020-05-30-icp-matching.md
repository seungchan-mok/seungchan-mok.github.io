---
layout: post
title: "Iterative Closest Point(ICP) 구현하기"
description: Iterative Closest Point(ICP) 구현하기
#tags: Algorithm
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

위 알고리즘이 반복되면서 점점 최대로 일치하는 변환 행렬에 가까워져 가는 방식으로 실행됩니다.

## Simple implementation with python sklearn

먼저 python에서 sci-kit learn을 이용해 간단히 구현해 보겠습니다.

<!-- skleran code -->
<!-- python 구현중 -->
```py
import sys
import random
import numpy as np
import matplotlib.pyplot as plt
from math import pi,sin,cos
from sklearn.neighbors import NearestNeighbors

def nearest_neighbor(src, dst):
    '''
    Find the nearest (Euclidean) neighbor in dst for each point in src
    Input:
        src: Nxm array of points
        dst: Nxm array of points
    Output:
        distances: Euclidean distances of the nearest neighbor
        indices: dst indices of the nearest neighbor
    '''
    neigh = NearestNeighbors(n_neighbors=1)
    neigh.fit(dst)
    distances, indices = neigh.kneighbors(src, return_distance=True)
    return distances.ravel(), indices.ravel()

# 이 함수 동작원리?
def best_fit_transform(A, B):
    '''
    Calculates the least-squares best-fit transform that maps corresponding points A to B in m spatial dimensions
    Input:
      A: Nxm numpy array of corresponding points
      B: Nxm numpy array of corresponding points
    Returns:
      T: (m+1)x(m+1) homogeneous transformation matrix that maps A on to B
      R: mxm rotation matrix
      t: mx1 translation vector
    '''

    assert A.shape == B.shape

    # get number of dimensions
    m = A.shape[1]

    # translate points to their centroids
    centroid_A = np.mean(A, axis=0)
    centroid_B = np.mean(B, axis=0)
    AA = A - centroid_A
    BB = B - centroid_B

    # rotation matrix
    H = np.dot(AA.T, BB)
    U, S, Vt = np.linalg.svd(H)
    R = np.dot(Vt.T, U.T)

    # special reflection case
    if np.linalg.det(R) < 0:
       Vt[m-1,:] *= -1
       R = np.dot(Vt.T, U.T)

    # translation
    t = centroid_B.T - np.dot(R,centroid_A.T)

    # homogeneous transformation
    T = np.identity(m+1)
    T[:m, :m] = R
    T[:m, m] = t

    return T, R, t

def getRandomTransform():
    theta = random.random()*2.0*pi
    x = random.random()*10.0
    y = random.random()*20.0
    return np.array([[cos(theta),-sin(theta),x],
                    [sin(theta),cos(theta),y],
                    [0,0,1]])

def getSamplePoints(start,end,step,addNoise=True):
    src_x = np.arange(start,end,step)
    src_y = np.sin(src_x)
    ones = np.ones(len(src_x))
    src = np.stack((src_x,src_y,ones))
    src_temp = np.copy(src)
    if addNoise:
        noise_x = np.random.rand(len(src_x))*0.2
        noise_y = np.random.rand(len(src_x))*0.2
        noise = np.stack((noise_x,noise_y,np.zeros(len(src_x))))
        src_temp = src_temp + noise
    dst = np.matmul(getRandomTransform(),src_temp)
    return src,dst    

def icp():
    src, dst = getSamplePoints(0,10,0.1)
    print(src)
    plt.scatter(src[0],src[1])
    plt.scatter(dst[0],dst[1])
    plt.show()

def main():
    icp()

if __name__ == "__main__":
    main()
```

## Algorithm

위에 서술한 알고리즘의 순서대로의 설명 및 구현입니다.

### source pointcloud와 reference pointcloud의 점 쌍 선택

먼저 점을 선택하기 위해서 가장 가까운 점을 찾아야 합니다. ICP에는 많은 변형 알고리즘들이 있고, 가장 가까운 점을 찾는 부분 부터 여러 방법으로 나뉩니다.

<!-- 구현코드 -->
## C++구현

```cpp
#include <iostream>
#include <vector>

int main()
{

}
```
<!-- 장단점 -->
<!-- 비슷한알고리즘 -->


---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [ICP( Iterative Closest Point )](https://m.blog.naver.com/tlaja/220666876033)
- [Iterative_closest_point - wiki](https://en.wikipedia.org/wiki/Iterative_closest_point)
- [Iterative Closest Point (ICP) implementation on python - stack overflow](https://stackoverflow.com/questions/20120384/iterative-closest-point-icp-implementation-on-python)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

