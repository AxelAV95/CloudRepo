

---

## **Azure Kubernetes Service (AKS) Cheatsheet**

### **1. Configuración Inicial**
- **Instalar Azure CLI**:  
  ```bash
  curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
  ```
- **Autenticación en Azure**:  
  ```bash
  az login
  ```
- **Instalar kubectl**:  
  ```bash
  az aks install-cli
  ```

---

### **2. Creación y Gestión de Clusters**
- **Crear un cluster AKS**:  
  ```bash
  az aks create --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME] --node-count [NUM_NODES] --generate-ssh-keys
  ```
- **Listar clusters AKS**:  
  ```bash
  az aks list --resource-group [RESOURCE_GROUP]
  ```
- **Obtener credenciales para un cluster**:  
  ```bash
  az aks get-credentials --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME]
  ```
- **Eliminar un cluster AKS**:  
  ```bash
  az aks delete --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME]
  ```

---

### **3. Comandos Básicos de kubectl**
- **Ver nodos del cluster**:  
  ```bash
  kubectl get nodes
  ```
- **Ver pods en ejecución**:  
  ```bash
  kubectl get pods
  ```
- **Ver servicios**:  
  ```bash
  kubectl get services
  ```
- **Ver deployments**:  
  ```bash
  kubectl get deployments
  ```
- **Ver logs de un pod**:  
  ```bash
  kubectl logs [POD_NAME]
  ```
- **Ejecutar un comando en un pod**:  
  ```bash
  kubectl exec -it [POD_NAME] -- [COMMAND]
  ```
- **Describir un recurso**:  
  ```bash
  kubectl describe [RESOURCE_TYPE] [RESOURCE_NAME]
  ```

---

### **4. Despliegues y Escalado**
- **Crear un deployment**:  
  ```bash
  kubectl create deployment [DEPLOYMENT_NAME] --image=[IMAGE_NAME]
  ```
- **Exponer un deployment como servicio**:  
  ```bash
  kubectl expose deployment [DEPLOYMENT_NAME] --type=LoadBalancer --port=[PORT]
  ```
- **Escalar un deployment**:  
  ```bash
  kubectl scale deployment [DEPLOYMENT_NAME] --replicas=[NUM_REPLICAS]
  ```
- **Actualizar una imagen en un deployment**:  
  ```bash
  kubectl set image deployment/[DEPLOYMENT_NAME] [CONTAINER_NAME]=[NEW_IMAGE]
  ```
- **Rollback de un deployment**:  
  ```bash
  kubectl rollout undo deployment/[DEPLOYMENT_NAME]
  ```

---

### **5. Configuración de Redes**
- **Ver direcciones IP externas**:  
  ```bash
  kubectl get services
  ```
- **Habilitar balanceador de carga**:  
  ```bash
  kubectl expose deployment [DEPLOYMENT_NAME] --type=LoadBalancer --port=[PORT]
  ```
- **Configurar Ingress**:  
  Crear un archivo YAML para Ingress y aplicarlo:  
  ```bash
  kubectl apply -f [INGRESS_FILE].yaml
  ```

---

### **6. Almacenamiento en AKS**
- **Crear un PersistentVolume (PV)**:  
  Definir un PV en un archivo YAML y aplicarlo:  
  ```bash
  kubectl apply -f [PV_FILE].yaml
  ```
- **Crear un PersistentVolumeClaim (PVC)**:  
  Definir un PVC en un archivo YAML y aplicarlo:  
  ```bash
  kubectl apply -f [PVC_FILE].yaml
  ```
- **Montar un PVC en un pod**:  
  Especificar el PVC en la sección `volumes` del archivo YAML del pod.

---

### **7. Monitoreo y Logging**
- **Habilitar Azure Monitor para contenedores**:  
  ```bash
  az aks enable-addons --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME] --addons monitoring
  ```
- **Ver logs en Azure Monitor**:  
  Usar la consola de Azure Portal.

---

### **8. Autoscaling**
- **Habilitar autoscaling de nodos (Cluster Autoscaler)**:  
  ```bash
  az aks update --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME] --enable-cluster-autoscaler --min-count [MIN_NODES] --max-count [MAX_NODES]
  ```
- **Habilitar autoscaling de pods (Horizontal Pod Autoscaler)**:  
  ```bash
  kubectl autoscale deployment [DEPLOYMENT_NAME] --cpu-percent=[CPU_PERCENT] --min=[MIN_PODS] --max=[MAX_PODS]
  ```

---

### **9. Seguridad**
- **Habilitar Azure Active Directory (AAD) para AKS**:  
  ```bash
  az aks update --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME] --enable-aad
  ```
- **Crear un Service Account y asociarlo a un rol**:  
  ```bash
  kubectl create serviceaccount [SERVICE_ACCOUNT_NAME]
  kubectl create rolebinding [ROLEBINDING_NAME] --clusterrole=[ROLE] --serviceaccount=[NAMESPACE]:[SERVICE_ACCOUNT_NAME]
  ```

---

### **10. Actualización y Mantenimiento**
- **Actualizar un cluster AKS**:  
  ```bash
  az aks upgrade --resource-group [RESOURCE_GROUP] --name [CLUSTER_NAME] --kubernetes-version [VERSION]
  ```
- **Actualizar nodos**:  
  ```bash
  az aks nodepool upgrade --resource-group [RESOURCE_GROUP] --cluster-name [CLUSTER_NAME] --name [NODEPOOL_NAME] --kubernetes-version [VERSION]
  ```

---

### **11. Buenas Prácticas**
- Usar **namespaces** para organizar recursos.
- Implementar **límites de recursos** (requests/limits) en los pods.
- Usar **Helm** para gestionar aplicaciones complejas.
- Habilitar **Network Policies** para seguridad de red.
- Monitorear con **Azure Monitor** y **Prometheus**.

---

### **12. Comandos Útiles Adicionales**
- **Ver eventos del cluster**:  
  ```bash
  kubectl get events
  ```
- **Drenar un nodo para mantenimiento**:  
  ```bash
  kubectl drain [NODE_NAME] --ignore-daemonsets --delete-local-data
  ```
- **Marcar un nodo como no programable**:  
  ```bash
  kubectl cordon [NODE_NAME]
  ```

---

Este cheatsheet cubre los aspectos más importantes de AKS. Para más detalles, consulta la [documentación oficial de AKS](https://docs.microsoft.com/en-us/azure/aks/).
