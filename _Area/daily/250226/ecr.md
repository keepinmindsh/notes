# Image Push 

```shell 
aws ecr get-login-password --region ap-northeast-2 --profile {profile_name} | docker login --username AWS --password-stdin {account}
```