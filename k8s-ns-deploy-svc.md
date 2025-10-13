apiVersion: v1
kind: Namespace
metadata:
  name: Project1
---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: test-deployment
  annotations:
    kubernetes.io/change-cause: "updating to v1"
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-test-app
  template:
    metadata:
      labels:
        app: my-test-app
    spec:
      containers:
      - name: test-container
        image: wiings09/test-image:v1     #version 2:  wiings09/test-image:v2
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: app-svc
spec:
  type: NodePort
  selector:
    app: my-test-app
  ports:
    - port: 80              # External port
      targetPort: 80        # Container port
      nodePort: 30000