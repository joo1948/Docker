# ๐ก Docker



<div align="center">
	<h3> ๋์ปค ์ ๋ฆฌ </h3>
 	<img src="https://d1.awsstatic.com/acs/characters/Logos/Docker-Logo_Horizontel_279x131.b8a5c41e56b77706656d61080f6a0217a3ba356d.png" />
</div>

# 1

### ์ค์นํ๊ธฐ
1. ubuntu ์ค์น(๋๋ ์ฐ๋ถํฌ ์ค์น ํ mobaxterm์ ์ฌ์ฉํ๋ค.)
2. docker ์ค์น

#### ์คํฌ๋ฆฝํธ ์ค์น
``` shell
$ sudo wget -qO- https://get.docker.com/ | sh
```
``` shell
$ sudo curl -fsSL https://get.docker.com | sh
```
#### ์ฐ๋ถํฌ ํจํค์ง๋ก ์ค์น
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

#### ๋์ปค ์๋น์ค ์คํ
``` shell
$ sudo systemctl start docker
```


#### ๋ถํํ์ ๋ ์๋์ผ๋ก ์คํํ๊ธฐ
``` shell
$ sudo systemctl start docker
```

> ๐ข ์ด๋ 
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down

ํด๋น ์ค๋ฅ๊ฐ ๋ฐ์ํ  ์ ์๋ค.

๋ง์ฝ ubuntu 20.04 ๋ฒ์ ์ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ ๋ฐ์ํ๋ ์ค๋ฅ๋ค.
> Ubuntu 20.04์์๋ system daemon์ผ๋ก ๋ถํ๋๋ ๊ฒ ์๋๋ผ์ ์ด ๋ช๋ น์ด๋ฅผ ๋ชป ์ด๋ค.

  ### service์ฌ์ฉ
  ๋๋ systemctl ๋์ ์ service๋ก ํ์ธํ  ์ ์์๋ค.

  ``` shell
  $ sudo service --status-all
  ```
   [ - ]  apparmor
   [ - ]  apport
   [ - ]  atd
  ...
  [-]๋ก docker๊ฐ ๋นํ์ฑํ ๋์ด ์๋ค.

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
  docker๋ถ๋ถ์ด [+]๋ก ์ฑ์์ก๋ค. ์ด์  ๋์ปค๋ฅผ ์คํํด๋ณด๋ฉด

  ``` shell
  $ sudo docker run hello-world

  Hello from Docker!
  This message shows that your installation appears to be working correctly.
  ```

  ์ค๋ฅ ํด๊ฒฐํ๋ค.

#### ์ด๋ฏธ์ง ํ์ธ
``` shell
$ sudo docker images

REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
ubuntu        latest    216c552ea5ba   2 weeks ago     77.8MB
hello-world   latest    feb5d9fea6a5   13 months ago   13.3kB

```

# 2์ผ์ฐจ

1. Docker ์ ๋ช๋ น์ docker run, docker push์ ๊ฐ์ด docker [๋ช๋ น] ํ์.
2. ํญ์ root ๊ถํ์ผ๋ก ์คํ.

### ์ด๋ฏธ์ง ๋ค์ด๋ก๋

์ฐ๋ถํฌ ์ด๋ฏธ์ง ๋ค์ด๋ก๋
``` shell
sudo docker pull ubuntu:20.04

```
20.04๋ผ๋ ๊ฒ์ ์ฐ๋ถํฌ ๋ฒ์ 
> โ ์ฐ๋ถํฌ OS ์์ฒด๋ฅผ ๊ฐ์ ธ์ค๋ ๊ฒ์ด ์๋ ์ฐ๋ถํฌ OS์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ๊ฐ์ ํจํค์ง๋ฅผ ์ป๋ ๊ฒ

docker pull <์ด๋ฏธ์ง ์ด๋ฆ>:<ํ๊ทธ> ํ์์๋๋ค. latest๋ฅผ ์ค์ ํ๋ฉด ์ต์  ๋ฒ์ ์ ๋ฐ์ต๋๋ค.

### ์ด๋ฏธ์ง ์ ๋ณด ํ์ธํ๊ธฐ
#### search ๋ช๋ น์ด
์ฐ๋ฆฌ๋ docker์์ ์ด๋ค ์ด๋ฏธ์ง๋ฅผ ๋ฐ์์ผ ํ๋์ง ๋ชจ๋ฅผ ์ ์๋ค.
๊ทธ๋์ ์ ๊ณตํ๋ ๊ฒ์ด **search ๋ช๋ น์ด**๊ฐ ์กด์ฌํ๋ค.

