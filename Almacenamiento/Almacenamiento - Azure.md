

---

## **Azure Blob Storage**
- **Descripción**: Almacenamiento de objetos escalable y duradero para datos no estructurados.
- **Casos de uso**: Backup, archivo, análisis de datos, hosting de sitios web estáticos, distribución de contenido.
- **Niveles de acceso**:
  - **Hot**: Acceso frecuente.
  - **Cool**: Acceso menos frecuente.
  - **Archive**: Acceso raro (latencia de horas).
- **Características**:
  - Durabilidad del 99.999999999% (11 nueves).
  - Escalabilidad automática.
  - Control de acceso mediante RBAC y SAS (Shared Access Signatures).
  - Versionamiento de objetos.
  - Lifecycle Management (reglas de transición y eliminación).
- **Comandos útiles (Azure CLI)**:
  - Subir un archivo:  
    ```bash
    az storage blob upload --account-name [ACCOUNT_NAME] --container-name [CONTAINER_NAME] --name [BLOB_NAME] --file [ARCHIVO]
    ```
  - Descargar un archivo:  
    ```bash
    az storage blob download --account-name [ACCOUNT_NAME] --container-name [CONTAINER_NAME] --name [BLOB_NAME] --file [DESTINO]
    ```
  - Crear un contenedor:  
    ```bash
    az storage container create --account-name [ACCOUNT_NAME] --name [CONTAINER_NAME]
    ```
  - Eliminar un contenedor:  
    ```bash
    az storage container delete --account-name [ACCOUNT_NAME] --name [CONTAINER_NAME]
    ```

---

## **Azure Disk Storage**
- **Descripción**: Almacenamiento en bloque de alto rendimiento para máquinas virtuales (VMs).
- **Casos de uso**: Bases de datos, sistemas de archivos, aplicaciones empresariales.
- **Tipos de discos**:
  - **HDD estándar**: Discos HDD, costo reducido.
  - **SSD estándar**: Discos SSD de rendimiento moderado.
  - **SSD premium**: Discos SSD de alto rendimiento.
- **Características**:
  - Escalabilidad dinámica (aumentar tamaño sin downtime).
  - Snapshots automáticos y manuales.
  - Cifrado automático de datos en reposo.
- **Comandos útiles (Azure CLI)**:
  - Crear un disco:  
    ```bash
    az disk create --resource-group [RESOURCE_GROUP] --name [DISK_NAME] --size-gb [SIZE_GB] --sku [SKU]
    ```
  - Adjuntar un disco a una VM:  
    ```bash
    az vm disk attach --resource-group [RESOURCE_GROUP] --vm-name [VM_NAME] --disk [DISK_NAME]
    ```
  - Crear un snapshot:  
    ```bash
    az snapshot create --resource-group [RESOURCE_GROUP] --name [SNAPSHOT_NAME] --source [DISK_NAME]
    ```

---

## **Azure Files**
- **Descripción**: Almacenamiento de archivos compartidos (SMB/NFS) para aplicaciones.
- **Casos de uso**: Aplicaciones que requieren sistemas de archivos compartidos (CMS, CI/CD, etc.).
- **Características**:
  - Compatible con SMB y NFS.
  - Escalable y de alto rendimiento.
  - Integración con VMs y servicios en la nube.
- **Comandos útiles (Azure CLI)**:
  - Crear un recurso compartido de archivos:  
    ```bash
    az storage share create --account-name [ACCOUNT_NAME] --name [SHARE_NAME]
    ```

---

## **Azure SQL Database**
- **Descripción**: Base de datos relacional administrada (SQL Server).
- **Casos de uso**: Aplicaciones web, análisis de datos, sistemas transaccionales.
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas.
  - Replicación y alta disponibilidad.
- **Comandos útiles (Azure CLI)**:
  - Crear una instancia de SQL Database:  
    ```bash
    az sql db create --resource-group [RESOURCE_GROUP] --server [SERVER_NAME] --name [DB_NAME] --edition [EDITION] --capacity [SIZE_GB]
    ```

---

## **Azure Table Storage**
- **Descripción**: Almacenamiento NoSQL para datos semi-estructurados.
- **Casos de uso**: Almacenamiento de datos simples, aplicaciones escalables.
- **Características**:
  - Escalabilidad horizontal.
  - Bajo costo.
  - Consultas flexibles.
- **Comandos útiles (Azure CLI)**:
  - Crear una tabla:  
    ```bash
    az storage table create --account-name [ACCOUNT_NAME] --name [TABLE_NAME]
    ```

---

## **Azure Queue Storage**
- **Descripción**: Almacenamiento de mensajes para aplicaciones distribuidas.
- **Casos de uso**: Colas de mensajes, desacoplamiento de servicios.
- **Características**:
  - Escalabilidad horizontal.
  - Bajo costo.
  - Integración con Azure Functions.
- **Comandos útiles (Azure CLI)**:
  - Crear una cola:  
    ```bash
    az storage queue create --account-name [ACCOUNT_NAME] --name [QUEUE_NAME]
    ```

---

## **Azure Archive Storage**
- **Descripción**: Almacenamiento de bajo costo para datos a los que se accede raramente.
- **Casos de uso**: Archivo de datos a largo plazo, cumplimiento normativo.
- **Características**:
  - Muy bajo costo.
  - Acceso con latencia de horas.
- **Comandos útiles (Azure CLI)**:
  - Mover un blob a Archive:  
    ```bash
    az storage blob set-tier --account-name [ACCOUNT_NAME] --container-name [CONTAINER_NAME] --name [BLOB_NAME] --tier Archive
    ```

---

## **Resumen de Comparación**
| **Servicio**           | **Tipo**          | **Casos de Uso**                     | **Escalabilidad** | **Acceso**         |
|-------------------------|-------------------|---------------------------------------|-------------------|--------------------|
| **Blob Storage**        | Almacenamiento de objetos | Backup, archivo, análisis           | Alta              | API, CLI, Web      |
| **Disk Storage**        | Almacenamiento en bloque | Bases de datos, sistemas de archivos | Media             | VM-attached        |
| **Azure Files**         | Almacenamiento de archivos | Aplicaciones compartidas            | Media             | SMB/NFS            |
| **SQL Database**        | Base de datos relacional | Aplicaciones web, transacciones     | Media             | SQL                |
| **Table Storage**       | Almacenamiento NoSQL | Datos semi-estructurados            | Alta              | API, CLI           |
| **Queue Storage**       | Almacenamiento de mensajes | Colas de mensajes                  | Alta              | API, CLI           |
| **Archive Storage**     | Archivo de bajo costo | Archivo de datos a largo plazo      | Alta              | API, CLI           |

---

## **Consejos Adicionales**
1. **Lifecycle Management**: Configura reglas de ciclo de vida en Blob Storage para ahorrar costos.
2. **Snapshots**: Usa snapshots en Disk Storage para backups frecuentes.
3. **Cifrado**: Todos los servicios de almacenamiento en Azure cifran los datos en reposo por defecto.
4. **Monitorización**: Usa Azure Monitor para supervisar el uso y rendimiento del almacenamiento.

---

Este cheatsheet te ayudará a elegir el servicio de almacenamiento adecuado según tus necesidades y a gestionarlo eficientemente en Azure. ¡Espero que te sea útil! 🚀
