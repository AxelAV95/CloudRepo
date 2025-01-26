

---

## **DevOps en Google Cloud Cheatsheet**

### **Conceptos Básicos**
- **DevOps**: Cultura y prácticas que combinan desarrollo de software (Dev) y operaciones de TI (Ops) para acelerar la entrega de aplicaciones.
- **CI/CD**: Integración Continua (CI) y Entrega Continua (CD) para automatizar la construcción, prueba y despliegue de aplicaciones.
- **Infraestructura como Código (IaC)**: Gestión de infraestructura mediante código en lugar de procesos manuales.
- **Monitoreo y Observabilidad**: Supervisión del rendimiento y salud de las aplicaciones en tiempo real.

---

### **Servicios y Herramientas de Google Cloud para DevOps**

#### **CI/CD**
1. **Cloud Build**:
   - Servicio de CI/CD completamente gestionado.
   - Integración con GitHub, Bitbucket y GitLab.
   - Ejemplo de configuración en `cloudbuild.yaml`:
     ```yaml
     steps:
       - name: 'gcr.io/cloud-builders/gcloud'
         args: ['app', 'deploy']
     timeout: '1600s'
     ```

2. **Artifact Registry**:
   - Almacenamiento y gestión de imágenes Docker y paquetes.
   - Ejemplo de subida de imagen Docker:
     ```bash
     docker tag my-image us-central1-docker.pkg.dev/my-project/my-repo/my-image:tag
     docker push us-central1-docker.pkg.dev/my-project/my-repo/my-image:tag
     ```

3. **Deployment Manager**:
   - Herramienta de IaC para gestionar recursos de Google Cloud.
   - Ejemplo de configuración en YAML:
     ```yaml
     resources:
       - type: compute.v1.instance
         name: my-vm
         properties:
           zone: us-central1-a
           machineType: zones/us-central1-a/machineTypes/n1-standard-1
           disks:
             - deviceName: boot
               type: PERSISTENT
               boot: true
               autoDelete: true
               initializeParams:
                 sourceImage: projects/debian-cloud/global/images/family/debian-10
           networkInterfaces:
             - network: global/networks/default
     ```

#### **Gestión de Infraestructura**
1. **Terraform con Google Cloud**:
   - Herramienta de IaC para gestionar recursos en múltiples proveedores.
   - Ejemplo de configuración en Terraform:
     ```hcl
     provider "google" {
       project = "my-project"
       region  = "us-central1"
     }

     resource "google_compute_instance" "default" {
       name         = "my-vm"
       machine_type = "n1-standard-1"
       zone         = "us-central1-a"

       boot_disk {
         initialize_params {
           image = "debian-cloud/debian-10"
         }
       }

       network_interface {
         network = "default"
       }
     }
     ```

2. **Kubernetes Engine (GKE)**:
   - Servicio gestionado de Kubernetes para orquestar contenedores.
   - Ejemplo de despliegue en GKE:
     ```bash
     gcloud container clusters create my-cluster --num-nodes=3 --zone=us-central1-a
     kubectl create deployment my-app --image=us-central1-docker.pkg.dev/my-project/my-repo/my-image:tag
     kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080
     ```

#### **Monitoreo y Observabilidad**
1. **Cloud Monitoring (Stackdriver)**:
   - Supervisión de métricas, logs y alertas.
   - Ejemplo de creación de una alerta:
     ```bash
     gcloud alpha monitoring policies create --policy-from-file=alert-policy.json
     ```

2. **Cloud Logging**:
   - Centralización y análisis de logs.
   - Ejemplo de consulta de logs:
     ```bash
     gcloud logging read "logName=projects/my-project/logs/my-log"
     ```

#### **Seguridad**
1. **Cloud IAM**:
   - Gestión de identidades y acceso.
   - Ejemplo de asignación de roles:
     ```bash
     gcloud projects add-iam-policy-binding my-project --member=user:example@example.com --role=roles/editor
     ```

2. **Security Command Center**:
   - Supervisión de la seguridad y detección de amenazas.

---

### **Mejores Prácticas**
1. **Automatización**: Automatiza todo lo posible, desde pruebas hasta despliegues.
2. **Infraestructura como Código**: Usa herramientas como Terraform o Deployment Manager.
3. **Monitoreo Continuo**: Implementa alertas y dashboards para supervisar el rendimiento.
4. **Seguridad Integrada**: Asegura cada etapa del pipeline CI/CD.
5. **Escalabilidad**: Diseña aplicaciones y pipelines para ser escalables.

---

### **Ejemplo de Pipeline CI/CD en Google Cloud**
1. **Código en GitHub**:
   - Los desarrolladores envían código a un repositorio de GitHub.

2. **Cloud Build**:
   - Cloud Build detecta cambios y ejecuta pruebas y construcción de imágenes Docker.
   - Configuración en `cloudbuild.yaml`:
     ```yaml
     steps:
       - name: 'gcr.io/cloud-builders/docker'
         args: ['build', '-t', 'us-central1-docker.pkg.dev/my-project/my-repo/my-image:tag', '.']
       - name: 'gcr.io/cloud-builders/docker'
         args: ['push', 'us-central1-docker.pkg.dev/my-project/my-repo/my-image:tag']
       - name: 'gcr.io/cloud-builders/gcloud'
         args: ['app', 'deploy']
     ```

3. **Artifact Registry**:
   - Las imágenes Docker se almacenan en Artifact Registry.

4. **Despliegue en GKE**:
   - Las imágenes se despliegan en Kubernetes Engine.
   - Ejemplo de despliegue:
     ```bash
     kubectl set image deployment/my-app my-app=us-central1-docker.pkg.dev/my-project/my-repo/my-image:tag
     ```

5. **Monitoreo**:
   - Cloud Monitoring y Logging supervisan el rendimiento y la salud de la aplicación.

---

### **Recursos Adicionales**
- [Documentación Oficial de Cloud Build](https://cloud.google.com/build/docs)
- [Documentación Oficial de GKE](https://cloud.google.com/kubernetes-engine/docs)
- [Documentación Oficial de Terraform con Google Cloud](https://registry.terraform.io/providers/hashicorp/google/latest/docs)
- [Precios de Google Cloud DevOps](https://cloud.google.com/pricing/)

---

Esta cheatsheet te proporciona una referencia rápida y completa para implementar **DevOps en Google Cloud**. ¡Espero que te sea útil! 😊
