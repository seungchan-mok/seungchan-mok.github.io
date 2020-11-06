---
layout: post
title: "NIPA docker 한줄설치"
description: NIPA docker 한줄설치
tags: etc
date: 2020-11-06 18:14:13
comments: true
---

# NIPA docker 한줄설치

```
wget https://gist.githubusercontent.com/msc9533/1e2bf4c5acbe60a89726f7a2d3a77f6f/raw/a4aac3c007464c726401ff609fe7034c2ff85250/nipa_docker_install.sh && sudo chmod +x nipa_docker_install.sh && ./nipa_docker_install.sh
```

이후에 컨테이너 내부에서 실행된 `nvidia-smi`가 보이면 정상설치완료 입니다.

추후 sudo 없이 docker사용을 위해 

```
sudo usermod -aG docker {유저명}
sudo reboot
```

한 뒤 다시 접속하시면 됩니다.


---

<details>
<summary>참고문서</summary>
<div markdown="1">

- [NIPA x Docker !](https://jjerry-k.github.io/deeplearning/2020/06/28/nipa_docker/)

</div>
</details>
<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

