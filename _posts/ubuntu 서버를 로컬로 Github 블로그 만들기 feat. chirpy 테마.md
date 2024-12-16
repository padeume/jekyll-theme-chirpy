# ubuntu 24.04 LTS를 로컬로 사용하는 chirpy 테마 Github 블로그 만들기 (배포용)

with Chat GPT for 어설픈 초보 feat. vscode by folk

----------

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

#### sudo apt update

#### sudo passwd root

  
  

# 1. Github 가입

가입 과정은 생략, 일단 가입만 하고 로그인까지만 진행합니다.

-   어렵지 않습니다.
    

  

# 2. chirpy 초기화를 위한 npm 설치

#### sudo apt update && sudo apt upgrade -y

----------

만약 아래와 같은 화면이 나온다면,

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfo2SMhEbBQzFtQCiVM--r61bkKKXXImjQr69qQF2xnohljLise-gJEb0ca8rnhkdX4hJZsT61zWO97_49xZMhyoI5LEk3EZj5Pm5Wyw27LASAZx7_5_LwOeG8BXme81NPYBEmkig?key=Yh1qn3tRuuHwWUleH_6lDD37)

대부분의 경우, **keep the local version currently installed**를 선택하는 것이 좋습니다. 이 옵션은 로컬에서 수정한 설정(sshd_config)을 유지하며, SSH 접속 환경이 기존과 동일하게 유지됩니다.

----------

  

#### curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

#### sudo apt install -y nodejs

  

설치 확인

#### node -v

#### npm -v

  

최신버전 확인

#### sudo npm install -g npm@latest

  

# 3. chirpy 테마 설치

## 1. ubuntu Git 설정

Git 사용자 정보를 설정. 아이디와 이메일은 예시이니, 각자 변경해서 사용하시기 바랍니다.

#### git config --global user.name "codepoemkr"

#### git config --global user.email "codepoemkr@kmail.kr"

  

# Git 설정 확인

#### git config --list

  

## 2. 블로그 프로젝트 생성

home 디렉토리에서 작업합니다.

현재 /home/ubuntu 라면 cd .. 명령어로 /home/ 디렉토리로 이동합니다.

ubuntu@ip-XXX-XX-XX-XXX:/home$ 이런 식으로 나와야 합니다.

#### sudo mkdir -p /home/blog

#### sudo chown -R ubuntu:ubuntu /home/blog

#### cd /home/blog

이 때 vscode도 작업 디렉토리를 이동합니다.

  
  

## 3. 로컬과 github 통신을 위한 access token 등록

1.  Github 로그인 후 오른쪽 상단 프로필 이미지 클릭
    
2.  중간부분에 Settings
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcPIwQw9A7-HkbmTJ2o1w-z4fkb0lPNCE5Sn-IZJvl-s8QmF6OQSSsopj-iqOfWZS-vlp2byMnZ9g26QeB1I9xRgtKIgziBEmhQ0mgaZXKB9evQ687PT3BGg97YpGeG2BEiqnLEGA?key=Yh1qn3tRuuHwWUleH_6lDD37)

3.  왼쪽 맨 하단에 <> Developer Settings
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeHnEc5wQ36UY4O0jpHary6_f4AUUIaLG6snk8u-M2T2GvmnMYpGQpxwz545kYDLCu1LxgDU4_dnQYk7IbiMLQJebsMH_yWqiqXtBR3mwOxcydG1AoDmRBXeIlRqbYuyQqCHi5uQQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

4.  왼쪽 하단 Personal access tokens
    

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXde1lVGs1OLAjI5FomRpuojq6hn0xNzFq7X0nRfB1clmUr8lEM7wE6QREU1dkxKxmcXZ29RhJCgyLaetW7fXdQCVQr2OwwSHZ5QD-gJWeyOakN6mAmxkHCNRj7mXt6emqUAZK7oRQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

5.  tokens (classic) - Generate new token (classic) ( 보통 90일용 선택 )
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcVHW8MUfO6TkqLtNMT8lFkaSELWut-RMaA1v4AQg_ta5cTStTRdhBq9iU_FeaUGHll8QgIwCC2gu_3gYPNY7csaKRdTX3WNYIgaGxzZQlaTP3bYaBgP1zDhUoKbcDNneTP7-hBYA?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdSBa44HFidvNtVbT3kyUkDmncyxL673VYtikrVxea40q-zFYbFLjil24DR6IUIDyRdPyt4cNQMGwDBugjrGqCBggS6J-J1i9Zxo3Cs84G9_f300Prvd8xrlH17Y91lqmJiR7Ul?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeERzGIs46dADO7gmS6K5C7IbVlvrG779wi5uTpTgybpv0tT46PdjRFVY6vZSngM8FLdQkdEfFK8vEqNxBOq1UaNgIBiwYHy5B5JB2VjFL6kvHmJNXSMCJD-n2AY9edItOQTjsRgQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

