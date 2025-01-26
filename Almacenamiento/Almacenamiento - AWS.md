

---

## **Amazon S3 (Simple Storage Service)**
- **Descripción**: Almacenamiento de objetos escalable y duradero para datos no estructurados.
- **Casos de uso**: Backup, archivo, análisis de datos, hosting de sitios web estáticos, distribución de contenido.
- **Clases de almacenamiento**:
  - **S3 Standard**: Acceso frecuente.
  - **S3 Intelligent-Tiering**: Optimización automática de costos.
  - **S3 Standard-IA (Infrequent Access)**: Acceso menos frecuente.
  - **S3 One Zone-IA**: Acceso menos frecuente, almacenamiento en una sola zona.
  - **S3 Glacier**: Archivo de bajo costo (acceso en minutos u horas).
  - **S3 Glacier Deep Archive**: Archivo de muy bajo costo (acceso en horas).
- **Características**:
  - Durabilidad del 99.999999999% (11 nueves).
  - Escalabilidad automática.
  - Control de acceso mediante IAM y ACLs.
  - Versionamiento de objetos.
  - Lifecycle Management (reglas de transición y eliminación).
- **Comandos útiles (AWS CLI)**:
  - Subir un archivo:  
    ```bash
    aws s3 cp [ARCHIVO] s3://[BUCKET_NAME]/
    ```
  - Descargar un archivo:  
    ```bash
    aws s3 cp s3://[BUCKET_NAME]/[ARCHIVO] [DESTINO]
    ```
  - Crear un bucket:  
    ```bash
    aws s3 mb s3://[BUCKET_NAME]
    ```
  - Eliminar un bucket:  
    ```bash
    aws s3 rb s3://[BUCKET_NAME] --force
    ```
  - Configurar ACLs:  
    ```bash
    aws s3api put-object-acl --bucket [BUCKET_NAME] --key [ARCHIVO] --acl [PERMISSION]
    ```

---

## **Amazon EBS (Elastic Block Store)**
- **Descripción**: Almacenamiento en bloque de alto rendimiento para instancias EC2.
- **Casos de uso**: Bases de datos, sistemas de archivos, aplicaciones empresariales.
- **Tipos de volúmenes**:
  - **gp2/gp3**: Uso general (SSD).
  - **io1/io2**: Alto rendimiento (SSD, para cargas de trabajo intensivas).
  - **st1**: Almacenamiento optimizado para throughput (HDD).
  - **sc1**: Almacenamiento de bajo costo (HDD).
- **Características**:
  - Escalabilidad dinámica (aumentar tamaño sin downtime).
  - Snapshots automáticos y manuales.
  - Cifrado automático de datos en reposo.
- **Comandos útiles (AWS CLI)**:
  - Crear un volumen:  
    ```bash
    aws ec2 create-volume --availability-zone [AZ] --size [SIZE_GB] --volume-type [TYPE]
    ```
  - Adjuntar un volumen a una instancia EC2:  
    ```bash
    aws ec2 attach-volume --volume-id [VOLUME_ID] --instance-id [INSTANCE_ID] --device [DEVICE_NAME]
    ```
  - Crear un snapshot:  
    ```bash
    aws ec2 create-snapshot --volume-id [VOLUME_ID]
    ```

---

## **Amazon EFS (Elastic File System)**
- **Descripción**: Almacenamiento de archivos compartidos (NFS) para aplicaciones.
- **Casos de uso**: Aplicaciones que requieren sistemas de archivos compartidos (CMS, CI/CD, etc.).
- **Características**:
  - Compatible con NFS.
  - Escalable y de alto rendimiento.
  - Integración con EC2 y Lambda.
- **Comandos útiles (AWS CLI)**:
  - Crear un sistema de archivos EFS:  
    ```bash
    aws efs create-file-system --creation-token [TOKEN]
    ```

---

## **Amazon RDS (Relational Database Service)**
- **Descripción**: Base de datos relacional administrada (MySQL, PostgreSQL, Oracle, SQL Server, etc.).
- **Casos de uso**: Aplicaciones web, análisis de datos, sistemas transaccionales.
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas.
  - Replicación y alta disponibilidad.
