---
title: TIL-20210814
date: 2021-08-14
category: 도커
tags: TIL
---

실행중인 도커 컨테이너 보기  
-a : 모두 보기

```shell
$ docker container ps -a
```

도커 컨테이너 생성과 실행(대화형으로 실행)  
-i : interaction

-t : tty

```
$ docker container run -it --name {container_name}
```

도커 컨테이너 생성과 실행(백그라운드로 실행)  
-d: detach 백그라운드로 실행

-p: port {host port}:{container port}

```
$ docker container run -d -p 80:80 --name apache httpd:latest
```
