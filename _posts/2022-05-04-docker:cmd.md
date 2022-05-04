# Docker 명령어

> 실행중인 컨테이너를 보여줌

```bash
docker ps
```



>  실행이 종료된 것을 포함해서 모든 컨테이너를 보여줌

```bash
docker ps -a
```



>  생성된 혹은 다운로드 된 이미지를 보여줌

```bash
docker images
```



>  모든 이미지를 보여줌

```bash
docker images -a 
```



>  도커 이미지를 생성

```bash
docker build -t '도커허브에 가입한 계정명'/'이미지명 (프로젝트명 권장)':'버전'
```



> 도커 이미지를 컨테이너로 실행 시킴

```bash
docker run --name '컨테이너 명' -d -p '호스트포트':'도커포트' '이미지 명':'버전' .
```



> 도커 터미널 입력 옵션으로 실행

```bash
docker run -it '이미지 명' /bin/bash
```



> 백그라운드 실행

```bash
docker run -d 
```



>  실행된 도커 컨테이너에 로그를 보여줌

```bash
docker logs '컨테이너 명'
```



> 컨테이너 삭제

```bash
docker rm '컨테이너 명' 
```



> 이미지 삭제

```bash
docker rmi '이미지 명'
```

