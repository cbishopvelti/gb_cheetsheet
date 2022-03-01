### AWS login failed 


```Delete 340571153065.dkr.ecr.us-east-2.amazonaws.com``` from ```~/.docker/config.json```

```
docker login -u AWS -p $(aws ecr get-login-password --region us-east-2) 340571153065.dkr.ecr.us-east-2.amazonaws.com
```

### EKS kubectl timeout

Ensure dns server is at the top