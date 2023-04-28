---
title: "Chapter 01. Docker 소개"

tags:
 - [Kubernetes]

category: Kubernetes
toc: true
toc_sticky: true

date: 2023-04-26
last_modified_at: 2023-04-26

#img: ":aws 1.png"

---

<!-- outline-start -->

### 시작 전.<br/>

- Google Cloud 기본 창<br/>

사진첨부
<br/><br/>

- Cloud BashShell<br/>

사진첨부
<br/><br/>

사진첨부
<br/>
잘 열린 것을 확인할 수 있다.
<br/><br/>


### 개요<br/>

- `Docker` : 애플리케이션을 개발, 출시, 실행하는 데 사용하는 개방형 플랫폼.<br/><br/>

1. Docker 컨테이너 빌드, 실행, 디버그하기<br/>
2. Docker Hub 및 Google Artifact Registry에서 Docker 이미지 가져오기<br/>
3. Docker 이미지를 Google Artifact Registry로 푸시하기<br/><br/>

### 시작<br/>

- 사용 중인 계정 이름 목록 표시<br/>

```yaml
gcloud auth list
```

<br/>

사진첨부
<br/><br/>

- 프로젝트 ID 목록 표시<br/>

```yaml
gcloud config list project
```

<br/>

사진첨부
<br/><br/>

### 작업 01. Hello World<br/>

1. 먼저 Cloud Shell을 열고 `hello world 컨테이너를 실행`한다.<br/>

```yaml
docker run hello-world
```

<br/>

사진첨부
<br/>

- 결과는 `Hello from Docker!`를 반환한다.<br/>
- Docker 데몬이 hello-world 이미지를 검색했으나 로컬에서 이미지를 찾지 못했고, Docker Hub라는 공개 레지스트리에서 이미지를 가져오고, 가져온 이미지에서 컨테이너를 생성하고, 컨테이너를 실행했다.
<br/><br/>

2. Docker Hub에서 가져온 컨테이너 `이미지를 확인`하기.<br/>

```yaml
docker images
```

<br/>

사진첨부
<br/>

- 위와 같은 결과는 Docker Hub 공개 레지스트리에서 가져온 이미지이다.<br/>
- 이미지 ID는 `SHA256 해시` 형식임.<br/>
- 여기서는 프로비저닝된 Docker 이미지를 지정한다.<br/>
- Docker 데몬이 로컬에서 이미지를 찾을 수 없으면 기본적으로 공개 레지스트리에서 이미지를 검색함.<br/><br/>

3. 컨테이너 `다시 실행`하기<br/>

```yaml
docker run hello-world
```

<br/>

사진첨부
<br/>

- 두 번째 실행했을 때, Docker 데몬이 로컬 레지스트리에서 이미지를 찾은 뒤 이 이미지에서 컨테이너를 실행하였음을 확인할 수 있음.<br/>
- Docker 데몬이 반드시 Docker Hub에서 이미지를 가져올 필요는 없음.<br/><br/>

4. `실행 중인 컨테이너 확인`<br/>

```yaml
docker ps
```

<br/>

사진첨부
<br/>

- 실행중인 컨테이너가 없다. 전에 실행한 hello-world 컨테이너를 이미 종료했다.<br/><br/><br/>

5. 실행이 완료된 컨테이너를 포함하여 모든 컨테이너 보기.<br/>

```yaml
docker ps -a
```

<br/>

사진첨부
<br/>

- Docker가 컨테이너를 식별하기 위해 생성한 UUID인 `CONTAINER ID`와 실행에 관한 추가 메타데이터가 표시된다.<br/>
- 컨테이너 `Names`도 무작위로 생성되지만 `docker run --name [container-name] hello-world`를 사용하여 지정할 수 있음.<br/><br/>


### 작업 02. 빌드<br/>

- 간단한 노드 애플리케이션을 기반으로 한 Docker 이미지를 빌드하기.<br/><br/>

1. `test`라는 이름의 폴더를 만들고 이 폴더로 이동하기.<br/>

```yaml
mkdir test && cd test
```

<br/>

사진첨부
<br/><br/>

2. `Dockerfile` 만들기<br/>

```yaml
cat > Dockerfile <<EOF
# Use an official Node runtime as the parent image
FROM node:lts
# Set the working directory in the container to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
ADD . /app
# Make the container's port 80 available to the outside world
EXPOSE 80
# Run app.js using node when the container launches
CMD ["node", "app.js"]
EOF
```

<br/>

