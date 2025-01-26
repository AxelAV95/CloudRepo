

---

## **AWS Networking - Cheatsheet**

### **1. Conceptos Básicos**

- **VPC (Virtual Private Cloud)**: Red privada virtual en la nube.
- **Subnets**: Segmentos de IP dentro de una VPC.
- **Route Tables**: Tablas de enrutamiento para dirigir el tráfico.
- **Security Groups**: Reglas de firewall a nivel de instancia.
- **Network ACLs**: Reglas de firewall a nivel de subred.
- **Internet Gateway (IGW)**: Permite la comunicación entre la VPC e Internet.
- **NAT Gateway**: Permite instancias en subredes privadas acceder a Internet.
- **Elastic Load Balancing (ELB)**: Balanceo de carga para aplicaciones.
- **Route 53**: Servicio de DNS administrado.
- **Direct Connect**: Conexión dedicada entre AWS y tu red local.
- **VPN**: Conexión segura entre AWS y tu red local.

---

### **2. Comandos Básicos de AWS CLI para Redes**

#### **Crear una VPC**
```bash
aws ec2 create-vpc \
  --cidr-block [RANGO_IP]
```
- Ejemplo:
  ```bash
  aws ec2 create-vpc --cidr-block 10.0.0.0/16
  ```

#### **Crear una Subred**
```bash
aws ec2 create-subnet \
  --vpc-id [ID_VPC] \
  --cidr-block [RANGO_IP] \
  --availability-zone [ZONA]
```
- Ejemplo:
  ```bash
  aws ec2 create-subnet \
    --vpc-id vpc-0abcdef1234567890 \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a
  ```

#### **Listar VPCs**
```bash
aws ec2 describe-vpcs
```

#### **Listar Subredes**
```bash
aws ec2 describe-subnets
```

#### **Eliminar una VPC**
```bash
aws ec2 delete-vpc --vpc-id [ID_VPC]
```

#### **Eliminar una Subred**
```bash
aws ec2 delete-subnet --subnet-id [ID_SUBRED]
```

---

### **3. Security Groups y Network ACLs**

#### **Crear un Security Group**
```bash
aws ec2 create-security-group \
  --group-name [NOMBRE_GRUPO] \
  --description "Descripción del grupo" \
  --vpc-id [ID_VPC]
```

#### **Agregar Reglas a un Security Group**
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

#### **Crear una Network ACL**
```bash
aws ec2 create-network-acl \
  --vpc-id [ID_VPC]
```

#### **Agregar Reglas a una Network ACL**
```bash
aws ec2 create-network-acl-entry \
  --network-acl-id [ID_ACL] \
  --rule-number [NUMERO_REGLA] \
  --protocol [PROTOCOLO] \
  --rule-action allow \
  --egress \
  --cidr-block [RANGO_IP] \
  --port-range From=[PUERTO_INICIAL],To=[PUERTO_FINAL]
```

---

### **4. Internet Gateway y NAT Gateway**

#### **Crear un Internet Gateway**
```bash
aws ec2 create-internet-gateway
```

#### **Asociar un Internet Gateway a una VPC**
```bash
aws ec2 attach-internet-gateway \
  --internet-gateway-id [ID_IGW] \
  --vpc-id [ID_VPC]
```

#### **Crear un NAT Gateway**
```bash
aws ec2 create-nat-gateway \
  --subnet-id [ID_SUBRED] \
  --allocation-id [ID_IP_ELASTICA]
```

---

### **5. Elastic Load Balancing (ELB)**

#### **Crear un Balanceador de Carga (ALB)**
```bash
aws elbv2 create-load-balancer \
  --name [NOMBRE_ALB] \
  --subnets [ID_SUBRED1] [ID_SUBRED2] \
  --security-groups [ID_GRUPO_SEGURIDAD]
```

#### **Crear un Target Group**
```bash
aws elbv2 create-target-group \
  --name [NOMBRE_TARGET_GROUP] \
  --protocol HTTP \
  --port 80 \
  --vpc-id [ID_VPC]
```

#### **Agregar Instancias a un Target Group**
```bash
aws elbv2 register-targets \
  --target-group-arn [ARN_TARGET_GROUP] \
  --targets Id=[ID_INSTANCIA]
```

---

### **6. Route 53 (DNS)**

#### **Crear una Zona DNS**
```bash
aws route53 create-hosted-zone \
  --name [DOMINIO] \
  --caller-reference [REFERENCIA]
```

#### **Agregar Registros DNS**
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id [ID_ZONA] \
  --change-batch file://[ARCHIVO_JSON]
```

---

### **7. Direct Connect y VPN**

#### **Crear una Conexión Direct Connect**
```bash
aws directconnect create-connection \
  --location [LOCATION] \
  --bandwidth [ANCHO_BANDA] \
  --connection-name [NOMBRE_CONEXION]
```

#### **Crear un VPN Gateway**
```bash
aws ec2 create-vpn-gateway \
  --type ipsec.1
```

#### **Crear un Túnel VPN**
```bash
aws ec2 create-vpn-connection \
  --customer-gateway-id [ID_CUSTOMER_GATEWAY] \
  --vpn-gateway-id [ID_VPN_GATEWAY] \
  --type ipsec.1
```

---

### **8. Buenas Prácticas**

- Usa **VPC peering** para conectar VPCs.
- Aplica **reglas de seguridad mínimas** (principio de menor privilegio).
- Usa **Elastic Load Balancing** para alta disponibilidad y escalabilidad.
- Implementa **Route 53** para gestionar dominios de manera centralizada.
- Considera **Direct Connect** o **VPN** para conexiones híbridas.
- Usa **NAT Gateway** para instancias en subredes privadas.

---

Este cheatsheet cubre los aspectos más importantes de redes en AWS. Para más detalles, consulta la [documentación oficial de AWS Networking](https://aws.amazon.com/networking/).
