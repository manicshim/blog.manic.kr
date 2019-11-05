---
title: "[docker] registry 구성방법"
comments: true
date: 2018-12-06 17:08:05
categories: ["developer","system"]
tags: ["docker","system","linux"]
thumbnail: https://user-images.githubusercontent.com/22788288/48998859-2ac9ee80-f198-11e8-8d42-9a705e6dc7f8.png
---
## Docker의 registry 구성하는 방법
1. registry 컨테이너 구성
2. -v 옵션으로 저장소 디렉토리 확인
3. 컨테이너 구동
4. docker hub 에서 우분투 18.04 pull
5. 나만의 이미지를 위한 tag 부여
6. 확인 / docker images & docker ps -a
7. 저장소에 저장을 위한 push
8. 우분투 이미지 삭제 & 등록된 나만의 이미지 삭제
9. 확인 / docker images & docker ps -a
10. 내려받기

{% asciinema OfYAYuHPo3w9IoXkkKBG6Xoht %}

나만의 이미지를 생성할 경우 Dockerfile 를 생성하여 관리하는것이
차후 서비스를 보다 효율적으로 운용할때 편리함
Dockerfile 생성의 예제는 다음을 기약하며