- **Comandos útiles (AWS CLI)**:
  - Crear una instancia de RDS:  
    ```bash
    aws rds create-db-instance --db-instance-identifier [INSTANCE_ID] --engine [ENGINE] --db-instance-class [INSTANCE_CLASS] --allocated-storage [SIZE_GB]
    ```

---

## **Amazon DynamoDB**
- **Descripción**: Base de datos NoSQL de alto rendimiento para grandes volúmenes de datos.
- **Casos de uso**: Aplicaciones en tiempo real, IoT, sistemas de recomendación.
- **Características**:
  - Escalabilidad horizontal.
  - Baja latencia.
  - Integración con Lambda y API Gateway.
- **Comandos útiles (AWS CLI)**:
  - Crear una tabla:  
    ```bash
    aws dynamodb create-table --table-name [TABLE_NAME] --attribute-definitions AttributeName=[ATTR_NAME],AttributeType=[ATTR_TYPE] --key-schema AttributeName=[ATTR_NAME],KeyType=HASH --provisioned-throughput ReadCapacityUnits=[RCU],WriteCapacityUnits=[WCU]
    ```

---

## **Amazon Glacier**
- **Descripción**: Servicio de archivo de bajo costo para datos a los que se accede raramente.
- **Casos de uso**: Archivo de datos a largo plazo, cumplimiento normativo.
- **Características**:
  - Muy bajo costo.
  - Acceso en minutos u horas (Glacier) o horas (Glacier Deep Archive).
- **Comandos útiles (AWS CLI)**:
  - Subir un archivo a Glacier:  
    ```bash
    aws glacier upload-archive --vault-name [VAULT_NAME] --body [ARCHIVO]
    ```

---

## **Amazon FSx**
- **Descripción**: Servicio de sistemas de archivos compartidos para cargas de trabajo específicas.
- **Casos de uso**: Aplicaciones de Windows (FSx for Windows File Server), cargas de trabajo de alto rendimiento (FSx for Lustre).
- **Características**:
  - Integración con Active Directory (FSx for Windows).
  - Alto rendimiento (FSx for Lustre).
- **Comandos útiles (AWS CLI)**:
  - Crear un sistema de archivos FSx:  
    ```bash
    aws fsx create-file-system --file-system-type [TYPE] --storage-capacity [SIZE_GB] --subnet-ids [SUBNET_ID]
    ```

---

## **Resumen de Comparación**
| **Servicio**       | **Tipo**          | **Casos de Uso**                     | **Escalabilidad** | **Acceso**         |
|---------------------|-------------------|---------------------------------------|-------------------|--------------------|
| **S3**              | Almacenamiento de objetos | Backup, archivo, análisis           | Alta              | API, CLI, Web      |
| **EBS**             | Almacenamiento en bloque | Bases de datos, sistemas de archivos | Media             | EC2-attached       |
| **EFS**             | Almacenamiento de archivos | Aplicaciones compartidas            | Media             | NFS                |
| **RDS**             | Base de datos relacional | Aplicaciones web, transacciones     | Media             | SQL                |
| **DynamoDB**        | Base de datos NoSQL | Aplicaciones en tiempo real         | Alta              | API, CLI           |
| **Glacier**         | Archivo de bajo costo | Archivo de datos a largo plazo      | Alta              | API, CLI           |
| **FSx**             | Sistemas de archivos compartidos | Cargas de trabajo específicas      | Media             | NFS/SMB            |

---

## **Consejos Adicionales**
1. **Lifecycle Management**: Configura reglas de ciclo de vida en S3 para ahorrar costos.
2. **Snapshots**: Usa snapshots en EBS para backups frecuentes.
3. **Cifrado**: Todos los servicios de almacenamiento en AWS cifran los datos en reposo por defecto.
4. **Monitorización**: Usa CloudWatch para supervisar el uso y rendimiento del almacenamiento.

---

Este cheatsheet te ayudará a elegir el servicio de almacenamiento adecuado según tus necesidades y a gestionarlo eficientemente en AWS. ¡Espero que te sea útil! 🚀
