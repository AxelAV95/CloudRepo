
---

## **AWS Cheatsheet Detallado**

### **1. Compute (Cómputo)**
- **EC2 (Elastic Compute Cloud)**: Servidores virtuales en la nube.
  - **AMI (Amazon Machine Image)**: Plantilla para lanzar instancias.
  - **Tipos de instancias**: General-purpose (t2, t3), Compute-optimized (c5), Memory-optimized (r5), GPU (p3), etc.
  - **Almacenamiento**: EBS (Elastic Block Store) para discos persistentes.
  - **Auto Scaling**: Escalado automático de instancias.
  - **Load Balancer**: Distribuye el tráfico entre instancias (ALB, NLB, CLB).

- **Lambda**: Ejecuta código sin gestionar servidores (serverless).
  - **Eventos**: Activado por S3, DynamoDB, API Gateway, etc.
  - **Lenguajes**: Node.js, Python, Java, Go, etc.

- **ECS (Elastic Container Service)**: Orquestación de contenedores Docker.
  - **Fargate**: Ejecuta contenedores sin gestionar servidores.
  - **ECR (Elastic Container Registry)**: Almacén de imágenes Docker.

- **EKS (Elastic Kubernetes Service)**: Servicio gestionado de Kubernetes.

---

### **2. Storage (Almacenamiento)**
- **S3 (Simple Storage Service)**: Almacenamiento de objetos escalable.
  - **Clases de almacenamiento**: S3 Standard, S3 Intelligent-Tiering, S3 Glacier (archivo).
  - **Versioning**: Mantiene múltiples versiones de un objeto.
  - **Lifecycle Policies**: Automatiza la transición entre clases de almacenamiento.

- **EBS (Elastic Block Store)**: Discos duros virtuales para EC2.
  - **Tipos de volúmenes**: gp2 (SSD general), io1 (SSD de alto rendimiento), st1 (HDD de bajo costo).

- **EFS (Elastic File System)**: Almacenamiento de archivos compartido para EC2.

- **Glacier**: Almacenamiento de archivo de bajo costo para backups.

---

### **3. Databases (Bases de Datos)**
- **RDS (Relational Database Service)**: Bases de datos relacionales gestionadas.
  - **Motores soportados**: MySQL, PostgreSQL, Oracle, SQL Server, MariaDB, Aurora.
  - **Aurora**: Base de datos compatible con MySQL/PostgreSQL de alto rendimiento.

- **DynamoDB**: Base de datos NoSQL completamente gestionada.
  - **Escalabilidad automática**: Ajusta capacidad según la demanda.
  - **DAX (DynamoDB Accelerator)**: Caché en memoria para DynamoDB.

- **Redshift**: Almacenamiento de datos (data warehouse) para análisis.

- **ElastiCache**: Caché en memoria (Redis o Memcached).

---

### **4. Networking (Redes)**
- **VPC (Virtual Private Cloud)**: Red privada virtual en AWS.
  - **Subnets**: Segmentos de red dentro de una VPC.
  - **Route Tables**: Define rutas de tráfico.
  - **NAT Gateway/NAT Instance**: Permite a instancias privadas acceder a Internet.
  - **Security Groups**: Firewall a nivel de instancia.
  - **Network ACLs**: Firewall a nivel de subred.

- **CloudFront**: Red de entrega de contenido (CDN).
  - **Edge Locations**: Puntos de presencia globales.

- **Route 53**: Servicio de DNS gestionado.
  - **Tipos de registros**: A, CNAME, MX, etc.
  - **Health Checks**: Monitorea la salud de los recursos.

- **API Gateway**: Crea, publica y gestiona APIs.

---

### **5. Security (Seguridad)**
- **IAM (Identity and Access Management)**: Gestiona usuarios, roles y permisos.
  - **Políticas**: Define permisos en formato JSON.
  - **Roles**: Permisos temporales para servicios o usuarios.

- **KMS (Key Management Service)**: Gestiona claves de cifrado.
  - **CMKs (Customer Master Keys)**: Claves maestras para cifrado.

- **Secrets Manager**: Almacena y gestiona secretos (contraseñas, API keys).

- **WAF (Web Application Firewall)**: Protege aplicaciones web de ataques.

- **CloudTrail**: Registra actividad de la cuenta de AWS.

---

### **6. Monitoring & Management (Monitoreo y Gestión)**
- **CloudWatch**: Monitorea recursos y aplicaciones.
  - **Métricas**: Datos de rendimiento (CPU, memoria, etc.).
  - **Alarmas**: Notificaciones basadas en métricas.
  - **Logs**: Almacena y analiza registros.

- **CloudFormation**: Infraestructura como código (IaC).
  - **Plantillas**: Define recursos en formato JSON o YAML.

- **Systems Manager**: Gestiona recursos y automatiza tareas.

- **Trusted Advisor**: Recomendaciones para optimizar costos, seguridad y rendimiento.

---

### **7. Developer Tools (Herramientas para Desarrolladores)**
- **CodeCommit**: Repositorio de código Git gestionado.
- **CodeBuild**: Compila y prueba código.
- **CodeDeploy**: Despliega aplicaciones en instancias.
- **CodePipeline**: Automatiza pipelines de CI/CD.

---

### **8. Analytics (Análisis)**
- **Athena**: Consulta datos en S3 usando SQL.
- **EMR (Elastic MapReduce)**: Procesamiento de big data (Hadoop, Spark).
- **Kinesis**: Procesamiento de datos en tiempo real.
  - **Kinesis Data Streams**: Captura y procesa datos en tiempo real.
  - **Kinesis Data Firehose**: Entrega datos a destinos como S3 o Redshift.

---

### **9. Machine Learning (Aprendizaje Automático)**
- **SageMaker**: Plataforma para construir, entrenar y desplegar modelos de ML.
- **Rekognition**: Análisis de imágenes y videos.
- **Polly**: Conversión de texto a voz.
- **Lex**: Creación de chatbots (usado en Alexa).

---

### **10. Cost Management (Gestión de Costos)**
- **Cost Explorer**: Visualiza y analiza costos.
- **Budgets**: Define presupuestos y alertas.
- **Savings Plans**: Ahorra en costos de EC2 y Lambda con compromisos a largo plazo.
- **Reserved Instances**: Descuentos por reservar instancias EC2.

---

### **11. Migration (Migración)**
- **DMS (Database Migration Service)**: Migra bases de datos a AWS.
- **Snowball**: Dispositivo físico para transferir grandes volúmenes de datos.
- **Server Migration Service**: Migra servidores locales a AWS.

---

### **12. IoT (Internet de las Cosas)**
- **IoT Core**: Conecta dispositivos IoT a la nube.
- **Greengrass**: Ejecuta computación local en dispositivos IoT.

---

### **13. Misceláneos**
- **SNS (Simple Notification Service)**: Envía notificaciones (email, SMS, etc.).
- **SQS (Simple Queue Service)**: Colas de mensajes para aplicaciones distribuidas.
- **Step Functions**: Orquesta funciones Lambda y servicios AWS.

---

### **Comandos CLI de AWS**
- **Configurar CLI**:
  ```bash
  aws configure
  ```
- **Listar instancias EC2**:
  ```bash
  aws ec2 describe-instances
  ```
- **Crear un bucket S3**:
  ```bash
  aws s3 mb s3://nombre-del-bucket
  ```
- **Subir un archivo a S3**:
  ```bash
  aws s3 cp archivo.txt s3://nombre-del-bucket
  ```

---

Este cheatsheet es una guía general. Para más detalles, consulta la [documentación oficial de AWS](https://aws.amazon.com/documentation/).
