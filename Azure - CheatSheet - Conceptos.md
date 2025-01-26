
---

## **Conceptos Básicos de Azure**
1. **Regiones y Zonas de Disponibilidad**:
   - **Regiones**: Ubicaciones geográficas donde Azure tiene centros de datos.
   - **Zonas de Disponibilidad**: Centros de datos separados físicamente dentro de una región para alta disponibilidad.

2. **Resource Groups (Grupos de Recursos)**:
   - Contenedores lógicos para agrupar recursos relacionados (máquinas virtuales, redes, bases de datos, etc.).
   - Los recursos pueden pertenecer solo a un grupo de recursos.

3. **Suscripciones**:
   - Unidades de facturación y gestión en Azure.
   - Puedes tener múltiples suscripciones bajo un solo tenant de Azure Active Directory (AAD).

4. **Azure Resource Manager (ARM)**:
   - Servicio de gestión y despliegue de recursos en Azure.
   - Usa plantillas JSON para definir infraestructura como código (IaC).

---

## **Servicios Principales de Azure**
### **Compute**
1. **Azure Virtual Machines (VMs)**:
   - Máquinas virtuales escalables en la nube.
   - Soporta Windows, Linux y otros sistemas operativos.

2. **Azure Kubernetes Service (AKS)**:
   - Servicio gestionado para orquestar contenedores usando Kubernetes.

3. **Azure App Service**:
   - Plataforma para hospedar aplicaciones web, API y móviles.
   - Soporta múltiples lenguajes (Node.js, .NET, Python, etc.).

4. **Azure Functions**:
   - Computación sin servidor (serverless) para ejecutar código en respuesta a eventos.

### **Storage**
1. **Azure Blob Storage**:
   - Almacenamiento de objetos para archivos grandes, como imágenes, videos y backups.

2. **Azure Files**:
   - Almacenamiento de archivos accesible mediante SMB (Server Message Block).

3. **Azure Disk Storage**:
   - Discos gestionados para VMs (SSD y HDD).

4. **Azure Table Storage**:
   - Almacenamiento NoSQL para datos semi-estructurados.

### **Networking**
1. **Virtual Network (VNet)**:
   - Redes privadas en la nube para conectar recursos de Azure.

2. **Azure Load Balancer**:
   - Distribuye el tráfico entrante entre múltiples recursos.

3. **Azure Application Gateway**:
   - Balanceador de carga para aplicaciones web con funcionalidades de seguridad.

4. **Azure VPN Gateway**:
   - Conexiones seguras entre redes locales y Azure.

5. **Azure CDN**:
   - Red de entrega de contenido para distribuir contenido estático a usuarios finales.

### **Databases**
1. **Azure SQL Database**:
   - Base de datos SQL gestionada en la nube.

2. **Azure Cosmos DB**:
   - Base de datos NoSQL multi-modelo y distribuida globalmente.

3. **Azure Database for MySQL/PostgreSQL**:
   - Bases de datos relacionales gestionadas para MySQL y PostgreSQL.

### **Seguridad**
1. **Azure Active Directory (AAD)**:
   - Servicio de identidad y acceso gestionado.

2. **Azure Key Vault**:
   - Almacenamiento seguro de secretos, claves y certificados.

3. **Azure Security Center**:
   - Herramienta de gestión de seguridad y protección contra amenazas.

4. **Azure Firewall**:
   - Firewall gestionado para proteger redes virtuales.

### **Monitoring y Management**
1. **Azure Monitor**:
   - Supervisión de aplicaciones y infraestructura.

2. **Azure Log Analytics**:
   - Análisis de registros y telemetría.

3. **Azure Automation**:
   - Automatización de tareas y configuración de recursos.

4. **Azure Policy**:
   - Aplicación de reglas y compliance en recursos.

---

## **Azure CLI Cheatsheet**
### Instalación
- **Windows**: Descargar el instalador desde [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
- **Linux**: 
  ```bash
  curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
  ```
- **macOS**:
  ```bash
  brew install azure-cli
  ```

### Comandos Básicos
1. **Iniciar sesión**:
   ```bash
   az login
   ```

2. **Listar suscripciones**:
   ```bash
   az account list --output table
   ```

3. **Establecer suscripción**:
   ```bash
   az account set --subscription <SubscriptionID>
   ```

4. **Crear un grupo de recursos**:
   ```bash
   az group create --name <ResourceGroupName> --location <Region>
   ```

5. **Listar máquinas virtuales**:
   ```bash
   az vm list --output table
   ```

6. **Crear una VM**:
   ```bash
   az vm create --resource-group <ResourceGroupName> --name <VMName> --image UbuntuLTS --admin-username <Username> --generate-ssh-keys
   ```

7. **Eliminar un recurso**:
   ```bash
   az resource delete --resource-group <ResourceGroupName> --name <ResourceName> --resource-type <ResourceType>
   ```

8. **Ver registros de actividad**:
   ```bash
   az monitor activity-log list --output table
   ```

---

## **Buenas Prácticas en Azure**
1. **Usar etiquetas (Tags)**:
   - Aplica etiquetas a los recursos para organizarlos y facilitar la gestión de costos.

2. **Implementar alta disponibilidad**:
   - Usa zonas de disponibilidad y conjuntos de disponibilidad para VMs críticas.

3. **Automatizar con ARM Templates o Terraform**:
   - Define tu infraestructura como código para facilitar despliegues repetibles.

4. **Monitorear y optimizar costos**:
   - Usa Azure Cost Management para analizar y reducir gastos.

5. **Aplicar seguridad desde el diseño**:
   - Usa Azure Security Center y Azure Policy para cumplir con estándares de seguridad.

6. **Backup y recuperación ante desastres**:
   - Configura Azure Backup y Azure Site Recovery para proteger tus datos.

---

## **Enlaces Útiles**
- [Documentación Oficial de Azure](https://docs.microsoft.com/azure/)
- [Azure CLI Documentation](https://docs.microsoft.com/cli/azure/)
- [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)
- [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/)

---

Este cheatsheet es un punto de partida para trabajar con Azure. ¡A medida que profundices en el uso de Azure, podrás explorar más servicios y funcionalidades avanzadas!
