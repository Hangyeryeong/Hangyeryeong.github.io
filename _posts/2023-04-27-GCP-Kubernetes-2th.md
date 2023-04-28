---
title: "Chapter 01. Kubernetes Engine: Qwik Start"

tags:
 - [Kubernetes]

category: Kubernetes
toc: true
toc_sticky: true

date: 2023-04-27
last_modified_at: 2023-04-27

#img: ":aws 1.png"

---

<!-- outline-start -->


### 개요<br/>

- GKE 클러스터를 실행하면 Google Cloud의 고급 클러스터 관리 기능을 활용할 수 있다.<br/>
    ex. Computer Engine 인스턴스용 `부하 분산`,<br/>
    `노드 풀`로 클러스터 안에 하위 노드 집합을 지정하여 유연성 강화,<br/>
    클러스터의 노드 인스턴스 개수 `자동 확장`,<br/>
    클러스터의 노드 소프트웨어 `자동 업그레이드`,<br/>
    `노드 자동 복구`로 노드 상태 및 가용성을 유지 관리,<br/>
    Cloud Monitoring을 통한 `로깅 및 모니터링`으로 클러스터에 대한 가시성 확보<br/><br/>

- GKE를 사용하여 컨테이너화된 애플리케이션을 30분 이내에 배포하는 방법.<br/><br/>

### 시작 전<br/>

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

### 작업 01. 기본 컴퓨팅 영역 설정<br/>

- `컴퓨터 영역`이란 클러스터와 리소스가 존재하는 리전 내 대략적인 위치를 의미함. 예를 들어 `us-central1-a`는 `us-central1` 리전에 속한 영역임. Cloud Shell에서 새 세션을 시작함.<br/><br/>

1. 기본 컴퓨팅 리전 설정<br/>

```yaml
gcloud config set compute/region assigned_at_lab_start
```

<br/>

사진첨부
<br/><br/>

2. 기본 컴퓨팅 영역 설정<br/>

```yaml
gcloud config set compute/zone assigned_at_lab_start
```

<br/>

사진첨부
<br/><br/>

### 작업 02. GKE 클러스터 만들기<br/>

- `클러스터`는 1개 이상의 클러스터 마스터 머신과 노드라는 여러 작업자 머신으로 구성된다. 노드란 클러스터를 구성하기 위해 필요한 Kubernetes 프로세스를 실행하는 `Compute Engine 가상머신(VM) 인스턴스`이다.<br/><br/>

- 참고) 클러스터 이름은 영문자로 시작하고 영숫자 문자로 끝나야 하며 40자(영문 기준)이하여야 한다.<br/><br/>

1. 클러스터 만들기<br/>

```yaml
gcloud container clusters crate --machine-type=e2-medium --zone=assigned_at_lab_start lab-cluster
```

<br/>

사진첨부
<br/><br/>

- 출력에 표시되는 경고는 모두 무시해도 상관 없다. 클러스터 생성이 완료되기까지 몇 분이 걸릴 수 있다.<br/><br/>

### 작업 03. 클러스터의 사용자 인증 정보 얻기<br/>

- 클러스터를 만든 후 클러스터와 상호작용하려면 사용자 인증 정보가 피룡하다.<br/><br/>

1. 클러스터에 인증<br/>

```yaml
gcloud container clusters get-credentials lab-cluster
```

<br/>

사진첨부
<br/><br/>

### 작업 04. 클러스터에 애플리케이션 배포<br/>

- 이제 클러스터에 컨테이너화된 애플리케이션을 배포할 수 있음.<br/>
- `hello-app`을 클러스터에서 실행한다.<br/><br/>

- GKE는 Kubernetes 객체를 사용하여 클러스터의 리소스를 만들고 관리한다. 웹 서버와 같은 스테이트리스(Stateless) 애플리케이션을 배포할 때는 Kubernetes에서 `배포` 객체를 사용한다. `서비스` 객체는 인터넷에서 애플리케이션에 액세스하기 위한 규칙과 부하 분산 방식을 정의한다.<br/><br/>

1. `hello-app` 컨테이너 이미지에서 새 배포 `hello-server`를 생성하려면 다음 `kubectl create` 명령어를 실행한다.<br/>

```yaml
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

<br/>

사진첨부
<br/><br/>

- 이 Kubernetes 명령어를 사용하면 `hello-server`를 나타내는 배포 객체가 생성된다. 여기서 `--image`는 배포할 컨테이너 이미지를 지정한다. 해당 명령어는 `Container Registry` 버킷에서 예시 이미지를 가져온다. `gcr.io/google-samples/hello-app:1.0`은 가져올 이미지 버전을 나타낸다. 버전이 지정되지 않은 경우 최신 버전이 사용된다.<br/><br/>

2. 애플리케이션을 외부 트래픽에 노출할 수 있는 Kubernetes 리소스인 Kubernetes Service를 생성하려면 다음 `kubectl expose` 명령어를 실행한다.<br/><br/>

```yaml
kubectl expose deployment hello-server --type-LoadBalancer --port 8080
```

<br/>

- `--port` : 컨테이너가 노출될 포트를 지정<br/>
- `type="LoadBalancer"` : 컨테이너의 Compute Engine 부하 분산기를 생성<br/><br/>

사진첨부
<br/><br/>

3. `hello-server` 서비스를 검사하려면 `kubectl get`을 실행<br/>

```yaml
kubectl get service
```

<br/>

사진첨부
<br/><br/>

- 참고) 외부 IP 주소가 생성되기까지 1분 정도 걸릴 수 있다. `EXTERNAL-IP` 열이 `대기중` 상태이면 위 명령어를 다시 실행하자.<br/><br/>

4. 웹 브라우저에서 애플리케이션을 보려면 새 탭을 열고 다음 주소를 입력한다. 여기서 `[EXTERNAL IP]`sms `hello-server`의 `EXTERNAL-IP`로 바꾼다.<br/><br/>

```yaml
http://[EXTERNAL-IP]:8080
```

<br/>

사진첨부
<br/><br/>

접근이 잘 되는 것을 확인할 수 있다.
<br/><br/>

### 작업 05. 클러스터 삭제<br/>

1. 클러스터를 삭제<br/>

```yaml
gcloud container clusters delete lab-cluster
```

<br/><br/>

2. 메시지가 표시되면 `Y`를 입력하여 확인함.<br/>
- 클러스터를 삭제하는 데 몇 분 정도 걸릴 수 있다.
<br/><br/>

사진첨부
<br/><br/>

삭제가 완료되었다.
<br/><br/>

