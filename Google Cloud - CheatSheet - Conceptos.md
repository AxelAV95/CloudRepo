---

## **Google Cloud Platform (GCP) Cheatsheet Completo**

---

### **1. Conceptos Básicos**
- **Proyecto**: Unidad básica para organizar recursos, facturación y permisos.
- **Regiones y Zonas**:
  - **Región**: Área geográfica con múltiples zonas (ej: `us-central1`).
  - **Zona**: Área específica dentro de una región (ej: `us-central1-a`).
- **IAM (Identity and Access Management)**: Control de acceso y permisos.
- **Facturación**: Asociado a un proyecto, con alertas y presupuestos.

---

### **2. Jerarquía de Recursos en GCP**

#### **Estructura de la Jerarquía**
1. **Organización** (Opcional, pero recomendado para empresas):
   - Nivel superior de la jerarquía.
   - Representa a una empresa o entidad.
   - Se asocia con un dominio de Google Workspace o Cloud Identity.
   - Permite gestionar políticas y permisos a nivel global.

2. **Carpetas** (Opcional):
   - Subniveles dentro de una organización.
   - Se utilizan para agrupar proyectos relacionados (por departamento, equipo, aplicación, etc.).
   - Permiten aplicar políticas y permisos a grupos de proyectos.

3. **Proyectos**:
   - Unidad básica de recursos en GCP.
   - Contiene todos los recursos (VMs, bases de datos, buckets, etc.).
   - Cada proyecto tiene un ID único y está asociado a una facturación.

4. **Recursos**:
   - Servicios y componentes dentro de un proyecto (Compute Engine, Cloud Storage, BigQuery, etc.).

#### **Beneficios de la Jerarquía**
- **Control de Acceso Centralizado**: Aplicar políticas de IAM a nivel de organización, carpeta o proyecto.
- **Gestión de Costos**: Asignar presupuestos y alertas a diferentes niveles de la jerarquía.
- **Aislamiento de Recursos**: Separar entornos (producción, desarrollo, pruebas) en proyectos o carpetas diferentes.
- **Escalabilidad**: Facilita la administración de múltiples proyectos y equipos.

#### **Políticas de IAM y Herencia**
- Las políticas de IAM se pueden aplicar en cualquier nivel de la jerarquía.
- Las políticas se heredan de manera descendente:
  - Una política aplicada a la organización afecta a todas las carpetas y proyectos dentro de ella.
  - Una política aplicada a una carpeta afecta a todos los proyectos dentro de esa carpeta.
  - Una política aplicada a un proyecto afecta solo a ese proyecto.

#### **Comandos Útiles para Gestionar la Jerarquía**
- **Organización**:
  ```bash
  gcloud organizations list
  gcloud organizations add-iam-policy-binding [ORGANIZATION_ID] --member=[MEMBER] --role=[ROLE]
  ```
- **Carpetas**:
  ```bash
  gcloud resource-manager folders create --display-name=[NOMBRE] --organization=[ORGANIZATION_ID]
  gcloud resource-manager folders list --organization=[ORGANIZATION_ID]
  gcloud resource-manager folders add-iam-policy-binding [FOLDER_ID] --member=[MEMBER] --role=[ROLE]
  ```
- **Proyectos**:
  ```bash
  gcloud projects create [PROJECT_ID] --name=[NOMBRE] --folder=[FOLDER_ID]
  gcloud projects move [PROJECT_ID] --folder=[FOLDER_ID]
  gcloud projects add-iam-policy-binding [PROJECT_ID] --member=[MEMBER] --role=[ROLE]
  ```

---

### **3. Servicios Principales**

#### **Compute**
- **Compute Engine**: Máquinas virtuales (VMs).
  - Comandos comunes:
    ```bash
    gcloud compute instances create [INSTANCE_NAME] --machine-type=[TYPE] --zone=[ZONE]
    gcloud compute instances list
    gcloud compute ssh [INSTANCE_NAME]
    ```
- **Kubernetes Engine (GKE)**: Orquestación de contenedores.
  - Comandos comunes:
    ```bash
    gcloud container clusters create [CLUSTER_NAME] --num-nodes=3 --zone=[ZONE]
    gcloud container clusters get-credentials [CLUSTER_NAME]
    kubectl get pods
    ```
- **App Engine**: Plataforma para aplicaciones web escalables.
  - Comandos comunes:
    ```bash
    gcloud app deploy
    gcloud app browse
    ```

