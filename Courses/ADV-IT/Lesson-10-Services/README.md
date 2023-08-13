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

