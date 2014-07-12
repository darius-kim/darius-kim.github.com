---
layout: post
title: "Visual Studio를 이용하여 리눅스 개발하기"
description: ""
category: 리눅스
tags: [Linux Application]
---
{% include JB/setup %}


## 1. 개요

 일반적으로 윈도우에서 리눅스 app를 개발 할 때, samba로 연결하고, notepad++나 ultraedit로 편집하고, ssh로 들어가서 컴파일 하는 방법을 사용 합니다. 그러나 위의 툴들은 IDE들에 비해서 기능이 떨어 지는 것은 사실입니다. IDE의 끝판왕이라 불리는 Visual studio와 비교 하면 더말 할 필요도 없습니다. 그렇다면, visual studio를 리눅스 app을 개발 하는데 써보면 어떨까요? 물론 디버깅은 불가능 하지만, **편집, 컴파일**까지는 아주 편안하게 할 수 있습니다. 난 죽어도 리눅스에서 개발 못하겠다. 윈도우에서 해야 겠다 라고 생각 하시면, visual studio도 하나의 좋은 대안이 될 수 있습니다.

## 2. 준비물

    1. 네트워크가 연결되어 있고, 데비안이 깔린 라즈베리 파이 또는 BBB
    2. Visual Studio (여기서는 2008을 가지고 진행 합니다.)
    3. sshfs 또는 samba (여기서는 sshfs를 기준으로 설명 합니다.) 
    4. putty
    
## 3. 사용법

 사용 방법은 일반적으로 visual studio에서 사용 하는 방법과 동일 합니다. new project를 선택해서 project를 생성하고 진행 하면 되는데, 프로젝트 타입만 바꿔서 지정 하면 됩니다.
 
![image]({{ site.url }}/assets/images/2014-07-11-develop-linux-app-using-visual-studio.img/Windows_7_2.png)


프로젝트 타입을 Makefile 로 설정 하고, 이름을 지정 후에 저장 합니다.

그리고, 빈프로젝트에서 소스를 추가 하고, project 설정을 확인 합니다.

![image]({{ site.url }}/assets/images/2014-07-11-develop-linux-app-using-visual-studio.img/Windows_7_1.png)

여기에서 가장 중요한 부분이 Build Command Line/Rebuild Command Line/Clean Command Line 입니다.

**Build Command Line**
    
    "C:\Program Files\PuTTY\plink" ubuntu@192.168.10.4 -pw temppwd "cd make1/make1;g++ -o hello main.cpp"

**Rebuild Command Line**
    
    "C:\Program Files\PuTTY\plink" ubuntu@192.168.10.4 -pw temppwd "cd make1/make1;rm -f *.o;g++ -o hello main.cpp"

**Clean Command Line**
    
    "C:\Program Files\PuTTY\plink" ubuntu@192.168.10.4 -pw temppwd "cd make1/make1;rm -f *.o"

여기서 주의 깊게 봐야 하는 부분이 `-pw temppwd` 부분인데, `-pw`는 비밀번호 이후에 오는 것은 ssh 연결해서 실행할 명령어가 됩니다. `make` 파일을 만들었다면, `make` 명령어를 넣으면 되고, 지금 처럼 개별 파일만 있을 경우에는 컴파일 명령어만 넣어 주시면 됩니다.

인텔리 센스를 이용하기 위해서는 `Include Search Path`에 헤더 파일이 들어 있는 경로를 지정 합니다. sshfs는 /를 바로 마운트 할 수 있기 때문에 /로 마운트 하였습니다.

![image]({{ site.url }}/assets/images/2014-07-11-develop-linux-app-using-visual-studio.img/Windows_7.png)

`Include Search Path` 아직 특별한 헤더가 필요하지 않기 때문에, `/usr/include`만 추가 했습니다.

    Y:\usr\include

이제 Build 메뉴에서 build/rebuild/clean을 사용 할 준비가 되었습니다. visual studio의 강력한 인텔리센스를 이용하여 리눅스 앱을 개발 하실 수 있습니다.

**주의) 반드시 프로젝트 파일과 소스는 타겟 디바이스에 저장 되어야 합니다. 에디터만 PC를 이용하는 것이기 때문에, 소스 저장과 컴파일은 타겟에서 이뤄 집니다. 본 강좌의 예제는 BBB를 사용 하였기 때문에, /home/ubuntu 밑에 저장 하였습니다.**
