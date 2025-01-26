
---

## **AWS IAM Cheatsheet**

### **1. Conceptos Clave**
- **IAM**: Servicio que gestiona acceso seguro a los recursos de AWS.
- **Recursos**: Servicios de AWS como EC2, S3, RDS, etc.
- **Identidades**:
  - **Usuarios**: Personas o aplicaciones que interactúan con AWS.
  - **Grupos**: Colección de usuarios con los mismos permisos.
  - **Roles**: Identidades temporales que pueden ser asumidas por usuarios, servicios o aplicaciones.
- **Políticas**: Documentos JSON que definen permisos (qué acciones se permiten o deniegan en qué recursos).
- **Roles predefinidos**: Roles creados por AWS con permisos preconfigurados (por ejemplo, `AmazonS3ReadOnlyAccess`).
- **Roles personalizados**: Roles creados por el usuario con permisos específicos.

---

### **2. Comandos Básicos de IAM**
- **Ver políticas de IAM**:  
  ```bash
  aws iam list-policies
  ```
- **Ver usuarios de IAM**:  
  ```bash
  aws iam list-users
  ```
- **Ver grupos de IAM**:  
  ```bash
  aws iam list-groups
  ```
- **Ver roles de IAM**:  
  ```bash
  aws iam list-roles
  ```

---

### **3. Gestión de Usuarios**
- **Crear un usuario**:  
  ```bash
  aws iam create-user --user-name [USER_NAME]
  ```
- **Asignar una política a un usuario**:  
  ```bash
  aws iam attach-user-policy --user-name [USER_NAME] --policy-arn [POLICY_ARN]
  ```
- **Eliminar un usuario**:  
  ```bash
  aws iam delete-user --user-name [USER_NAME]
  ```
- **Crear una clave de acceso para un usuario**:  
  ```bash
  aws iam create-access-key --user-name [USER_NAME]
  ```
- **Eliminar una clave de acceso**:  
  ```bash
  aws iam delete-access-key --user-name [USER_NAME] --access-key-id [ACCESS_KEY_ID]
  ```

---

### **4. Gestión de Grupos**
- **Crear un grupo**:  
  ```bash
  aws iam create-group --group-name [GROUP_NAME]
  ```
- **Agregar un usuario a un grupo**:  
  ```bash
  aws iam add-user-to-group --user-name [USER_NAME] --group-name [GROUP_NAME]
  ```
- **Asignar una política a un grupo**:  
  ```bash
  aws iam attach-group-policy --group-name [GROUP_NAME] --policy-arn [POLICY_ARN]
  ```
- **Eliminar un grupo**:  
  ```bash
  aws iam delete-group --group-name [GROUP_NAME]
  ```

---

### **5. Gestión de Roles**
- **Crear un rol**:  
  ```bash
  aws iam create-role --role-name [ROLE_NAME] --assume-role-policy-document file://[TRUST_POLICY_FILE].json
  ```
  Ejemplo de archivo JSON (`trust-policy.json`):  
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Service": "ec2.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
    ]
  }
  ```
- **Asignar una política a un rol**:  
  ```bash
  aws iam attach-role-policy --role-name [ROLE_NAME] --policy-arn [POLICY_ARN]
  ```
- **Eliminar un rol**:  
  ```bash
  aws iam delete-role --role-name [ROLE_NAME]
  ```

---

### **6. Gestión de Políticas**
- **Crear una política personalizada**:  
  ```bash
  aws iam create-policy --policy-name [POLICY_NAME] --policy-document file://[POLICY_FILE].json
  ```
  Ejemplo de archivo JSON (`policy.json`):  
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "s3:ListBucket",
        "Resource": "arn:aws:s3:::example-bucket"
      }
    ]
  }
  ```
- **Ver políticas adjuntas a un usuario/grupo/rol**:  
  ```bash
  aws iam list-attached-user-policies --user-name [USER_NAME]
  aws iam list-attached-group-policies --group-name [GROUP_NAME]
  aws iam list-attached-role-policies --role-name [ROLE_NAME]
  ```
- **Eliminar una política**:  
  ```bash
  aws iam delete-policy --policy-arn [POLICY_ARN]
  ```

---

### **7. Buenas Prácticas**
1. **Principio de menor privilegio**: Asigna solo los permisos necesarios.
2. **Usa grupos**: Asigna políticas a grupos en lugar de usuarios individuales.
3. **Usa roles para aplicaciones**: Asigna roles a servicios en lugar de usar credenciales de usuario.
4. **Habilita MFA**: Requiere autenticación multifactor para usuarios con acceso crítico.
5. **Audita permisos**: Usa herramientas como **IAM Access Analyzer** para revisar permisos.

---

### **8. Comandos Útiles Adicionales**
- **Ver políticas en línea (inline) de un usuario/grupo/rol**:  
  ```bash
  aws iam list-user-policies --user-name [USER_NAME]
  aws iam list-group-policies --group-name [GROUP_NAME]
  aws iam list-role-policies --role-name [ROLE_NAME]
  ```
- **Ver todas las políticas de un rol**:  
  ```bash
  aws iam list-attached-role-policies --role-name [ROLE_NAME]
  ```
- **Simular políticas**:  
  ```bash
  aws iam simulate-principal-policy --policy-source-arn [PRINCIPAL_ARN] --action-names [ACTION1,ACTION2]
  ```

---

### **9. Ejemplos Comunes**
- **Asignar acceso a un bucket de S3**:  
  ```bash
  aws iam attach-user-policy --user-name [USER_NAME] --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
  ```
- **Crear un rol para EC2**:  
  ```bash
  aws iam create-role --role-name EC2-S3-Access --assume-role-policy-document file://trust-policy.json
  aws iam attach-role-policy --role-name EC2-S3-Access --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
  ```
- **Asignar un rol a una instancia de EC2**:  
  ```bash
  aws ec2 associate-iam-instance-profile --instance-id [INSTANCE_ID] --iam-instance-profile Name=[INSTANCE_PROFILE_NAME]
  ```

---

### **10. Herramientas Adicionales**
- **IAM Access Analyzer**: Identifica recursos compartidos externamente.
  ```bash
  aws accessanalyzer list-analyzers
  ```
- **IAM Policy Simulator**: Prueba y depura políticas.
  - Disponible en la consola de AWS.

---

Este cheatsheet cubre los aspectos más importantes de IAM en AWS. Para más detalles, consulta la [documentación oficial de IAM](https://docs.aws.amazon.com/iam/).
