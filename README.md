# 💡 Docker



<div align="center">
	<h3> 도커 정리 </h3>
 	<img src="https://d1.awsstatic.com/acs/characters/Logos/Docker-Logo_Horizontel_279x131.b8a5c41e56b77706656d61080f6a0217a3ba356d.png" />
</div>

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
 **ubuntu이미지는 사용자의 입력을 할 수 있다.**
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

ps ex로 확인해보자
``` shell
root@e3d889ea9f16:/# ps ex
PID TTY      STAT   TIME COMMAND
1 pts/0    Ss     0:00 /bin/bash PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin HOSTNAME=e3d889ea9f16 TERM=xterm HOME=/root
10 pts/0    R+     0:00 ps ex HOSTNAME=e3d889ea9f16 PWD=/ HOME=/root LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;
   
```
$ sudo docker run -i -t ubuntu:20.04 /bin/bash 해당 명령어로 실행한 /bin/bash가 PID 1로 실행되고 있다.

#### git 설치 해보기
``` shell
//먼저 update
root@e3d889ea9f16:/# apt-get update

//git 설치
root@e3d889ea9f16:/# apt-get install git
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  ca-certificates git-man krb5-locales less libasn1-8-heimdal libbrotli1 libbsd0 libcbor0.6 libcurl3-gnutls libedit2 liberror-perl libexpat1 libfido2-1 libgdbm-compat4 libgdbm6 libgssapi-krb5-2 libgssapi3-heimdal
  libhcrypto4-heimdal libheimbase1-heimdal
  .
  .
  .


```
> git은 우리가 사용하고 있는 OS에 git을 설치한 것이다. --- X
git은 /bin/bash로 만들어진 컨텡이너에 git을 설치한 것이다. --- O

따라서 당연한 말이지만,
설치한 git은 컨테이너 밖인 우리의 실질적 OS에서 사용이 불가능하다.
컨테이너 안에서만 사용되는 **독립적인 것**이다.

### 종료
``` shell
root@e3d889ea9f16:/# exit
exit

joozero@DESKTOP-1I1JMBR:~$ //종료 되었음을 의미.

```
메인 실행파일이 종료가 된다면 컨테이너도 종료된 것을 의미.

``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps
[sudo] password for joozero:
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
ps로 확인해보니 도커가 존재하지 않는다.
``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                     PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Exited (1) 2 minutes ago             inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago                hopeful_merkle

```
프로세스의 상태를 알 수 있는 ps 명령어.
ps에서 -a옵션을 사용하면 어떤 프로세스가 종료되었는지도 조회할 수 있다.

-----
그렇다면 ? 
1. 종료하지 않고, 실행된 상태에서 빠져나올 수 있을까 ?
= 가능하다

1. 일단 먼저 도커 다시 접속 하기

### 도커 재접속
``` shell
jykang@DESKTOP-1I1JMBR:~$ sudo docker start inspiring_lalande
inspiring_lalande
// 이름이 한 번 더 표시된다면 실행 된 것을 의미한다.
```
여기서 inspiring_lalande이 무엇인가 ? 
docker 서비스를 처음 시작할 때 이름을 부여할 수 있다. 
지정을 하지 않은 경우 도커는 임의의 이름을 자동으로 부여하는데, 그것이 바로 **inspiring_lalande**이 된 것이다.

**inspiring_lalande인지 어떻게 안 것인가?**
```shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                     PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Exited (1) 2 minutes ago             inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago                hopeful_merkle

```
docker ps -a 명령으로 나온 도커의 Names이다.
CONTAINER ID로 사용하여도 무방하다. 

#### 왜 실행이 됐는데 root# 이런 식으로 안나오는 거지 ? 
>- 컨테이너가 띄워져 있는데 안으로 들어가지 않은 것이다. 
	-	 run을 하면 실행함과 동시에 도커로 들어감.
	- start를 하면 실행만 하게 됨.
    
``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                  PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Up 7 seconds                      inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago             hopeful_merkle
```
실행 되어 있는 것 확인 가능하다.

#### 들어가려면 ?
``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker attach inspiring_lalande
root@e3d889ea9f16:/#
```
컨테이너 안으로 들어간 것 확인 가능하다 !

### exit 안하고 빠져나오는 방법은 없나?
#### control + P+ Q
``` shell
root@e3d889ea9f16:/# read escape sequence // 단축키 사용 한 것

joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                  PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Up 3 minutes                      inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago             hopeful_merkle
joozero@DESKTOP-1I1JMBR:~$

```
inspiring_lalande가 종료되지 않은 것을 확인하고, 내 자신이 빠져나온 것을 확인 할 수 있다.

* 종료를 하고 싶다면 **exit** 혹은 **ctrl + D**

### stop
- 도커를 종료 시킬 때 사용되는 명령어
``` shell
//종료
joozeroOP-1I1JMBR:~$ sudo docker stop inspiring_lalande
inspiring_lalande

//종료 된 것 확인
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                     PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Exited (0) 5 seconds ago             inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago                hopeful_merkle
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

### rm
* 없는 도커 제거
``` shell
//제거
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker rm inspiring_lalande
inspiring_lalande

//제거 된 것 확인
jykang@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
### rmi
#### 도커 이미지 중 필요 없는 것 제거하고 싶다면?

지금까지 앞에서 봐온 생성 혹은 제거는 모두 컨테이너를 생성하고 제거한 것이다. 
bin/bash 부분!!
지금부터는 이미지 제거이다.

``` shell

joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
ubuntu        latest    216c552ea5ba   2 weeks ago     77.8MB
ubuntu        20.04     817578334b4d   2 weeks ago     72.8MB
hello-world   latest    feb5d9fea6a5   13 months ago   13.3kB

```

#### 사용하고 있는 ubuntu:20.04만 사용하고 ubuntu:latest 와 hello-world는 제거하고 싶은 경우

``` shell
//hello-world NAMES로 제거
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:18a657d0cc1c7d0678a3fbea8b7eb4918bba25968d3e1b0adebfa71caddbc346
Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
Deleted: sha256:e07ee1baac5fae6a26f30cabfe54a36d3402f96afda318fe0a96cec4ca393359

//제거된 것 확인
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    216c552ea5ba   2 weeks ago   77.8MB
ubuntu       20.04     817578334b4d   2 weeks ago   72.8MB

//ubuntu:latest IMAGE ID로 제거
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker rmi 216c552ea5ba
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:35fb073f9e56eb84041b0745cb714eff0f7b225ea9e024f703cab56aaa5c7720
Deleted: sha256:216c552ea5ba7b0e3f6e33624e129981c39996021403518019d19b8843c27cbc
Deleted: sha256:17f623af01e277c5ffe6779af8164907de02d9af7a0e161662fc735dd64f117b

//제거 된 것 확인
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       20.04     817578334b4d   2 weeks ago   72.8MB
```

> 💡 컨테이너는 이미지가 하나지만 여러개의 컨테이너를 생성할 수 있다.
- **컨테이너 안에서 파일을 만들거나 어떤 것을 설치한다는 것은 컨테이너에 영향을 주는 것이지, 이미지에 영향을 주지 않는다 ! !**

<label style=font-size:5px>이 포인트가 흠 .. 매우 신기하다  뭔가 내가 OS와 비슷한 것을 생성하여 사용할 수 있다는 것이 .. ㅋㅋㅋ</label>

같은 내용 : velog.io/@joo_zero
