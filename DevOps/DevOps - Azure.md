

---

## **DevOps en Azure Cheatsheet**

### **Conceptos Básicos**
- **DevOps**: Cultura y prácticas que combinan desarrollo de software (Dev) y operaciones de TI (Ops) para acelerar la entrega de aplicaciones.
- **CI/CD**: Integración Continua (CI) y Entrega Continua (CD) para automatizar la construcción, prueba y despliegue de aplicaciones.
- **Infraestructura como Código (IaC)**: Gestión de infraestructura mediante código en lugar de procesos manuales.
- **Monitoreo y Observabilidad**: Supervisión del rendimiento y salud de las aplicaciones en tiempo real.

---

### **Servicios y Herramientas de Azure para DevOps**

#### **CI/CD**
1. **Azure DevOps Pipelines**:
   - Servicio de CI/CD para automatizar pipelines de lanzamiento.
   - Ejemplo de configuración en `azure-pipelines.yml`:
     ```yaml
     trigger:
       branches:
         include:
           - main

     pool:
       vmImage: 'ubuntu-latest'

     steps:
       - script: echo "Building the application..."
         displayName: 'Build'
       - script: echo "Running tests..."
         displayName: 'Test'
       - script: echo "Deploying the application..."
         displayName: 'Deploy'
     ```

2. **Azure Repos**:
   - Repositorios de código Git gestionados en Azure.
   - Ejemplo de clonación de repositorio:
     ```bash
     git clone https://dev.azure.com/organization/project/_git/repo
     ```

3. **Azure Artifacts**:
   - Almacenamiento y gestión de paquetes (NuGet, npm, Maven, etc.).
   - Ejemplo de subida de paquete:
     ```bash
     nuget push my-package.1.0.0.nupkg -Source https://pkgs.dev.azure.com/organization/project/_packaging/feed/nuget/v3/index.json
     ```

#### **Gestión de Infraestructura**
1. **Azure Resource Manager (ARM)**:
   - Herramienta de IaC para gestionar recursos de Azure.
   - Ejemplo de configuración en JSON:
     ```json
     {
       "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "resources": [
         {
           "type": "Microsoft.Compute/virtualMachines",
           "apiVersion": "2021-04-01",
           "name": "myVM",
           "location": "[resourceGroup().location]",
           "properties": {
             "hardwareProfile": {
               "vmSize": "Standard_D2s_v3"
             },
             "storageProfile": {
               "imageReference": {
                 "publisher": "Canonical",
                 "offer": "UbuntuServer",
                 "sku": "18.04-LTS",
                 "version": "latest"
               }
             }
           }
         }
       ]
     }
     ```

2. **Terraform con Azure**:
   - Herramienta de IaC para gestionar recursos en múltiples proveedores.
   - Ejemplo de configuración en Terraform:
     ```hcl
     provider "azurerm" {
       features {}
     }

     resource "azurerm_resource_group" "example" {
       name     = "example-resources"
       location = "West Europe"
     }

     resource "azurerm_virtual_network" "example" {
       name                = "example-network"
       address_space       = ["10.0.0.0/16"]
       location            = azurerm_resource_group.example.location
       resource_group_name = azurerm_resource_group.example.name
     }
     ```

3. **Azure Kubernetes Service (AKS)**:
   - Servicio gestionado de Kubernetes para orquestar contenedores.
   - Ejemplo de despliegue en AKS:
     ```bash
     az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 3 --generate-ssh-keys
     kubectl create deployment my-app --image=my-repo/my-image:tag
     kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080
     ```

#### **Monitoreo y Observabilidad**
1. **Azure Monitor**:
   - Supervisión de métricas, logs y alertas.
   - Ejemplo de creación de una alerta:
     ```bash
     az monitor metrics alert create --name "CPUAlert" --resource-group myResourceGroup --scopes /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM --condition "avg Percentage CPU > 80" --action email my-email@example.com
     ```

2. **Application Insights**:
   - Rastreo de solicitudes y análisis de rendimiento.
   - Ejemplo de configuración en `ApplicationInsights.config`:
     ```xml
     <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
       <InstrumentationKey>your-instrumentation-key</InstrumentationKey>
     </ApplicationInsights>
     ```

#### **Seguridad**
1. **Azure Active Directory (AAD)**:
   - Gestión de identidades y acceso.
   - Ejemplo de asignación de roles:
     ```bash
     az role assignment create --assignee user@example.com --role Contributor --resource-group myResourceGroup
     ```

2. **Azure Security Center**:
   - Supervisión de la seguridad y detección de amenazas.

---

### **Mejores Prácticas**
1. **Automatización**: Automatiza todo lo posible, desde pruebas hasta despliegues.
2. **Infraestructura como Código**: Usa herramientas como ARM o Terraform.
3. **Monitoreo Continuo**: Implementa alertas y dashboards para supervisar el rendimiento.
4. **Seguridad Integrada**: Asegura cada etapa del pipeline CI/CD.
5. **Escalabilidad**: Diseña aplicaciones y pipelines para ser escalables.

---

### **Ejemplo de Pipeline CI/CD en Azure**
1. **Código en Azure Repos**:
   - Los desarrolladores envían código a un repositorio de Azure Repos.

2. **Azure DevOps Pipelines**:
   - Azure Pipelines detecta cambios y ejecuta pruebas y construcción de imágenes Docker.
   - Configuración en `azure-pipelines.yml`:
     ```yaml
     trigger:
       branches:
         include:
           - main

     pool:
       vmImage: 'ubuntu-latest'

     steps:
       - script: echo "Building the application..."
         displayName: 'Build'
       - script: echo "Running tests..."
         displayName: 'Test'
       - script: echo "Deploying the application..."
         displayName: 'Deploy'
     ```

3. **Azure Artifacts**:
   - Los paquetes se almacenan en Azure Artifacts.
   - Ejemplo de subida de paquete:
     ```bash
     nuget push my-package.1.0.0.nupkg -Source https://pkgs.dev.azure.com/organization/project/_packaging/feed/nuget/v3/index.json
     ```

4. **Despliegue en AKS**:
   - Las imágenes se despliegan en Azure Kubernetes Service.
   - Ejemplo de despliegue:
     ```bash
     kubectl set image deployment/my-app my-app=my-repo/my-image:tag
     ```

5. **Monitoreo**:
   - Azure Monitor y Application Insights supervisan el rendimiento y la salud de la aplicación.

---

### **Recursos Adicionales**
- [Documentación Oficial de Azure DevOps](https://docs.microsoft.com/azure/devops/)
- [Documentación Oficial de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/)
- [Documentación Oficial de Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/)
- [Precios de Azure DevOps](https://azure.microsoft.com/pricing/details/devops/azure-devops-services/)

---

Esta cheatsheet te proporciona una referencia rápida y completa para implementar **DevOps en Azure**. ¡Espero que te sea útil! 😊
