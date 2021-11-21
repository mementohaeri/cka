# kubectl apply command

이전 강의에서는 declarative(선언형) 방식으로 오브젝트를 관리하는 방법을 배움.

```yaml
# nginx.yaml
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
 labels:
  app: myapp
  type: front-end-service
spec:
 containers:
 - name: nginx-container
   image: nginx:1.18
```

```
# 오브젝트 생성
$ kubectl apply -f nginx.yaml
$ kubectl apply -f /path/to/config-files

# 오브젝트 업데이트
$ kubectl apply -f nginx.yaml
```

<br>

### 1. Kubectl Apply

만일 오브젝트가 존재하지 않는다면, `kubectl apply -f nginx.yaml` 명령어 실행 시 오브젝트가 새로 생성됨.

Local 영역에서는,

```yaml
# nginx.yaml
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
 labels:
  app: myapp
  type: front-end-service
spec:
 containers:
 - name: nginx-container
   image: nginx:1.18
```

Kubernetes 영역에서는, 아래와 같이 쿠버네티스 클러스터 내에서 특정 포맷에 맞게끔 오브젝트의 설정 정보가 저장됨.

```yaml
# Live object configuration
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
 labels:
  app: myapp
  type: front-end-service
spec:
 containers:
 - name: nginx-container
   image: nginx:1.18
status:
 conditions:
 - lastProbeTime: null
   status: "True"
   type: Initialized
```

하지만 `kubectl apply` 명령어를 사용한다면 아래와 같이 json 포맷으로 설정 파일이 변환되어 저장됨.

```json
{
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata":{
        "annotations": {},
        "labels":{
            "run": "myapp-pod",
            "type": "front-end-service"
        },
        "name": "myapp-pod",
    },
    "spec":{
        "containers":{
            "image": "nginx:1.18",
            "name": "nginx-container"
        }
    }
}
```

위와 같은 상황에서 local 영역의 파일에서 수정이 발생한다면, `kubectl apply` 명령어 실행 시 kubernetes 영역의 yaml 파일과 비교해 업데이트를 반영하고 yaml 파일도 순차적으로 업데이트가 반영됨

즉, local(yaml) -> kubernetes(yaml) -> last applied configuration(json)

<br>

### 2. Last applied Configuration

가장 최근에 적용된 설정이 저장된 json 파일은 kubernetes 영역의 yaml 파일에 저장되어 있음

```yaml
# Live object configuration
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
 annotations:
  kubectl.kubernetes.io/last-applied-configuration:
  {"apiVersion": "v1","kind": "Pod","metadata":{"annotations": {},"labels":{"run": "myapp-pod","type": "front-end-service"},"name": "myapp-pod",},"spec":{"containers":{"image": "nginx:1.18","name": "nginx-container"}}}
 labels:
  app: myapp
  type: front-end-service
spec:
 containers:
 - name: nginx-container
   image: nginx:1.18
status:
 conditions:
 - lastProbeTime: null
   status: "True"
   type: Initialized
```

이는 오로지 `kubectl apply` 명령어 사용 시에만 kubernetes 영역의 yaml 파일에 가장 최근에 적용된 설정이 저장됨 -> `kubectl create`나 `kubectl replace` 명령어는 저장 안됨.

<br>

<br>

출처 : Certified Kubernetes Administrator (CKA) with Practice Tests 강의