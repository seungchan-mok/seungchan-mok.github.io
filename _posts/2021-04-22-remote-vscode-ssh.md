---
layout: post
title: "vscode와 ssh를 이용한 원격 개발환경 세팅"
description: vscode, ssh를 이용한 원격 개발환경 구축에 대해 다룹니다.
tags: etc
date: 2021-04-22 17:26:32
comments: true
---

# 원격 개발 환경

[vscode](https://github.com/microsoft/vscode)는 마이크로스프트사의 오픈소스 기반 코드 에디터입니다.
다양한 확장프로그램들이 개발되어 있고, 그중에서 원격 개발환경 구축을 도와주는 [remote-development](https://github.com/Microsoft/vscode-remote-release)가 매우 유용해 소개해 드리려고 합니다.
ssh를 이용한 원격 개발환경, docker container로의 원격 개발 환경 그리고 마지막으로 원격 PC의 container로의 원격 개발환경 설정방법을 다룹니다.
추가적으로 vscode 를 이용한 ssh 포트 포워딩을 알아보겠습니다.

> 용어 혼란을 막기위해 앞으로 원격으로 이용하고자 하는 서버(워크스테이션, PC..)는 'Host'으로.  
> 원격으로 접속하는 내 PC(랩탑...)는 'Remote PC'로 통일합니다.

## remote - ssh

먼저 host IP 및 ssh 접속 Port와 당연하게도 접속 사용자와 password가 필요합니다.
remote PC의 vscode에서 [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh-edit) 확장프로그램을 설치해줍니다.  (링크가 동작하지 않는다면 vscode에서 ctrl+shift+x, cmd+shift+x로 extension창을 열어준뒤 'remote ssh'로 검색하면 됩니다.) 다음과 같은 화면에서 설치 합니다.

![img](https://i.imgur.com/tqvCELO.png)

다음으로 ssh config 파일을 수정해야 합니다. `ctrl` or `cmd` +`shift + p` 로 명령어 팔레트를 열어준 뒤, `Remote-SSH:Open Configuration File...` 메뉴를 입력해 선택해 줍니다. 

![img](https://i.imgur.com/hmaOtmB.png)

추가하고자 하는 Host를 `config` 파일에 다음과 같이 추가해 줍니다.

```bash
Host MyHost # 서버의 별명
    HostName dev.example.com # 서버의 IP address or domain
    User msc9533 # 서버에서 사용할 User
    Port 2322 # 서버 ssh 포트, 기본값 22
```

### authorized_keys 추가하기

이 단계를 이용한 방법은 필수는 아니지만 추천드리는 방법입니다. Host의 authorized_keys에 자주 사용하는 remote PC의 ssh pub key를 추가해 놓는 경우 비밀번호를 입력하지 않아도 되고, docker remote를 이용하기 위해 필요한 단계입니다. 

먼저 remote PC의 터미널에서 ssh-keygen을 실행합니다. 이 글에서는 Ubuntu를 기준으로 하고, Open-ssh가 설치되어있는 Windows 10, Mac 에서 동일하게 실행 됩니다.

### Forward a port by vscode

서버에서 port를 사용하는 경우 (ex. jupyter notebook) ssh를 이용해 포워딩을 하면 좀더 쉽게 이용을 할 수 있습니다. 터미널에서 명령어를 이용한 포워딩을 할 수 있지만 vscode에서도 지원하는 기능입니다. 주피터 노트북을 이용하는 경우를 예를 들겠습니다.  

서버에서 `8888`번 포트를 이용하는 경우 `ctrl` or `cmd` +`shift + p` 로 명령어 팔레트를 열어준 뒤, `Forward a Port` 메뉴를 입력해 선택해 줍니다. 


![img](https://i.imgur.com/PONCs2K.png)

메뉴를 선택하면 다음과 같은 화면을 확인 할 수 있습니다. vscode내부에서 실행된 경우(터미널..) 자동으로 포트포워딩이 되며, `Add Port`를 이용해 직접 추가해 줄 수 있습니다.

![img](https://i.imgur.com/QpJmRWz.png)

포트포워딩이 끝나면, localhost포트로 포워딩이 되며, 이 경우 `http://localhost:8888`를 브라우저에서 접속하면 서버의 8888포트로 연결이 됩니다.

<details>
<summary>참고문서</summary>
<div markdown="1">

- https://baked-corn.tistory.com/52
- https://linuxize.com/post/using-the-ssh-config-file/#ssh-config-file-example
- https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack

</div>
</details>

---

<script id="dsq-count-scr" src="//msc9533.disqus.com/count.js" async></script>

