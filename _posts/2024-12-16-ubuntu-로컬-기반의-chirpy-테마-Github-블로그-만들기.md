---
title:  "ubuntu 로컬 기반의 chirpy 테마 Github 블로그 만들기"
date: 2024-12-16
---
# ubuntu 24.04 LTS를 로컬로 사용하는 chirpy 테마 Github 블로그 만들기 (배포용)

<p align="right"> with Chat GPT for 어설픈 초보 feat. vscode by folk </p>

---
Last Edited : 2024-12-15

AWS Lightsail에서 ubuntu 24.04 인스턴스 설치 및 각종 ssh 관련 내용은 생략합니다.

-   어렵지 않아서 생략합니다.
    

  

남들 다 만드는 pc windows환경이 아니라 굳이 remote 서버에 설치하는 이유는 내 맘입니다.

  

remote 서버에 각종 프로그램등을 설치할 때는, 반드시 사전에 설치 환경부터 알아봐야 함을 역시 삽질을 통해 한 수 깨닫고 다시 이 작업을 진행합니다.

  

vscode와 ubuntu 24.04 LTS 연결방법은 생략합니다. (알아서 오시라는 말씀)

-   다만, remote ssh 연결관련하여, .ppk 를 반드시 .pem 으로 변환해야 하는 데, 이 때 Puttygen을 이용하는 방법이 가장 깔끔합니다.
    

  

이 포스팅은 ubuntu 24.04 LTS에 따른 프로그램들이 설치되지 않은 상태에서 작성된 내용입니다. 혹시 다른 프로그램이 이미 설치되어 있는 인스턴스에 설치하는 경우 예상못한 에러 등이 발생할 수 있습니다.

  

special thanks to : https://www.irgroup.org/posts/jekyll-chirpy/#google_vignette

----------

# 0. 최소세팅 (vscode 이용)

vscode로 진입하여 다음 작업 진행하기.

별다른 안내가 있을 때까지 ubuntu 디렉토리에서 작업합니다.

ubuntu@ip-XXX-XX-X-XXX:~$ 이런 프롬프트를 확인하시면 됩니다.

#### <span style="color:blue">sudo apt update</span>

#### <span style="color:blue">sudo passwd root</span>

  
  

# 1. Github 가입

가입 과정은 생략, 일단 가입만 하고 로그인까지만 진행합니다.

-   어렵지 않습니다.
    

  

# 2. chirpy 초기화를 위한 npm 설치

#### <span style="color:blue">sudo apt update && sudo apt upgrade -y</span>

----------

만약 아래와 같은 화면이 나온다면,

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcKSM6Ut19hkt0MHM6uQ8nkWy1yjTLp-zprAGnpL1O2G72apC8obEY46ISZplNwzuSM_mvvs1yTpTUoT3C1KhvIiSvliF5a7f3wNBcJrf-Ar_hVmVQxde-Yl7GqLEJwsfO-OHnDsA?key=Yh1qn3tRuuHwWUleH_6lDD37)

대부분의 경우, **keep the local version currently installed**를 선택하는 것이 좋습니다. 이 옵션은 로컬에서 수정한 설정(sshd_config)을 유지하며, SSH 접속 환경이 기존과 동일하게 유지됩니다.

----------

  

#### <span style="color:blue">curl -fsSL http<hi>s://deb.nodesource.com/setup_18.x | sudo -E bash -</span>

#### <span style="color:blue">sudo apt install -y nodejs<span>

  

설치 확인

#### <span style="color:blue">node -v</span>

#### <span style="color:blue">npm -v</span>

  

최신버전 확인

#### <span style="color:blue">sudo npm install -g npm@latest</span>

  

# 3. chirpy 테마 설치

## 1. ubuntu Git 설정

Git 사용자 정보를 설정. 아이디와 이메일은 예시이니, 각자 변경해서 사용하시기 바랍니다.

#### <span style="color:blue">git config --global user<hi>.name "codepoemkr"</span>

#### <span style="color:blue">git config --global user.email "codepoemkr<hi>@kmail.kr"</span>

  

# Git 설정 확인

#### <span style="color:blue">git config --list</span>

  

## 2. 블로그 프로젝트 생성

home 디렉토리에서 작업합니다.

현재 /home/ubuntu 라면 cd .. 명령어로 /home/ 디렉토리로 이동합니다.

ubuntu@ip-XXX-XX-XX-XXX:/home$ 이런 식으로 나와야 합니다.

#### <span style="color:blue">sudo mkdir -p /home/blog</span>

