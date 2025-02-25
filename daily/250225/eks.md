# 개요

- [개요](#개요)
- [Elastic Kubernetes 구성](#elastic-kubernetes-구성)
    - [EKS 설정 사전 체크](#eks-설정-사전-체크)
    - [사전 설명과 이해해야할 사항](#사전-설명과-이해해야할-사항)
    - [비용 정책 정의](#비용-정책-정의)
    - [관리형 쿠버네티스에 대한 툴](#관리형-쿠버네티스에-대한-툴)
- [EKS 사용가이드](#eks-사용가이드)
    - [EKS 로컬에서 접근하기](#eks-로컬에서-접근하기)
    - [EKS를 Fargate를 이용해서 테라폼을 활용한 서비스 구성하기](#eks를-fargate를-이용해서-테라폼을-활용한-서비스-구성하기)
        - [기본 테라폼 모듈](#기본-테라폼-모듈)
        - [EKS Fargate 사용을 위한 추가 설정](#eks-fargate-사용을-위한-추가-설정)
        - [IAM Policy 설정에 대하여](#iam-policy-설정에-대하여)
        - [Role와 IAMPolicy 바인딩 처리](#role와-iampolicy-바인딩-처리)
        - [EKS 에서의 ALB 사용](#eks-에서의-alb-사용)
        - [IAM Policy Json(APPLICATION LOAD BALANCER 샘플)](#iam-policy-jsonapplication-load-balancer-샘플)
        - [EKS 접근이 안되는 경우](#eks-접근이-안되는-경우)
            - [EKS Access Refreences](#eks-access-refreences)
- [References](#references)

# Elastic Kubernetes 구성

- Amazon EKS + EC2
    - EC2 인스턴스에서 Kubernetes 클러스터를 실행할 수 있도록 지원하는 서비스입니다. 사용자는 EC2 인스턴스를 시작하고, Kubernetes 노드를 등록하여 Kubernetes 클러스터를 실행할 수 있습니다.
- Amazon EKS + Fargate
    - AWS Fargate에서 Kubernetes 클러스터를 실행할 수 있도록 지원하는 서비스입니다. Fargate는 컨테이너를 실행하는 데 필요한 인프라를 자동으로 관리하므로, 사용자는 Kubernetes 클러스터를 더 쉽게 관리할 수 있습니다.

## EKS 설정 사전 체크

- AWS Command Line Interface 체크

```shell
aws --version | cut -d / -f2 | cut -d ' ' -f1
```

## 사전 설명과 이해해야할 사항

- 서비스 컨테이너와 ECS와 달리 쿠버네티스는 클러스터 내에서 로드 밸런서, 영구 볼륨 등도 정의할 수 있습니다. 이때 정의하는 쿠버네티스안에서 하지만 실제 만들어지는 것은 쿠버네티스와 무관한 AWS 리소스입니다. 쿠버네티스 자체는 AWS 구조와 독립적이고, 태그등 다른 방법을 통해 쿠버네티스 리소스가 위치할 AWS 리소스를 찾게 됩니다.

## 비용 정책 정의

- [Amazone EC2 온디맨드 요금](https://aws.amazon.com/ko/ec2/pricing/on-demand/)
- [AWS Fargate 요금](https://aws.amazon.com/ko/fargate/pricing/)

## 관리형 쿠버네티스에 대한 툴

- [eksctl](https://eksctl.io/)
- [AWS Management Console](https://ap-northeast-2.console.aws.amazon.com/eks/home?region=ap-northeast-2#/clusters)
- [AWS CLI Command References](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/eks/index.html)
- [EKS API References](https://docs.aws.amazon.com/eks/latest/APIReference/Welcome.html)
- [Kubectl 및 EKSCTL 설정](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/install-kubectl.html)
- [Terraform EKS](https://tf-eks-workshop.workshop.aws/)

# EKS 사용가이드

- [Amzone Elastic Kubernetes Service 설명서](https://docs.aws.amazon.com/ko_kr/eks/)
- [EKS를 이용한 Application 서비스 하기](https://devblog.kakaostyle.com/ko/2022-03-31-2-web-application-using-eks/)
- [Terraform 활용하기](https://devblog.kakaostyle.com/ko/2022-03-31-3-build-eks-cluster-with-terraform/)

## EKS 로컬에서 접근하기

```shell 
# /.aws/credential 에 접근하기 
aws configure

aws eks update-kubeconfig --region ap-northeast-2 --name lines-eks-cluster --alias {cluster_name}

aws eks --region ap-northeast-2 update-kubeconfig --name imdr-eks-cluster --alias {cluster_name} --profile {profile_name}
```

## EKS를 Fargate를 이용해서 테라폼을 활용한 서비스 구성하기


### 기본 테라폼 모듈

```terraform
provider "aws" {
  region  = var.availability_zone[0]
  shared_config_files = [var.environment_config]
  shared_credentials_files = [var.environment_config]
  profile = "terraform"
}

locals {
  cluster_name = "cluster_name"
  ecr_backend_name        = "backend_project_name"
  ecr_front_name  = "backend_project_typescript_name"
}

module "vpc" {
  source = "terraform-aws-modules/vpc/aws"

  name = "lines-vpc"
  cidr = "10.194.0.0/16"

  azs             = ["ap-northeast-2a", "ap-northeast-2c"]
  public_subnets  = ["10.194.0.0/24", "10.194.1.0/24"]
  private_subnets = ["10.194.100.0/24", "10.194.101.0/24"]

  enable_nat_gateway     = true
  one_nat_gateway_per_az = true

  enable_dns_hostnames = true

  public_subnet_tags = {
    "kubernetes.io/cluster/${local.cluster_name}" = "shared"
    "kubernetes.io/role/elb"                      = "1"
  }

  private_subnet_tags = {
    "kubernetes.io/cluster/${local.cluster_name}" = "shared"
    "kubernetes.io/role/internal-elb"             = "1"
  }
}

resource "aws_iam_role" "cluster" {
  name = "${local.cluster_name}-eks-cluster-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Effect = "Allow",
      Principal = {
        Service = "eks.amazonaws.com"
      },
      Action = "sts:AssumeRole"
    }]
  })
}
resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSClusterPolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
  role       = aws_iam_role.cluster.name
}
resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSVPCResourceController" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSVPCResourceController"
  role       = aws_iam_role.cluster.name
}


module "eks" {
  source = "terraform-aws-modules/eks/aws"

  cluster_name                    = local.cluster_name
  cluster_version                 = "1.30"
  cluster_endpoint_private_access = false
  cluster_endpoint_public_access  = true

  create_iam_role = false
  iam_role_arn = aws_iam_role.cluster.arn

  cluster_addons = {
    coredns = {
      addon_version = "v1.11.1-eksbuild.9"
      configuration_values = jsonencode({
        computeType = "Fargate"
        resources = {
          limits = {
            cpu    = "1"
            memory = "512M"
          }
          requests = {
            cpu    = "1"
            memory = "512M"
          }
        }
      })
    }
    kube-proxy = {}
    vpc-cni = {
      addon_version = "v1.18.3-eksbuild.1"
      configuration_values = jsonencode({
        env = {
          ENABLE_POD_ENI           = "true"
          ENABLE_PREFIX_DELEGATION = "true"
          WARM_PREFIX_TARGET       = "1"
        }
        init = {
          env = {
            DISABLE_TCP_EARLY_DEMUX = "true"
          }
        }
      })
    }
  }

  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets

  cloudwatch_log_group_retention_in_days = 1

  fargate_profiles = {
    default = {
      name = "default"
      selectors = [
        {
          namespace = "kube-system"
        },
        {
          namespace = "default"
        },
        {
          namespace = "bidify"
        }
      ]
    }
  }

  depends_on = [ # see https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_cluster#example-usage
    aws_iam_role_policy_attachment.cluster_AmazonEKSClusterPolicy,
    aws_iam_role_policy_attachment.cluster_AmazonEKSVPCResourceController,
  ]
}

resource "aws_ecr_repository" "lines_g2b_manager_python_repo" {
  name = local.ecr_backend_name

  force_delete = true 
}

resource "aws_s3_bucket" "lines-bidify-bucket" {
  bucket = "lines-bidify-bucket"

  tags = {
    environment = "bidify-production"
  }
}

resource "aws_s3_bucket_public_access_block" "public-access" {
  bucket = aws_s3_bucket.lines-bidify-bucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_policy" "bucket-policy" {
  bucket = aws_s3_bucket.lines-bidify-bucket.id

  depends_on = [
    aws_s3_bucket_public_access_block.public-access
  ]

  policy = <<POLICY
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"PublicRead",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::${aws_s3_bucket.lines-bidify-bucket.id}/*"]
    }
  ]
}
POLICY
}
```

### EKS Fargate 사용을 위한 추가 설정

Fargate를 사용할 경우 EC2 인스턴스가 아닌 내부에서 모두 자동화 관리되기 때문에 해당 설정이 기존의 K8S의서 Scheduler라 리소스를 할당할 위치를 찾는 추가적인 설정이 필요하다.

- 첫번째 방법

```
Modify CoreDNS to Run on Fargate:

If you want CoreDNS to run on Fargate, you'll need to ensure that the CoreDNS deployment tolerates the Fargate-specific taint. This can be done by adding a toleration to the CoreDNS deployment:

tolerations:
  - key: "eks.amazonaws.com/compute-type"
    operator: "Equal"
    value: "fargate"
    effect: "NoSchedule"

Modify the CoreDNS deployment or daemonset (usually located in the kube-system namespace) to include this toleration.
```

- 두번째 방법


```
kubectl patch deployment coredns \
    -n kube-system \
    --type json \
    -p='[{"op": "remove", "path": "/spec/template/metadata/annotations/eks.amazonaws.com~1compute-type"}]'
```

### IAM Policy 설정에 대하여

EKS 내에서 다양한 AWS 의 서비스에 접근하기 위해서는 반드시 아래의 2가지를 세팅해야한다.

- AWSLoadBalancerControllerIAMPolicy
    - 기본적인 IAM 정책 / 어떤 서비스에 어떤 액션을 처리할 수 있는지에 대한 정의

```
curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.0/docs/install/iam_policy.json
```

```
aws iam delete-policy --policy-arn arn:aws:iam::471112615042:policy/AWSLoadBalancerControllerIAMPolicy-bidify
```

```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy-bidify \
    --policy-document file://iam_policy.json
```

```json 
{
    "Policy": {
        "PolicyName": "AWSLoadBalancerControllerIAMPolicy-bidify",
        "PolicyId": "ANPAW3MEAASBKWN4KNH6R",
        "Arn": "arn:aws:iam::471112615042:policy/AWSLoadBalancerControllerIAMPolicy-bidify",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "CreateDate": "2024-08-28T13:39:33+00:00",
        "UpdateDate": "2024-08-28T13:39:33+00:00"
    }
}
```

- AmazonEKSLoadBalancerControllerRole
    - Role은 IAM Policy에 근거하여 실제 호출하게되는 계정의 권한을 연결한다.


```
ACCOUNT_ID=$(aws sts get-caller-identity | jq -r .Account)
OIDC_URL=$(aws eks describe-cluster --name lines-eks-cluster --query "cluster.identity.oidc.issuer" --output text | sed "s|https://||")
```

```
cat > load-balancer-role-trust-policy.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::${ACCOUNT_ID}:oidc-provider/$OIDC_URL"
            },
            "Action": [
              "sts:AssumeRoleWithWebIdentity",
            ],
            "Condition": {
                "StringEquals": {
                    "${OIDC_URL}:aud": "sts.amazonaws.com",
                    "${OIDC_URL}:sub": "system:serviceaccount:kube-system:aws-load-balancer-controller"
                }
            }
        }
    ]
}
EOF
```

```
aws iam create-role \
    --role-name AmazonEKSLoadBalancerControllerRole-bidify \
    --assume-role-policy-document file://"load-balancer-role-trust-policy.json"
```

```
 aws iam delete-role --role-name AmazonEKSLoadBalancerControllerRole-bidify
```


### Role와 IAMPolicy 바인딩 처리

```
aws iam attach-role-policy \
    --policy-arn arn:aws:iam::${ACCOUNT_ID}:policy/AWSLoadBalancerControllerIAMPolicy-bidify \
    --role-name AmazonEKSLoadBalancerControllerRole-bidify
```


```
 aws iam detach-role-policy \
    --policy-arn arn:aws:iam::${ACCOUNT_ID}:policy/AWSLoadBalancerControllerIAMPolicy-bidify \
    --role-name AmazonEKSLoadBalancerControllerRole-bidify
```

### EKS 에서의 ALB 사용

EKS에서 ALB를 사용하기 위해서는 위의 Role, IamPolicy 권한이 반드시 존재해야하며, Ingress Object를 Kubernetes를 통해서 생성할 때 실제 ALB를 EKS 내부에서 생성하도록 프로세스가 동작한다.

- http, https를 모두 서비스해야하는 경우

아래의 alb.ingress.kubernetes.io/certificate-arn 은 ACM에서 발급한 인증서를 기본으로 하여 사용된다.

```yaml 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: lines-g2b-manager-python-alb
  namespace: bidify
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:ap-northeast-2:471112615042:certificate/cddd4595-4f48-456d-ace0-1062fe04e4b2
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
    alb.ingress.kubernetes.io/ssl-redirect: '443'
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: lines-g2b-manager-python-service
                port:
                  number: 10050
```

### IAM Policy Json(APPLICATION LOAD BALANCER 샘플)

```json 
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateServiceLinkedRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": "elasticloadbalancing.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeAccountAttributes",
                "ec2:DescribeAddresses",
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeInternetGateways",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpcPeeringConnections",
                "ec2:DescribeSubnets",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeInstances",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeTags",
                "ec2:GetCoipPoolUsage",
                "ec2:DescribeCoipPools",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeLoadBalancerAttributes",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:DescribeListenerCertificates",
                "elasticloadbalancing:DescribeSSLPolicies",
                "elasticloadbalancing:DescribeRules",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DescribeTargetGroupAttributes",
                "elasticloadbalancing:DescribeTargetHealth",
                "elasticloadbalancing:DescribeTags",
                "elasticloadbalancing:AddTags"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cognito-idp:DescribeUserPoolClient",
                "acm:ListCertificates",
                "acm:DescribeCertificate",
                "iam:ListServerCertificates",
                "iam:GetServerCertificate",
                "waf-regional:GetWebACL",
                "waf-regional:GetWebACLForResource",
                "waf-regional:AssociateWebACL",
                "waf-regional:DisassociateWebACL",
                "wafv2:GetWebACL",
                "wafv2:GetWebACLForResource",
                "wafv2:AssociateWebACL",
                "wafv2:DisassociateWebACL",
                "shield:GetSubscriptionState",
                "shield:DescribeProtection",
                "shield:CreateProtection",
                "shield:DeleteProtection"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:RevokeSecurityGroupIngress"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateSecurityGroup"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags"
            ],
            "Resource": "arn:aws:ec2:*:*:security-group/*",
            "Condition": {
                "StringEquals": {
                    "ec2:CreateAction": "CreateSecurityGroup"
                },
                "Null": {
                    "aws:RequestTag/elbv2.k8s.aws/cluster": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags"
            ],
            "Resource": "arn:aws:ec2:*:*:security-group/*",
            "Condition": {
                "Null": {
                    "aws:RequestTag/elbv2.k8s.aws/cluster": "true",
                    "aws:ResourceTag/elbv2.k8s.aws/cluster": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:RevokeSecurityGroupIngress",
                "ec2:DeleteSecurityGroup"
            ],
            "Resource": "*",
            "Condition": {
                "Null": {
                    "aws:ResourceTag/elbv2.k8s.aws/cluster": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:CreateLoadBalancer",
                "elasticloadbalancing:CreateTargetGroup"
            ],
            "Resource": "*",
            "Condition": {
                "Null": {
                    "aws:RequestTag/elbv2.k8s.aws/cluster": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:CreateListener",
                "elasticloadbalancing:DeleteListener",
                "elasticloadbalancing:CreateRule",
                "elasticloadbalancing:DeleteRule"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:AddTags",
                "elasticloadbalancing:RemoveTags"
            ],
            "Resource": [
                "arn:aws:elasticloadbalancing:*:*:targetgroup/*/*",
                "arn:aws:elasticloadbalancing:*:*:loadbalancer/net/*/*",
                "arn:aws:elasticloadbalancing:*:*:loadbalancer/app/*/*"
            ],
            "Condition": {
                "Null": {
                    "aws:RequestTag/elbv2.k8s.aws/cluster": "true",
                    "aws:ResourceTag/elbv2.k8s.aws/cluster": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:AddTags",
                "elasticloadbalancing:RemoveTags"
            ],
            "Resource": [
                "arn:aws:elasticloadbalancing:*:*:listener/net/*/*/*",
                "arn:aws:elasticloadbalancing:*:*:listener/app/*/*/*",
                "arn:aws:elasticloadbalancing:*:*:listener-rule/net/*/*/*",
                "arn:aws:elasticloadbalancing:*:*:listener-rule/app/*/*/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:ModifyLoadBalancerAttributes",
                "elasticloadbalancing:SetIpAddressType",
                "elasticloadbalancing:SetSecurityGroups",
                "elasticloadbalancing:SetSubnets",
                "elasticloadbalancing:DeleteLoadBalancer",
                "elasticloadbalancing:ModifyTargetGroup",
                "elasticloadbalancing:ModifyTargetGroupAttributes",
                "elasticloadbalancing:DeleteTargetGroup"
            ],
            "Resource": "*",
            "Condition": {
                "Null": {
                    "aws:ResourceTag/elbv2.k8s.aws/cluster": "false"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:RegisterTargets",
                "elasticloadbalancing:DeregisterTargets"
            ],
            "Resource": "arn:aws:elasticloadbalancing:*:*:targetgroup/*/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:SetWebAcl",
                "elasticloadbalancing:ModifyListener",
                "elasticloadbalancing:AddListenerCertificates",
                "elasticloadbalancing:RemoveListenerCertificates",
                "elasticloadbalancing:ModifyRule"
            ],
            "Resource": "*"
        }
    ]
}
```

### EKS 접근이 안되는 경우

```shell 
E1115 14:10:17.293515 
64521 memcache.go:265] couldn't get current server API group list: the server has asked for the client to provide credentials E1115 14:10:17.733509 
64521 memcache.go:265] couldn't get current server API group list: the server has asked for the client to provide credentials E1115 14:10:18.202302 
64521 memcache.go:265] couldn't get current server API group list: the server has asked for the client to provide credentials E1115 14:10:18.648730 
64521 memcache.go:265] couldn't get current server API group list: the server has asked for the client to provide credentials E1115 14:10:19.093619 
64521 memcache.go:265] couldn't get current server API group list: the server has asked for the client to provide credentials error: You must be logged in to the server (the server has asked for the client to provide credentials)
```

EKS 로 접근이 안되는 경우에는 EKS로 접근할 수 있는 롤을 정의해서 등록해야합니다.

- https://github.com/kubernetes-sigs/aws-iam-authenticator#readme

![Image1](https://github.com/keepinmindsh/dev_logs/blob/main/assets/eks_access_setting.png)

클러스터 내의 위의 이미지와 같은 설정을 필요로 한다.

#### EKS Access Refreences

- https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
- https://docs.aws.amazon.com/eks/latest/userguide/cluster-auth.html
- https://docs.aws.amazon.com/eks/latest/userguide/grant-k8s-access.html
- https://docs.aws.amazon.com/eks/latest/userguide/access-entries.html
- https://docs.aws.amazon.com/eks/latest/userguide/grant-k8s-access.html

# References

- https://akku-dev.tistory.com/198
- https://three-beans.tistory.com/entry/AWSEKS-%EC%BD%98%EC%86%94%EB%A1%9C-%EC%83%9D%EC%84%B1%ED%95%98%EB%8A%94-EKS-%E2%91%A1-EKS-Cluster-%EC%83%9D%EC%84%B1
- https://stackoverflow.com/questions/37668134/how-to-install-jq-on-mac-on-the-command-line
- https://docs.docker.com/build/building/multi-platform/
- https://repost.aws/knowledge-center/eks-resolve-pending-fargate-pods
- https://kubernetes.io/docs/tasks/administer-cluster/namespaces/
- https://docs.aws.amazon.com/eks/latest/userguide/fargate-getting-started.html
- https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.0/docs/install/iam_policy.json
- https://three-beans.tistory.com/entry/AWSEKS-%EC%BD%98%EC%86%94%EB%A1%9C-%EC%83%9D%EC%84%B1%ED%95%98%EB%8A%94-EKS-%E2%91%A1-EKS-Cluster-%EC%83%9D%EC%84%B1
- [Kakao Style 기술 블로그](https://devblog.kakaostyle.com/ko/2022-03-31-2-web-application-using-eks/)