- 첫 번째 줄 : 기본 상위 이미지를 지정함. 이 경우 기본 상위 이미지는 노드 버전 장기적 지원(LTS)의 공식 Docker 이미지임.<br/>
- 두 번째 줄 : 컨테이너의 (현재) 작업 디렉터리를 설정.<br/>
- 세 번째 줄 : 현재 디렉터리의 콘텐츠(`"."`로 표시)를 컨테이너에 추가함.<br/>
- 컨테이너의 포트를 공개하여 해당 포트에서의 연결을 허용하고 마지막으로 노드 명령어를 실행하여 애플리케이션을 시작함.<br/><br/>

3. 노드 애플리케이션 생성<br/>

```yaml
cat > app.js <<EOF
const http = require('http');
const hostname = '0.0.0.0';
const port = 80;
const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World\n');
});
server.listen(port, hostname, () => {
    console.log('Server running at http://%s:%s/', hostname, port);
});
process.on('SIGINT', function() {
    console.log('Caught interrupt signal and will exit');
    process.exit();
});
EOF
```

<br/>

- 포트 80에서 수신대기하고 'Hello World'를 반환하는 간단한 HTTP 서버임.<br/><br/>

사진첨부
<br/><br/>



4. Dockerfile이 있는 디렉터리에서 다음 명령어를 실행한다. 현재 디렉터리를 의미하는 `"."`를 주의하자.<br/>

```yaml
docker build -t node-app:0.1 .
```

<br/>

사진첨부
<br/>

- 이 명령어는 시간이 조금 걸린다.
<br/><br/>

- 결과는 다음과 비슷하게 출력됨.<br/>

```yaml
Sending build context to Docker daemon 3.072 kB
Step 1 : FROM node:lts
6: Pulling from library/node
...
...
...
Step 5 : CMD node app.js
 ---> Running in b677acd1edd9
 ---> f166cd2a9f10
Removing intermediate container b677acd1edd9
Successfully built f166cd2a9f10
```

<br/>

- `-t` : `name:tag` 문법을 사용하여 이미지의 이름과 태그를 지정하는 역할을 함. 이미지 이름은 `node-app`이고 `태그`는 `0.1`임. Docker 이미지를 빌드할 때는 태그를 사용하는 것이 좋음. 태그를 지정하지 않으면 태그가 기본값인 `latest`로 지정되어 최신 이미지와 기존 이미지를 구분하기 어려워짐.<br/><br/>

5. 빌드한 이미지 확인하기.<br/>

```yaml
docker images
```

<br/>

사진첨부
<br/>

- `node`는 기본 이미지, `node-app`은 빌드한 이미지.<br/>
- `node`를 삭제하려면 우선 `node-app`을 삭제해야 함. 이미지의 크기는 VM에 비해 상대적으로 작음. `node:slim` 및 `node:alpine`과 같은 노드 이미지의 다른 버전을 사용하면 더 작은 이미지를 제공하여 이식성을 높일 수 있음.<br/>
- 노드의 공식 저장소에서 모든 버전을 확인할 수 있음.<br/><br/>

### 작업 03. 실행<br/>

1. 빌드한 이미지를 기반으로 하는 컨테이너를 실행함.<br/>

```yaml
docker run -p 4000:80 --name my-app node-app:0.1
```

<br/>

사진첨부
<br/>

- `--name` 플래그를 사용하면 원하는 경우 컨테이너 이름을 지정할 수 있음.<br/>
- `-p`는 Docker가 컨테이너의 포트 80에 호스트의 포트 4000을 매핑하도록 지시하는 플래그임.<br/>
- `http://localhost:4000`에서 서버에 접속할 수 있다. 포트 매핑이 없으면 localhost에서 컨테이너에 접속할 수 없다.<br/><br/>

2. 다른 터미널을 열고(Cloud Shell에서 `+`아이콘을 클릭) 서버를 테스트하기.<br/>

```yaml
curl http://localhost:40
```

<br/>

사진첨부
<br/>

- 초기 터미널이 실행되는 동안 컨테이너가 실행됨. 컨테이너를 터미널 세션에 종속시키지 않고 백그라운드에서 실행하려면 `-d` 플래그를 지정해야 함.<br/><br/>

3. 초기 터미널을 닫은 후 컨테이너를 중지하고 삭제하기.<br/>

```yaml
docker stop my-app && docker rm my-app
```

<br/>

사진첨부
<br/><br/>

4. 백그라운드에서 컨테이너를 시작하기.<br/>

```yaml
docker run -p 4000:80 --name my-app -d node-app:0.1 docker ps
```

<br/>

사진첨부
<br/><br/>

