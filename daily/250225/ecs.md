# Git Action 및 ECS 활용하기

## 개요

- [Git Action 및 ECS 활용하기](#git-action-및-ecs-활용하기)
    - [개요](#개요)
    - [절차 정리](#절차-정리)
    - [ECR 이미지 업로드](#ecr-이미지-업로드)
    - [Task Definition 정의](#task-definition-정의)
    - [클러스터 생성](#클러스터-생성)
    - [서비스 생성](#서비스-생성)
    - [ECS 외부 접속 허용](#ecs-외부-접속-허용)
        - [Application Load Balancer 세팅](#application-load-balancer-세팅)
        - [Domain 설정](#domain-설정)
        - [접속 호출 테스트](#접속-호출-테스트)
- [Cluster / Service / Task 의 리소스 관리](#cluster--service--task-의-리소스-관리)
    - [Git Action을 통해서 CD를 적용하게 되면,](#git-action을-통해서-cd를-적용하게-되면)
    - [추천 하는 방향](#추천-하는-방향)
- [References](#references)
    - [Elastic IP 고정해서 사용하기](#elastic-ip-고정해서-사용하기)

## 절차 정리

- Image 생성
    - 이미지의 경우에는 Docker Build시 플랫폼을 정확히 확인하고 필드 처리
- LoadBalancer 구성
    - 로드밸런서에서 사용할 포트는 Image 내의 포트를 확인해야함.
- ECS 구성
    - Task 정의
    - Cluster 정의
    - Service 정의
        - Task 연결
        - LoadBalancer 연결
            - https://repost.aws/ko/knowledge-center/create-alb-auto-register
            - https://aws.amazon.com/ko/blogs/korea/using-static-ip-addresses-for-application-load-balancers/
                - Network LoadBalancer
                - Application LoadBalancer
        - Service 연결

## ECR 이미지 업로드

아래의 작업전에 profile의 설정을 맞춰놓고 진행해야함.

```shell 
vi ~/.aws/config 
```

```shell 
aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin *****
```

```shell 
docker tag {local_image_name}:{tag} {upload_image_name}:{tag}
```

```shell 
docker push {upload_image_name}:{tag}
```

## Task Definition 정의

해당 정의된 파일은 Json 또는 콘솔에서 생성가능하다. 현재는 github action에 연동하기 위해서 github repository 내에서 관리된다.

- [Task Definition 정의]()
    - [Task Definition Json]()

## 클러스터 생성

- [ECS Cluster 구성]()

## 서비스 생성

정의한 Task를 연결해서 사용하기 위해서 서비스를 생성합니다.

- [Repair Backend Service]()


현재 구성된 방식은

- Rolling Update 방식으로 구성되었음. 추후에는 Blue/Green 방식을 이용해서 무중단 배포로 단계적인 구성 필요
    - Deployment Options
        - Rolling Update
        - Blue/Green Deployment
- Service auto scaling 를 이용한 Scale Out을 적용할 수 있음.


## ECS 외부 접속 허용

### Application Load Balancer 세팅

- http / 80 구성
- https / 443 구성

### Domain 설정

Application Load Balancer를 직접 연동하여 구성하였음.

- backend.owner.bike 내에 하위의 ALB를 구성하였음.
    - repair-backend-alb-1605063382.ap-northeast-2.elb.amazonaws.com

### 접속 호출 테스트

- 초기 정보

```shell 
curl —location 'https://***.***.***/commons/banks' \
—header 'repairshop_token: '
```

- Load Balancer 접근

```shell 
curl —location 'http://***.***.***commons/banks' \
—header 'repairshop_token: '
```

# Cluster / Service / Task 의 리소스 관리

- Cluster의 스펙 구성은 t2.2xlarge(8vcpu/32GB)로 구성되어 있을 때,
- Task의 스펙은 3vcpu/14GB로 구성할 수 있음
    - 추가로 할수 있을수 있는지는 코드 검토 필요함.

## Git Action을 통해서 CD를 적용하게 되면,

기존의 Task와 신규 Task가 교체되는 순간에 Task 하나에 정의한 리소스의 약 2배 정도의 리소스가 순간적으로 사용됨.

배포 방식에 따라 조금의 차이가 있을 수 있지만, 일시적으로 2개의 리소스를 유지해야하기 때문에 이로 인하여 Cluster의 메모리가 위와 같이 구성됨.


## 추천 하는 방향

기본적으로 컨테이너로 사용되는 이미지에 대해서 1~2vcpu, 4~5gb의 메모리로 구성하고 cpu 사용량이 70% 이상일 경우 AutoScaling을 통해서  
Task가 동적으로 늘어나는 구조를 추천하며, 현재 백엔드 서비스 내의 실제 처리 가능한 범위를 검토하여 처리를 필요로 합니다.

# References

- https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth
- [AWS CLI 설치 및 CLI 로그인 방법 알아보기](https://sh-t.tistory.com/62)
- https://chinggin.tistory.com/875
- https://junuuu.tistory.com/803
- https://chinggin.tistory.com/875
- https://a3magic3pocket.github.io/posts/aws-rds-allow-ec2/
- https://velog.io/@gun_123/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4-ECS%EB%A1%9C-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0
- https://rok93.tistory.com/entry/SpringBoot-Application-ECS-%EB%B0%B0%ED%8F%AC

### Elastic IP 고정해서 사용하기

- https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/alb.html
- https://goodahn.tistory.com/55#google_vignette
- https://aws.amazon.com/ko/blogs/korea/using-static-ip-addresses-for-application-load-balancers/
