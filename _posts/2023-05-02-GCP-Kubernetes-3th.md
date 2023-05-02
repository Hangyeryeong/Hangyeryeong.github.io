---
title: "Chapter 03. Kubernetes를 통한 클라우드 조정"

tags:
 - [Kubernetes]

category: Kubernetes
toc: true
toc_sticky: true

date: 2023-05-02
last_modified_at: 2023-05-02

#img: ":aws 1.png"

---

<!-- outline-start -->


### 개요<br/>

- Kubernetes Engine을 사용하여 완전한 Kubernetes 클러스터를 프로비저닝함<br/>
- `kubectl`을 사용하여 Docker 컨테이너를 배포하고 관리함<br/>
- Kubernetes의 디플로이먼트 및 서비스를 사용하여 애플리케이션을 마이크로서비스로 분할함<br/><br/>

- Kubernetes는 애플리케이션에 중점을 둔다. 이 파트는 'app'이라는 예제 애플리케이션을 사용하여 실습을 한다.<br/><br/>

- `App`은 Github에서 호스팅되며 애플리케이션을 제공한다.<br/><br/>

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

#### Google Kubernetes Engine<br/>

- 영역 설정<br/>

```yaml
gcloud config set compute/zone us-central1-b
```

<br/><br/>

- 사용할 클러스터 시작<br/>

```yaml
gcloud container clusters create io
```

<br/>

사진첨부
<br/><br/>

- Kubernetes Engine이 백그라운드에서 몇몇 가상 머신을 프로비저닝하고 있으므로 클러스터를 만드는 데 다소 시간이 걸린다.<br/><br/>


### 샘플 코드 가져오기<br/>

- GitHub 저장소를 클론하기<br/>

```yaml
gsutil cp -r gs://spls/gsp021/* .
```

<br/><br/>

- 디렉터리로 변경하기<br/>

```yaml
cd orchestrate-with-kubernetes/kubernetes
```

<br/><br/>

- 파일을 나열하여 작업 중인 파일을 확인<br/>

```yaml
ls
```

<br/>

사진첨부
<br/><br/>

### 간략한 Kubernetes 데모<br/>

- Kubernetes를 시작하는 가장 쉬운 방법은 `kubectl create`명령어를 사용하는 것임. 이를 사용하여 nginx 컨테이너의 단일 인스턴스를 실행한다.<br/><br/>

```yaml
kubectl create deployment nginx --image=nginx:1.10.0
```

<br/><br/>

- Kubernetes가 배포를 생성함. 배포 덕분에 pod가 작동하고 있으며, pod가 실행하는 노드에 오류가 발생해도 계속해서 작동한다.<br/>
- Kubernetes에서 모든 컨테이너는 포드에서 실행된다. `kubectl get pods`명령어를 사용하여 실행중인 nginx 컨테이너를 확인함.<br/><br/>

```yaml
kubectl get pods
```

<br/>

사진첨부
<br/><br/>

- nginx 컨테이너가 실행되면 `kubectl expose`명령어를 사용하여 Kubenetes 외부로 노출시킬 수 있다.<br/>

```yaml
kubectl expose deploymen nginx --port 80 --type LoadBalancer
```

<br/><br/>

- Kubernetes가 백그라운드에서 공개 IP주소가 첨부된 외부 부하 분산기를 만들었다. 이 공개 IP 주소를 조회하는 모든 클라이언트는 서비스 백그라운드에 있는 포드로 라우팅된다. 이 경우에는 nginx 포드로 라우팅된다.<br/><br/>

- `kubectl get services` 명령어를 사용하여 서비스를 나열하기<br/>

```yaml
kubectl get services
```

<br/>

사진첨부
<br/><br/>

- 참고 : `ExternalIP` 필드가 채워지는 데 시간이 어느정도 소요될 수 있다.<br/><br/>

- 원격으로 Nginx 컨테이너를 조회하려면 이 명령어에 외부 IP를 추가한다.<br/>

```yaml
curl http://<External IP>:80
```

<br/>

사진첨부
<br/><br/>

- Kubernetes는 `kubectl` 실행 및 노출 명령어로 바로 사용할 수 있는 간편한 워크플로를 지원한다.<br/><br/>


### 포드<br/>

- Kubernetes의 핵심에는 `포드`가 있다.<br/>
- 포드는 1개 이상의 컨테이너가 포함된 모음을 나타낸다. 일반적으로 상호 의존성이 높은 컨테이너가 여러 개 있으면 이를 하나의 포드에 패키징한다.<br/><br/>

