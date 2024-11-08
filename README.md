## 실습 1

## install kubectl (Kubernetes)

- [https://kubernetes.io/docs/tasks/tools/](https://kubernetes.io/docs/tasks/tools/)

```bash
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

$ kubectl version --client
Client Version: v1.31.2
Kustomize Version: v5.4.2

$ kubectl version --client --output=yaml
clientVersion:
  buildDate: "2024-10-22T20:35:25Z"
  compiler: gc
  gitCommit: 5864a4677267e6adeae276ad85882a8714d69d9d
  gitTreeState: clean
  gitVersion: v1.31.2
  goVersion: go1.22.8
  major: "1"
  minor: "31"
  platform: linux/amd64
kustomizeVersion: v5.4.2
```


## minikube start
- [https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download)
```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
$ rm minikube-linux-amd64

$ minikube start
```

## minikube dashboard
`$ minikube dashboard`

## deploy

```bash
$ minikube addons enable dashboard
$ minikube addons enable metrics-server
$ minikube addons list
$ minikube dashboard --url
```
- 웹사이트에서 + 버튼 누르고 app name, 도커 이미지와 포트 지정.

`$ kubectl get svc` 로 실행중 확인

`$ minikube service <app name> --url` 로 접속 가능


## 실습 2

```bash
# pod 확인
$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-test-686f6ff54d-gkk9h   1/1     Running   0          29m

# pod 상세정보 - 도커 컨테이너 정보
$ kubectl describe pod nginx-test-686f6ff54d-gkk9h

# 해당 도커 컨테이너로 접속
# $ kubectl exec -it <pod-name> -c <container-name> -- /bin/bash
$ kubectl exec -it nginx-test-686f6ff54d-gkk9h -c nginx-test -- /bin/bash

# 로그확인
$ kubectl logs -f nginx-test-686f6ff54d-gkk9h -c nginx-test
```

## 생성 및 변경 적용
$ kubectl apply -f httpd-deployment.yaml
deployment.apps/httpd-deployment created
service/httpd-service created

$ kubectl apply -f https://raw.githubusercontent.com/dMario24/k2s/refs/tags/v1.1.1/httpd-deployment.yaml
deployment.apps/httpd-deployment unchanged
service/httpd-service unchanged

## 배포로 생성된 Pod를 나열
$ kubectl get pods -l app=httpd
NAME                                READY   STATUS    RESTARTS   AGE
httpd-deployment-599bf897b4-m9vn5   1/1     Running   0          15m

## 상세정보
$ kubectl describe pod httpd-deployment-599bf897b4-m9vn5

## 삭제
$ kubectl delete deployment nginx-deployment

## 실습 3

httpd-deployment.yaml, httpd-hpa.yaml, nginx-config.yaml, nginx-deployment.yaml

성능 분석


