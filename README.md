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

## HorizontalPodAutoscaler 

## Services

## Ingress Controllers

## Helm

## Kubespray