사진첨부
<br/><br/>

- 이 예시는 모놀리식 및 nginx 컨테이너가 포함된 포드가 있다.<br/>
- 포드에는 `볼륨` 또한 포함되어 있다. 볼륨은 포드가 존재하는 한 계속해서 존재하는 데이터 디스크이며 포드에 포함된 컨테이너에 의해 사용될 수 있다. 포드는 콘텐츠에 공유된 네임스페이스를 제공한다. 즉 이 예시의 포드 안에 있는 2개의 컨테이너는 서로 통신할 수 있으며 첨부된 볼륨도 공유한다.<br/>
- 또한 포드는 네트워크 네임스페이스도 공유한다. 즉 포드는 IP 주소를 1개씩 갖고 있다.<br/><br/>

### 포드 만들기<br/>

- 포드는 포드 구성 파일을 사용하여 만들 수 있다.<br/>
- 모놀리식 포드 구성 파일을 살펴보자.<br/><br/>

```yaml
cat pods/monolith.yaml
```

<br/>

사진첨부
<br/><br/>

- 포드가 1개의 컨테이너(모놀리식)로 구성되어 있다.<br/>
- 시작할 때 컨테이너로 몇 가지 인수가 전달된다.<br/>
- HTTP 트래픽용 포드 870이 개방된다.<br/><br/>

- `kubectl`을 사용하여 모놀리식 포드를 만든다.<br/>

```yaml
kubectl create -f pods/monolith.yaml
```

<br/>

사진첨부
<br/><br/>

- `kubectl get pods` 명령어를 사용하여 기본 네임스페이스에서 실행 중인 모든 포드를 나열한다.<br/>

```yaml
kubectl get pods
```

<br/>

사진첨부
<br/><br/>

- 참고 : 모놀리식 pod가 작동하는 데는 몇 초 걸릴 수 있다. 이를 실행하기 위해 Docker Hub에서 모놀리식 컨테이너 이미지를 가져와야 한다.<br/><br/>

- 포드가 실행되면 `kubectl describe` 명령어를 사용하여 모놀리식 포드에 관해 자세히 알아보자.<br/>

```yaml
kubectl describe pods monolith
```

<br/>

사진첨부
<br/><br/>

- 포드 IP 주소 및 이벤트 로그를 포함한 모놀리식 포드에 관한 여러 정보가 표시된다. 이 정보는 문제 해결 시 유용하게 사용된다.<br/>
- Kubernetes를 사용하면 구성 파일에 포드에 관해 설명하여 간편하게 포드를 만들 수 있으며, 포드가 실행 중일 때 정보를 쉽게 확인할 수 있다. 이제 디플로이먼트에 필요한 모든 포드를 만들 수 있다.<br/><br/>

### 포드와 상호작용하기<br/>

- 포드에는 기본적으로 비공개 IP 주소가 부여되며 클러스터 밖에서는 접근할 수 없다.<br/>
- `kubectl port-forward` 명령어를 사용하여 로컬 포트를 모놀리식 포드 안의 포트로 매핑한다.<br/><br/>

- Cloud Shell 터미널 2개를 연다. 하나는 `kubectl port-forward` 명령어를 실행하고 다른 하나는 `curl` 명령어를 실행하기 위한 것임.<br/><br/>

- 두 번째 터미널에서 포드 전달을 설정한다.<br/>

```yaml
kubectl port-forward monolith 10080:80
```

<br/>

사진첨부
<br/><br/>

- 첫 번째 터미널에서 `curl`을 사용하여 pod와 통신을 시작하기.<br/>

```yaml
curl http://127.0.0.1:10080
```

<br/>

사진첨부
<br/><br/>

- 컨테이너가 'Hello'라고 인사를 건네는 것을 확인할 수 있다.<br/><br/>

- `curl` 명령어를 사용하여 보안이 설정된 엔드포인트를 조회해 보자.<br/>

```yaml
curl http://127.0.0.1:10080/secure
```

<br/>

- 문제가 발생했다. 모놀리식에서 다시 인증 토큰을 얻기 위해 로그인을 시도한다.<br/>

```yaml
curl -u user http://127.0.0.1:10080/login
```

<br/>

- 로그엔 메시지에서 비밀번호 `password`를 사용하여 로그인하기<br/><br/>

