

---

## **Amazon Elastic Kubernetes Service (EKS) Cheatsheet**

### **1. Configuración Inicial**
- **Instalar AWS CLI**:  
  ```bash
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip
  sudo ./aws/install
  ```
- **Configurar AWS CLI**:  
  ```bash
  aws configure
  ```
- **Instalar kubectl**:  
  ```bash
  curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
  chmod +x kubectl
  sudo mv kubectl /usr/local/bin/
  ```
- **Instalar eksctl (herramienta CLI para EKS)**:  
  ```bash
  curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
  sudo mv /tmp/eksctl /usr/local/bin
  ```

---

### **2. Creación y Gestión de Clusters**
- **Crear un cluster EKS con eksctl**:  
  ```bash
  eksctl create cluster --name [CLUSTER_NAME] --region [REGION] --nodegroup-name [NODEGROUP_NAME] --node-type [INSTANCE_TYPE] --nodes [NUM_NODES]
  ```
- **Listar clusters EKS**:  
  ```bash
  eksctl get clusters --region [REGION]
  ```
- **Obtener credenciales para un cluster**:  
  ```bash
  aws eks update-kubeconfig --name [CLUSTER_NAME] --region [REGION]
  ```
- **Eliminar un cluster EKS**:  
  ```bash
  eksctl delete cluster --name [CLUSTER_NAME] --region [REGION]
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

### **6. Almacenamiento en EKS**
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
- **Habilitar CloudWatch para logs**:  
  Configura Fluent Bit para enviar logs a CloudWatch:  
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml
  ```
- **Ver logs en CloudWatch**:  
  Usar la consola de AWS CloudWatch.

---

### **8. Autoscaling**
- **Habilitar autoscaling de nodos (Cluster Autoscaler)**:  
  Instalar el Cluster Autoscaler:  
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
  ```
- **Habilitar autoscaling de pods (Horizontal Pod Autoscaler)**:  
  ```bash
  kubectl autoscale deployment [DEPLOYMENT_NAME] --cpu-percent=[CPU_PERCENT] --min=[MIN_PODS] --max=[MAX_PODS]
  ```

---

### **9. Seguridad**
- **Habilitar IAM Roles for Service Accounts (IRSA)**:  
  Crear un IAM OIDC provider para el cluster:  
  ```bash
  eksctl utils associate-iam-oidc-provider --cluster [CLUSTER_NAME] --region [REGION] --approve
  ```
- **Crear un IAM Role y asociarlo a un Service Account**:  
  ```bash
  eksctl create iamserviceaccount --name [SERVICE_ACCOUNT_NAME] --namespace [NAMESPACE] --cluster [CLUSTER_NAME] --attach-policy-arn [POLICY_ARN] --approve
  ```

---

### **10. Actualización y Mantenimiento**
- **Actualizar un cluster EKS**:  
  ```bash
  eksctl upgrade cluster --name [CLUSTER_NAME] --region [REGION]
  ```
- **Actualizar nodos**:  
  ```bash
  eksctl upgrade nodegroup --name [NODEGROUP_NAME] --cluster [CLUSTER_NAME] --region [REGION]
  ```

---

### **11. Buenas Prácticas**
- Usar **namespaces** para organizar recursos.
- Implementar **límites de recursos** (requests/limits) en los pods.
- Usar **Helm** para gestionar aplicaciones complejas.
- Habilitar **Network Policies** para seguridad de red.
- Monitorear con **CloudWatch** y **Prometheus**.

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

Este cheatsheet cubre los aspectos más importantes de EKS. Para más detalles, consulta la [documentación oficial de EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html).
