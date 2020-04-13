layout: post
title: "Otsu's Method 구현하기"
description: Otsu's Method 구현하기
# tags: Algorithm
date: 2020-04-09 10:26:00
comments: true
---

# Otsu's method

<!-- 무슨알고리즘인지 간략하게 -->
<!-- 핵심 알고리즘 원리 -->
<!-- 수식적으로 해석 -->
<!-- 직접구현 cpp? python? -->
<!-- 파생 알고리즘은 뭐가 있는지 -->
```py
import matplotlib.pyplot as plt
from PIL import Image
import numpy as np

img = np.array(Image.open('lenna.png').convert('L'))
plt.imshow(img,cmap='gray',vmin=0,vmax=255)
plt.show()


def otsu(arg):
    N = 220*220
    histogram = [0]*255
    # make histo
    for line in arg:
        for pixel in line:
            histogram[pixel] = histogram[pixel] + 1
    
    sumofHistogram = sum(histogram)
    meanofHistogram = float(sumofHistogram)/(N*2)
    for index in range(len(histogram)):
        if meanofHistogram > histogram[index]:
            histogram[index] = 0
    
    threshold = 0
    sumA = 0.0
    sumB = 0.0
    q1 = 0
    q2 = 0
    varMax = 0.0
    for i in range(len(histogram)):
        sumA += i*histogram[i]
    for i in range(len(histogram)):
        q1 += histogram[i]
        if q1 == 0:
            continue
        q2 = N - q1
        if q2 == 0:
            break
        sumB += float(i*histogram[i])
        m1 = sumB / q1
        m2 = (sumA - sumB) / q2
        varBetween = float(q1) * float(q2) * (m1 - m2) * (m1 - m2)
        if varBetween > varMax:
            varMax = varBetween
            threshold = i
    return threshold

thres = otsu(img)
for i in range(len(img)):
    for j in range(len(img[i])):
        if thres > img[i][j]:
            img[i][j] = 0
        else:
            img[i][j] = 255

plt.imshow(img,cmap='gray',vmin=0,vmax=255)
plt.show()
```
---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [wikipedia-Otsu's method](https://en.wikipedia.org/wiki/Otsu%27s_method)
- ["A threshold selection method from gray-level histograms",Otsu, N. (1979).](http://webserver2.tecgraf.puc-rio.br/~mgattass/cg/trbImg/Otsu.pdf)
- [Otsu 방법을 사용해서 이미지 이진화하기 (matlab 소스코드 포함)](https://bskyvision.com/49)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