6.  비번 입력 후 하단의 classic 선택
    
7.  적당히 사용용도 입력 ( 필자는 vscode라고 함 )
    
8.  나도 잘 몰라서 그냥 다 선택 후 맨 하단 Generate token
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeECqFLiXKp9sqfIE5axZ-wwcS5QDUt89ST_S3dQ_N7eZviMYARCkNSBl_fe5vzgVXfMtPdJXqPOoec5cSqeICZ3fttbkapMGpkI_rm0FexELWLz2AOJAOY4nI89hQeL3s8tIMuGQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

9.  ghp_로 시작하는 토큰을 잘 복사해서 보관
    

  

#### sudo git remote set-url origin https://codepoemkr:ghp_XXXXXXXXXXXXXXXXXXXXXXXXX@github.com/codepoemkr/codepoemkr.github.io.git

  
  
  

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

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfwdDESEC037h5OqjtG0KGKP4CxZWOWUx4-mYfXNts74DAbR2NxT-0SlYU_O6RDBNU3WJD62ra-cKzTgkbjUJGg8UqTzruBgkSEKxNPxRyvz-xRPFvzsOtTtsvpiee0hPXoKujt?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcEjkX43y7YWEItvzMhcTSLDBbFbDrv_rps4k7NFslYAAqV2WZl46j6AzRnvoCMSfbKDsbBA4o9gNeYpxk32z6Ss4Dtakby3U1LV0RkYz2UKtLgUPMSWDIZ3sWzQyFIUVcQqdUflg?key=Yh1qn3tRuuHwWUleH_6lDD37)

folk 성공

  

## 5. Repository 이름 바꾸기

----------

github의 블로그를 기본 도메인을 사용할 때는 repository 이름을 <github 아이디>.github.io 형식으로 만들어야 합니다.

-   github 블로그는 기본적으로 아이디당 하나의 블로그 (주소)를 제공합니다.
    

-   github > fork 받은 저장소 > Settings 화면으로 이동
    
-   Repository name을 <github 아이디>.github.io 형식으로 변경
    
-   중간정도 내려서 Features - issues에도 체크 권장
    
-   rename 버튼 클릭
    
-   저장소 Code 화면으로 돌아오게 되며, 상단에 보면 방금 바뀐 이름으로 저장소 명이 보이게 될 겁니다.
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdfgbE6i355EYqY8JG33ad685YWbmj-zHmIXyZ6H99F-2Rg7PC5gip6ICxez9pYIDuVQ4Nc_qA8T6yymoFpNx5s_vDd-kZxtXSvLBEScMNx9AWIU4InM44qa64fdKFwOMAy3_pw?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdUL9485BfVIw60yPW77_XVY8joqWiUASVM5r3kpz2auPxHM6vf2lLUFCGbZ2eW2W3-ppNJQ9gl5JJ7K2pAV-4-TLf_xUGDgpkIejy5c-B7-B-_xyBAbmHKgwEedmHLdY92osKK?key=Yh1qn3tRuuHwWUleH_6lDD37)

----------

  

## 6. 소스 클론 내려받기

개인적으로 이 부분이 그동안 3일 밤낮의 삽질을 종결시켜준 부분이며, Github의 여러 기능 중 백미가 아닐까 생각됩니다. 그야말로 신세계가 펼쳐지는 기분.

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXek9YqVyk5p21a7oxiWYcYt1F_1Z5rTUKTT6KTy8NLjtS8-P52sy2V-7ySDvNIgjXbaL9HpabveW8khR8aQhJVcT97VSUnPuNruLzSOXUCz9Zn-jq7oQrzqLtwsKLRnLS2pa2XKqA?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

HTTPS 복사

vscode 터미널에서 ubuntu@ip-XXX-XX-X-XXX:/home/blog$ 상태 확인

#### git clone https://github.com/codepoemkr/codepoemkr.github.io.git

  

성공!

  

이제 디렉토리를 codepoemkr.github.io 로 터미널과 vscode에서도 File -> Open Folder에서 변경해 줍니다.

  

ubuntu@ip-XXX-XX-X-XXX:/home/blog/codepoemkr.github.io$

