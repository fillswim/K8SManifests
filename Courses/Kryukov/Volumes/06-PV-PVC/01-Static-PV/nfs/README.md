# Статический способ определения Persistent Volume (на NFS сервере)

## Создание PV
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/nfs/01-PV.yaml
```

Просмотр созданного PV:
```bash
kubectl get pv

NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
test-nfs   1Gi        RWX            Delete           Available                                   59s
```

## Создание PVC
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/nfs/02-PVC.yaml
```

Просмотр созданного PVC:
```bash
kubectl get pvc -n volumes-sample

NAME            STATUS   VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
claim-nfs-pvc   Bound    test-nfs   1Gi        RWX                           51s
```

Просмотр PV:
```bash
kubectl get pv

NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                          STORAGECLASS   REASON   AGE
test-nfs   1Gi        RWX            Delete           Bound    volumes-sample/claim-nfs-pvc                           4m8s
```

## Создание Пода
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/nfs/03-Pod.yaml
```

Volume монтируется в папку "/mnt/nfs" внутри контейнера и папку "/var/nfs/static-pv" на NFS сервере

## Создание Deployment
```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/01-Static-PV/nfs/04-Deployment.yaml
```
Т.к. 
Количество реплик:
```bash
# 04-Deployment.yaml

spec:                                         ### Спецификация
  replicas: 3                                 # Т.к. в PV и PVC AccessModes: ReadWriteMany (Volume том может быть смонтирован к множеству подов в режиме чтения и записи)
```
Монтируемые volumes:
```bash
# 04-Deployment.yaml

        volumeMounts:
          - mountPath: /mnt/nfs             # куда монтируется PVC внутри контейнера
            name: claim-volume
      volumes:
        - name: claim-volume
          persistentVolumeClaim:
            claimName: claim-nfs-pvc        # наименование монтируемого PVC
```

Во все поды монтируется Volume в папку "/mnt/nfs" внутри контейнеров и папку "/var/nfs/static-pv" на NFS сервере