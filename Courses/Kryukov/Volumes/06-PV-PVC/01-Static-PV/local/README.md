# Статический способ определения Persistent Volume (на одной ноде)

## На K8s Worker 1 (node4) создать папку /opt/local-pv
```bash
sudo mkdir /opt/local-pv
```

Добавить права для папки /opt/local-pv
```bash
sudo chmod 777 /opt/local-pv
```

## Создать PV с именем "test-local":
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/local/01-PV.yaml
```

Просмотр созданного PV:
```bash
kubectl get pv

NAME         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
test-local   1Gi        RWO            Retain           Available                                   42s
```

## Создание PVC с именем "claim-local-pvc", который ссылается на PVC "test-local":
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/local/02-PVC.yaml
```

Просмотр созданного PVC:
```bash
kubectl get pvc -n volumes-sample

NAME              STATUS   VOLUME       CAPACITY   ACCESS MODES   STORAGECLASS   AGE
claim-local-pvc   Bound    test-local   1Gi        RWO                           53s
```

Просмтор PV:
```bash
kubectl get pv

NAME         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                            STORAGECLASS   REASON   AGE
test-local   1Gi        RWO            Retain           Bound    volumes-sample/claim-local-pvc                           21m
```

## Создание Пода
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/local/03-Pod.yaml
```

Монтируемый Volume:
```bash
    volumeMounts:
          - mountPath: /mnt/local           # куда монтируется PVC внутри контейнера
            name: claim-volume              # наименование монтируемого PVC
  volumes:
    - name: claim-volume                    # наименование монтируемого PVC в рамках манифеста пода
      persistentVolumeClaim:
        claimName: claim-local-pvc          # наименование монтируемого PVC
```

Volume монтируется в папку "/mnt/local" внутри контейнера и папку "/opt/local-pv" на node4

## Создание Deployment
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/local/04-Deployment.yaml
```
