
# Kubernetes and OpenShift Deployment Cheat Sheet

## Kubernetes Deployment Cheat Sheet

### 1. Set Up Your Environment
- **Install Tools:** Ensure `kubectl` is installed.
- **Configure Kubectl:** Connect `kubectl` to your cluster with `kubectl config use-context <context>`.

### 2. Create and Use a Namespace
- **Create Namespace:** `kubectl create namespace <namespace-name>`
- **Set Default Namespace:** `kubectl config set-context --current --namespace=<namespace-name>`

### 3. Write Deployment and Service YAML
- **Deployment YAML Example:**
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: app-deployment
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
          image: <image-name>
          ports:
          - containerPort: 8080
  ```
- **Service YAML Example:**
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

### 4. Deploy Application
- **Apply YAML Files:** `kubectl apply -f <filename>.yaml`

### 5. Verify Deployment
- **Check Pods:** `kubectl get pods`
- **Check Services:** `kubectl get services`

### 6. Access Application
- **Get External IP:** `kubectl get svc app-service` and note the external IP if using LoadBalancer.

### 7. Update and Rollback
- **Update Image:** `kubectl set image deployment/app-deployment app=<new-image>`
- **Rollback:** `kubectl rollout undo deployment/app-deployment`

## OpenShift Deployment Cheat Sheet

### 1. Set Up Your Environment
- **Install Tools:** Ensure `oc` (OpenShift CLI) is installed.
- **Log In to Cluster:** `oc login -u <username> -p <password> --server=<server-url>`

### 2. Create and Use a Project
- **Create Project:** `oc new-project <project-name>`

### 3. Deploy Application
- **From Image (Docker Hub):**
  ```bash
  oc new-app <docker-image> --name=<app-name>
  ```
- **From Source Code Using S2I:**
  ```bash
  oc new-app <language>~<source-repository-url> --name=<app-name>
  ```
- **Expose Service to Create Route:**
  ```bash
  oc expose svc/<service-name>
  ```

### 4. Verify Deployment
- **Check Pods:** `oc get pods`
- **Check Services:** `oc get svc`
- **Check Routes:** `oc get routes`

### 5. Access Application
- **Via Web Browser:** Navigate to the URL given by `oc get routes`.

### 6. Manage Application
- **Scale Up:** `oc scale --replicas=<number> deployment/<deployment-name>`
- **View Logs:** `oc logs -f <pod-name>`
- **Stream Logs:** `oc logs -f dc/<deployment-config-name>`

### 7. Clean Up
- **Delete Project:** `oc delete project <project-name>`
