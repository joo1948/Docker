기본편에선 도커에 운영체제를 설치한 것과 비슷한 것을 하였다.
그렇다면 운영체제가 아닌 내가 필요한 애플리케이션만 설치할 수 있을까?  
 **할 수 있다.**


# 필요한 애플리케이션의 필요한 이미지를 설치하는 것

## ex ) nginx 설치해보기


``` shell
$ sudo docker pull nginx:latest
latest: Pulling from library/nginx
bd159e379b3b: Pull complete
6659684f075c: Pull complete
679576c0baac: Pull complete
22ca44aeb873: Pull complete
b45acafbea93: Pull complete
bcbbe1cb4836: Pull complete
Digest: sha256:5ffb682b98b0362b66754387e86b0cd31a5cb7123e49e7f6f6617690900d20b2
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest

```
``` shell
$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    5d58c024174d   2 weeks ago   142MB
ubuntu       20.04     817578334b4d   4 weeks ago   72.8MB
```
## image 실행 시 이름 지정

``` shell
$ sudo docker run -d --name hello-nginx nginx:latest
5ea4872129542d05cf1cac03534af0a960f70588566927dec99faf1a775f7dcd
```
** hellog-nginx로 이름 지정  **


``` shell
$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
5ea487212954   nginx:latest   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    hello-nginx

```

**> 여기서 ! 만약 80번 포트를 사용하고 있던 nginx가 있다면 ?? >> 충돌난다 !**

## 이미지 실행 시 충돌 방지 명령어

``` shell
$ sudo docker run -d --name hello-nginx -p 8080:80 nginx:latest
```
- -p : 포트 옵션
- 의미 : 외부에서 접속 할 때 내 컴퓨터의 운영체제의 8000번으로 들어왔을 때, 도커의 80번 포트로 포트포워딩 해준다.
-       도커 내부적으로는 항상 기본적으로 80번으로 접속 될 것이다.

->모든 것은 이미지 만들어 질 때 소스코드도 포함되어있다 !

## exec
외부에서 컨테이너 안의 명령을 실행하는 것.
``` shell
$ sudo docker exec hello echo "Hello World"
Hello World
```