``` shell
$ sudo docker search ubuntu
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                           Ubuntu is a Debian-based Linux operating sysโฆ   15118     [OK]
websphere-liberty                WebSphere Liberty multi-architecture images โฆ   290       [OK]
ubuntu-upstart                   DEPRECATED, as is Upstart (find other procesโฆ   112       [OK]
neurodebian                      NeuroDebian provides neuroscience research sโฆ   93        [OK]
ubuntu/nginx                     Nginx, a high-performance reverse proxy & weโฆ   63
open-liberty                     Open Liberty multi-architecture images basedโฆ   55        [OK]
ubuntu-debootstrap               DEPRECATED; use "ubuntu" instead                48        [OK]
ubuntu/apache2                   Apache, a secure & extensible open-source HTโฆ   43
ubuntu/mysql                     MySQL open source fast, stable, multi-threadโฆ   38
ubuntu/squid                     Squid is a caching proxy for the Web. Long-tโฆ   38
ubuntu/prometheus                Prometheus is a systems and service monitoriโฆ   32
kasmweb/ubuntu-bionic-desktop    Ubuntu productivity desktop for Kasm Workspaโฆ   31
ubuntu/bind9                     BIND 9 is a very flexible, full-featured DNSโฆ   30
.
.
.

```

ubuntu, centOs๋ฑ OS๋ ํ๋ก๊ทธ๋จ ์ด๋ฆ์ ๊ฐ์ง ๊ฒ์ด ๊ณต์ ์ด๋ฏธ์ง.
id๊ฐ ์์ ๋ถ์ ๊ฒ์ ๋ค๋ฅธ ์ฌ๋๋ค์ด ์ปค์คํํ์ฌ ์ฌ๋ฆฐ ๊ฒ์ ์๋ฏธํ๋ค.


#### docker hub ์์์ search 
๋ ์ด๋ฏธ์ง์ ๋ํ ๋ค๋ฅธ ๋ฐฉ๋ฒ์ ์์๊น ?

https://hub.docker.com/_/ubuntu/tags

ํด๋น url๋ก ์ ์ํ์ฌ ๋ฒ์ ์ ํ์ธํ ํ pull๋ฐ์ผ๋ฉด ๋๋ค ! 

### ๐๐ป ์ด๋ฏธ์ง์ ์ปจํ์ด๋ ๋ณต์ต
์ด๋ฏธ์ง : ์คํํ์ผ๊ณผ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ์กฐํฉ๋ ๊ฒ
์ปจํ์ด๋ : ์ด๋ฏธ์ง๋ฅผ ์คํํ ์ํ

์ฝ๊ฒ ๋งํด, 
	์ด๋ฏธ์ง : ์ผ์ข์ ์คํ ํ์ผ
    ์ปจํ์ด๋ : ์ด๋ฏธ์ง๋ฅผ ์คํ์ํจ ๊ฒ

### ์ปจํ์ด๋๋ฅผ ์์ --> run
> ์ปจํ์ด๋๋ฅผ ์์ฑํ๋ ๊ฒ๊ณผ ๋์์ ์ปจํ์ด๋๋ฅผ ์์ํ๋ ๊ฒ.
== OS๋ผ๊ณ  ํ์ ๋, ์ฝ๊ฒ ๋งํด ๋ถํํ๋ค๋ ์๋ฏธ.


