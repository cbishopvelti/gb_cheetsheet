
Argo efs
```
ssh -i ~/.ssh/gb-argo.pem ec2-user@ec2-3-142-212-58.us-east-2.compute.amazonaws.com

 ip-10-0-1-23.us-east-2.compute.internal

cd /argo

```

```
kubectl get all -n argo
kubectl exec -it -n argo -c main pod/cpsd-csim -- /bin/sh
```


db connection test:
postgres:
2011

virtuoso:
1112
8891

Datalodaing creds

Connectionstring, host ip tools machine
aws gate
ec2-3-142-212-58.us-east-2.compute.amazonaws.com



### FORWARDING
on dataloading:
```
bin/aws_tunnel

on aws GB_TOOLS
```

### ARGO UI
```
kubectl port-forward svc/argo-server 2746:2746 -n argo
```
```
https://localhost:2746/workflows?limit=500
```

### tunnel youseqs

tunnel.sh
```
while [ 1 ]; do
    ssh  -g -o 'ServerAliveInterval 240' \
    -R 2021:localhost:2021 \
    -i ~/.ssh/gb-argo.pem \
    ec2-user@ec2-3-142-212-58.us-east-2.compute.amazonaws.com \
    "nc -l -p 2022 -c \"nc 127.0.0.1 2021\" &"
    sleep 10
done
```

```
ssh gbjhvice073
tmux
~/tunnel.sh
ctrl + d, b
```

### HELM

```
helm uninstall marrs-stage1
helm install marrs-stage1 ./
helm upgrade marrs-stage1 ./

kubectl exec -it -n argo -c main marrs-stage1-ns6ft -- /bin/sh
```
