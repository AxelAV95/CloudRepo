

---

## **AWS EC2 - Cheatsheet de Instancias**

### **1. Tipos de Instancias**
AWS ofrece una amplia variedad de tipos de instancias optimizadas para diferentes cargas de trabajo:

- **General Purpose (T3, T4g, M5, M6g)**: Equilibrio entre CPU, memoria y red.
  - Ejemplo: `t3.micro`, `m5.large`
- **Compute Optimized (C5, C6g)**: Alto rendimiento de CPU.
  - Ejemplo: `c5.xlarge`
- **Memory Optimized (R5, R6g, X1)**: Alta relación memoria/CPU.
  - Ejemplo: `r5.4xlarge`, `x1e.32xlarge`
- **Storage Optimized (I3, D2)**: Alto rendimiento de almacenamiento.
  - Ejemplo: `i3.16xlarge`, `d2.8xlarge`
- **Accelerated Computing (P3, P4, G4)**: Para cargas de trabajo de GPU.
  - Ejemplo: `p3.8xlarge`, `g4dn.xlarge`

---

### **2. Comandos Básicos de AWS CLI para Instancias**

#### **Crear una Instancia (Run Instances)**
```bash
aws ec2 run-instances \
  --image-id [AMI_ID] \
  --instance-type [TIPO_INSTANCIA] \
  --key-name [NOMBRE_LLAVE] \
  --security-group-ids [ID_GRUPO_SEGURIDAD] \
  --subnet-id [ID_SUBNET] \
  --count [NUMERO_INSTANCIAS]
```
- Ejemplo:
  ```bash
  aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type t3.micro \
    --key-name mi-llave \
    --security-group-ids sg-0abcdef1234567890 \
    --subnet-id subnet-0abcdef1234567890
  ```

#### **Listar Instancias**
```bash
aws ec2 describe-instances
```

#### **Detener una Instancia**
```bash
aws ec2 stop-instances --instance-ids [ID_INSTANCIA]
```

#### **Iniciar una Instancia**
```bash
aws ec2 start-instances --instance-ids [ID_INSTANCIA]
```

#### **Terminar una Instancia**
```bash
aws ec2 terminate-instances --instance-ids [ID_INSTANCIA]
```

#### **Conectar a una Instancia (SSH)**
```bash
ssh -i [RUTA_LLAVE_PRIVADA] ec2-user@[IP_PUBLICA]
```
- Nota: `ec2-user` es el usuario por defecto para Amazon Linux. Para otras distribuciones:
  - Ubuntu: `ubuntu`
  - CentOS: `centos`
  - RHEL: `ec2-user`

#### **Cambiar Tipo de Instancia**
```bash
aws ec2 modify-instance-attribute \
  --instance-id [ID_INSTANCIA] \
  --instance-type [NUEVO_TIPO]
```

---

### **3. Gestión de Discos (EBS)**

#### **Crear un Volumen EBS**
```bash
aws ec2 create-volume \
  --availability-zone [ZONA] \
  --size [TAMAÑO] \
  --volume-type [TIPO_VOLUMEN]
```
- Ejemplo:
  ```bash
  aws ec2 create-volume \
    --availability-zone us-east-1a \
    --size 100 \
    --volume-type gp2
  ```

#### **Adjuntar un Volumen a una Instancia**
```bash
aws ec2 attach-volume \
  --volume-id [ID_VOLUMEN] \
  --instance-id [ID_INSTANCIA] \
  --device [DISPOSITIVO]
```
- Ejemplo:
  ```bash
  aws ec2 attach-volume \
    --volume-id vol-0abcdef1234567890 \
    --instance-id i-0abcdef1234567890 \
    --device /dev/xvdf
  ```

#### **Desadjuntar un Volumen**
```bash
aws ec2 detach-volume --volume-id [ID_VOLUMEN]
```

---

### **4. Redes y Seguridad**

