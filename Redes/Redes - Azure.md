
---

## **Azure Networking - Cheatsheet**

### **1. Conceptos Básicos**

- **Virtual Network (VNet)**: Red privada virtual en la nube.
- **Subnets**: Segmentos de IP dentro de una VNet.
- **Network Security Groups (NSG)**: Reglas de firewall a nivel de subred o interfaz de red.
- **Load Balancer**: Balanceo de carga para aplicaciones.
- **Application Gateway**: Balanceador de carga de capa 7 (HTTP/HTTPS).
- **Azure DNS**: Servicio de DNS administrado.
- **ExpressRoute**: Conexión dedicada entre Azure y tu red local.
- **VPN Gateway**: Conexión segura entre Azure y tu red local.
- **Azure Firewall**: Servicio de firewall administrado.
- **Content Delivery Network (CDN)**: Red de entrega de contenido.

---

### **2. Comandos Básicos de Azure CLI para Redes**

#### **Crear una Virtual Network (VNet)**
```bash
az network vnet create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_VNET] \
  --address-prefix [RANGO_IP] \
  --subnet-name [NOMBRE_SUBRED] \
  --subnet-prefix [RANGO_IP_SUBRED]
```
- Ejemplo:
  ```bash
  az network vnet create \
    --resource-group mi-grupo \
    --name mi-vnet \
    --address-prefix 10.0.0.0/16 \
    --subnet-name mi-subred \
    --subnet-prefix 10.0.1.0/24
  ```

#### **Crear una Subred**
```bash
az network vnet subnet create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --vnet-name [NOMBRE_VNET] \
  --name [NOMBRE_SUBRED] \
  --address-prefix [RANGO_IP]
```

#### **Listar VNets**
```bash
az network vnet list --resource-group [NOMBRE_GRUPO_RECURSOS]
```

#### **Listar Subredes**
```bash
az network vnet subnet list \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --vnet-name [NOMBRE_VNET]
```

#### **Eliminar una VNet**
```bash
az network vnet delete \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_VNET]
```

#### **Eliminar una Subred**
```bash
az network vnet subnet delete \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --vnet-name [NOMBRE_VNET] \
  --name [NOMBRE_SUBRED]
```

---

### **3. Network Security Groups (NSG)**

#### **Crear un NSG**
```bash
az network nsg create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_NSG]
```

#### **Agregar Reglas a un NSG**
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

#### **Listar Reglas de NSG**
```bash
az network nsg rule list \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --nsg-name [NOMBRE_NSG]
```

---

### **4. Load Balancer y Application Gateway**

#### **Crear un Load Balancer**
```bash
az network lb create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_LB] \
  --sku Basic \
  --public-ip-address [NOMBRE_IP_PUBLICA]
```

#### **Crear un Application Gateway**
```bash
az network application-gateway create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_AG] \
  --capacity 2 \
  --sku Standard_v2 \
  --public-ip-address [NOMBRE_IP_PUBLICA] \
  --vnet-name [NOMBRE_VNET] \
  --subnet [NOMBRE_SUBRED]
```

---

### **5. Azure DNS**

#### **Crear una Zona DNS**
```bash
az network dns zone create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [DOMINIO]
```

#### **Agregar Registros DNS**
```bash
az network dns record-set a add-record \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --zone-name [DOMINIO] \
  --record-set-name [NOMBRE_REGISTRO] \
  --ipv4-address [IP]
```

---

### **6. ExpressRoute y VPN Gateway**

#### **Crear una Conexión ExpressRoute**
```bash
az network express-route create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_EXPRESSROUTE] \
  --bandwidth [ANCHO_BANDA] \
  --peering-location [LOCATION] \
  --provider [PROVEEDOR]
```

#### **Crear un VPN Gateway**
```bash
az network vnet-gateway create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_GATEWAY] \
  --vnet [NOMBRE_VNET] \
  --gateway-type Vpn \
  --sku VpnGw1 \
  --public-ip-address [NOMBRE_IP_PUBLICA]
```

---

### **7. Azure Firewall**

#### **Crear un Azure Firewall**
```bash
az network firewall create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_FIREWALL] \
  --location [REGION]
```

#### **Agregar Reglas al Firewall**
```bash
az network firewall network-rule create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --firewall-name [NOMBRE_FIREWALL] \
  --collection-name [NOMBRE_COLECCION] \
  --name [NOMBRE_REGLA] \
  --protocols [PROTOCOLO] \
  --source-addresses [RANGO_IP] \
  --destination-addresses [RANGO_IP] \
  --destination-ports [PUERTO]
```

---

### **8. Content Delivery Network (CDN)**

#### **Crear un Perfil CDN**
```bash
az cdn profile create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --name [NOMBRE_PERFIL] \
  --sku Standard_Microsoft
```

#### **Agregar un Endpoint CDN**
```bash
az cdn endpoint create \
  --resource-group [NOMBRE_GRUPO_RECURSOS] \
  --profile-name [NOMBRE_PERFIL] \
  --name [NOMBRE_ENDPOINT] \
  --origin [URL_ORIGEN]
```

---

### **9. Buenas Prácticas**

- Usa **VNet peering** para conectar VNets.
- Aplica **reglas de NSG mínimas** (principio de menor privilegio).
- Usa **Load Balancer** o **Application Gateway** para alta disponibilidad y escalabilidad.
- Implementa **Azure DNS** para gestionar dominios de manera centralizada.
- Considera **ExpressRoute** o **VPN Gateway** para conexiones híbridas.
- Usa **Azure Firewall** para proteger tus redes.

---

Este cheatsheet cubre los aspectos más importantes de redes en Azure. Para más detalles, consulta la [documentación oficial de Azure Networking](https://learn.microsoft.com/en-us/azure/networking/).