#### <span style="color:blue">sudo chown -R ubuntu:ubuntu /home/blog</span>

#### <span style="color:blue">cd /home/blog</span>

이 때 vscode도 작업 디렉토리를 이동합니다.

  
  

## 3. 로컬과 github 통신을 위한 access token 등록

1.  Github 로그인 후 오른쪽 상단 프로필 이미지 클릭
    
2.  중간부분에 Settings
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcPug37k41mfT8vJuSSqU20Nzl7d5yGzMeZ63C4Zf7mCxrMy0aJVyyFjRLvyS7eh6thxlP_DLxF2dPgRXmW95akxjzNsWKBBc7wKj4krC7QpIRsG_ye3lnX_8z9HodQKV8_veWwsQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

3.  왼쪽 맨 하단에 <> Developer Settings
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfz6rmQC0mnvFPYB9w3BV5OjcfQ7FGXS4QGAIuhpTGv0EaD-p5fc8FyA3guP36DSP34ITKsDbOuCkMu8JNO4_KF6P-EGQcxqC_R7rbDHZOXhebQ5y9o88g_f0HJlaLst7fz0UnR6w?key=Yh1qn3tRuuHwWUleH_6lDD37)

4.  왼쪽 하단 Personal access tokens
    

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcTqgCLtVAJwnRwaqnCSowd2X1acpEK_Sv317bmnkKPEhhMBNet1HXngk_bhmheDaRroNtiF7N4dpatUEVIlqOneZCNV4W7IaL5w4eVm0A61pEBc3G41jMe_4EOuL__mk_I8Kx54Q?key=Yh1qn3tRuuHwWUleH_6lDD37)

5.  tokens (classic) - Generate new token (classic) ( 보통 90일용 선택 )
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdmjC_L2Zbubekk6JFjWFysSoMr9Kb2obFFbuZLueQT1UFwkmQzrSNLxqSQmZQP7urgpjXaAbcng-iOI29FMupRRLoee03rh6IT7REvY0XKH7SVbKkC2qarUig2VKhZ2Bg_VMVf5g?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcxcRXzGht_7mS6jwt9E44pZSbiAmt4SvMhtBQ34jlmNBjmBWQQk_zM_aY1m-H6ixDNW6UrPRSULs5dDO99z_V1GdnYTBGFxacNWminqdA5_2bCNRDGW6N_ITBKvPkqZS9IzrqV?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfgAiRdpSOq5DyhVQP4x9QXjorRVmKucjpKKpKSj0wNA8r6eBAEIjLlztznnhdBC84RrgFeP4h10TBn27WbjefsSRATspqIhemtcbFU9eLZIC4MpniIv2SilgzUtiT_AHlk2mIF-w?key=Yh1qn3tRuuHwWUleH_6lDD37)

6.  비번 입력 후 하단의 classic 선택
    
7.  적당히 사용용도 입력 ( 필자는 vscode라고 함 )
    
8.  나도 잘 몰라서 그냥 다 선택 후 맨 하단 Generate token
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXegUWqIj1xt4F9mTC9XpQ03WVQUkAU7r5USs1lxxuVM8_qY8HH7QDQZ69b6sOkfvLbd7ZN5zDy7Utasy4WvZtefOjhQ1nUGHUICwxFPfrkhz715mzfTGGJI3pGiu-9PoEqfvdelVw?key=Yh1qn3tRuuHwWUleH_6lDD37)

9.  ghp_로 시작하는 토큰을 잘 복사해서 보관
    

  

#### <span style="color:blue">sudo git remote set-url origin http<hi>s://codepoemkr:ghp_XXXXXXXXXXXXXXXXXXXXXXXXX@github.com/codepoemkr/codepoemkr.github.io.git</span>

  
  
  

## 4. Chirpy 테마 fork

----------

  

chirpy 테마는 여러가지 방법으로 받을 수 있는 데, remote 서버를 로컬로 사용할 때는 folk가 가장 효율성과 성공율이 높은 듯 합니다.

아울러 커스터마이징, 애드센스 용도로도 다른 방법보다 좀 더 유리하다고 합니다.

일반 pc가 아닌 remote 서버에 jekyll 테마를 최초 설치하고자, 약 3일 밤낮을 삽질하였으나, 결론은 이 두번째 방법이 성공했고, 효율적인 측면에서도 필자는 이 방법으로 chirpy 테마를 설치합니다.

아울러 현재 보통의 Github는 default로 main 브랜치를 권장하고 있으나, chirpy 테마를 folk하면 default branch가 master로 세팅됩니다.

