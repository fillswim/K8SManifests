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
