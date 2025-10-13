# Headless Kubernetes Service

### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-db-headless-service
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
```

### Statefulset

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-statefulset
spec:
  serviceName: "my-db-headless-service"   # Ensure this matches the service name
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:latest
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: "rootpassword"
          ports:
            - containerPort: 3306
```

### Command for DNS check

`kubectl run -it --rm --restart=Never --image=busybox dns-test -- nslookup mysql-statefulset-0.my-db-headless-service.default.svc.cluster.local`