5. `docker ps`의 출력된 결과에서 컨테이너가 실행중임을 확인할 수 있음. `docker logs [container_id]`를 실행하면 로그를 볼 수 있음.<br/><br/>

- 참고) 앞부분에 입력한 문자들로 컨테이너를 고유하게 식별할 수 있다면 전체 컨테이너 ID를 입력할 필요는 없다. 예를 들어 컨테이너 ID가 `17bcaca6f....` 인 경우 `docker logs 17b`를 실행할 수 있다.<br/><br/>


```yaml
docker logs d534dc739701
```

<br/>

사진첨부
<br/><br/>

#### 애플리케이션 수정하기.<br/>

1. Cloud Shell에 앞서 실습에서 만든 테스트 디렉터리를 열기.<br/>

```yaml
cd test
```

<br/><br/>

2. 원하는 텍스트 편집기(예: nano 또는 vim)fh `app.js`를 편집하고 'Hello World`를 다른 문자열로 바꾸기.<br/>

```yaml
vi app.js
```

<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

3. 새 이미지를 빌드하고 `0.2`로 태그를 지정하기.<br/>

```yaml
docker build -t node-app:0.2 .
```

<br/>

사진첨부
<br/>

- 2단계에서 기존 캐시 레이어를 사용하고 있음을 확인할 수 있다. 3단계 이해부터는 `app.js`를 변경했기 때문에 레이어가 수정 되었다.<br/><br/>

4. 새 이미지 버전으로 다른 컨테이너를 실행하기. 이때 호스트 포트를 80 대신 8080으로 매핑하는 방법을 확인하기. 호스트 포트 4000은 이미 사용 중이므로 사용할 수 없음.<br/>

```yaml
docker run -p 8080:80 --name my-app-2 -d node-app:0.2
docker ps
```

<br/><br/>

사진첨부
<br/><br/>

5. 컨테이너 테스트하기.<br/>

```yaml
curl http://localhost:80
```

<br/>

사진첨부
<br/><br/>

6. 처음 작성한 컨테이너를 테스트하기.<br/>

```yaml
curl http://localhost:4000
```

<br/>

사진첨부
<br/><br/>

### 작업 04. 디버그<br/>

- 디버깅 사례 살펴보기<br/><br/>

1. `docker logs [container_id]`를 사용해 컨테이너의 로그를 볼 수 있음. 컨테이너가 실행 중일 때 로그 출력을 확인하려면 `-f` 옵션을 사용함.<br/><br/>

```yaml
docker logs -f [container_id]
```

<br/><br/>

사진첨부
<br/>

- 실행 중인 컨테이너에서 대화형 Bash 세션을 시작해야 할 수 있다.<br/><br/>

2. 이 경우 `docker exec`를 사용함. 다른 터미널을 열고(Cloud Shell에서 `+` 아이콘을 클릭) 다음 명령어를 작성하기.<br/><br/>

```yaml
docker exec -it [container_id] bash
```

<br/>

- `-it` 플래그는 pseudo-tty를 할당하고 stdin을 열린 상태로 유지하여 컨테이너와 상호작용할 수 있도록 함. `Dockerfile`에 지정된 `WORKDIR` 디렉터리(/app)에서 bash가 실행된 것을 확인할 수 있음. 그럼 이제 디버깅할 컨테이너 내에서 대화형 셸세션을 사용할 수 있음.<br/><br/>

사진첨부
<br/><br/>

3. 디렉터리 확인<br/>

```yaml
ls
```

<br/>

4. Bash 세션 종료<br/>

```yaml
exit
```

<br/>

사진첨부
<br/><br/>

5. Docker inspect를 통해 Docker에서 컨테이너의 메타데이터를 검토할 수 있음.<br/>

```yaml
docker inspect [container_id]
```

<br/>

사진첨부
<br/><br/>

6. `--format`을 사용하여 반환된 JSON의 특정 필드를 검사함.<br/>

```yaml
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_id]
```

<br/><br/>

사진첨부
<br/><br/>

### 작업 05. 게시<br/>

- 이미지를 `Google Artifact Registry`로 푸시함.<br/>
- 모든 컨테이너와 이미지를 삭제하여 새로운 환경을 시뮬레이션하고 컨테이너를 가져와서 실행함.<br/>
- 이를 통해 Docker 컨테이너의 이식성을 확인할 수 있음.<br/>
- Artifact Registry에서 호스팅하는 비공개 레지스트리에 이미지를 푸기하려면 이미지에 레지스트리 이름으로 태그를 지정해야 함. 형식은 `<regional-repository>-docker.pkg.dev/my-project/my-repo/my-image` 임.<br/><br/>

