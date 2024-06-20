# Kubernetes Deployment Examples

## Exam Question 1: Deploy a Simple Python Flask Application on Kubernetes

### Background
You are tasked with deploying a simple Python Flask application to a Kubernetes cluster. The application displays a welcome message on the root URL and is containerized. Your job is to deploy this application, expose it externally, and ensure that it runs with two replicas for high availability.

### Requirements
1. Dockerfile:
	```dockerfile
	FROM python:3.8-slim
	WORKDIR /app
	COPY . /app
	RUN pip install Flask
	ENV FLASK_APP=app.py
	CMD ["flask", "run", "--host=0.0.0.0", "--port=8080"]
	```
2. The Application Code:
	```python
	from flask import Flask
	app = Flask(__name__)

	@app.route("/")
	def hello():
		return "Welcome to the Flask App on Kubernetes!"

	if __name__ == "__main__":
		app.run(host='0.0.0.0', port=8080)
	```

### Solution Steps

#### Step 1: Create a Deployment YAML File (flask-deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
  labels:
    app: flask-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-app
        image: yourusername/flask-app:latest
        ports:
        - containerPort: 8080
```

#### Step 2: Create a Service YAML File (flask-service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
  selector:
    app: flask-app
```

#### Step 3: Apply the Deployment
```bash
kubectl apply -f flask-deployment.yaml
```

#### Step 4: Apply the Service
```bash
kubectl apply -f flask-service.yaml
```

#### Step 5: Verify Deployment
```bash
kubectl get pods
kubectl get services
```

#### Step 6: Check the Route to Access the Application
```bash
kubectl get services flask-service
```

## Exam Question 2: Deploy a Flask Application with MySQL on Kubernetes

### Background
You are required to deploy a Flask application which connects to a MySQL database. The application retrieves a message from the MySQL database and displays it on the root URL. You need to set up both the Flask application and the MySQL database using Kubernetes CLI tools.

### Requirements
1. Dockerfile for Flask App:
	```dockerfile
	FROM python:3.8-slim
	WORKDIR /app
	COPY . /app
	RUN pip install Flask mysql-connector-python
	ENV FLASK_APP=app.py
	CMD ["flask", "run", "--host=0.0.0.0", "--port=8080"]
	```
2. The Application Code:
	```bash
	from flask import Flask
	import mysql.connector
	import os

	app = Flask(__name__)

	@app.route("/")
	def hello():
		db = mysql.connector.connect(
			host=os.environ['MYSQL_HOST'],
			user=os.environ['MYSQL_USER'],
			password=os.environ['MYSQL_PASSWORD'],
			database=os.environ['MYSQL_DATABASE']
		)
		cursor = db.cursor()
		cursor.execute("SELECT message FROM greetings WHERE id=1")
		message = cursor.fetchone()[0]
		db.close()
		return f"Message from the database: {message}"

	if __name__ == "__main__":
		app.run(host='0.0.0.0', port=8080)
	```
3. The Database SQL:
	```sql
	CREATE TABLE greetings (id INT AUTO_INCREMENT PRIMARY KEY, message VARCHAR(255));
	INSERT INTO greetings (message) VALUES ('Hello from MySQL!');
	```

### Solution Steps

#### Step 1: Create a Deployment YAML File for MySQL (mysql-deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  labels:
    app: mysql
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
        image: mysql:8
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: strongpassword
        - name: MYSQL_USER
          value: user
        - name: MYSQL_PASSWORD
          value: password
        - name: MYSQL_DATABASE
          value: flaskdb
        ports:
        - containerPort: 3306
```

#### Step 2: Create a Service YAML File for MySQL (mysql-service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  ports:
  - port: 3306
    targetPort: 3306
    protocol: TCP
  selector:
    app: mysql
```

#### Step 3: Create a Deployment YAML File for Flask App (flask-deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
  labels:
    app: flask-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-app
        image: yourusername/flask-app:latest
        ports
        - containerPort: 8080
        env:
        - name: MYSQL_HOST
          value: mysql-service
        - name: MYSQL_USER
          value: user
        - name: MYSQL_PASSWORD
          value: password
        - name: MYSQL_DATABASE
          value: flaskdb
```

#### Step 4: Create a Service YAML File for Flask App (flask-service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
  selector:
    app: flask-app
```

#### Step 5: Apply the MySQL Deployment
```bash
kubectl apply -f mysql-deployment.yaml
```

#### Step 6: Apply the MySQL Service
```bash
kubectl apply -f mysql-service.yaml
```

#### Step 7: Apply the Flask Deployment
```bash
kubectl apply -f flask-deployment.yaml
```

#### Step 8: Apply the Flask Service
```bash
kubectl apply -f flask-service.yaml
```

#### Step 9: Verify Deployment
```bash
kubectl get pods
kubectl get services
```

#### Step 10: Check the Route to Access the Application
```bash
kubectl get services flask-service
```