``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker run -i -t ubuntu:20.04 /bin/bash
root@e3d889ea9f16:/#
```
์ด๋ฏธ์ง ์์ ์๋ bash์คํํ์ผ์ ์คํํ  ๊ฒ์ด๋ค. ๋ผ๋ ์๋ฏธ

#### run ์ต์

 **ubuntu์ด๋ฏธ์ง๋ ์ฌ์ฉ์์ ์๋ ฅ์ ํ  ์ ์๋ค.**
 
 -i : ์ฌ์ฉ์๊ฐ ์์ถ๋ ฅ ํ  ์ ์๋๋ก ํ๊ฒ ๋ค.
 
 -t : ๊ฐ์ ํฐ๋ฏธ๋ ํ๊ฒฝ์ ์ ๋ฎฌ๋ ์ด์ ๋๋๋ก ํ ๊ฒ.
 
>  ์กฐ๊ธ ์ ๋ฌธ์ ์ธ ๋ด์ฉ์ด๋ ๋จ์ํ ์ปจํ์ด๋๋ฅผ ์คํ ์ํฌ ๋ -i ์ -t๋ฅผ ์ฌ์ฉํ๋ค๊ณ  ์์๋์.
ubuntu 20.04์ปจํ์ด๋ ์์ ์๋ bin bash Shell์ ์ฌ์ฉํ๊ฒ ๋ค๋ ์๋ฏธ์ด๋ค.

#### ํ์ฌ ์ฐ๋ฆฌ์ ์ํ๋ 
root@e3d889ea9f16:/#๋ก ๋ณ๊ฒฝ์ด ๋์๋๋ฐ ์ด๊ฒ ๋ฌด์จ ๋ป์ธ๊ฐ? 
> 
docker ์ปจํ์ด๋ ์์ผ๋ก ๋ค์ด์จ ๊ฒ
== ubuntu 20.04๋ผ๋ ์ด๋ฏธ์ง๊ฐ bin/bash๋ก ๋ง๋ค์ด๋ธ ์ปจํ์ด๋๋ก ๋ค์ด์จ๊ฒ 
== ํด๋น ์ปจํ์ด๋ ์์์ ์์์ ํ๊ฒ ๋๋ ๊ฒ ==> ๋ง์น ์๋ก์ด ์ด์์ฒด์ ์ ๋ค์ด์จ ๊ฒ๋ง๊ฐ์.

``` shell
root@e3d889ea9f16:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
```

์ด๋ ๋ณด์ฌ์ง๋ ํ๊ฒฝ์ ๋ฐ๊นฅ์ ๋ฆฌ๋์ค ํ๊ฒฝ๊ณผ ๋ค๋ฅธ ๊ณณ.
๊ฐ์ํ์ ๋น์ทํ๋ค๊ณ  ์๊ฐํ๋ฉด ์ฝ๋ค.

ps ex๋ก ํ์ธํด๋ณด์
``` shell
root@e3d889ea9f16:/# ps ex
PID TTY      STAT   TIME COMMAND
1 pts/0    Ss     0:00 /bin/bash PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin HOSTNAME=e3d889ea9f16 TERM=xterm HOME=/root
10 pts/0    R+     0:00 ps ex HOSTNAME=e3d889ea9f16 PWD=/ HOME=/root LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;
   
```
$ sudo docker run -i -t ubuntu:20.04 /bin/bash ํด๋น ๋ช๋ น์ด๋ก ์คํํ /bin/bash๊ฐ PID 1๋ก ์คํ๋๊ณ  ์๋ค.

#### git ์ค์น ํด๋ณด๊ธฐ
``` shell
//๋จผ์  update
root@e3d889ea9f16:/# apt-get update

//git ์ค์น
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
> git์ ์ฐ๋ฆฌ๊ฐ ์ฌ์ฉํ๊ณ  ์๋ OS์ git์ ์ค์นํ ๊ฒ์ด๋ค. --- X
git์ /bin/bash๋ก ๋ง๋ค์ด์ง ์ปจํก์ด๋์ git์ ์ค์นํ ๊ฒ์ด๋ค. --- O

๋ฐ๋ผ์ ๋น์ฐํ ๋ง์ด์ง๋ง,
์ค์นํ git์ ์ปจํ์ด๋ ๋ฐ์ธ ์ฐ๋ฆฌ์ ์ค์ง์  OS์์ ์ฌ์ฉ์ด ๋ถ๊ฐ๋ฅํ๋ค.
์ปจํ์ด๋ ์์์๋ง ์ฌ์ฉ๋๋ **๋๋ฆฝ์ ์ธ ๊ฒ**์ด๋ค.

### ์ข๋ฃ
``` shell
root@e3d889ea9f16:/# exit
exit

joozero@DESKTOP-1I1JMBR:~$ //์ข๋ฃ ๋์์์ ์๋ฏธ.

```
๋ฉ์ธ ์คํํ์ผ์ด ์ข๋ฃ๊ฐ ๋๋ค๋ฉด ์ปจํ์ด๋๋ ์ข๋ฃ๋ ๊ฒ์ ์๋ฏธ.

``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps
[sudo] password for joozero:
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
ps๋ก ํ์ธํด๋ณด๋ ๋์ปค๊ฐ ์กด์ฌํ์ง ์๋๋ค.
``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                     PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Exited (1) 2 minutes ago             inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago                hopeful_merkle

