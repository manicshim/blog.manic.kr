---
title: "Sendmail - Spam 발송 계정 및 IP 확인하기"
comments: true
date: 2018-11-28 15:17:49
categories: [developer,system]
tags: [mail,sendmail,spam]
thumbnail:
---

Sendmail로 구축한 메일 서버 운영시 별도의 방화벽 정책이나 Relay 설정을 하지 않았다면 
로컬 계정의 패스워드 취약점을 이용하여 스팸 메일 발송에 악용 당하는 경우가 종종 발생 한다

 
어떤 IP에서 어떤 메일 계정에 접속하여 스팸을 발송 하는지 아래와 같은 방법으로 확인

##### 1. 메일 계정별 접속 횟수 통계로 확인
```
zgrep "authid=" /var/log/maillog* | awk '{print $8}' | sort | uniq -c | grep authid | sort -r
```


##### 2. IP별 접속 횟수 통계로 확인
```
zgrep "authid=" /var/log/maillog* |  awk '{print $7}' | sort | grep relay |  uniq -c | sort -r
```