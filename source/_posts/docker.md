---
title: "[Docker] 도커를 사용하는 기본 명령어"
comments: true
date: 2018-11-26 15:43:31
categories: ["developer","system"]
tags: ["docker","system","linux"]
thumbnail: https://user-images.githubusercontent.com/22788288/48998859-2ac9ee80-f198-11e8-8d42-9a705e6dc7f8.png
---
### 설치

##### Linux
```
curl -fsSL https://get.docker.com/ | sudo sh 

# apt-get 또는 yum  설치 할 경우 버전확인
apt-get install docker.io
#OR
yum install docker
```

##### Version 확인
```
docker version
```

##### sudo 없이 사용하기
```
sudo usermod -aG docker $USER # 현재 접속중인 사용자에게 권한주기
sudo usermod -aG docker your-user # your-user 사용자에게 권한주기
```

---

### 삭제하기

##### 컨테이너 삭제
```
docker ps # 구동중인 컨테이너 확인
docker ps -a # 컨테이너 전체 확인
docker rm 컨테이너ID # 복수일경우 , 구분
docker ps -a # 컨테이너 확인
```

##### 이미지 삭제
```
docker images # 이미지 확인
docker rmi 이미지ID
docker rmi -f 이미지ID # -f , --force 옵션으로 컨테이너 포함하여 강제삭제
```