#### **Storage**
- **Cloud Storage**: Almacenamiento de objetos.
  - Comandos comunes:
    ```bash
    gsutil cp [FILE] gs://[BUCKET_NAME]
    gsutil ls gs://[BUCKET_NAME]
    gsutil rm gs://[BUCKET_NAME]/[FILE]
    ```
- **Persistent Disk**: Almacenamiento persistente para VMs.
  - Comandos comunes:
    ```bash
    gcloud compute disks create [DISK_NAME] --size=[SIZE_GB] --zone=[ZONE]
    gcloud compute instances attach-disk [INSTANCE_NAME] --disk=[DISK_NAME]
    ```

#### **Bases de Datos**
- **Cloud SQL**: Bases de datos relacionales (MySQL, PostgreSQL, SQL Server).
  - Comandos comunes:
    ```bash
    gcloud sql instances create [INSTANCE_NAME] --database-version=[VERSION]
    gcloud sql connect [INSTANCE_NAME] --user=[USER]
    ```
- **Firestore**: Base de datos NoSQL.
- **BigQuery**: Análisis de datos a gran escala.
  - Comandos comunes:
    ```bash
    bq query "SELECT * FROM [DATASET].[TABLE]"
    bq mk [DATASET]
    ```

#### **Networking**
- **VPC (Virtual Private Cloud)**: Redes privadas en la nube.
  - Comandos comunes:
    ```bash
    gcloud compute networks create [NETWORK_NAME]
    gcloud compute firewall-rules create [RULE_NAME] --network=[NETWORK_NAME] --allow=[PORTS]
    ```
- **Cloud Load Balancing**: Balanceo de carga.
- **Cloud CDN**: Red de entrega de contenido.

#### **Big Data & AI/ML**
- **BigQuery**: Análisis de datos.
- **Dataflow**: Procesamiento de datos en streaming y batch.
- **AI Platform**: Entrenamiento y despliegue de modelos de ML.
- **Vertex AI**: Plataforma unificada para ML.

#### **DevOps**
- **Cloud Build**: CI/CD automatizado.
  - Comandos comunes:
    ```bash
    gcloud builds submit --config=[CONFIG_FILE]
    ```
- **Cloud Monitoring**: Monitoreo y alertas.
- **Cloud Logging**: Registros centralizados.

---

### **4. Comandos de `gcloud` (CLI de GCP)**
- **Autenticación**:
  ```bash
  gcloud auth login
  gcloud auth activate-service-account --key-file=[KEY_FILE]
  ```
- **Configuración**:
  ```bash
  gcloud config set project [PROJECT_ID]
  gcloud config set compute/zone [ZONE]
  ```
- **Información del proyecto**:
  ```bash
  gcloud projects describe [PROJECT_ID]
  gcloud compute project-info describe
  ```
- **Listar recursos**:
  ```bash
  gcloud compute instances list
  gcloud container clusters list
  gcloud sql instances list
  ```

---

### **5. Precios y Facturación**
- **Calculadora de precios**: [Google Cloud Pricing Calculator](https://cloud.google.com/products/calculator)
- **Presupuestos y alertas**:
  ```bash
  gcloud billing budgets create --billing-account=[ACCOUNT_ID] --display-name=[NAME] --budget-amount=[AMOUNT]
  ```

---

### **6. Seguridad**
- **IAM**:
  ```bash
  gcloud projects add-iam-policy-binding [PROJECT_ID] --member=[MEMBER] --role=[ROLE]
  ```
- **Claves de servicio**:
  ```bash
  gcloud iam service-accounts keys create [KEY_FILE] --iam-account=[SA_EMAIL]
  ```
- **Encriptación**: Automática en reposo y en tránsito.

---

### **7. Buenas Prácticas**
1. **Usa Carpetas para Agrupar Proyectos**: Facilita la gestión de permisos y políticas.
2. **Aplica el Principio de Mínimo Privilegio**: Otorga solo los permisos necesarios en cada nivel.
3. **Usa Etiquetas (Labels)**: Para organizar y filtrar recursos dentro de un proyecto.
4. **Centraliza la Facturación**: Asocia todos los proyectos a una misma cuenta de facturación.
5. **Monitorea y Audita**: Usa Cloud Logging y Cloud Monitoring para supervisar la actividad.

---

### **8. Enlaces Útiles**
- [Documentación oficial de GCP](https://cloud.google.com/docs)
- [Google Cloud Status Dashboard](https://status.cloud.google.com/)
- [Google Cloud Blog](https://cloud.google.com/blog/)

---

Este cheatsheet es una guía completa para trabajar con GCP. ¡Espero que te sea de gran ayuda! 🚀
