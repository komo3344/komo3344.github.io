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

# Dockerfile

FROM -> base image

RUN -> 이미지를 빌드할때 실행됨

CMD -> 이미지 빌드 후 컨테이너에서 실행
ENTRYPOINT -> 이미지 빌드 후 컨테이너에서 실행

차이점은 ENTRYPOINT는 불변
CMD는 사용자가 컨테이너에서 수정가능

---

ONBUILD -> 베이스 이미지에서 자주 사용됨

> Dockerfile.base

```docker
FROM ubuntu:18.04

ONBUILD RUN echo "Hello, Docker!"
```

```shell
ex)
sudo docker build -t [이미지이름] [Dockerfile 경로]
-f 옵션은 Dockerfile의 파일명을 명시적으로 알려줌
$ sudo docker build -t base -f Dockerfile.base .
```

빌드 중 hello, Docker 라는 텍스트가 보임

> Dockerfile

```docker
FROM base
```

```shell
sudo docker build -t hello .
```

마찬가지로 빌드 중 hello, Docker 라는 텍스트가 보임

ENV -> 환경변수 설정  
ENV에서 할당된 변수는 RUN, CMD, ENTRYPOINT 명령에서 모두 사용 가능하며 선언된 환경변수를 사용하기 위해서는 $를 앞에 붙이면 된다.  
WORKDIR -> 작업디렉토리 할당 (없으면 생성 후 이동)  
USER -> 유저할당
LABLE -> 이미지 버전정보, 작성자 등 이미지 레이블 정보 등록  
ARG -> Dockerfile 내부 변수 할당
