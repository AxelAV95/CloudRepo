

---

## **Azure IAM Cheatsheet**

### **1. Conceptos Clave**
- **IAM**: Servicio que gestiona el acceso seguro a los recursos de Azure.
- **Recursos**: Servicios de Azure como máquinas virtuales, cuentas de almacenamiento, bases de datos, etc.
- **Identidades**:
  - **Usuarios**: Personas o aplicaciones que interactúan con Azure.
  - **Grupos**: Colección de usuarios con los mismos permisos.
  - **Service Principals**: Identidades para aplicaciones o servicios.
  - **Managed Identities**: Identidades gestionadas automáticamente por Azure.
- **Roles**: Conjuntos de permisos que definen lo que una identidad puede hacer.
  - **Roles predefinidos**: Roles creados por Azure con permisos preconfigurados (por ejemplo, `Contributor`, `Reader`).
  - **Roles personalizados**: Roles creados por el usuario con permisos específicos.
- **Ámbito (Scope)**: Nivel al que se asignan los roles (por ejemplo, suscripción, grupo de recursos o recurso individual).

---

### **2. Comandos Básicos de IAM**
- **Ver roles asignados a un recurso**:  
  ```bash
  az role assignment list --resource-group [RESOURCE_GROUP]
  ```
- **Asignar un rol a una identidad**:  
  ```bash
  az role assignment create --assignee [USER_PRINCIPAL_NAME] --role [ROLE_NAME] --resource-group [RESOURCE_GROUP]
  ```
  Ejemplo:  
  ```bash
  az role assignment create --assignee john@example.com --role "Reader" --resource-group my-resource-group
  ```
- **Eliminar una asignación de rol**:  
  ```bash
  az role assignment delete --assignee [USER_PRINCIPAL_NAME] --role [ROLE_NAME] --resource-group [RESOURCE_GROUP]
  ```
- **Ver roles disponibles**:  
  ```bash
  az role definition list
  ```
- **Ver detalles de un rol**:  
  ```bash
  az role definition list --name [ROLE_NAME]
  ```

---

### **3. Gestión de Usuarios y Grupos**
- **Crear un usuario**:  
  ```bash
  az ad user create --display-name "[DISPLAY_NAME]" --password [PASSWORD] --user-principal-name [USER_EMAIL]
  ```
- **Listar usuarios**:  
  ```bash
  az ad user list
  ```
- **Crear un grupo**:  
  ```bash
  az ad group create --display-name "[GROUP_NAME]" --mail-nickname "[NICKNAME]"
  ```
- **Agregar un usuario a un grupo**:  
  ```bash
  az ad group member add --group [GROUP_ID] --member-id [USER_ID]
  ```
- **Listar grupos**:  
  ```bash
  az ad group list
  ```

---

### **4. Gestión de Service Principals**
- **Crear un service principal**:  
  ```bash
  az ad sp create-for-rbac --name [APP_NAME]
  ```
- **Listar service principals**:  
  ```bash
  az ad sp list
  ```
- **Asignar un rol a un service principal**:  
  ```bash
  az role assignment create --assignee [APP_ID] --role [ROLE_NAME] --resource-group [RESOURCE_GROUP]
  ```
- **Eliminar un service principal**:  
  ```bash
  az ad sp delete --id [APP_ID]
  ```

---

### **5. Gestión de Managed Identities**
- **Crear una managed identity**:  
  ```bash
  az identity create --resource-group [RESOURCE_GROUP] --name [IDENTITY_NAME]
  ```
- **Asignar un rol a una managed identity**:  
  ```bash
  az role assignment create --assignee [IDENTITY_PRINCIPAL_ID] --role [ROLE_NAME] --resource-group [RESOURCE_GROUP]
  ```
- **Listar managed identities**:  
  ```bash
  az identity list --resource-group [RESOURCE_GROUP]
  ```

---

### **6. Roles Personalizados**
- **Crear un rol personalizado**:  
  Crear un archivo JSON con la definición del rol (`custom-role.json`):  
  ```json
  {
    "Name": "Custom Role",
    "Description": "Can read and list resources.",
    "Actions": [
      "Microsoft.Resources/subscriptions/resourceGroups/read",
      "Microsoft.Storage/storageAccounts/listKeys/action"
    ],
    "AssignableScopes": ["/subscriptions/[SUBSCRIPTION_ID]"]
  }
  ```
  Luego, crear el rol:  
  ```bash
  az role definition create --role-definition @custom-role.json
  ```
- **Actualizar un rol personalizado**:  
  ```bash
  az role definition update --role-definition @custom-role.json
  ```
- **Eliminar un rol personalizado**:  
  ```bash
  az role definition delete --name "Custom Role"
  ```

---

### **7. Buenas Prácticas**
1. **Principio de menor privilegio**: Asigna solo los permisos necesarios.
2. **Usa grupos**: Asigna roles a grupos en lugar de usuarios individuales.
3. **Usa managed identities**: Para aplicaciones y servicios, usa managed identities en lugar de credenciales explícitas.
4. **Habilita MFA**: Requiere autenticación multifactor para usuarios con acceso crítico.
5. **Audita permisos**: Usa **Azure Policy** y **Azure Security Center** para revisar y auditar permisos.

---

### **8. Comandos Útiles Adicionales**
- **Ver asignaciones de roles en un ámbito específico**:  
  ```bash
  az role assignment list --scope /subscriptions/[SUBSCRIPTION_ID]
  ```
- **Ver roles asignados a un usuario**:  
  ```bash
  az role assignment list --assignee [USER_PRINCIPAL_NAME]
  ```
- **Ver roles asignados a un grupo**:  
  ```bash
  az role assignment list --assignee [GROUP_ID]
  ```

---

### **9. Ejemplos Comunes**
- **Asignar acceso a un grupo de recursos**:  
  ```bash
  az role assignment create --assignee john@example.com --role "Contributor" --resource-group my-resource-group
  ```
- **Asignar acceso a una suscripción**:  
  ```bash
  az role assignment create --assignee john@example.com --role "Reader" --scope /subscriptions/[SUBSCRIPTION_ID]
  ```
- **Crear un rol personalizado para acceso limitado**:  
  Crear un archivo JSON (`custom-role.json`) y ejecutar:  
  ```bash
  az role definition create --role-definition @custom-role.json
  ```

---

### **10. Herramientas Adicionales**
- **Azure Policy**: Define y aplica reglas para recursos.
  ```bash
  az policy assignment create --name [POLICY_NAME] --policy [POLICY_DEFINITION_ID]
  ```
- **Azure Security Center**: Monitorea y mejora la seguridad de los recursos.
- **Azure AD Privileged Identity Management (PIM)**: Gestiona el acceso con privilegios just-in-time.

---

Este cheatsheet cubre los aspectos más importantes de IAM en Azure. Para más detalles, consulta la [documentación oficial de Azure IAM](https://docs.microsoft.com/en-us/azure/role-based-access-control/).