사진첨부
<br/><br/>

- 로그인하여 JWT 토큰이 출력된 것을 확인할 수 있다. Cloud Shell은 긴 문자열을 제대로 복사하지 못하니 토큰을 위한 환경 변수를 만든다.<br/><br/>

```yaml
TOCKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')
```

<br/>

사진첨부
<br/><br/>

- 호스트 비밀번호를 묻는 메시지가 나오면 비밀번호 `password`를 다시 입력한다.<br/>
- 그 다음 토큰을 복사하고, 이 토큰을 `curl`을 사용하여 보안이 설정된 엔드포인트를 조회한다.<br/><br/>

```yaml
curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
```

<br/><br/>

- 애플리케이션으로부터 모두 제대로 작동한다는 응답이 전송된다.<br/>
- `kubectl logs` 명령어를 사용하여 `monolith` 포드의 로그를 확인한다.<br/>

```yaml
kubectl logs monolith
```

<br/>

사진첨부
<br/><br/>

- 세 번째 터미널을 열고 `-f` 플래그를 사용하여 실시간 로그 스트림을 가져온다.<br/>

```yaml
kubectl logs -f monolith
```

<br/>

사진첨부
<br/><br/>

- 첫 번재 터미널에서 `curl`을 사용하여 모놀리식 pod와 상호작용했다면 세 번째 터미널에서 로그가 업데이트 되는 것을 확인할 수 있다.<br/><br/>

```yaml
curl http://127.0.0.1:10080
```

<br/>

사진첨부
<br/><br/>

- `kubectl exec` 명령어를 사용하여 모놀리식 포드의 대화형 셸을 실행한다. 이는 컨테이너 내부에서 문제를 해결할 때 유용하다.<br/>

```yaml
kubectl exec monolith --stdin --tty -c monolith /bin/sh
```

<br/>

사진첨부
<br/><br/>

- 예를 들어 모놀리식 컨테이너에 셸이 있으면 `ping` 명령어를 사용하여 외부 연결을 테스트 할 수 있다.<br/>

```yaml
ping -c 3 google.com
exit
```

<br/>

- 대화형 셸 사용을 완료한 후에는 반드시 로그아웃 한다.<br/><br/>

- 이와 같이 포드와의 상호작용은 `kubectl` 명령을 사용하는 것만큼 쉽다. 원격으로 컨테이너를 조회하거나 로그인 셸이 필요한 경우 Kubernetes가 작업에 필요한 모든 것을 제공한다.<br/><br/>


### 서비스<br/>

- 포드는 영구적으로 지속되지 않는다. 활성 여부 또는 준비 상태 검사 오류와 같은 다양한 이유로 중지되거나 시작될 수 있으며, 이로 인해 문제가 발생한다.<br/>
- 포드 집합과 통신해야 하는 경우 어떻게 해야 할까? 포드가 다시 시작되면 IP 주소가 바뀔 수도 있다.<br/>
- 이와 같은 상황에서 `서비스`가 유용하다. 서비스는 포드를 위해 안정적으로 엔드포인트를 제공한다.<br/><br/>

사진첨부
<br/><br/>

- 서비스는 라벨을 사용하여 어떤 포드에서 작동할지 결정한다. 포드에 라벨이 정확히 지정되어 있다면 서비스가 이를 자동으로 감지하고 노출시킨다.<br/>
- 서비스가 제공하는 포드 집합에 대한 액세스 수준은 서비스 유형에 따라 다르다. 현대 3가지 유형이 있다.<br/>

1. `ClusterIP(내부)` - 기본 유형이며 이 서비스는 클러스터 안에서만 볼 수 있다.<br/>
2. `NodePort` - 클러스터의 각 노드ㅔㅇ 외부에서 액세스 가능한 IP 주소를 제공한다.<br/>
3. `LoadBalancer` - 클라우드 제공업체로부터 부하 분산기를 추가하여 서비스에서 유입되는 트래픽을 내부에 있는 노드로 전달한다.<br/><br/>


### 서비스 만들기<br/>

- 서비스를 만들기 전에 https 트래픽을 처리할 수 있도록 보안이 설정된 포드를 만든다.<br/>
- 디렉터리를 수정했을 경우 `~/orchestrate-with-kubernetes/kubernetes` 디렉터리로 다시 돌아간다.<br/><br/>

```yaml
cd ~/orchestrate-with-kubernetes/kubernetes
```

