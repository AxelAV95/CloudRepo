

---

## **Google Cloud Storage (GCS)**
- **Descripción**: Almacenamiento de objetos escalable y duradero para datos no estructurados.
- **Casos de uso**: Backup, archivo, análisis de datos, hosting de sitios web estáticos, distribución de contenido.
- **Clases de almacenamiento**:
  - **Standard**: Acceso frecuente.
  - **Nearline**: Acceso menos frecuente (1 vez al mes o menos).
  - **Coldline**: Acceso raro (1 vez al año o menos).
  - **Archive**: Acceso muy raro (1 vez al año o menos, con mayor latencia).
- **Características**:
  - Durabilidad del 99.999999999% (11 nueves).
  - Escalabilidad automática.
  - Control de acceso mediante IAM y ACLs.
  - Versionamiento de objetos.
  - Lifecycle Management (reglas de retención y eliminación).
- **Comandos útiles**:
  - Subir un archivo:  
    ```bash
    gsutil cp [ARCHIVO] gs://[BUCKET_NAME]/
    ```
  - Descargar un archivo:  
    ```bash
    gsutil cp gs://[BUCKET_NAME]/[ARCHIVO] [DESTINO]
    ```
  - Crear un bucket:  
    ```bash
    gsutil mb gs://[BUCKET_NAME]/
    ```
  - Eliminar un bucket:  
    ```bash
    gsutil rm -r gs://[BUCKET_NAME]/
    ```
  - Configurar ACLs:  
    ```bash
    gsutil acl ch -u [USER]:[PERMISSION] gs://[BUCKET_NAME]/[ARCHIVO]
    ```

---

## **Persistent Disk (PD)**
- **Descripción**: Almacenamiento en bloque de alto rendimiento para instancias de VM.
- **Casos de uso**: Bases de datos, sistemas de archivos, aplicaciones empresariales.
- **Tipos**:
  - **Standard**: Discos HDD, costo reducido.
  - **SSD**: Discos de estado sólido, alto rendimiento.
- **Características**:
  - Escalabilidad dinámica (aumentar tamaño sin downtime).
  - Snapshots automáticos y manuales.
  - Cifrado automático de datos en reposo.
- **Comandos útiles**:
  - Crear un disco:  
    ```bash
    gcloud compute disks create [DISK_NAME] --size=[SIZE_GB] --zone=[ZONE]
    ```
  - Adjuntar un disco a una VM:  
    ```bash
    gcloud compute instances attach-disk [INSTANCE_NAME] --disk=[DISK_NAME]
    ```
  - Crear un snapshot:  
    ```bash
    gcloud compute disks snapshot [DISK_NAME] --snapshot-names=[SNAPSHOT_NAME]
    ```

---

## **Filestore**
- **Descripción**: Almacenamiento de archivos compartidos (NFS) para aplicaciones.
- **Casos de uso**: Aplicaciones que requieren sistemas de archivos compartidos (CMS, CI/CD, etc.).
- **Características**:
  - Compatible con NFS.
  - Escalable y de alto rendimiento.
  - Integración con Kubernetes (GKE).
- **Comandos útiles**:
  - Crear una instancia de Filestore:  
    ```bash
    gcloud filestore instances create [INSTANCE_NAME] --zone=[ZONE] --tier=[TIER] --file-share=name=[SHARE_NAME],capacity=[SIZE_GB]
    ```

---

## **Cloud SQL**
- **Descripción**: Base de datos relacional administrada (MySQL, PostgreSQL, SQL Server).
- **Casos de uso**: Aplicaciones web, análisis de datos, sistemas transaccionales.
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas.
  - Replicación y alta disponibilidad.
- **Comandos útiles**:
  - Crear una instancia de Cloud SQL:  
    ```bash
    gcloud sql instances create [INSTANCE_NAME] --database-version=[DB_VERSION] --tier=[MACHINE_TYPE] --region=[REGION]
    ```

---

## **Cloud Bigtable**
- **Descripción**: Base de datos NoSQL de alto rendimiento para grandes volúmenes de datos.
- **Casos de uso**: Análisis en tiempo real, IoT, sistemas de recomendación.
- **Características**:
  - Escalabilidad horizontal.
  - Baja latencia.
  - Integración con Hadoop y Dataflow.
- **Comandos útiles**:
  - Crear una instancia de Bigtable:  
    ```bash
    cbt createinstance [INSTANCE_ID] [INSTANCE_NAME] [CLUSTER_ID] [ZONE] [NUM_NODES] [STORAGE_TYPE]
    ```

---

## **Firestore**
- **Descripción**: Base de datos NoSQL para aplicaciones móviles y web.
- **Casos de uso**: Aplicaciones en tiempo real, sincronización de datos offline.
- **Características**:
  - Escalabilidad automática.
  - Sincronización en tiempo real.
  - Consultas flexibles.
- **Comandos útiles**:
  - Agregar un documento:  
    ```bash
    firestore:add [COLLECTION_NAME] [DOCUMENT_DATA]
    ```

---

## **Cloud Spanner**
- **Descripción**: Base de datos relacional distribuida y globalmente escalable.
- **Casos de uso**: Aplicaciones globales, sistemas transaccionales.
- **Características**:
  - Escalabilidad horizontal.
  - Consistencia global.
  - SQL compatible.
- **Comandos útiles**:
  - Crear una instancia de Spanner:  
    ```bash
    gcloud spanner instances create [INSTANCE_NAME] --config=[CONFIG_NAME] --description=[DESCRIPTION] --nodes=[NUM_NODES]
    ```

---

## **Resumen de Comparación**
| **Servicio**       | **Tipo**          | **Casos de Uso**                     | **Escalabilidad** | **Acceso**         |
|---------------------|-------------------|---------------------------------------|-------------------|--------------------|
| **Cloud Storage**   | Almacenamiento de objetos | Backup, archivo, análisis           | Alta              | API, CLI, Web      |
| **Persistent Disk** | Almacenamiento en bloque | Bases de datos, sistemas de archivos | Media             | VM-attached        |
| **Filestore**       | Almacenamiento de archivos | Aplicaciones compartidas            | Media             | NFS                |
| **Cloud SQL**       | Base de datos relacional | Aplicaciones web, transacciones     | Media             | SQL                |
| **Bigtable**        | Base de datos NoSQL | IoT, análisis en tiempo real         | Alta              | API, CLI           |
| **Firestore**       | Base de datos NoSQL | Aplicaciones móviles, tiempo real   | Alta              | API, SDK           |
| **Cloud Spanner**   | Base de datos relacional distribuida | Aplicaciones globales              | Alta              | SQL                |

---

## **Consejos Adicionales**
1. **Lifecycle Management**: Configura reglas de ciclo de vida en GCS para ahorrar costos.
2. **Snapshots**: Usa snapshots en Persistent Disk para backups frecuentes.
3. **Cifrado**: Todos los servicios de almacenamiento en GCP cifran los datos en reposo por defecto.
4. **Monitorización**: Usa Cloud Monitoring para supervisar el uso y rendimiento del almacenamiento.

---

Este cheatsheet te ayudará a elegir el servicio de almacenamiento adecuado según tus necesidades y a gestionarlo eficientemente en Google Cloud. ¡Espero que te sea útil! 🚀
