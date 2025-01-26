

---

## **Azure Virtual Machines - Cheatsheet de Instancias**

### **1. Tipos de Instancias**
Azure ofrece una amplia variedad de tipos de instancias (tamaños de VM) optimizadas para diferentes cargas de trabajo:

- **General Purpose (B, Dsv3, Dv3, DSv2, Dv2)**: Equilibrio entre CPU, memoria y almacenamiento.
  - Ejemplo: `B1s`, `D2s_v3`
- **Compute Optimized (Fsv2)**: Alto rendimiento de CPU.
  - Ejemplo: `F2s_v2`
- **Memory Optimized (Esv3, Ev3, Mv2)**: Alta relación memoria/CPU.
  - Ejemplo: `E4s_v3`, `M64s`
- **Storage Optimized (Lsv2)**: Alto rendimiento de almacenamiento.
  - Ejemplo: `L8s_v2`
- **GPU Optimized (NC, ND, NV)**: Para cargas de trabajo de GPU.
  - Ejemplo: `NC6s_v3`, `NV12s_v3`
- **High Performance Computing (H)**: Para cargas de trabajo de HPC.
  - Ejemplo: `H16r`

---

### **2. Comandos Básicos de Azure CLI para Instancias**

#### **Crear una Instancia (VM)**
```bash
az vm create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_VM] \
  --image [IMAGEN] \
  --size [TAMAÑO_VM] \
  --admin-username [USUARIO_ADMIN] \
  --admin-password [CONTRASEÑA] \
  --location [REGION]
```
- Ejemplo:
  ```bash
  az vm create \
    --resource-group mi-grupo \
    --name mi-vm \
    --image UbuntuLTS \
    --size Standard_B1s \
    --admin-username azureuser \
    --admin-password MiContraseñaSegura123! \
    --location eastus
  ```

#### **Listar Instancias**
```bash
az vm list --resource-group [NOMBRE_GRUPO_RECURSOS]
```

#### **Detener una Instancia**
```bash
az vm stop --resource-group [NOMBRE_GRUPO_RECURSOS] --name [NOMBRE_VM]
```

#### **Iniciar una Instancia**
```bash
az vm start --resource-group [NOMBRE_GRUPO_RECURSOS] --name [NOMBRE_VM]
```

#### **Eliminar una Instancia**
```bash
az vm delete --resource-group [NOMBRE_GRUPO_RECURSOS] --name [NOMBRE_VM]
```

#### **Conectar a una Instancia (SSH)**
```bash
ssh [USUARIO_ADMIN]@[IP_PUBLICA]
```
- Nota: El usuario por defecto depende de la imagen:
  - Ubuntu: `azureuser`
  - CentOS: `azureuser`
  - Windows: Usa RDP en lugar de SSH.

#### **Cambiar Tamaño de la Instancia**
```bash
az vm resize \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_VM] \
  --size [NUEVO_TAMAÑO]
```

---

### **3. Gestión de Discos**

#### **Crear un Disco Administrado**
```bash
az disk create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_DISCO] \
  --size-gb [TAMAÑO] \
  --sku [TIPO_DISCO]
```
- Ejemplo:
  ```bash
  az disk create \
    --resource-group mi-grupo \
    --name mi-disco \
    --size-gb 100 \
    --sku Standard_LRS
  ```

#### **Adjuntar un Disco a una Instancia**
```bash
az vm disk attach \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --vm-name [NOMBRE_VM] \
  --disk [NOMBRE_DISCO]
```

#### **Desadjuntar un Disco**
```bash
az vm disk detach \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --vm-name [NOMBRE_VM] \
  --name [NOMBRE_DISCO]
```

---

### **4. Redes y Seguridad**

#### **Asignar una IP Pública**
```bash
az network public-ip create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_IP] \
  --allocation-method Static
```

#### **Crear un Grupo de Seguridad (NSG)**
```bash
az network nsg create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_NSG]
```

#### **Agregar Reglas al NSG**
```bash
az network nsg rule create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --nsg-name [NOMBRE_NSG] \
  --name [NOMBRE_REGLA] \
  --priority [PRIORIDAD] \
  --access Allow \
  --protocol [PROTOCOLO] \
  --direction Inbound \
  --source-address-prefixes [RANGO_IP] \
  --source-port-ranges "*" \
  --destination-address-prefixes "*" \
  --destination-port-ranges [PUERTO]
```
- Ejemplo:
  ```bash
  az network nsg rule create \
    --resource-group mi-grupo \
    --nsg-name mi-nsg \
    --name allow-ssh \
    --priority 100 \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --source-address-prefixes 0.0.0.0/0 \
    --source-port-ranges "*" \
    --destination-address-prefixes "*" \
    --destination-port-ranges 22
  ```

---

### **5. Imágenes y Snapshots**

#### **Crear una Imagen Personalizada**
```bash
az image create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_IMAGEN] \
  --source [NOMBRE_VM]
```

#### **Crear un Snapshot de un Disco**
```bash
az snapshot create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_SNAPSHOT] \
  --source [NOMBRE_DISCO]
```

#### **Restaurar un Snapshot**
```bash
az disk create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_DISCO] \
  --source [NOMBRE_SNAPSHOT]
```

---

### **6. Autoscaling y Conjuntos de Escalado**

#### **Crear un Conjunto de Escalado**
```bash
az vmss create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_CONJUNTO] \
  --image [IMAGEN] \
  --vm-sku [TAMAÑO_VM] \
  --instance-count [NUMERO_INSTANCIAS]
```

#### **Configurar Autoscaling**
```bash
az monitor autoscale create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --resource [NOMBRE_CONJUNTO] \
  --resource-type Microsoft.Compute/virtualMachineScaleSets \
  --min-count [MIN_INSTANCIAS] \
  --max-count [MAX_INSTANCIAS] \
  --count [CAPACIDAD_DESEADA]
```

---

### **7. Precios y Ahorro de Costos**

- **Instancias de Spot**: Hasta un 90% más baratas, pero pueden ser interrumpidas por Azure.
  ```bash
  az vm create \
    --resource-group [NOMBRE_GRUPO_RECURSOS] \
    --name [NOMBRE_VM] \
    --image [IMAGEN] \
    --size [TAMAÑO_VM] \
    --priority Spot \
    --max-price -1
  ```
- **Instancias Reservadas**: Descuentos por compromiso de uso a largo plazo (1 o 3 años).
- **Azure Hybrid Benefit**: Ahorros al usar licencias existentes de Windows Server o SQL Server.

---

### **8. Monitoreo y Logs**

#### **Ver Métricas de una Instancia**
```bash
az monitor metrics list \
  --resource [ID_RECURSO] \
  --metric "Percentage CPU"
```

#### **Ver Logs de una Instancia**
```bash
az monitor log-analytics query \
  --workspace [ID_WORKSPACE] \
  --analytics-query "AzureMetrics | where ResourceId == '[ID_RECURSO]'"
```

---

### **9. Buenas Prácticas**
- Usa **etiquetas (tags)** para organizar recursos.
- Aprovecha **imágenes personalizadas** para configuraciones repetibles.
- Usa **snapshots** para backups regulares.
- Considera **instancias Spot** para cargas de trabajo tolerantes a interrupciones.

---

Este cheatsheet cubre los aspectos más importantes de las instancias en Azure Virtual Machines. Para más detalles, consulta la [documentación oficial de Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/).
