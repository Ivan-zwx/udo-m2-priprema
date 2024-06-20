# OpenShift Deployment Examples

## Exam Question 1: Deploy a Simple Python Flask Application on OpenShift

### Background
You are tasked with deploying a simple Python Flask application to an OpenShift cluster. The application displays a welcome message on the root URL and is containerized. Your job is to deploy this application, expose it externally, and ensure that it runs with two replicas for high availability.

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
		return "Welcome to the Flask App on OpenShift!"

	if __name__ == "__main__":
		app.run(host='0.0.0.0', port=8080)
	```

### Solution Steps

#### Step 1: Create a New Project
```bash
oc new-project flask-deployment
```

#### Step 2: Create a New Application
```bash
oc new-app yourusername/flask-app --name=flask-app
```

#### Step 3: Expose the Application
```bash
oc expose svc/flask-app
```

#### Step 4: Scale the Application
```bash
oc scale --replicas=2 deployment/flask-app
```

#### Step 5: Verify Deployment
```bash
oc get pods
oc get route
```

#### Step 6: Check the Route to Access the Application
```bash
oc get route
```

## Exam Question 2: Deploy a Flask Application with MySQL on OpenShift

### Background
You are required to deploy a Flask application which connects to a MySQL database. The application retrieves a message from the MySQL database and displays it on the root URL. You need to set up both the Flask application and the MySQL database using OpenShift CLI tools.

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

#### Step 1: Create a New Project
```bash
oc new-project flask-mysql-app
```

#### Step 2: Deploy MySQL Database
```bash
oc new-app --docker-image=mysql:8 --name=mysql \
	-e MYSQL_ROOT_PASSWORD=strongpassword \
	-e MYSQL_USER=user \
	-e MYSQL_PASSWORD=password \
	-e MYSQL_DATABASE=flaskdb
```

#### Step 3: Deploy Flask Application
```bash
oc new-app yourusername/flask-app --name=flask-app \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=user \
    -e MYSQL_PASSWORD=password \
    -e MYSQL_DATABASE=flaskdb
```

#### Step 4: Expose the Flask Application
```bash
oc expose svc/flask-app
```

#### Step 5: Scale the Flask Application
```bash
oc scale --replicas=2 deployment/flask-app
```

#### Step 6: Verify Deployment
```bash
oc get pods
oc get services
```

#### Step 7: Check the Route to Access the Application
```bash
oc get route
```
