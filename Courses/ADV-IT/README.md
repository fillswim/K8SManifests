[Kubernetes от ADV-IT](https://youtube.com/playlist?list=PLg5SS_4L6LYvN1RqaVesof8KAf-02fJSi)

![K8s](images/K8s.png)

# K8s Manifests

## Pods
Create pod on the bash:
```bash
kubectl run mygifservice-pod 
            --image=fillswim/mygifservice:latest 
            --port=8080
```

Create pod from yaml file:
```bash
kubectl apply -f Lesson-08-Pods/Pod01.yaml
```

Pod description
```bash
kubectl describe pods mygifservice-pod
```

Run the command inside the pod
```bash
kubectl exec mygifservice-pod -- date
```
```bash
kubectl exec -it mygifservice-pod -- sh
ls -la
```

Viewing pod logs
```bash
kubectl logs mygifservice-pod
```

Recreate pod
```bash
kubectl apply -f Lesson-08-Pods/Pod01.yaml
```

Delete pod with bash
```bash
kubectl delete pods mygifservice-pod
```

Delete pod with yaml file
```bash
kubectl delete -f Lesson-08-Pods/Pod01.yaml
```

Port forwarding:
```bash
kubectl port-forward <pod-name> <locahost-port>:<pod-port>
```
```bash
kubectl port-forward mygifservice-pod 7788:8080
```

Checking the function of the pod from the Master Node:
```bash
curl --head http://localhost:7788/swagger-ui/index.html
```


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

## HorizontalPodAutoscaler

## Services
Create ClusterIP service:
```bash
kubectl expose deployment mygifservice-deployment --type=ClusterIP --port 8080
```
Checking the function of the ClusterIP service:
```bash
kubectl get services

# NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
# mygifservice-deployment   ClusterIP   10.233.20.75    <none>        8080/TCP   4m24s

curl --head http://10.233.20.75:8080/swagger-ui/index.html #(Inside the cluster)

# HTTP/1.1 200 
# Vary: Origin
# Vary: Access-Control-Request-Method
# Vary: Access-Control-Request-Headers
# Last-Modified: Tue, 06 Jun 2023 12:03:49 GMT
# Accept-Ranges: bytes
# Content-Type: text/html
# Content-Length: 3129
# Date: Mon, 12 Jun 2023 06:58:44 GMT
```

Create NodePort service:
```bash
kubectl expose deployment mygifservice-deployment --type=NodePort --port 8080
```
Checking the function of the NodePort service:
```bash
kubectl get services

# NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
# mygifservice-deployment   NodePort    10.233.54.18    <none>        8080:30487/TCP   8s

curl --head http://192.168.2.91:30487/swagger-ui/index.html

# HTTP/1.1 200
# Vary: Origin
# Vary: Access-Control-Request-Method
# Vary: Access-Control-Request-Headers
# Last-Modified: Tue, 06 Jun 2023 12:03:49 GMT
# Accept-Ranges: bytes
# Content-Type: text/html
# Content-Length: 3129
# Date: Mon, 12 Jun 2023 07:30:54 GMT
```
Create LoadBalancer service:
```bash
kubectl expose deployment mygifservice-deployment --type=LoadBalancer --port 8080
```
Checking the function of the LoadBalancer service:
```bash
kubectl get services

# NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
# mygifservice-deployment   LoadBalancer   10.233.28.19    192.168.2.153   8080:30711/TCP   10s

curl --head http://192.168.2.153:8080/swagger-ui/index.html

# HTTP/1.1 200
# Vary: Origin
# Vary: Access-Control-Request-Method
# Vary: Access-Control-Request-Headers
# Last-Modified: Tue, 06 Jun 2023 12:03:49 GMT
# Accept-Ranges: bytes
# Content-Type: text/html
# Content-Length: 3129
# Date: Mon, 12 Jun 2023 08:05:44 GMT

```
## Ingress Controllers

Create Ingress controller
```bash
kubectl apply -f Lesson-11-Ingress_Controllers/Create-Nginx-Ingress-Service.yaml
```

Check Ingress Controller
```bash
kubectl get services -A
```

Delete Ingress Controller
```bash
kubectl delete -f Lesson-11-Ingress_Controllers/Create-Nginx-Ingress-Service.yaml
```

Create Ingress Resources
```bash
kubectl apply -f Lesson-11-Ingress_Controllers/Ingress01-hosts.yaml
```

Check Ingress Rules
```bash
kubectl get ingress

kubectl describe ingress
```

Delete Ingress Rules
```bash
kubectl delete -f Lesson-11-Ingress_Controllers/Ingress01-hosts.yaml
```

## Helm

## Kubespray
