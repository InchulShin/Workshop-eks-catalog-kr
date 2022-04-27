# 컨테이너 이미지 만들기

본 실습은 컨테이너 및 컨테이너 이미지에 대해 보다 더 자세히 알고 싶은 분들을 위한 독립적인 실습입니다. 본 실습을 진행하지 않으셔도 Amazon EKS로 웹페이지를 구성하는 실습을 진행하시는 데에는 지장이 없습니다. 본 실습을 원치 않으신 분들은 4-2 Amazon ECR에 이미지 올리기 페이지로 이동합니다.

## 컨테이너 이미지 직접 만들기

![](../images/container-image-overview.png)

Docker File이란 컨테이너 이미지를 만들기 위한 설정 파일입니다. 즉, 빌드될 이미지의 청사진이라고 생각하면 됩니다. 이런 이미지가 컨테이너가 되면 실질적으로 application이 구동되는 것입니다.

1. root 폴더(/home/ec2-user/environment) 위치에서 아래의 값을 붙여넣습니다.

```
cd ~/environment/
```

```
cat << EOF > Dockerfile
FROM nginx:latest
RUN  echo '<h1> test nginx web page </h1>'  >> index.html
RUN cp /index.html /usr/share/nginx/html
EOF
```

Docker File에서 각각이 의미하는 바는 아래와 같습니다.

1. FROM : Base Image 지정(OS 및 버전 명시, Base Image에서 시작해서 커스텀 이미지를 추가)

2. RUN : shell command를 해당 docker image에 실행시킬 때 사용함

3. WORKDIR : Docker File에 있는 RUN, CMD, ENTRYPOINT, COPY, ADD 등의 지시를 수행할 곳

4. EXPOSE : 호스트와 연결할 포트 번호를 지정

5. CMD : application을 실행하기 위한 명령어

6. docker build 명령어로 이미지를 만듭니다. name에는 컨테이너 이미지 이름을 기재하고 tag의 경우, 따로 작성하지 않으면 latest라는 값을 가지게 됩니다. 본 실습에서는 컨테이너 이미지 이름으로 test-image를 적습니다.

```
docker build -t test-image . 
```

7. docker images 명령어로 생성된 이미지를 확인합니다.

```
docker images
```

8. docker run 명령어로 이미지를 컨테이너로 실행시킵니다. 아래의 명령어는 test-image라는 컨테이너 이미지를 사용하여 test-nginx라는 이름의 컨테이너를 실행하는데, 호스트 8080 포트와 컨테이너의 80 포트가 맵핑된다는 의미입니다.

```
docker run -p 8080:80 --name test-nginx test-image
```

즉, 호스트의 8080 포트로 전달된 정보들이 도커를 통해, 컨테이너의 80 포트로 포워딩되는 것입니다.

9. docker ps 명령어로 현재의 호스트에서 실행 중인 컨테이너를 확인할 수 있습니다. AWS Cloud9에서 새로운 터미널 창을 열고 아래의 명령어를 입력합니다.

```
docker ps
```

![](../images/docker-ps.png)

10. docker logs 명령어로 해당 컨테이너의 로그를 출력해서 상태를 확인할 수 있습니다.

```
docker logs -f test-nginx
```

11. docker exec 명령어로 컨테이너 내부 쉘 환경으로 접근할 수 있습니다.

```
docker exec -it test-nginx /bin/bash
```

12. AWS Cloud9에서 상단 Tools > Preview > Preview Running Application 을 클릭하면 현재 실행 중인 애플리케이션을 확인할 수 있습니다.

![](../images/docker-preview-application-01.png)

![](../images/docker-preview-application-02.png)

13. docker stop 명령어로 실행 중인 컨테이너를 중지합니다.

```
docker stop test-nginx
```

다시 docker ps 명령어를 치면 방금까지 실행되었던 컨테이너가 목록에서 사라진 것을 확인할 수 있습니다.

14. docker rm 명령어로 컨테이너를 삭제합니다. 컨테이너 삭제는 컨테이너가 정지되어 있어야 가능합니다.

```
docker rm test-nginx
```

docker rmi 명령어로 컨테이너 이미지를 삭제합니다.

```
docker rmi test-image
```

docker images 명령어를 치면 생성되었던 컨테이너 이미지가 목록에 없음을 확인할 수 있습니다.

이 밖에도 Docker 명령어를 통해, cpu, memory 제한, 호스트와 디렉토리 공유 등의 작업을 수행할 수 있습니다.

[Previous](./40-container.md) | [Next](./200-eks.md)
