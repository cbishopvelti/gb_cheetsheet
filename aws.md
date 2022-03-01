### re authenticate aws docker
```
docker login -u AWS -p $(aws ecr get-login-password --region us-east-2) 340571153065.dkr.ecr.us-east-2.amazonaws.com
```
```
ssh -i ~/.ssh/gb-argo.pem ec2-user@ec2-3-142-212-58.us-east-2.compute.amazonaws.com
```