# Kubernetes

    docker : portability
    -> docker compose
    -> kubernetes orchestration vs docker swarm


### why?
- user & commmunity size
- kubernetes 통합 실행 환경
- 단일 확장 API

### setup
- minikube
- docker on mac
- kops : kubernetes-aws.io
- EKS : aws


### EKS
    kubernetes
    upstream
    with aws service : cloudwatch, x-ray, etc

### stage
1. create eks cluster
2. create worker load
3. add-on



### log
    여러 pod에서 한 파일에 log를 모으면
    I/O 충돌 일어날듯
    -> AWS : fluentd + elasticsearch + kibana

    pod 정보 : kubelet, cAdvisor
    app 정보 : prometheus

https://eksworkshop.com


### storage
- 임시 볼륨
- 호스트 볼륨
- 네트워크 볼륨

- 임시/호스트 볼륨은 stateless app에 부적합
- AWS : PV, PVC(PV에 req 요청함)


