
hasura
```
kubectl port-forward svc/hasura-svc 8080:8080
```

postgres
un: postgres
pw: postgres
```
kubectl port-forward svc/postgres-svc 5431:5431
```
apollo-server-gb
```

kubectl port-forward svc/apollo-server 4000:80 -n apollo
kubectl port-forward svc/apollo-server-public-service 4000:80 -n apollo

kubectl port-forward svc/apollo-client 4003:80 -n apollo
```

```
kubectl port-forward svc/rabbitmq 5672:5672 15672:15672 -n apollo
```



```
amqp://efauI1Qr5G2tw1kS01qexuUKz4Jfm-Ax:LzFbcDifpxFYWAUe11DPncfgQsLgq7ad@rabbitmq.default.svc.cluster.local:5672/
```

```
kubectl exec apollo-server-deployment-696ccb4965-99sgf -it -- /bin/sh
```

```
/Applications/Google Chrome.app/Contents/MacOS/Google\ Chrome --allow-running-insecure-content

```

```
kubectl patch deployment apollo-server-deployment -p '{"spec":{"template":{"spec":{"terminationGracePeriodSeconds":36}}}}' -n apollo

```