<br/><br/>

- 모놀리식 서비스 구성 파일을 살펴보기<br/>

```yaml
cat pods/secure-monolith.yaml
```

<br/>

사진첨부
<br/><br/>

- 보안이 설정된 모놀리식 포드와 구성 데이터를 만든다.<br/><br/>

```yaml
kubectl create secret generic tls-certs --from-file tls/
kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
kubectl create -f pods/secure-monolith.yaml
```

<br/><br/>

- 보안이 설정된 포드가 있으니 이를 외부로 노출시킨다. 이렇게 하기 위해 Kubernetes 서비스를 만든다.<br/>
- 모놀리식 서비스 구성 파일을 살펴본다.<br/>

```yaml
cat services/monolith.yaml
```

<br/>

사진첨부
<br/><br/>

- 참고<br/>

1. `app: monolith` 및 `secure: enabled` 라벨이 지정된 포드를 자동으로 찾고 노출시키는 선택지가 있다.<br/>
2. 외부 트래픽을 포드 31000에서 포트 443의 nginx로 전달하기 위해 NodePort를 노출시켜야 한다.<br/><br/>

- `kubectl create` 명령어를 사용하여 모놀리식 서비스 구성 파일에서 모놀리식 서비스를 만든다.<br/>

```yaml
kubectl create -f services/monolith.yaml
```

<br/>

사진첨부
<br/><br/>

- 서비스 노출 시에는 pod가 사용된다. 즉 다른 앱이 서버 중 하나의 포트 31000과 연결을 시도하면 포트 충돌이 발생할 수 있다.<br/>
- 일반적으로 포트 할당은 Kubernetes가 처리한다. 여기서는 포트를 선택했기 때문에 추후 더 쉽게 상태 확인을 설정할 수 있다.<br/>
- `gcloud compute firewall-rules` 명령어를 사용하여 트래픽을 노출된 NodePort의 모놀리식 서비스로 보낸다.<br/>

```yaml
gcloud compute firewall-rules create allow-monolith-nodeport \
  --allow=tcp:31000
```

<br/>

사진첨부
<br/><br/>

- 포트 전달 없이 클러스터 밖에서 안전한 모놀리식 서비스를 조회할 수 있다.<br/>

- 노드 1개의 외부 IP 주소를 가져온다.<br/>

```yaml
gcloud compute instances list
```

<br/>

사진첨부
<br/><br/>

- `curl`을 사용하여 보안이 설정된 모놀리식 서비스를 조회한다.<br/>

```yaml
curl -k https://<EXTERNAL_IP>:31000
```

<br/>

- 시간이 초과되었다.<br/><br/>


### 포드에 라벨 추가하기<br/>

- 현재 모놀리식 서비스에는 엔드포인트가 없다. 이와 같은 문제를 해결하는 방법 중 하나는 라벨 쿼리와 함께 `kubectl get pods` 명령어를 사용하는 것임.<br/>
- 모놀리식 라벨이 지정되어 실행되는 포드 몇 개가 있다는 사실을 확인할 수 있다.<br/>

```yaml
kubectl get pods -l "app=monolith"
```

<br/><br/>

사진첨부
<br/><br/>

- 그런데 'app=monolith'와 'secure=enabled'는?<br/>

```yaml
kubectl get pods -l "app=monolith,secure=enabled"
```

<br/>

사진첨부
<br/><br/>

- 이 라벨 쿼리로는 결과가 출력되지 않는다. 'secure=enabled' 라벨을 추가해야 할 것 같다.<br/><br/>

- `kubectl label` 명령어를 사용하여 보안이 설정된 모놀리식 포드에 누락된 `secure=enabled` 라벨을 추가한다. 그런 다음 라벨이 업데이트 되었는지 확인한다.<br/>

```yaml
kubectl label pods secure-monilith 'secure=enabled'
kubectl get pods secure-monolith --show-labels
```

<br/><br/>

- 모놀리식 서비스의 엔드포인트 목록 확인하기<br/>

```yaml
kubectl describe services monolith | grep Endpoints
```

<br/>

사진첨부
<br/><br/>

- 엔드포인트가 하나 있는 것을 확인할 수 있다.<br/><br/>

- 노드 중 하나를 조회하여 이 엔드포인트를 테스트하기<br/>

```yaml
gcloud compute instances list
curl -k https://<EXTERNAL_IP>:31000
```

<br/><br/>

