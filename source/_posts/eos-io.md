---
title: "[EOS.IO] 설치 및 실행"
comments: true
date: 2018-11-07 10:14:35
categories: ["developer","blockchain"]
tags: ["blockchain","eos","eos.io","nodeos","cleos","keosd","블록체인","이오스","이오스설치","eos설치","ubunutu"]
thumbnail: /thumb/eos.png
---
# 블록체인 EOS.IO 설치 및 실행
EOS에 관련한 자료들이 그리 풍부하지 않아 반복했던 실수를 줄이고자 작성하는 내용이며
다소 정확하지 않거나 전혀 틀린 방법들이 존재할수 있음을 사전 명시합니다
작성된 내용은 작성자 본인의 학습 또는 진행의 도움을 갖고자 기록하는 기록물이니
혹시 필요하신 분들에게 있어 길라잡이가 아닌 도움말 정도로 이용해주시길 바랍니다
EOS.IO 개발자포털 바로가기 [https://developers.eos.io/](https://developers.eos.io/)

---

### EOS.IO 설치
##### 1. 설치환경
`Mem: 16G`
`OS: Ubuntu 18.04.1 LTS (Bionic Beaver)`
`Kernel: Linux 4.15.0-38-generic`

![Ubuntu 설치환경](https://user-images.githubusercontent.com/22788288/48110715-0497e980-e291-11e8-8e9d-a4ceb0a7ec68.png)

##### 2. 소스내려받기
```EOS.IO
git clone https://github.com/EOSIO/eos --recursive
```
해당 위치는 ~/eos 생성되었으며 클론 이후 submodule 내용들을 update 진행

##### 3. 빌드하기
* 운영체제 별 시스템 기본 요구사항 확인(1번 yes 선택)
* 필요한 라이브러리 설치 (boost, mongodb, wasm)
* 빌드 실행
* 결과적으로, build 폴더 내부에 주요 결과물들이 생성
* **~/opt/** 사전에 해당경로 pwd 확인필수 (~/opt 자동생성 됨)

**빌드진행되면 상당한 시간소요. 절대끈기가 필요 (진짜 속터짐 한숨자고 오세요 ㅋㅋ)**

```
cd eos
git pull
git submodule update --init --recursive
sudo apt update && sudo apt upgrade
./eosio_build.sh -s EOS
```
>-s 옵션을 통해 eosio 시스템 토큰의 심볼을 정의합니다. 소스의 시스템 토큰이 기본적으로 SYS이기에, -s EOS 옵션이 필요

아래와 같은 화면들을 만나게된다

![기본 요구사항 확인](https://user-images.githubusercontent.com/22788288/48120723-4edf9180-e2b6-11e8-9d7f-c5829b522e48.png)
![빌드가 진행중인 화면](https://user-images.githubusercontent.com/22788288/48111448-d1eff000-e294-11e8-9866-8008aff53166.png)
![빌드 완료](https://user-images.githubusercontent.com/22788288/48112287-2eeda500-e299-11e8-9376-2be07e400b96.png)
### 빌드 확인

필수사항은 아니지만 빌드한 결과물 확인(시간 다량 소요됨)
우선 mongodb를 실행(~/opt 폴더 내에 빌드됨)
```
export PATH=${HOME}/opt/mongodb/bin:$PATH
~/opt/mongodb/bin/mongod -f ~/opt/mongodb/mongod.conf &
```
하위 build 디렉토리에 이동하여 실행
```
cd ~/eos/build
sudo make test
```

### 전역 실행파일 생성
```
cd ~/eos/build/programs
pwd
ls -al
```
![주요 실행파일인 nodeos, cleos, keosd가 같은 이름의 폴더 내에 생성됨](https://user-images.githubusercontent.com/22788288/48112437-c9e67f00-e299-11e8-8817-d51d271b5dc7.png)

**중요한부분**
필자가 찾아본 자료들에서는 
>실행파일들이 특정하위 디렉토리에 존재하기때문에 매번 해당경로를 직접지정해서 실행야하는 번거로움

때문에 아래와 같이 진행했다고들 한다

```
cd ~/eos/build
sudo make install #(/usr/local/bin 은 관리자 계정이기에 sudo 필요)
cd /usr/local/bin/
```
찾아보면 없다?! 삽질 ㅠ.ㅠ

```
cd /usr/local/eosio/bin/
ls -al
```
상기 위치에 해당하여 생성되며 전역에서 명령어가 실행되지 않는다
이삽질 덕분에 짜증이 이만저만이 아니였다... 결국 모르면 모르는 놈만 답답하지 ㅠ.ㅠ
**해결방안은 2가지** 이다 

![생성된 실행파일의 내용과 위치](https://user-images.githubusercontent.com/22788288/48112660-c1db0f00-e29a-11e8-8c0d-3b521c94f392.png)

한가지는 - 그냥 복사해버렸다ㅋㅋㅋ 어떤문제를 야기할지 모른다
```
sudo cp -rf /usr/local/eosio/bin/* /usr/local/bin
```

또다른 한가지는 - [HERE](https://github.com/EOSIO/eos#ubuntu-1804-debian-package-install)
```
cd /usr/local/src
wget https://github.com/eosio/eos/releases/download/v1.4.3/eosio-1.4.3.ubuntu-18.04-x86_64.deb
sudo apt install ./eosio-1.4.3.ubuntu-18.04-x86_64.deb
```

필자는 두번째 방법을 추천한다.
드디어 위 과정을 통해, 어느 위치에서든 eos 관련 프로그램을 실행가능하게 됨

---

### EOS.IO 실행
eos 의 메인이 되는 eos 노드를 실행명령

```
nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin
```
위 명령의 결과로 다음과 같이 블록이 계속 생성됨을 확인

![주기적으로 블록이 생성됨](https://user-images.githubusercontent.com/22788288/48113485-6743b200-e29e-11e8-898f-76511ce6f7f6.png)

### EOS 커맨드라인 인터페이스 실행 (cleos)
cleos 를 통해 nodeos에 명령을 내리고 상태를 확인, 정보를 출력한다

```
cleos get info
```
![EOS 노드의 정보가 출력됨](https://user-images.githubusercontent.com/22788288/48113528-99edaa80-e29e-11e8-935c-896d9b64de5a.png)

cleos가 명령을 정상적으로 수행
실행 중인 nodeos와 올바르게 통신했음 확인한다


### nodeos 의 오류 사항
어떤 문제로 인해서인지 오류를 뱉어내고 멈출때가 있는데
Docker 로의 진행에 대한 자료는 많지만 실질적인 내용이 없어 추가한다
![nodeos의 오류화면](https://user-images.githubusercontent.com/22788288/48239815-b2cb9c80-e413-11e8-8f3e-5e16aa9b49c8.png)

해결방법은 블록을 다 지우고 다시 생성하는것
```
./nodeos --delete-all-blocks 
#OR
nodeos --delete-all-blocks  
```
블록이 다 지워지면 재실행한다

```
nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin
```
> ps -ef 확인된 프로세스를 kill -2로  프로세스를 죽인다. nodeos는 kill -9로 죽이면 엄청난 오류를 토하며 아예 서버를 재설치 하는게 바람직하다


그리고 최신 EOS 경우 지갑데몬이 자동으로 뜨지않아 수동설정해야한다는데
확인은 해봐야 할듯..

### <span style=color:red;>지갑데몬을 구성하고 cleos 로 지갑설정을 조정한다</span>

```
keosd --http-server-address 127.0.0.1:8900 --config-dir $HOME/eosio-wallet
```
해당 디렉토리 `/root/eosio-wallet/config.ini` 파일이 존재하며
상위에 옵션은 설정파일에 작성하면 되지않을까?!

![지갑데몬 구동화면](https://user-images.githubusercontent.com/22788288/48244914-989db880-e42b-11e8-913a-57fd01acf77c.png)


### cleos 지갑데몬 설정 예제
```
cleos --wallet-url http://127.0.0.1:8900 wallet create --to-console
```
> 지갑데몬과 wallet_url 의 포트를 주의하자

명령어 귀찮아서 Shell Script를 작성
ex) restart.sh / keosd.sh / block_del.sh 등등
```
# pwd 는 /root
cd ~
vi restart.sh

# 아래와 같이 생성한다
#!/bin/sh
if [ -z "`ps -eaf | grep nodoes`" ]; then
	echo "Nodeos was not started."
else
	ps -eaf | grep nodeos | awk '{print $2}' |
	while read PID
		do
		echo "Killing $PID ..."
		kill -2 $PID
		echo
		echo "Nodeos is being shutdowned."
		done
	$HOME/start.sh # start.sh 파일은 알아서들 생성하시길
fi
```
이하 권한은 755 를 부여하고 구동은 아래와같이한다
```
chmod +x restart.sh

./restart.sh #OR
sh restart.sh #OR
bash restart.sh
```
> 상위에 거론한 kill -9으로 죽이지말고  kill -2 로 자동처리

```
cleos wallet create # -n, --name [임의적 생성된 지갑이름]
cleos wallet open -n random
cleos wallet unlock -n random
```
각각의 옵션들과 기타사항들의 도움말은 `cleos --help` 하자
