---
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
```cpp
int Otzu_implementation(std::vector<pcl::PointXYZI> *input_vector,int threshold_max,int intensity_max)
{
    if(input_vector->size() == 0)
    {
        // std::cout << "return empty!" << endl;
        return intensity_max;
    }
    int* histogram = new int[intensity_max+1];
    //initializing
    for(int i = 0;i < intensity_max;i++)
    {
        histogram[i] = 0;
    }
    

    //make histogram
    for(size_t i = 0;i<input_vector->size();i++)
    {
        int histo_index = (int)input_vector->at(i).intensity;
        if(histo_index > intensity_max)
        {
            // cout << "out of histo index!" << endl;
            // histo_index = intensity_max;
        }
        else
        {
            histogram[histo_index]++;
        }
    }
    
    size_t N = input_vector->size();

    //high pass filter
    long long int sum_of_histo = 0;
    for(int i = 0;i < intensity_max;i++)
    {
        sum_of_histo += histogram[i];
    }
    double mean_of_histo = (double)sum_of_histo / (2.0*N);
    for(int i = 0;i < intensity_max;i++)
    {
        if(mean_of_histo > histogram[i])
        {
            histogram[i] = 0;
        }
    }

    int threshold = 0;
    float sum = 0;
    float sumB = 0;
    int q1 = 0;
    int q2 = 0;
    float varMax = 0;

    // Auxiliary value for computing m2
    for (int i = 0; i < intensity_max; i++){
      sum += i * ((int)histogram[i]);
    }

    for (int i = 0 ; i < intensity_max ; i++) {
      // Update q1
      q1 += histogram[i];
      if (q1 == 0)
        continue;
      // Update q2
      q2 = N - q1;

      if (q2 == 0)
        break;
      // Update m1 and m2
      sumB += (float) (i * ((int)histogram[i]));
      float m1 = sumB / q1;
      float m2 = (sum - sumB) / q2;

      // Update the between class variance
      float varBetween = (float) q1 * (float) q2 * (m1 - m2) * (m1 - m2);

      // Update the threshold if necessary
      if (varBetween > varMax) {
        varMax = varBetween;
        threshold = i;
      }
    }

    delete[] histogram;
    if (threshold > threshold_max)
    {
        threshold = threshold_max;
    }

    return threshold;
}
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

