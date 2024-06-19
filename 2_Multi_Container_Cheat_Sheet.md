
# Kubernetes and OpenShift Deployment Cheat Sheet for Multi-Container Applications

## Kubernetes Deployment for Multi-Container Applications

### 1. Deploy MySQL Container
- **MySQL Deployment YAML:**
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: mysql
  spec:
    replicas: 1
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
          image: mysql:5.7
          env:
          - name: MYSQL_ROOT_PASSWORD
            value: password
          - name: MYSQL_DATABASE
            value: appdb
          ports:
          - containerPort: 3306
  ```
- **MySQL Service YAML:**
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: mysql-service
  spec:
    ports:
    - port: 3306
    selector:
      app: mysql
  ```
- **Deploy MySQL:**
  ```bash
  kubectl apply -f mysql-deployment.yaml
  kubectl apply -f mysql-service.yaml
  ```

### 2. Deploy Application That Connects to MySQL
- **Application Deployment YAML:**
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: app
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: app
    template:
      metadata:
        labels:
          app: app
      spec:
        containers:
        - name: app
          image: <app-image>
          env:
          - name: DATABASE_HOST
            value: mysql-service
          - name: DATABASE_USER
            value: user
          - name: DATABASE_PASSWORD
            value: password
          ports:
          - containerPort: 8080
  ```
- **Application Service YAML:**
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: app-service
  spec:
    type: LoadBalancer
    ports:
    - port: 80
      targetPort: 8080
    selector:
      app: app
  ```
- **Deploy Application:**
  ```bash
  kubectl apply -f app-deployment.yaml
  kubectl apply -f app-service.yaml
  ```

## OpenShift Deployment for Multi-Container Applications

### 1. Deploy MySQL Using OpenShift Templates
- **Deploy MySQL:**
  ```bash
  oc new-app mysql-persistent --name=mysql     -p MYSQL_ROOT_PASSWORD=password     -p MYSQL_USER=user     -p MYSQL_PASSWORD=password     -p MYSQL_DATABASE=appdb
  ```

### 2. Deploy Application That Connects to MySQL
- **Deploy Application from Image:**
  ```bash
  oc new-app <app-image> --name=app
  oc set env dc/app DATABASE_HOST=mysql DATABASE_USER=user DATABASE_PASSWORD=password
  oc expose svc/app
  ```
