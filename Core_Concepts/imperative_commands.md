### 유용한 옵션

`--dry-run`: 명령어 실행 시 리소스 즉시 생성됨. 단순히 명령을 테스트하려면 `--dry-run=client` 옵션 사용하기. 리소스 생성 여부와 명령이 올바름 유무를 알려줌.

`-o yaml`: 리소스 정의를 yaml 형식으로 출력

<br>

### 명령어 (imperative)

#### POD

`kubectl run nginx --image=nginx`: nginx 파드 생성

`kubectl run nginx --image=nginx --dry-run=client -o yaml`: 파드 매니페스트 파일을 yaml 형식으로 생성. (파드를 생성하지는 않음)

#### Deployment

`kubectl create deployment --image=nginx nginx`: 디플로이먼트 생성

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`: 디플로이먼트 매니페스트 파일을 yaml 형식으로 생성. (디플로이먼트를 생성하지는 않음)

`kubectl create deployment nginx --image=nginx --replicas=4`: 4개의 레플리카를 가지는 디플로이먼트 생성 

`kubectl scale deployment nginx --replicas=4`: 디플로이먼트 확장 (4개의 레플리카로)

`kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml`: 4개의 레플리카로 디플로이먼트 확장 (위와 동일한 명령어이나 yaml 파일을 수정하는 형식)

#### Service

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`: redis-service라는 clusterIP유형의 서비스 생성하여 6379번 포트에서 파드 노출

-> 파드의 라벨을 selector로 사용

`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`:  redis-service라는 clusterIP유형의 서비스 생성하여 6379번 포트에서 파드 노출

-> 파드의 라벨을 selector로 사용하지 않음 (app=redis로 가정) 

`kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml`: nginx라는 NodePort유형의 서비스 생성하여 nginx의 80번 포트를 노드의 30080번 포트에 노출시킴.

`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml` : 위와 동일한 명령어

<br>

출처 : Certified Kubernetes Administrator (CKA) with Practice Tests 강의