본인은 이것을 main 으로 변경 작업하고자 여러번 시도하였으나, 실력과 경험이 미천하여 그냥 master로 사용합니다.

----------

  
  

[https://github.com/cotes2020/jekyll-theme-chirpy/fork](https://github.com/cotes2020/jekyll-theme-chirpy/fork)

Github에 로그인 되어 있는지 확인하고, 로그인 상태로 위 링크를 사용해서 소스를 folk 받습니다. Create fork 클릭.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXewctXKVGCX3cLv53uMeGZ9G5sfzW9MfYDwEnG4Ri3IdtAWjD1fbQDX0kbcRjJdlJsjMRPH3rG5uHBuCdLttwUn17-WpBUe6LHC1K6tdvPUMdsix50HuuIFH7fL-lOndZUKJ0OS?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfNYkXWGQ8SbNNqP7rD8DsofysvE10k8O2PJz20asLQ0vAYOnCL1LqE2KUmExz0Uxs7IKY7bMZnC6EKHH1Jm5VLClwz1a1kH2VA9nebMxBOH1_q4pZU0_9a8ozI-kWi8AzF88ZKQA?key=Yh1qn3tRuuHwWUleH_6lDD37)

folk 성공

  

## 5. Repository 이름 바꾸기


github의 블로그를 기본 도메인을 사용할 때는 repository 이름을 <github 아이디>.github.io 형식으로 만들어야 합니다.

-   github 블로그는 기본적으로 아이디당 하나의 블로그 (주소)를 제공합니다.
    

-   github > fork 받은 저장소 > Settings 화면으로 이동
    
-   Repository name을 <github 아이디>.github.io 형식으로 변경
    
-   중간정도 내려서 Features - issues에도 체크 권장
    
-   rename 버튼 클릭
    
-   저장소 Code 화면으로 돌아오게 되며, 상단에 보면 방금 바뀐 이름으로 저장소 명이 보이게 될 겁니다.
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcTvbxQZ5eVRKv8Y2WFr1apTNQs3ABPZ9uE57CAY5gpENrckhWucJ3p7Bj1_uCMb95rW0PV3bdilvIMq_cB6XJ35DuIJV9xod0nvtYthF29ShfqKYeDmX9-Dt6F3iAXdB9wQiZA?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfZHoB4VWqzzoRrbq8HWDktH7-KuqnoCl4476zkxmMMeYbtWs8YsSUeeOksNnilKq9T9bN0Ny4-7R47ROMVOqP3BMPk3PbXk8KEkTsEOtsKUg7At89lUGpYDZJ_1cU_0Wjoh6Df?key=Yh1qn3tRuuHwWUleH_6lDD37)

----------

  

## 6. 소스 클론 내려받기

개인적으로 이 부분이 그동안 3일 밤낮의 삽질을 종결시켜준 부분이며, Github의 여러 기능 중 백미가 아닐까 생각됩니다. 그야말로 신세계가 펼쳐지는 기분.

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdFTh2OGFj1HNAvxe4XqEYlvSr1r-yVpIw9Jo-txvHeuOz7eiiT-0MGXeMaueEYIFl_2_ZOjG3gk_RmFyEIcOA2NqR7DebNVSgxxk7qy1WNPdRQKNOLNzeTYhUVLb7ZxR-qTp31Xw?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

HTTPS 복사

vscode 터미널에서 ubuntu@ip-XXX-XX-X-XXX:/home/blog$ 상태 확인

#### <span style="color:blue">git clone http<hi>s://github.com/codepoemkr/codepoemkr.github.io.git</span>

  

성공!

  

이제 디렉토리를 codepoemkr<hi>.github.io 로 터미널과 vscode에서도 File -> Open Folder에서 변경해 줍니다.

  

ubuntu@ip-XXX-XX-X-XXX:/home/blog/codepoemkr.github.io$

이런 상태가 인지 확인하세요.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcTm5WQXSg9Z_0jhI8P1AwGK17Y6i9KzVrTVCbIwnNfSqw0rN0rd4gXWmiTVWVHoKpFckqX355jR3eX0i4N2547Y-A16amH-t1VK0mz-5Vpu1lvOmuAWgBUYHqi63AQtWh0PbYJrg?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXde5aOXedblydU-Dd8N5PEUwUf99yvWzUVRZtFT3CMD5zAWa_cbLhh3BRYA--IN4icNR3ZgtoAtrV0smPXNuI5RkeOB_VVnCFj9NKYTJHHC6tLyUcbaHoofIICN7olzM_Y7CdfVIg?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

