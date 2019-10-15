# ANSWERS ARE IN BOTTOM OF DOCUMENT !

# Setup

Install kubectl

Do all commands in group-8-instructions.txt

# Create svc / deployment

## svc

`kubectl create -f frontend-svc.yaml`

`kubectl create -f api-svc.yaml`

`kubectl create -f redis-svc.yaml`

## deployment

`kubectl create -f frontend-deploy.yaml`

`kubectl create -f api-deploy.yaml`

`kubectl create -f redis-deploy.yaml`

# Watch results

`watch kubectl get all`

This should output something like that :

```
NAME                            READY   STATUS    RESTARTS   AGE
pod/api-8b899f8f5-4nkkk         1/1     Running   0          24s
pod/frontend-6566ddc7dc-cwbd9   1/1     Running   0          28s
pod/frontend-6566ddc7dc-cxl5z   1/1     Running   0          28s
pod/frontend-6566ddc7dc-z4w2m   1/1     Running   0          28s
pod/redis-6c8d7db57b-dj6zk      1/1     Running   0          20s

NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP                                                              PORT(S)        AGE
service/api-svc        ClusterIP      10.100.71.252   <none>                                                                   8081/TCP       38s
service/frontend-svc   LoadBalancer   10.100.111.49   a5457b5ffef1711e9b2390a897df827d-515519190.eu-west-1.elb.amazonaws.com   80:31977/TCP   42s
service/redis-svc      ClusterIP      10.100.73.230   <none>                                                                   6379/TCP       33s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/api        1/1     1            1           24s
deployment.apps/frontend   3/3     3            3           28s
deployment.apps/redis      1/1     1            1           20s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/api-8b899f8f5         1         1         1       24s
replicaset.apps/frontend-6566ddc7dc   3         3         3       28s
replicaset.apps/redis-6c8d7db57b      1         1         1       20s
```

You can browse the url returned by frontend-svc (exemple : http://a5457b5ffef1711e9b2390a897df827d-515519190.eu-west-1.elb.amazonaws.com) to use application.

# Delete all pods (for demo)

`kubectl delete --all pods`

The pods are terminated and new ones are created instatly.

# Cleanup env

`kubectl delete -f frontend-svc.yaml`

`kubectl delete -f api-svc.yaml`

`kubectl delete -f redis-svc.yaml`

`kubectl delete deployment.apps/redis`

`kubectl delete deployment.apps/api`

`kubectl delete deployment.apps/frontend`

When finishing to call all commands, `kubectl get all` must output :

```
No resources found.
```

# Possible improvements

There are some code duplication :

The deploy's files `containers` are the same as pod's files `containers`

# Answers

## What value must be set for this URL?

http://api-svc:8081

## Use only 1 instance for the Redis-Server. Why?

Because the data are in only one database, it's difficult to replicate data and ensure that the database will be not corrupted.

## What happens if you delete a Frontend or API Pod? How long does it take for the system to react?

The pod is killed (terminated) and directly an other one is created. It takes a few seconds.

## What happens when you delete the Redis Pod?

The data are cleared, because we don't have a volume.

## How can you change the number of instances temporarily to 3?

kubectl scale deployment.apps/api --replicas=3

## What autoscaling features are available? Which metrics are used ?

We can defined creation of pods if the cpu is too high (horizontal scaling)

`kubectl autoscale deployment.v1.apps/nginx-deployment --min=10 --max=15 --cpu-percent=80`

## How can you update a component? (see update in the deployment documentation)

We can update the image of the pod

`kubectl --record deployment.apps/nginx-deployment set image deployment.v1.apps/nginx-deployment nginx=nginx:1.9.1`
