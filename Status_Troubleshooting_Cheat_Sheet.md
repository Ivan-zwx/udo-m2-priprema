
# Status Checking and Troubleshooting Cheat Sheet for Kubernetes and OpenShift

## Kubernetes

### Checking Status
1. **Pods:**
   - List all pods and their status:
     ```bash
     kubectl get pods
     ```
   - Detailed pod information:
     ```bash
     kubectl describe pod <pod-name>
     ```

2. **Deployments:**
   - Check the status of deployments:
     ```bash
     kubectl get deployments
     ```
   - Detailed deployment information:
     ```bash
     kubectl describe deployment <deployment-name>
     ```

3. **Services:**
   - View services and their endpoints:
     ```bash
     kubectl get services
     ```
   - Detailed service information:
     ```bash
     kubectl describe service <service-name>
     ```

4. **Logs:**
   - Fetch logs of a specific pod:
     ```bash
     kubectl logs <pod-name>
     ```

### Basic Troubleshooting Steps
1. **Pod Failures:**
   - Check logs:
     ```bash
     kubectl logs <pod-name>
     ```
   - Check events:
     ```bash
     kubectl describe pod <pod-name>
     ```

2. **Service Connectivity:**
   - Ensure correct selector labels:
     ```bash
     kubectl describe svc <service-name>
     ```
   - Test internal connectivity:
     ```bash
     kubectl exec -it <pod-name> -- curl <service-name>
     ```

3. **Persistent Volumes:**
   - Check PVCs and volumes:
     ```bash
     kubectl describe pvc <pvc-name>
     ```

4. **Configuration Errors:**
   - Review configuration files for errors.

## OpenShift

### Checking Status
1. **Pods and Deployments:**
   - List and describe pods:
     ```bash
     oc get pods
     oc describe pod <pod-name>
     ```

2. **Routes:**
   - Check routes:
     ```bash
     oc get routes
     ```
   - Verify external accessibility:
     ```bash
     curl <route-hostname>
     ```

3. **Projects:**
   - Project resources status:
     ```bash
     oc status
     ```

4. **Logs and Events:**
   - Fetch logs:
     ```bash
     oc logs <pod-name>
     ```

### Basic Troubleshooting Steps
1. **Deployment Issues:**
   - Verify deployments:
     ```bash
     oc describe dc <deployment-config-name>
     ```

2. **Application Access Issues:**
   - Check route and service configurations:
     ```bash
     oc get route <route-name>
     oc describe svc <service-name>
     ```

3. **Resource Quotas:**
   - Check resource limits:
     ```bash
     oc describe quota
     ```

4. **Security and Permissions:**
   - Review service account permissions:
     ```bash
     oc describe sa <service-account-name>
     ```
   - Check SCCs:
     ```bash
     oc describe scc <scc-name>
     ```
