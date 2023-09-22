# Longhorn

## Установка Longhorn:

Добавление Helm репозитория:
```bash
helm repo add longhorn https://charts.longhorn.io
```

Обновление Helm репозиториев:
```bash
helm repo update
```

Установка Longhorn:
```bash
helm install \
	longhorn \
	longhorn/longhorn \
	--namespace longhorn-system \
	--create-namespace \
	--version 1.5.1
```

Изменил тип сервиса longhorn-frontend с ClusterIP на LoadBalancer.
[Longhorn Dashboard](http://192.168.2.153/#/dashboard)

## Создание PVC:

```bash
# 01-PVC.yaml

spec:
  storageClassName: "longhorn"
  accessModes:
    - ReadWriteMany
```

```bash
kubectl apply -f Courses/Kryukov/Volumes/09-Longhorn/01-PVC.yaml
```

Просмотр созданного PVC:
```bash
kubectl get pvc -n volumes-sample

NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
claim-longhorn-pvc   Bound    pvc-0c5ae448-3521-4a3f-8683-9ca7c6048fbb   10Mi       RWX            longhorn       30m
```

Просмотр автоматически созданного PV:
```bash
kubectl get pv

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                               STORAGECLASS   REASON   AGE
pvc-0c5ae448-3521-4a3f-8683-9ca7c6048fbb   10Mi       RWX            Delete           Bound    volumes-sample/claim-longhorn-pvc   longhorn                31m
```

## Создание Deployment:

```bash
# 02-Deployment.yaml

spec:                                         ### Спецификация
  replicas: 3                                 # Т.к. в PV и PVC AccessModes: ReadWriteMany (Volume том может быть смонтирован к множеству подов в режиме чтения и записи) 
```

```bash
# 02-Deployment.yaml

        volumeMounts:
          - mountPath: /mnt/longhorn             # куда монтируется PVC внутри контейнера
            name: claim-volume
      volumes:
        - name: claim-volume
          persistentVolumeClaim:
            claimName: claim-longhorn-pvc        # наименование монтируемого PVC
```

```bash
kubectl apply -f Courses/Kryukov/Volumes/09-Longhorn/02-Deployment.yaml
```

Во все поды монтируется Volume в папку "/mnt/longhorn" внутри контейнеров. Volume создается в папке "/mnt/data/" на нодах Longhorn.

