# PV Provisioner (Динамическое создание PV)

## Установка Provisioner

Добавление Helm репозитория:
```bash
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner
```

Обновление Helm репозиториев
```bash
helm repo update
```

Установка provisioner-а
```bash
helm install \
	nfs-subdir-external-provisioner \
	nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
    --set nfs.server=192.168.2.59 \
    --set nfs.path=/var/nfs/dynamic-pv
```

Просмотр созданных StorageClass
```bash
kubectl get storageclass

NAME         PROVISIONER                                     RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
nfs-client   cluster.local/nfs-subdir-external-provisioner   Delete          Immediate           true                   38m
```

## Создание PVC:
Указывание StorageClass при создании PVC:
```bash
# 01-PVC.yaml

spec:
  storageClassName: "nfs-client"
```

PV создается автоматически

Просмотр созданного PVC:
```bash
kubectl get pvc -n volumes-sample

NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
claim-nfs-pvc   Bound    pvc-c85319fc-2d84-4ff2-8435-1175ea2d9f0e   1Mi        RWX            nfs-client     79s
```
Просмотр созданного автоматически PV:
```bash
kubectl get pv

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                          STORAGECLASS   REASON   AGE
pvc-c85319fc-2d84-4ff2-8435-1175ea2d9f0e   1Mi        RWX            Delete           Bound    volumes-sample/claim-nfs-pvc   nfs-client              2m29s
```

## Создание Пода
Монтирование Volume:
```bash
    ports:
      - containerPort: 8080                 
    volumeMounts:
          - mountPath: /mnt/nfs                           # куда монтируется PVC внутри контейнера
            name: claim-volume                            # наименование монтируемого PVC
  volumes:
    - name: claim-volume                                  # наименование монтируемого PVC в рамках манифеста пода
      persistentVolumeClaim:
        claimName: claim-nfs-pvc                          # наименование монтируемого PVC
```

```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/02-Dynamic-PV/02-Pod.yaml
```

Во все поды монтируется Volume в папку "/mnt/nfs" внутри контейнеров.
Volume создается в папке "/var/nfs/dynamic-pv" на NFS сервере.

```bash
# NFS сервер

ls -la /var/nfs/dynamic-pv

drwxrwxrwx 2 root root 4096 сен 22 10:40 volumes-sample-claim-nfs-pvc-pvc-c85319fc-2d84-4ff2-8435-1175ea2d9f0e
```

```bash
# NFS сервер

ls -la /var/nfs/dynamic-pv/volumes-sample-claim-nfs-pvc-pvc-c85319fc-2d84-4ff2-8435-1175ea2d9f0e/

-rw-r--r-- 1 root root    0 сен 22 10:56 test-dynamic.txt
```

## Создание Deployment
```bash
# 03-Deployment.yaml

spec:                                         ### Спецификация
  replicas: 3                                 # Т.к. в PV и PVC AccessModes: ReadWriteMany (Volume том может быть смонтирован к множеству подов в режиме чтения и записи) 
```

```bash
# 03-Deployment.yaml

        volumeMounts:
          - mountPath: /mnt/nfs             # куда монтируется PVC внутри контейнера
            name: claim-volume
      volumes:
        - name: claim-volume
          persistentVolumeClaim:
            claimName: claim-nfs-pvc        # наименование монтируемого PVC
```

```bash
kubectl apply -f Courses/Kryukov/Volumes/06-PV-PVC/02-Dynamic-PV/03-Deployment.yaml
```

Во все поды монтируется Volume в папку "/mnt/nfs" внутри контейнеров.
Volume создается в папке "/var/nfs/dynamic-pv" на NFS сервере.