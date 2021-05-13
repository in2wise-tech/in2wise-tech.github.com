---
layout: post
title: 01.Hello Minikube
summary: 쿠버네티스 튜토리얼
featured-img: 
categories: Kubernetes
---

From [Kubernetes/Tutorials/Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/)



# Hello Minikube

## Run Minikube on the Katacoda 

[kubernetes.io](https://kubernetes.io/)에서는 개발용 Kubernetes인 minikube를 브라우저에서 예제 어플리케이션을 운영해볼 수 있는 Katacoda를 제공합니다.

Katacoda를 활용하여 Kubernetes를 맛보기를 해보겠습니다.



[Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/) 페이지에서 ***Launch Terminal*** 을 클릭하면 아래 그림과 같이 터미널이 열리며, 초기 세팅이 진행됩니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-01.PNG"| relative_url}})



## Show dashboard

Terminal에서 아래 명령어를 입력하여 Minikube의 Dashboard를 실행할 수 있습니다.

```sh
minikube dashboard
```

Terminal 위 ***preview Port 30000*** 버튼을 클릭하여 dashboard에 접근할 수 있으며, 아래 그림과 같이 kubernetes가 서비스되는 것을 확인할 수 있습니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-02.PNG"| relative_url}})



## Create Node

아래 명령어를 통해 Container Repository에서 echoserver 이미지를 사용하여 hello-node 이름으로 새로운 Node를 생성합니다.

 ```sh
 kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
 ```

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-03.PNG"| relative_url}})



## Check Deployments

아래 명령어를 통하여 현재 구성된 Deployments를 확인할 수 있습니다.

```sh
kubectl get deployments
```

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-04.PNG"| relative_url}})



## Check Pods

아래 명령어를 통하여 현재 구성된 Pods를 확인할 수 있습니다.

```sh
kubectl get pods
```

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-05.PNG"| relative_url}})



## View Cluster Event

아래 명령어를 통해 클러스터에서 발생한 이벤트를 확인할 수 있습니다.

```sh
kubectl get events
```

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-06.PNG"| relative_url}})



## View the `kubectl` configuration

아래 명령어를 통해 kubectl의 현재 설정 값을 확인할 수 있습니다.

```sh
kubectl config view
```

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-07.PNG"| relative_url}})



## Create a Service

### 포트 노출

이미 생성한 `hello-node` 컨테이너를 외부에서 접근가능하도록 8080포트를 노출시키기 위하여 아래 명령어를 사용합니다.

```sh
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

`--type=LoadBalancer`는 외부로 노출시키겠다는 옵션입니다.

```sh
kubectl get services
```

위 명령어를 입력하면 아래와 같이 서비스 정보를 얻을 수 있으며, 노출시킨 컨테이너의 8080 포트와 연결된 외부 포트 번호를 확인할 수 있습니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-08.PNG"| relative_url}})

### 서비스 실행

아래 명령어를 통해 hello-node 노드에서 서비스를 시작합니다.

```sh
minikube service hello-node
```

아래 결과가 나타난 이후, Terminal 상단의 Preview Port 30000 우측에 있는 **+**버튼을 눌러 ***select port to view on Host 1***을 선택합니다. 새로운 탭에서 페이지가 열리면 앞서 확인한 외부 포트 번호를 입력하고 ***display Port***를 클릭하여 결과를 확인합니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-09.PNG"| relative_url}})



## addons 활성화

아래 명령어를 통하여 현재 활성화 된 addons 정보를 얻을 수 있습니다.

```sh
minikube addons list
```

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-10.PNG"| relative_url}})

이 가운데 metrics-server를 활성화 및 비활성화 해보도록 하겠습니다.

```sh
minikube addons enable metrics-server
```

활성화가 완료되면 아래와 같은 결과가 출력됩니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-11.PNG"| relative_url}})

```sh
kubectl get pod,svc -n kube-system
```

위 명령어를 통해 pod와 service 정보를 포함한 kube-system의 정보를 확인할 수 있으며, 아래 그림과 같이 출력됩니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-12.PNG"| relative_url}})

```sh
minikube addons disable metrics-server
```

위 명령어를 활용하여 metrics-server를 다시 비활성화 시킬수 있으며 아래와 같은 결과가 출력됩니다.

![image]({{"/assets/img/posts/Kubernetes/01.hello_world/2021-05-14-13.PNG"| relative_url}})



## 마무리

본 포스팅에서 생성한 service, deployment, minikube를 종료 및 삭제하고 포스팅을 마치겠습니다.

### service 및 deployment 삭제

```sh
kubectl delete service hello-node
kubectl delete deployment hello-node
```

### stop Minikube

```sh 
minikube stop
```

### delete Minikube

```sh
minikube delete
```













