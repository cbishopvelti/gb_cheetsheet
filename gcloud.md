docker tag louder:latest gcr.io/louder-305021/louder:latest

docker push gcr.io/louder-305021/louder:latest


###

louder@louder-305021.iam.gserviceaccount.com

kubectl create secret docker-registry gcloud-registry \
  --docker-server=https://gcr.io \
  --docker-username=_json_key \
  --docker-email=chrisjbishop155@gmail.com \
  --docker-password="$(cat ~/louder-305021-728241c133e0.json)"


### Configer cluster context
```
gcloud container clusters get-credentials cluster-2 --zone us-central1-c --project louder-305021
```

kubectl apply -f kubernetesv2/louder

kubectl describe pods

kubectl delete all --all

kubectl exec -it louder-dep-7cb4f4c9d6-7xcqw -- bash