```
ํ๋ก์ธ์ค์ ์ํ๋ฅผ ์ ์ ์๋ ps ๋ช๋ น์ด.
ps์์ -a์ต์์ ์ฌ์ฉํ๋ฉด ์ด๋ค ํ๋ก์ธ์ค๊ฐ ์ข๋ฃ๋์๋์ง๋ ์กฐํํ  ์ ์๋ค.

-----
๊ทธ๋ ๋ค๋ฉด ? 
1. ์ข๋ฃํ์ง ์๊ณ , ์คํ๋ ์ํ์์ ๋น ์ ธ๋์ฌ ์ ์์๊น ?
= ๊ฐ๋ฅํ๋ค

1. ์ผ๋จ ๋จผ์  ๋์ปค ๋ค์ ์ ์ ํ๊ธฐ

### ๋์ปค ์ฌ์ ์
``` shell
jykang@DESKTOP-1I1JMBR:~$ sudo docker start inspiring_lalande
inspiring_lalande
// ์ด๋ฆ์ด ํ ๋ฒ ๋ ํ์๋๋ค๋ฉด ์คํ ๋ ๊ฒ์ ์๋ฏธํ๋ค.
```
์ฌ๊ธฐ์ inspiring_lalande์ด ๋ฌด์์ธ๊ฐ ? 
docker ์๋น์ค๋ฅผ ์ฒ์ ์์ํ  ๋ ์ด๋ฆ์ ๋ถ์ฌํ  ์ ์๋ค. 
์ง์ ์ ํ์ง ์์ ๊ฒฝ์ฐ ๋์ปค๋ ์์์ ์ด๋ฆ์ ์๋์ผ๋ก ๋ถ์ฌํ๋๋ฐ, ๊ทธ๊ฒ์ด ๋ฐ๋ก **inspiring_lalande**์ด ๋ ๊ฒ์ด๋ค.

**inspiring_lalande์ธ์ง ์ด๋ป๊ฒ ์ ๊ฒ์ธ๊ฐ?**
```shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                     PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Exited (1) 2 minutes ago             inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago                hopeful_merkle

```
docker ps -a ๋ช๋ น์ผ๋ก ๋์จ ๋์ปค์ Names์ด๋ค.
CONTAINER ID๋ก ์ฌ์ฉํ์ฌ๋ ๋ฌด๋ฐฉํ๋ค. 

#### ์ ์คํ์ด ๋๋๋ฐ root# ์ด๋ฐ ์์ผ๋ก ์๋์ค๋ ๊ฑฐ์ง ? 
>- ์ปจํ์ด๋๊ฐ ๋์์ ธ ์๋๋ฐ ์์ผ๋ก ๋ค์ด๊ฐ์ง ์์ ๊ฒ์ด๋ค. 
	-	 run์ ํ๋ฉด ์คํํจ๊ณผ ๋์์ ๋์ปค๋ก ๋ค์ด๊ฐ.
	- start๋ฅผ ํ๋ฉด ์คํ๋ง ํ๊ฒ ๋จ.
    
``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                  PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Up 7 seconds                      inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago             hopeful_merkle
```
์คํ ๋์ด ์๋ ๊ฒ ํ์ธ ๊ฐ๋ฅํ๋ค.

#### ๋ค์ด๊ฐ๋ ค๋ฉด ?
``` shell
joozero@DESKTOP-1I1JMBR:~$ sudo docker attach inspiring_lalande
root@e3d889ea9f16:/#
```
์ปจํ์ด๋ ์์ผ๋ก ๋ค์ด๊ฐ ๊ฒ ํ์ธ ๊ฐ๋ฅํ๋ค !

### exit ์ํ๊ณ  ๋น ์ ธ๋์ค๋ ๋ฐฉ๋ฒ์ ์๋?
#### control + P+ Q
``` shell
root@e3d889ea9f16:/# read escape sequence // ๋จ์ถํค ์ฌ์ฉ ํ ๊ฒ

joozero@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                  PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Up 3 minutes                      inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago             hopeful_merkle
joozero@DESKTOP-1I1JMBR:~$

```
inspiring_lalande๊ฐ ์ข๋ฃ๋์ง ์์ ๊ฒ์ ํ์ธํ๊ณ , ๋ด ์์ ์ด ๋น ์ ธ๋์จ ๊ฒ์ ํ์ธ ํ  ์ ์๋ค.

* ์ข๋ฃ๋ฅผ ํ๊ณ  ์ถ๋ค๋ฉด **exit** ํน์ **ctrl + D**