이런 상태가 인지 확인하세요.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe2f1o_n5XVcgbsaJcQXWtdvAcCSr7v1FU6ks8X4QNW1aVAEbp3ISdMSQ-NeV8EIhos3736mdsWPnIugWVLRNfgJ9YhMjk5y1sH0EYra5DZ7A3Oh-3FxhKCSZv0W-LrIMeWOo891w?key=Yh1qn3tRuuHwWUleH_6lDD37)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf5IsRSqsZC61f_kHFULBSgqx5ohSZnQ_v9JRR-ysN76Nk6puauPjjB8-arEOc8Zwx6nAPsmEtysE52-w3ZQ4ll9_7_TRyxG4UPnhsdaQlKJeOTC3W-jArbSjOCUXDdFW-nx9jq5w?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

## 7. chirpy 초기화 하기

#### cd codepoemkr.github.io

#### tools/init.sh

아주 기가 맥힙니다 ㅋ

중간에 노란색 warn나와도 쿨하게 넘어가세요.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeKYHAYF1wlThx_GVAXhf1UZfozk8r2Ie75rzV_jxWLndIdrUURbgAwrHH4QxfkpkmI9OfkEHrlgja-26gFQvKHTx1rYcoVt-vdsjT3AMKoC8GmxeP8uu7BWKtAkxvns1egH8hvMw?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

#### 토큰 설정

#### sudo git remote set-url origin https://codepoemkr:ghp_XXXXXXXXXXXXXXXXXXXXXXX@github.com/codepoemkr/codepoemkr.github.io.git

  

#### git config pull.rebase false

일단 강제 병합 설정정함.

  

그리고 나서 Sync Changes 한번 클릭

  
  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcVfQuXaGNp04FeK3uUZYG3wcdESjrQjckfV1eKll5GIsGpkeWGdUwmLZvTmuwiZ3fV6A5lNPxMP6RsewTkJsmXV6vfOn6UD9HysfBHfFl4b4rTh5wj6DogOFFjVpoy1bWX7Q3G?key=Yh1qn3tRuuHwWUleH_6lDD37)

물론 OK

github -> Actions 에서 기분좋은 성공

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfl-wlJEg4nGLZN9gzFHzFJTrfznikKrXnZlhy5Y_3dXtRpT1kv4J5vgiQRJL-5hSVdhCpWFVtptjDGhz0sWY8khAHKCyK2OM3T4ueVZf4UDOwNSMLjJOcCeflFBByhPwa7TPmNQ?key=Yh1qn3tRuuHwWUleH_6lDD37)

웹브라우저에서 codepoemkr.github.io 로 확인!

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeBwRLd2v8hwu5aLEX2JZnqCChIGHijoveYq4LXxc5Uf6DWdyF6RZgUQAGxJ6bwY20E7WqZe4wsny8JfdfOmB1zmgchmtwqSkG62IvhK_6SJFclVA7svldqapOj3MhC8yKbngiN?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

이제부터 좀 더 깔끔한 작업을 위한 2단계 클렌징. 어쩌면 이것은 깃허브의 위대한 기능이며, 이 과정을 잘 보시면 뭔가 굉장한 영감이 떠오르실 듯~ㅎ

이거슨 저만의 노하우 ㅋㅋㅋ

  

## 8. chirpy 클렌징

#### cd /home/blog

로컬 초기화

#### rm -rf codepoemkr.github.io

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXctqjQ4hEQ6_ZP2blwnhP7dWRcAU7CS6Nf7Mc6Yo52TQEbPLevFOuybArCVa8EMWZyTzW15zzM6tR_EsLy8dtFEoHlcs2rqmoJx-qg93T6TpE266hmTl2w_3vxdw-uOPAxdlJ3Meg?key=Yh1qn3tRuuHwWUleH_6lDD37)

다시 위에 사용했던 명령어로 git clone

이러면 이미 초기화된 github 블로그를 불러올 수 있음. (maybe 어디에서나!!!)

#### git clone https://github.com/codepoemkr/codepoemkr.github.io.git

#### cd codepoemkr.github.io

  

토큰 재설정

#### sudo git remote set-url origin https://codepoemkr:ghp_XXXXXXXXXXXXXXXXXXXXXXXXX@github.com/codepoemkr/codepoemkr.github.io.git

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcAutIjQ6ItHs0CClXOwgCFRjLNnSUSF0zcqTZXkJJvROEHA0jk7xgx9f443VraWjriMEvtgnxjqAyoxr5Fre94okWUEk3MD5p5Js0-V0IirVf1DrKEWKGvZN4N053vEd7CfgA8Fg?key=Yh1qn3tRuuHwWUleH_6lDD37)

  

이제 부터 본격적인 커스터마이징을 맘껏 하시면 됩니다.

이 때 최초 한번은

  

git add .

  

명령을 주셔야, vscode git에서 통상적인 commit and Sync가 진행될 것입니다.

  

고맙습니다.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc4NzM2OTUyMl19
-->