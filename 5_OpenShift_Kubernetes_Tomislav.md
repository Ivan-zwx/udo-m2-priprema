
# OpenShift Cheat Sheet

## Initial Setup and Information
- **Login to OpenShift CLI:**
  ```bash
  oc login -u developer -p developer https://api.ocp4.example.com:6443
  ```
- **Identify the URL for the OpenShift web console:**
  ```bash
  oc whoami --show-console
  ```
- **Identify the URL for the OpenShift API:**
  ```bash
  oc whoami --show-server
  ```

## Project and Cluster Management
- **Create a new project:**
  ```bash
  oc new-project myapp
  ```
- **Get cluster information:**
  ```bash
  oc cluster-info
  ```
- **List all API versions available in the cluster:**
  ```bash
  oc api-versions
  ```
- **Get cluster operators:**
  ```bash
  oc get clusteroperator
  ```
- **List all pods:**
  ```bash
  oc get pod
  ```
- **List all nodes:**
  ```bash
  oc get node
  ```
- **List all resources:**
  ```bash
  oc get all
  ```

## Resource Management
- **Create a resource from a YAML file:**
  ```bash
  oc create -f pod.yaml
  ```
- **Delete a pod:**
  ```bash
  oc delete pod quotes-ui
  ```
- **Get specific cluster operator details in YAML format:**
  ```bash
  oc get clusteroperators dns -o yaml
  ```

## Containerization Concepts
- **Definition:** A container is an encapsulated process that includes the required runtime dependencies for an application to run.
- **Purpose:** Containerization addresses the application development challenges around code portability, to aid in consistently running an application from diverse environments.
- **Benefits:** Containerization also aims to modularize applications to improve development and maintenance on the various components of the application.

## Scalability and Management
- **Challenge:** When running containers at scale, it becomes challenging to configure and deliver high availability applications and to set up networking without a container platform, such as Kubernetes.
- **Pods:** Pods are the smallest organizational unit for a containerized application in a Kubernetes cluster.
- **Enterprise Functions:** Red Hat OpenShift Container Platform (RHOCP) adds enterprise-class functions to the Kubernetes container platform to deliver the wider business needs.

## Administrative and Web Console Features
- **Administrative Tasks:** Most administrative tasks that cluster administrators and developers perform are available through the RHOCP web console.
- **Web Console Features:** Logs, metrics, alerts, terminal connections to the nodes and pods in the cluster, and many other features are available through the RHOCP web console.

## Kubernetes and OpenShift Resources
- **Manage API Resources:**
  ```bash
  oc api-resources
  oc api-resources --namespaced=false
  oc api-resources --api-group=''
  oc explain deployments
  ```

## Kubernetes CLI
- **Check Kubernetes CLI version:**
  ```bash
  kubectl version --client
  ```
- **Explain Kubernetes Pods:**
  ```bash
  kubectl explain pod
  ```
