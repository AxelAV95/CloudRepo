
---

## **Google Kubernetes Engine (GKE) Cheatsheet**

### **1. Configuración Inicial**
- **Instalar Google Cloud SDK**:  
  ```bash
  curl https://sdk.cloud.google.com | bash
  exec -l $SHELL
  gcloud init
  ```
- **Autenticación en GCP**:  
  ```bash
  gcloud auth login
  gcloud config set project [PROJECT_ID]
  ```
- **Instalar kubectl**:  
  ```bash
  gcloud components install kubectl
  ```

---

### **2. Creación y Gestión de Clusters**
- **Crear un cluster GKE**:  
  ```bash
  gcloud container clusters create [CLUSTER_NAME] --zone [ZONE] --num-nodes=[NUM_NODES]
  ```
- **Listar clusters**:  
  ```bash
  gcloud container clusters list
  ```
- **Obtener credenciales para un cluster**:  
  ```bash
  gcloud container clusters get-credentials [CLUSTER_NAME] --zone [ZONE]
  ```
- **Eliminar un cluster**:  
  ```bash
  gcloud container clusters delete [CLUSTER_NAME] --zone [ZONE]
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

### **6. Almacenamiento en GKE**
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
- **Habilitar Stackdriver Monitoring**:  
  Al crear el cluster:  
  ```bash
  gcloud container clusters create [CLUSTER_NAME] --zone [ZONE] --monitoring=SYSTEM,WORKLOADS
  ```
- **Ver logs de Stackdriver**:  
  Usar la consola de GCP o el comando:  
  ```bash
  gcloud logging read "resource.type=k8s_container"
  ```

---

### **8. Autoscaling**
- **Habilitar autoscaling de nodos**:  
  ```bash
  gcloud container clusters create [CLUSTER_NAME] --zone [ZONE] --enable-autoscaling --min-nodes=[MIN] --max-nodes=[MAX]
  ```
- **Habilitar autoscaling de pods (Horizontal Pod Autoscaler)**:  
  ```bash
  kubectl autoscale deployment [DEPLOYMENT_NAME] --cpu-percent=[CPU_PERCENT] --min=[MIN_PODS] --max=[MAX_PODS]
  ```

---

### **9. Seguridad**
- **Habilitar control de acceso basado en roles (RBAC)**:  
  Asegúrate de que RBAC esté habilitado al crear el cluster:  
  ```bash
  gcloud container clusters create [CLUSTER_NAME] --zone [ZONE] --enable-kubernetes-alpha --no-enable-legacy-authorization
  ```
- **Crear Service Accounts**:  
  ```bash
  kubectl create serviceaccount [SERVICE_ACCOUNT_NAME]
  ```
- **Asignar roles a Service Accounts**:  
  ```bash
  kubectl create rolebinding [ROLEBINDING_NAME] --clusterrole=[ROLE] --serviceaccount=[NAMESPACE]:[SERVICE_ACCOUNT_NAME]
  ```

---

### **10. Actualización y Mantenimiento**
- **Actualizar un cluster**:  
  ```bash
  gcloud container clusters upgrade [CLUSTER_NAME] --zone [ZONE] --master --cluster-version [VERSION]
  ```
- **Actualizar nodos**:  
  ```bash
  gcloud container clusters upgrade [CLUSTER_NAME] --zone [ZONE] --node-pool=[POOL_NAME]
  ```

---

### **11. Buenas Prácticas**
- Usar **namespaces** para organizar recursos.
- Implementar **límites de recursos** (requests/limits) en los pods.
- Usar **Helm** para gestionar aplicaciones complejas.
- Habilitar **Network Policies** para seguridad de red.
- Monitorear y auditar con **Stackdriver** y **Prometheus**.

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

Este cheatsheet cubre los aspectos más importantes de GKE. Para más detalles, consulta la [documentación oficial de GKE](https://cloud.google.com/kubernetes-engine/docs).