## 7. chirpy 초기화 하기

#### <span style="color:blue">cd codepoemkr<hi>.github.io</span>

#### <span style="color:blue">tools/init.sh</span>

아주 기가 맥힙니다 ㅋ

중간에 노란색 warn나와도 쿨하게 넘어가세요.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfivrLyZdox6S64IFi8GWcATIRnHTaeO21d8QTJbpGuUnQ9TdoZUNcA7Tf-NFVi9K14QYvdFir0YDW9Tc2AN37gsxNX22DdtnCkO27hweuZyH4M0f507-IMSsjMaO3Ww_ft06YlBQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

#### 토큰 설정

#### <span style="color:blue">sudo git remote set-url origin http<hi>s://codepoemkr:ghp_XXXXXXXXXXXXXXXXXXXXXXX@github.com/codepoemkr/codepoemkr.github.io.git</span>

  

#### <span style="color:blue">git config pull.rebase false</span>

일단 강제 병합 설정정함.

  

그리고 나서 Sync Changes 한번 클릭

  
  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdqxq2AC6Ft0pIgKWHN80Nfmzoa7BwiFe4IUtcLZpqNQ8-HsyTsXsVwbpbbDMzLVQACxblRtr1uynogZU_r4dWQT1ofhDIiRnfVQ4CfPKQWB6lyTBTADEelTnC3qL-SamhXBA7m?key=Yh1qn3tRuuHwWUleH_6lDD37)

물론 OK

github -> Actions 에서 기분좋은 성공

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXde2dvfs0AQ1egRqcsqLghlavfxXAZJ8GeamBG0ig6oVa0eB5_u12hpb6TX7nA8jb05JyhHwj7Z6TUWSxTLNuNidT5NV5DaJXEgc5Ye_k2jxwSCJCbsuA5u-tj2eHGqr4DSylnxkg?key=Yh1qn3tRuuHwWUleH_6lDD37)

웹브라우저에서 codepoemkr.github.io 로 확인!

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXehdnkka-El-aVtBrJA03333MOK5P8wBsG2bHikSz49qUQtI0UCcfIahohwM2E5xjlgIfm9vCEyYtR2U6uxXjVEEGslJxPdUDnalFOk4zc0eJmcGQdkrozPsJ2LKSSbExYrQRbu?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

이제부터 좀 더 깔끔한 작업을 위한 2단계 클렌징. 어쩌면 이것은 깃허브의 위대한 기능이며, 이 과정을 잘 보시면 뭔가 굉장한 영감이 떠오르실 듯~ㅎ

이거슨 저만의 노하우 ㅋㅋㅋ

  

## 8. chirpy 클렌징

#### <span style="color:blue">cd /home/blog</span>

로컬 초기화

#### <span style="color:blue">rm -rf codepoemkr<hi>.github.io</span>

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdX5yd3DaydrK-Ji-500l4R-R9CNOtaOjyV1OXCg9qWZ5emnhC55PfeZpA5h6vibw-1bx31ptbq3AB3PcTgFWTSwudeNbgsY_dNoOUfWoWEkdQ5NGvAxfql1OYbpLHl7CP6tHStrQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

다시 위에 사용했던 명령어로 git clone

이러면 이미 초기화된 github 블로그를 불러올 수 있음. (maybe 어디에서나!!!)

#### <span style="color:blue">git clone http<hi>s://github.com/codepoemkr/codepoemkr.github.io.git</span>

#### <span style="color:blue">cd codepoemkr<hi>.github.io</span>

  

토큰 재설정

#### <span style="color:blue">sudo git remote set-url origin http<hi>s://codepoemkr:ghp_XXXXXXXXXXXXXXXXXXXXXXXXX@github.com/codepoemkr/codepoemkr.github.io.git</span>

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdFGiTHgLCZ3nufjkqxQ5DMO-fpU24mCCr2kkdqKTafkoIe9LiMscsotdDIR-za5kICufXDpHUDrIHnRfErYGSIUpiiVXjgQ4EdVKwTk1xz69YSf8aAj8IZNKdbugz-A_5ehxHiJg?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

이제 부터 본격적인 커스터마이징을 맘껏 하시면 됩니다.

이 때 최초 한번은

  

git add .

  

명령을 주셔야, vscode git에서 통상적인 commit and Sync가 진행될 것입니다.

  

고맙습니다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAxNjIyNzA2MywtNzQ2MTk3NzQzXX0=
-->