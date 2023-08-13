## Deployments

Create deployment on the bash:
```bash
kubectl create deployment mygifservice-deployment --image fillswim/mygifservice:latest
```

Create deployment from yaml file:
```bash
kubectl apply -f Lesson-09-Deployments/Deployment01.yaml
```

Deployment description
```bash
kubectl describe deployments mygifservice-deployment
```

Delete deployment with bash
```bash
kubectl delete deployment mygifservice-deployment
```

Delete deployment with yaml file
```bash
kubectl delete -f Lesson-09-Deployments/Deployment01.yaml
```

Scaling a Deployment
```bash
kubectl scale deployment mygifservice-deployment --replicas 4
```

Horizontal Pod Autoscaling
```bash
kubectl autoscale deployment mygifservice-deployment --min=4 --max=6 --cpu-percent=80
```

Deployment status
```bash
kubectl rollout status deployment/mygifservice-deployment
```
Deployment history
```bash
kubectl rollout history deployment/mygifservice-deployment
```

Updating the image in the deployment
```bash
# old image:
kubectl create deployment nginx-deployment --image nginx:1.22-alpine
```
```bash
# new image:
kubectl set image deployment/nginx-deployment nginx=nginx:1.23-alpine --record=true
```
```bash
# updating from the latest to the latest image:
kubectl rollout restart deployment/mygifservice-deployment
```

Rollout deployment:
```bash
# rollout to a previous version
kubectl rollout undo deployment/mygifservice-deployment
```
```bash
# rollout to to version number
kubectl rollout undo deployment/mygifservice-deployment --to-revision=1
```
