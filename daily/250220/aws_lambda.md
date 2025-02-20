# Lambda를 이용한 배포 구성시

## 도커 빌드 ( Basic )

```dockerfile 
FROM public.ecr.aws/lambda/python:3.12

# Copy requirements.txt
COPY requirements.txt ${LAMBDA_TASK_ROOT}

# Install the specified packages
RUN pip install -r requirements.txt

# Copy function code
COPY lambda_function.py ${LAMBDA_TASK_ROOT}

# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
CMD [ "lambda_function.handler" ]
```

## 권한 세팅

실수로라도 Permission Boundary를 임의로 설정하지 말것.   
만약 설정할 것이라면 실제 오픈할 Role을 정확하게 세팅하지 않으면 gitaction에서 동작시 장애가 발생할 수 있음.

상세 권한은 아래의 이미지 참고할 것!

![image](https://github.com/user-attachments/assets/60e234e4-22c1-490e-b22e-605c5ce1279c)

- 필수 권한

빌드를 위한 필수 권한 구성 처리, Lambda의 함수를 업데이트해야할 경우 아래의 함수권한은 필요함.

```json 
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowLambdaUpdateCode",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "iam:ListRoles",
                "lambda:UpdateFunctionCode",
                "lambda:CreateFunction",
                "lambda:GetFunction",
                "lambda:UpdateFunctionConfiguration",
                "lambda:GetFunctionConfiguration"
            ],
            "Resource": "*"
        }
    ]
}
```

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:PutImage",
                "ecr:InitiateLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:CompleteLayerUpload",
                "ecr:DescribeRepositories"
            ],
            "Resource": "*"
        }
    ]
}
```

## gitaction 구성을 위한 해당 Repository의 Secret 구성

![image](https://github.com/user-attachments/assets/e4f0278b-e450-4946-920d-853b2e781bfd)

## gitaction 구성 ( zip 파일 기반 )

```yaml 
name: deploy_hospital_lambda
on:
  push:
    branches:
      - master
  pull_request:
    types:
      - completed
jobs:
  lambda-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - uses: jitterbit/get-changed-files@v1
        id: files
        with:
          format: space-delimited
      - name: AWS 자격 증명 구성
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-2
      - name: Python 설정
        uses: actions/setup-python@v3
        with:
          python-version: '3.11'
      - name: Lambda 빌드 & 업데이트
        run: |
          pip3 install awscli
          rm -f login.zip
          zip -r login.zip app tests main.py lambda.py
          aws lambda update-function-code --function-name erp_lambda --zip-file fileb://animal-hospital-python.zip --publish
```

## gitaction 구성 ( 도커 기반의 Lambda 자동 배포 구성 - GitOptions )

```yaml 
name: deploy_hospital_lambda_cicd
on:
  push:
    branches:
      - master
  pull_request:
    types:
      - completed

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Log in to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build, tag, and push image to Amazon ECR
        id: build-admin-image
        env:
          IMAGE_TAG: latest # ${{ github.sha }}
          ECR_REGISTRY: ${{ secrets.ACCOUNT }}.dkr.ecr.${{ secrets.AWS_REGION }}.amazonaws.com
        run: |
          # Build a docker container and
          # push it to ECR so that it can
          # be deployed to ECS.
          docker build -t ${{ secrets.ECR_REPOSITORY }}:$IMAGE_TAG -f Dockerfile .
          docker push ${{ secrets.ECR_REPOSITORY }}:$IMAGE_TAG
          echo "::set-output name=image::${{ secrets.ECR_REPOSITORY }}:$IMAGE_TAG"

      - name: Update Lambda Function
        env:  
          IMAGE_TAG: latest # ${{ github.sha }}
          ECR_REGISTRY: ${{ secrets.ACCOUNT }}.dkr.ecr.${{ secrets.AWS_REGION }}.amazonaws.com
        run: |
          aws lambda update-function-code \
            --function-name ${{ secrets.LAMBDA_FUNCTION_NAME }} \
            --image-uri ${{ secrets.ECR_REPOSITORY }}:$IMAGE_TAG

      - name: Logout from ECR
        env:
          ECR_REGISTRY: ${{ secrets.ACCOUNT }}.dkr.ecr.${{ secrets.AWS_REGION }}.amazonaws.com
        run: docker logout $ECR_REGISTRY
```

# API Gateway + AWA Lambda + FastAP(Magnum)

- API Gateway 람다 구성 상태

경로는 하위경로를 모두 인식 처리하기 위해서 /{proxy+}를 활용하였음.   
이를 처리하기 위해서는 초기 생성 시점에 경로를 정의할 때 "/{proxy+}" 값을 잘 활용할 것.

![image](https://github.com/user-attachments/assets/6d8a1467-fadd-4e8e-850a-d485f49c4203)

- Gateway Route 구성 상태

![image](https://github.com/user-attachments/assets/1356c986-48c8-45f7-9ea9-a885171c184a)

- 람다 인테그레이션 구성 상태

![image](https://github.com/user-attachments/assets/a33d47ed-e4ae-4b2a-b644-e4f8e617b923)

- API Gateway CORS 구성 상태

CORS의 경우, 이미 Python API 내에 구성되어 있기 때문에 Gateway 쪽의 CORS는 활성화 시키지 않음

![image](https://github.com/user-attachments/assets/b92dd851-8214-4673-8e60-806574fd7d8c)

- 람다의 API Gateway 연결을 볼 수 있는 Configuration 내의 설정

중요한 부분은 {proxy+} 로 구성되어 있는 부분이 중요함. 해당 부분을 통해서 람다 내의 magnum으로 정의된 handler로 전달됨.

![image](https://github.com/user-attachments/assets/dddf3da0-ee29-4331-86ec-b0182a4193cf)

## 작업시 참고 사항

- Gateway 설정에서 CORS를 중복으로 설정하지 말것
- 가능하면 magnum과 같이 사용하는 경우는 ECS를 사용하고 람다는 람다함수의 개념처럼, 하나의 함수를 대용해서 쓸 것
- Gateway 설정에서 HTTP와 REST로 생성할 때 뭐가 나을지 잘 고민해서 세팅할 것
- https://jypark1111.tistory.com/255