#### **Asignar una IP Elástica**
```bash
aws ec2 allocate-address
aws ec2 associate-address \
  --instance-id [ID_INSTANCIA] \
  --public-ip [IP_ELASTICA]
```

#### **Crear un Grupo de Seguridad**
```bash
aws ec2 create-security-group \
  --group-name [NOMBRE_GRUPO] \
  --description "Descripción del grupo"
```

#### **Agregar Reglas a un Grupo de Seguridad**
```bash
aws ec2 authorize-security-group-ingress \
  --group-id [ID_GRUPO] \
  --protocol [PROTOCOLO] \
  --port [PUERTO] \
  --cidr [RANGO_IP]
```
- Ejemplo:
  ```bash
  aws ec2 authorize-security-group-ingress \
    --group-id sg-0abcdef1234567890 \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0
  ```

---

### **5. Imágenes y Snapshots**

#### **Crear una Imagen (AMI) desde una Instancia**
```bash
aws ec2 create-image \
  --instance-id [ID_INSTANCIA] \
  --name [NOMBRE_IMAGEN]
```

#### **Crear un Snapshot de un Volumen EBS**
```bash
aws ec2 create-snapshot \
  --volume-id [ID_VOLUMEN] \
  --description "Descripción del snapshot"
```

#### **Restaurar un Snapshot**
```bash
aws ec2 create-volume \
  --snapshot-id [ID_SNAPSHOT] \
  --availability-zone [ZONA]
```

---

### **6. Autoscaling y Grupos de Instancias**

#### **Crear un Grupo de Auto Scaling**
```bash
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name [NOMBRE_GRUPO] \
  --launch-configuration-name [NOMBRE_CONFIGURACION] \
  --min-size [MIN_INSTANCIAS] \
  --max-size [MAX_INSTANCIAS] \
  --desired-capacity [CAPACIDAD_DESEADA] \
  --vpc-zone-identifier [SUBNETS]
```

#### **Configurar Políticas de Auto Scaling**
```bash
aws autoscaling put-scaling-policy \
  --policy-name [NOMBRE_POLITICA] \
  --auto-scaling-group-name [NOMBRE_GRUPO] \
  --scaling-adjustment [AJUSTE] \
  --adjustment-type [TIPO_AJUSTE]
```

---

### **7. Precios y Ahorro de Costos**

- **Instancias Spot**: Hasta un 90% más baratas, pero pueden ser interrumpidas por AWS.
  ```bash
  aws ec2 request-spot-instances \
    --spot-price [PRECIO_MAXIMO] \
    --instance-count [NUMERO_INSTANCIAS] \
    --launch-specification file://[ARCHIVO_JSON]
  ```
- **Instancias Reservadas**: Descuentos por compromiso de uso a largo plazo (1 o 3 años).
- **Savings Plans**: Descuentos por uso comprometido de forma flexible.

---

### **8. Monitoreo y Logs**

#### **Ver Métricas de una Instancia (CloudWatch)**
```bash
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=[ID_INSTANCIA] \
  --start-time [FECHA_INICIO] \
  --end-time [FECHA_FIN] \
  --period [PERIODO] \
  --statistics Average
```

#### **Ver Logs de una Instancia (CloudWatch Logs)**
```bash
aws logs filter-log-events \
  --log-group-name [NOMBRE_GRUPO] \
  --start-time [FECHA_INICIO] \
  --end-time [FECHA_FIN]
```

---

### **9. Buenas Prácticas**
- Usa **etiquetas (tags)** para organizar instancias.
- Aprovecha **AMIs personalizadas** para configuraciones repetibles.
- Usa **snapshots** para backups regulares.
- Considera **instancias Spot** para cargas de trabajo tolerantes a interrupciones.

---

Este cheatsheet cubre los aspectos más importantes de las instancias en AWS EC2. Para más detalles, consulta la [documentación oficial de AWS EC2](https://aws.amazon.com/ec2/).
