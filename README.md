# Setup

Install kubectl

Do all commands in group-8-instructions.txt

# Create svc / deployment

`kubectl create -f frontend-deploy.yaml`
`kubectl create -f frontend-deploy.yaml`
`kubectl create -f frontend-deploy.yaml`
`kubectl create -f frontend-deploy.yaml`
`kubectl create -f frontend-deploy.yaml`

# Watch results

`watch kubectl get all`

# Delete all pods (for demo)

`kubectl delete --all pods`

# Answers

- What value must be set for this URL?

http://api-svc:8081

- Use only 1 instance for the Redis-Server. Why?

Because the data are in only one database, it's difficult to replicate data and ensure that the database will be not corrupted.

- What happens if you delete a Frontend or API Pod? How long does it take for the system to react?

The pod is killed (terminated) and directly an other one is created. It takes a few seconds.

- What happens when you delete the Redis Pod?

The data are cleared, because we don't have a volume.

- How can you change the number of instances temporarily to 3?

kubectl scale deployment.apps/api --replicas=3

- What autoscaling features are available? Which metrics are used ?

We can defined creation of pods if the cpu is too high (horizontal scaling)

`kubectl autoscale deployment.v1.apps/nginx-deployment --min=10 --max=15 --cpu-percent=80`

- How can you update a component? (see update in the deployment documentation)

We can update the image of the pod

`kubectl --record deployment.apps/nginx-deployment set image deployment.v1.apps/nginx-deployment nginx=nginx:1.9.1`