### stop
- ๋์ปค๋ฅผ ์ข๋ฃ ์ํฌ ๋ ์ฌ์ฉ๋๋ ๋ช๋ น์ด
``` shell
//์ข๋ฃ
joozeroOP-1I1JMBR:~$ sudo docker stop inspiring_lalande
inspiring_lalande

//์ข๋ฃ ๋ ๊ฒ ํ์ธ
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS                     PORTS     NAMES
e3d889ea9f16   ubuntu:20.04   "/bin/bash"   30 hours ago   Exited (0) 5 seconds ago             inspiring_lalande
d6702cd7ef74   hello-world    "/hello"      2 days ago     Exited (0) 2 days ago                hopeful_merkle
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

### rm
* ์๋ ๋์ปค ์ ๊ฑฐ
``` shell
//์ ๊ฑฐ
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker rm inspiring_lalande
inspiring_lalande

//์ ๊ฑฐ ๋ ๊ฒ ํ์ธ
jykang@DESKTOP-1I1JMBR:~$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
### rmi
#### ๋์ปค ์ด๋ฏธ์ง ์ค ํ์ ์๋ ๊ฒ ์ ๊ฑฐํ๊ณ  ์ถ๋ค๋ฉด?

์ง๊ธ๊น์ง ์์์ ๋ด์จ ์์ฑ ํน์ ์ ๊ฑฐ๋ ๋ชจ๋ ์ปจํ์ด๋๋ฅผ ์์ฑํ๊ณ  ์ ๊ฑฐํ ๊ฒ์ด๋ค. 
bin/bash ๋ถ๋ถ!!
์ง๊ธ๋ถํฐ๋ ์ด๋ฏธ์ง ์ ๊ฑฐ์ด๋ค.

``` shell

joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
ubuntu        latest    216c552ea5ba   2 weeks ago     77.8MB
ubuntu        20.04     817578334b4d   2 weeks ago     72.8MB
hello-world   latest    feb5d9fea6a5   13 months ago   13.3kB

```

#### ์ฌ์ฉํ๊ณ  ์๋ ubuntu:20.04๋ง ์ฌ์ฉํ๊ณ  ubuntu:latest ์ hello-world๋ ์ ๊ฑฐํ๊ณ  ์ถ์ ๊ฒฝ์ฐ

``` shell
//hello-world NAMES๋ก ์ ๊ฑฐ
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:18a657d0cc1c7d0678a3fbea8b7eb4918bba25968d3e1b0adebfa71caddbc346
Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
Deleted: sha256:e07ee1baac5fae6a26f30cabfe54a36d3402f96afda318fe0a96cec4ca393359

//์ ๊ฑฐ๋ ๊ฒ ํ์ธ
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    216c552ea5ba   2 weeks ago   77.8MB
ubuntu       20.04     817578334b4d   2 weeks ago   72.8MB

//ubuntu:latest IMAGE ID๋ก ์ ๊ฑฐ
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker rmi 216c552ea5ba
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:35fb073f9e56eb84041b0745cb714eff0f7b225ea9e024f703cab56aaa5c7720
Deleted: sha256:216c552ea5ba7b0e3f6e33624e129981c39996021403518019d19b8843c27cbc
Deleted: sha256:17f623af01e277c5ffe6779af8164907de02d9af7a0e161662fc735dd64f117b

//์ ๊ฑฐ ๋ ๊ฒ ํ์ธ
joozeroOP@DESKTOP-1I1JMBR:~$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       20.04     817578334b4d   2 weeks ago   72.8MB
```

> ๐ก ์ปจํ์ด๋๋ ์ด๋ฏธ์ง๊ฐ ํ๋์ง๋ง ์ฌ๋ฌ๊ฐ์ ์ปจํ์ด๋๋ฅผ ์์ฑํ  ์ ์๋ค.
- **์ปจํ์ด๋ ์์์ ํ์ผ์ ๋ง๋ค๊ฑฐ๋ ์ด๋ค ๊ฒ์ ์ค์นํ๋ค๋ ๊ฒ์ ์ปจํ์ด๋์ ์ํฅ์ ์ฃผ๋ ๊ฒ์ด์ง, ์ด๋ฏธ์ง์ ์ํฅ์ ์ฃผ์ง ์๋๋ค ! !**

<label style=font-size:5px>์ด ํฌ์ธํธ๊ฐ ํ  .. ๋งค์ฐ ์ ๊ธฐํ๋ค  ๋ญ๊ฐ ๋ด๊ฐ OS์ ๋น์ทํ ๊ฒ์ ์์ฑํ์ฌ ์ฌ์ฉํ  ์ ์๋ค๋ ๊ฒ์ด .. ใใใ</label>

๊ฐ์ ๋ด์ฉ : velog.io/@joo_zero