#### 대상 Docker 저장소 만들기<br/>

- 이미지를 푸시하려면 먼저 저장소를 만들어야 함.<br/>
- 이미지를 푸시해도 저장소 만들기가 트리거되지 않으며 Cloud Build 서비스 계정에는 저장소를 만들 권히 없다.<br/><br/>

1. `타색 메뉴`의 CI/CD에서 `Artifact Registry > 저장소`로 이동하기<br/>

사진첨부
<br/><br/>

2. `저장소 만들기` 클릭.<br/>

사진첨부
<br/><br/>

3. 저장소 이름 : `my-respository`<br/>
4. 형식 : `Docker`<br/>
5. 위치 유형 - 리전 : `us-central1 (Iowa)`<br/>
6. `만들기` 클릭.<br/>

사진첨부
<br/><br/>

사진첨부
<br/>
잘 만들어진 것을 확인할 수 있다.
<br/><br/>

#### 인증 구성하기<br/>

- 이미지를 푸시하거자 가져오려면 먼저 Docker가 Artifact Registry에 대한 요청을 인증하는데 Google Cloud CLI를 사용하도록 구성해야 함.<br/><br/>

1. us-central1 리전의 Docker 저장소에 인증을 설정하려면 Cloud Shell에서 다음 명령어를 실행함.<br/>

```yaml
gcloud auth configure-docker us-central1-docker.pkg.dev
```

<br/>

2. 메시지가 표시되면 `Y`를 입력.<br/><br/>

- 이 명령어는 Docker 구성을 업데이트함.<br/>
- 이제 Google Cloud 프로젝트의 Artifact Registry와 연결하여 이미지를 푸시하고 가져올 수 있음.<br/><br/>


사진첨부
<br/><br/>

#### 컨테이너를 Artifact Registry로 푸시하기<br/>

1. 프로젝트 ID를 설정하고 Dockerfile이 포함된 디렉터리로 변경한다.<br/>

```yaml
export PROJECT_ID=$(gcloud config get-value project)
cd ~/test
```

<br/>

사진첨부
<br/><br/>

2. `node-app:0.2`에 태그를 지정하기<br/>

```yaml
docker build -t us-central1-docker.pkg.dev/$PROJECT_ID/my-repository/node-app:0.2 .
```

<br/><br/>

3. 빌드된 Docker 이미지를 확인하기<br/>

```yaml
docker images
```

<br/>

사진첨부
<br/><br/>

3. 이 이미지를 Artifact Registry로 푸시하기<br/>

```yaml
docker push us-central1-docker.pkg.dev/$PROJECT_ID/my-repository/node-app:0.2
```

<br/>

사진첨부
<br/>
시간이 조금 걸린다.
<br/><br/>

사진첨부
<br/>
빌드가 완료가 된 것을 확인할 수 있다.<br/><br/>

4. `탐색 메뉴`의 CI/CD에서 `Artifact Registry > 저장소`로 이동한다.<br/>
5. `my-repository`를 클릭하면 `node-app` Docker 컨테이너가 생성된 것을 볼 수 있다.<br/><br/>

사진첨부
<br/><br/>

#### 이미지 테스트하기<br/>

- 새로운 VM을 시작하고 SSH로 새 VM에 접속한 다음 gcloud를 설치할 수도 있지만 여기서는 간단하게 모든 컨테이너와 이미지를 삭제하여 새로운 환경을 시뮬레이션할 것이다.<br/><br/>

1. 모든 컨테이너를 중지하고 삭제한다.<br/>

```yaml
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
```

<br/>

사진첨부
<br/><br/>

- 노드 이미지를 삭제하기 전에 (`node:lts`의) 하위 이미지를 삭제헤야 한다.<br/><br/>

2. 모든 Docker 이미지를 삭제한다.<br/>

```yaml
docker rmi us-central1-docker.pkg.dev/$PROJECT_ID/my-repository/node-app:0.2
docker rmi node:lts
docker rmi -f $(docker images -aq) # remove remaining images
docker images
```

<br/>

- 이제 새로운 환경이나 다름 없다.<br/><br/>

3. 이미지를 가져와서 실행한다.<br/>

```yaml
docker pull us-central1-docker.pkg.dev/$PROJECT_ID/my-repository/node-app:0.2
docker run -p 4000:80 -d us-central1-docker.pkg.dev/$PROJECT_ID/my-repository/node-app:0.2
curl http://localhost:4000
```

<br/><br/>

- 결과)<br/>

```yaml
Welcome to Cloud
```

<br/><br/>


