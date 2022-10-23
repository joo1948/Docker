# Docker
### 도커 정리

# 1

### 설치하기
1. ubuntu 설치(나는 우분투 설치 후 mobaxterm을 사용했다.)
2. docker 설치

#### 스크립트 설치
``` shell
$ sudo wget -qO- https://get.docker.com/ | sh
```
``` shell
$ sudo curl -fsSL https://get.docker.com | sh
```
#### 우분투 패키지로 설치
``` shell
$ sudo apt update
$ sudo apt install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
    https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

#### 도커 서비스 실행
``` shell
$ sudo systemctl start docker
```


#### 부팅했을 때 자동으로 실행하기
``` shell
$ sudo systemctl start docker
```

> 📢 이때 
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down

해당 오류가 발생할 수 있다.

만약 ubuntu 20.04 버전을 사용하는 경우 발생하는 오류다.
> Ubuntu 20.04에서는 system daemon으로 부팅되는 게 아니라서 이 명령어를 못 쓴다.

  ### service사용
  나는 systemctl 대신에 service로 확인할 수 있었다.

  ``` shell
  $ sudo service --status-all
  ```
   [ - ]  apparmor
   [ - ]  apport
   [ - ]  atd
  ...
  [-]로 docker가 비활성화 되어 있다.

  ``` shell
  $ sudo service docker start
  * Starting Docker: docker
  ```

  ``` shell
  $ sudo service --status-all
  ...
   [ - ]  dbus
   [ + ]  docker
   [ ? ]  hwclock.sh
  ```
  docker부분이 [+]로 채워졌다. 이제 도커를 실행해보면

  ``` shell
  $ sudo docker run hello-world

  Hello from Docker!
  This message shows that your installation appears to be working correctly.
  ```

  오류 해결했다.

#### 이미지 확인
``` shell
$ sudo docker images

REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
ubuntu        latest    216c552ea5ba   2 weeks ago     77.8MB
hello-world   latest    feb5d9fea6a5   13 months ago   13.3kB

```

# 2일차

1. Docker 의 명령은 docker run, docker push와 같이 docker [명령] 형식.
2. 항상 root 권한으로 실행.

### 이미지 다운로드

우분투 이미지 다운로드
``` shell
sudo docker pull ubuntu:20.04

```
20.04라는 것은 우분투 버전
> ✔ 우분투 OS 자체를 가져오는 것이 아닌 우분투 OS의 라이브러리 같은 패키지를 얻는 것

docker pull <이미지 이름>:<태그> 형식입니다. latest를 설정하면 최신 버전을 받습니다.

### 이미지 정보 확인하기
#### search 명령어
우리는 docker에서 어떤 이미지를 받아야 하는지 모를 수 있다.
그래서 제공하는 것이 **search 명령어**가 존재한다.

``` shell
$ sudo docker search ubuntu
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                           Ubuntu is a Debian-based Linux operating sys…   15118     [OK]
websphere-liberty                WebSphere Liberty multi-architecture images …   290       [OK]
ubuntu-upstart                   DEPRECATED, as is Upstart (find other proces…   112       [OK]
neurodebian                      NeuroDebian provides neuroscience research s…   93        [OK]
ubuntu/nginx                     Nginx, a high-performance reverse proxy & we…   63
open-liberty                     Open Liberty multi-architecture images based…   55        [OK]
ubuntu-debootstrap               DEPRECATED; use "ubuntu" instead                48        [OK]
ubuntu/apache2                   Apache, a secure & extensible open-source HT…   43
ubuntu/mysql                     MySQL open source fast, stable, multi-thread…   38
ubuntu/squid                     Squid is a caching proxy for the Web. Long-t…   38
ubuntu/prometheus                Prometheus is a systems and service monitori…   32
kasmweb/ubuntu-bionic-desktop    Ubuntu productivity desktop for Kasm Workspa…   31
ubuntu/bind9                     BIND 9 is a very flexible, full-featured DNS…   30
.
.
.

```

ubuntu, centOs등 OS나 프로그램 이름을 가진 것이 공식 이미지.
id가 앞에 붙은 것은 다른 사람들이 커스텀하여 올린 것을 의미한다.


#### docker hub 에서의 search 
또 이미지에 대한 다른 방법은 없을까 ?

https://hub.docker.com/_/ubuntu/tags

해당 url로 접속하여 버전을 확인한 후 pull받으면 된다 ! 

### 🖐🏻 이미지와 컨테이너 복습
이미지 : 실행파일과 라이브러리가 조합된 것
컨테이너 : 이미지를 실행한 상태

쉽게 말해, 
	이미지 : 일종의 실행 파일
    컨테이너 : 이미지를 실행시킨 것

### 컨테이너를 시작 --> run
> 컨테이너를 생성하는 것과 동시에 컨테이너를 시작하는 것.
== OS라고 했을 때, 쉽게 말해 부팅한다는 의미.


``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker run -i -t ubuntu:20.04 /bin/bash
root@e3d889ea9f16:/#
```
이미지 안에 있는 bash실행파일을 실행할 것이다. 라는 의미

#### run 옵션
 -i : 사용자가 입출력 할 수 있도록 하겠다.
 -t : 가상 터미널 환경을 애뮬레이션 되도록 한 것.
>  조금 전문적인 내용이니 단순히 컨테이너를 실행 시킬 땐 -i 와 -t를 사용한다고 알아두자.
ubuntu 20.04컨테이너 안에 있는 bin bash Shell을 사용하겠다는 의미이다.

#### 현재 우리의 상태는 
root@e3d889ea9f16:/#로 변경이 되었는데 이게 무슨 뜻인가? 
> 
docker 컨테이너 안으로 들어온 것
== ubuntu 20.04라는 이미지가 bin/bash로 만들어낸 컨테이너로 들어온것 
== 해당 컨테이너 위에서 작업을 하게 되는 것 ==> 마치 새로운 운영체제에 들어온 것만같음.

``` shell
root@e3d889ea9f16:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
```

이때 보여지는 환경은 바깥의 리눅스 환경과 다른 곳.
가상화와 비슷하다고 생각하면 쉽다.

https://www.youtube.com/watch?v=Bhzz9E3xuXY&t=360s
현재 23:30 까지 들었따.
