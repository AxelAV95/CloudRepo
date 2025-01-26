
---

## **Google Cloud IAM Cheatsheet**

### **1. Conceptos Clave**
- **IAM**: Servicio que gestiona permisos y roles para recursos en GCP.
- **Recursos**: Proyectos, buckets de Cloud Storage, instancias de Compute Engine, etc.
- **Identidades**: Usuarios, grupos, service accounts y dominios.
- **Roles**: Conjuntos de permisos que definen lo que una identidad puede hacer.
  - **Roles primitivos**: Owner, Editor, Viewer.
  - **Roles predefinidos**: Específicos para servicios (por ejemplo, `roles/storage.admin`).
  - **Roles personalizados**: Definidos por el usuario.
- **Políticas de IAM**: Asignaciones de roles a identidades para recursos específicos.

---

### **2. Comandos Básicos de IAM**
- **Ver roles asignados a un recurso**:  
  ```bash
  gcloud projects get-iam-policy [PROJECT_ID]
  ```
- **Asignar un rol a una identidad**:  
  ```bash
  gcloud projects add-iam-policy-binding [PROJECT_ID] --member=[MEMBER] --role=[ROLE]
  ```
  Ejemplo:  
  ```bash
  gcloud projects add-iam-policy-binding my-project --member=user:john@example.com --role=roles/viewer
  ```
- **Eliminar un rol de una identidad**:  
  ```bash
  gcloud projects remove-iam-policy-binding [PROJECT_ID] --member=[MEMBER] --role=[ROLE]
  ```
- **Ver roles disponibles**:  
  ```bash
  gcloud iam roles list
  ```
- **Ver detalles de un rol**:  
  ```bash
  gcloud iam roles describe [ROLE_ID]
  ```

---

### **3. Gestión de Service Accounts**
- **Crear una service account**:  
  ```bash
  gcloud iam service-accounts create [SA_NAME] --display-name="[DISPLAY_NAME]"
  ```
- **Listar service accounts**:  
  ```bash
  gcloud iam service-accounts list
  ```
- **Asignar un rol a una service account**:  
  ```bash
  gcloud projects add-iam-policy-binding [PROJECT_ID] --member=serviceAccount:[SA_EMAIL] --role=[ROLE]
  ```
- **Generar una clave para una service account**:  
  ```bash
  gcloud iam service-accounts keys create [KEY_FILE].json --iam-account=[SA_EMAIL]
  ```
- **Eliminar una service account**:  
  ```bash
  gcloud iam service-accounts delete [SA_EMAIL]
  ```

---

### **4. Roles Personalizados**
- **Crear un rol personalizado**:  
  ```bash
  gcloud iam roles create [ROLE_ID] --project=[PROJECT_ID] --title="[TITLE]" --description="[DESCRIPTION]" --permissions=[PERMISSION1,PERMISSION2]
  ```
  Ejemplo:  
  ```bash
  gcloud iam roles create customRole --project=my-project --title="Custom Role" --description="Role with custom permissions" --permissions=storage.buckets.get,storage.buckets.list
  ```
- **Actualizar un rol personalizado**:  
  ```bash
  gcloud iam roles update [ROLE_ID] --project=[PROJECT_ID] --add-permissions=[PERMISSION]
  ```
- **Eliminar un rol personalizado**:  
  ```bash
  gcloud iam roles delete [ROLE_ID] --project=[PROJECT_ID]
  ```

---

### **5. Buenas Prácticas**
1. **Principio de menor privilegio**: Asigna solo los permisos necesarios.
2. **Usa grupos**: Asigna roles a grupos en lugar de usuarios individuales.
3. **Service accounts para aplicaciones**: Usa service accounts en lugar de cuentas de usuario para aplicaciones.
4. **Audita permisos**: Revisa regularmente las políticas de IAM.
5. **Evita roles primitivos**: Usa roles predefinidos o personalizados para mayor granularidad.

---

### **6. Comandos Útiles Adicionales**
- **Ver permisos de un recurso**:  
  ```bash
  gcloud asset search-all-iam-policies --scope=projects/[PROJECT_ID] --query="[RESOURCE_NAME]"
  ```
- **Ver roles asignados a una service account**:  
  ```bash
  gcloud projects get-iam-policy [PROJECT_ID] --flatten="bindings[].members" --filter="bindings.members:[SA_EMAIL]"
  ```
- **Ver roles asignados a un usuario**:  
  ```bash
  gcloud projects get-iam-policy [PROJECT_ID] --flatten="bindings[].members" --filter="bindings.members:user:[USER_EMAIL]"
  ```

---

### **7. Ejemplos Comunes**
- **Asignar acceso a un bucket de Cloud Storage**:  
  ```bash
  gcloud storage buckets add-iam-policy-binding gs://[BUCKET_NAME] --member=user:[USER_EMAIL] --role=roles/storage.objectViewer
  ```
- **Asignar acceso a una instancia de Compute Engine**:  
  ```bash
  gcloud compute instances add-iam-policy-binding [INSTANCE_NAME] --zone=[ZONE] --member=user:[USER_EMAIL] --role=roles/compute.instanceAdmin
  ```
- **Asignar acceso a un proyecto completo**:  
  ```bash
  gcloud projects add-iam-policy-binding [PROJECT_ID] --member=user:[USER_EMAIL] --role=roles/editor
  ```

---

### **8. Herramientas Adicionales**
- **IAM Recommender**: Sugiere permisos optimizados basados en el uso.
  ```bash
  gcloud recommender recommendations list --recommender=google.iam.policy.Recommender --project=[PROJECT_ID]
  ```
- **Policy Troubleshooter**: Diagnostica problemas de permisos.
  ```bash
  gcloud policy-troubleshooter [RESOURCE] --principal-email=[USER_EMAIL] --permission=[PERMISSION]
  ```

---

Este cheatsheet cubre los aspectos más importantes de IAM en GCP. Para más detalles, consulta la [documentación oficial de IAM](https://cloud.google.com/iam/docs).
