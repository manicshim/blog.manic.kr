---
title: "[EOS.IO] Centos7 - EOS 설치하기"
comments: true
date: 2018-11-13 10:34:16
categories: ["developer","blockchain"]
tags: ["blockchain","eos","eos.io","nodeos","cleos","keosd","블록체인","centos"]
thumbnail:
---
# Centos 7 - EOS 설치

역시 이또한 자료가 많지않아 직접 설치했던 기록들을 남긴다
길라잡이 아니고 참고 자료로 이용되길 바라며

---

### EOS.IO - centos 7 설치

##### 1. 설치환경
`Mem: 15G`
`OS: CentOS Linux 7 (Core)`
`Kernel: Kernel: Linux 3.10.0-862.14.4.el7.x86_64`

![Centos7 설치환경](https://user-images.githubusercontent.com/22788288/48385564-55e12680-e732-11e8-9f53-136f0933b3e5.png)

##### 2. 소스내려받기
```
git clone https://github.com/EOSIO/eos --recursive
cd eos 
git submodule update --init --recursive
sudo yum update
./eosio_build.sh -s EOS
```
![빌드 완료](https://user-images.githubusercontent.com/22788288/48385875-8ffef800-e733-11e8-88cc-7c360e2ae256.png)

### 빌드확인
Ubuntu 와 동일하다
```
export PATH=${HOME}/opt/mongodb/bin:$PATH
~/opt/mongodb/bin/mongod -f ~/opt/mongodb/mongod.conf &
cd ~/eos/build
sudo make test
```
테스트도 시간은 10여분 정도 소요되는데 왜 오류가 자꾸 나는지는 검토중이다

그리고 중요부분

>Ubuntu에서 삽질했던부분이다

`~/eos/build` 디렉터리에서 `sudo make install`을 실행해서 설치하면 
실행 파일이 v1.1.0 부터는 `/usr/local/eosio/bin`에 설치된다
따라서, 
```
sudo update-alternatives --install /usr/local/bin/nodeos nodeos /usr/local/eosio/bin/nodeos 1
sudo update-alternatives --install /usr/local/bin/cleos cleos /usr/local/eosio/bin/cleos 1
```
와 같이 해줘야 PATH 설정 없이도 편리하게 사용할 수 있다

### EOS.IO - nodeos 실행

```
nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin
```

블록생성이 확인된다

![주기적으로 블록이 생성됨](https://user-images.githubusercontent.com/22788288/48386898-e588d400-e736-11e8-8fec-99cfec2a21e6.png)

### EOS.IO - cleos 확인

`cleos get info` 명령어를 통해 아래와 같이 확인한다

![EOS 노드의 정보가 출력됨](https://user-images.githubusercontent.com/22788288/48386897-e588d400-e736-11e8-8465-940180f329aa.png)