사진첨부
<br/><br/>

- 성공한 것을 확인할 수 있다.<br/><br/>


### Kubernetes로 애플리케이션 배포하기<br/>

- 이번 목표 : 프로덕션의 컨테이너를 확장하고 관리하는 것<br/>
- 이와 같은 상황에서 `이플로이먼트`가 유용함. 디플로이먼트는 실행 중인 포드의 개수가 사용자가 명시한 포드 개수와 동일하게 만드는 선언적 방식임.<br/><br/>

사진첨부
<br/><br/>

- 배포의 주요 이점은 pod 관리에서 낮은 수준의 세부 정보를 추상화하는 데 있다.<br/>
- 배포는 백그라운드에서 `복제본 집합`을 사용하여 pod의 시작 및 중지를 관리한다. Pod를 업데이트하거나 확장해야 하는 경우 배포가 이를 처리한다.<br/>
- 또한 디플로이먼트는 어떤 이유로든 포드가 중지되면 재시작을 담당하여 처리한다.<br/><br/>

- ex.<br/>

사진첨부
<br/><br/>

- 포드는 생성 기반 노드의 전체 기간과 연결되어 있다.<br/>
- 예시에서 Node3이 중단되면서 포드도 중단되어있다. 직접 새로운 포드를 만들고 이를 위한 노드를 찾는 대신, 디플로이먼트가 새로운 포드를 만들고 Node2에서 실행했다.<br/>
- 아주 편리한 방식임.<br/>
- 포드와 서비스에 관해 배운 모든 지식을 바탕으로, 이제 디플로이먼트를 사용하여 모놀리식 애플리케이션을 작은 서비스로 분할해보자.<br/><br/>


### 디플로이먼트 만들기<br/>

- 모놀리식 앱을 다음 3가지 부분으로 나눈다.<br/>
1. `auth` - 인증된 사용자를 위한 JWT 토큰을 생성<br/>
2. `hello` - 인증된 사용자를 안내<br/>
3. `frontend` - 트래픽을 auth 및 hello 서비스로 전달<br/><br/>

- 각 서비스용 디플로이먼트를 만들 준비가 됐다. 그런 다음 auth 및 hello 디플로이먼트용 내부 서비스와 frontend 디플로이먼트용 외부 서비스를 정의하자. 이렇게 하면 모놀리식과 같은 방식으로 마이크로 서비스와 상호작용할 수 있으며, 각 버시르를 독립적으로 확장하고 배포할 수 있다.<br/><br/>

- auth 디플로이먼트 구성 파일을 검토하기<br/>

```yaml
cat deployment/auth.yaml
```

<br/>

사진첨부
<br/><br/>

- 디플로이먼트가 복제본 1개를 만들며, 여기서는 auth 컨테이너 2.0.0 버전을 사용한다.<br/>
- `kubectl create` 명령어를 실행하여 auth 디플로이먼트를 만들면 디플로이먼트 매니페스트 데이터를 준수하는 포드가 만들어진다. 즉 복제본 필드에 명시된 숫자를 변경하여 포드 숫자를 조정할 수 있다.<br/><br/>

- 디플로이먼트 개체 만들기<br/>

```yaml
kubectl create -f deployments/auth.yaml
```

<br/>

사진첨부
<br/><br/>

- auth 디플로이먼트용 서비스 만들기. `kubectl create` 명령어를 사용하여 auth 서비스를 만들기<br/>

```yaml
kubectl create -f services/auth.yaml
```

<br/>

사진첨부
<br/><br/>

- hello 디플로이먼트 만들기와 노출도 위와 같이 동일하게 진행<br/>

```yaml
kubectl create -f deployments/hello.yaml
kubectl create -f services/hello.yaml
```

<br/><br/>

- frontend 디플로이먼트 만들기와 노출 또한 위와 동일하게 진행<br/>

```yaml
kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
kubectl create -f deployments/frontend.yaml
kubectl create -f services/frontend.yaml
```

<br/><br/>

- frontend를 만들기 위해 컨테이너에 구성 데이터를 보관해야 하기 때문에 추가 단계를 진행한다.<br/><br/>

- 외부 IP 주소를 확보하고 curl 명령어를 사용하여 frontend와 상호작용한다.<br/>

```yaml
kubectl get services fronetend
curl -k https://<EXTERNAL-IP>
```

<br/><br/>

사진첨부
<br/><br/>

사진첨부
<br/><br/>

