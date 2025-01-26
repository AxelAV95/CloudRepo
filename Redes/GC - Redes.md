
---

## **Google Cloud Networking - Cheatsheet**

### **1. Conceptos Básicos**

- **VPC (Virtual Private Cloud)**: Red privada virtual en la nube.
- **Subredes**: Segmentos de IP dentro de una VPC.
- **Firewall Rules**: Reglas para controlar el tráfico entrante y saliente.
- **Cloud Load Balancing**: Balanceo de carga global o regional.
- **Cloud CDN**: Red de entrega de contenido.
- **Cloud DNS**: Servicio de DNS administrado.
- **Cloud Interconnect**: Conexión de alta disponibilidad entre GCP y tu red local.
- **Cloud VPN**: Conexión segura entre GCP y tu red local.

---

### **2. Comandos Básicos de `gcloud` para Redes**

#### **Crear una VPC**
```bash
gcloud compute networks create [NOMBRE_VPC] \
  --subnet-mode=[MODO_SUBRED]
```
- Modos de subred:
  - `auto`: Subredes automáticas en cada región.
  - `custom`: Subredes definidas manualmente.

#### **Crear una Subred**
```bash
gcloud compute networks subnets create [NOMBRE_SUBRED] \
  --network=[NOMBRE_VPC] \
  --region=[REGION] \
  --range=[RANGO_IP]
```
- Ejemplo:
  ```bash
  gcloud compute networks subnets create mi-subred \
    --network=mi-vpc \
    --region=us-central1 \
    --range=10.0.0.0/24
  ```

#### **Listar VPCs**
```bash
gcloud compute networks list
```

#### **Listar Subredes**
```bash
gcloud compute networks subnets list
```

#### **Eliminar una VPC**
```bash
gcloud compute networks delete [NOMBRE_VPC]
```

#### **Eliminar una Subred**
```bash
gcloud compute networks subnets delete [NOMBRE_SUBRED] \
  --region=[REGION]
```

---

### **3. Firewall Rules**

#### **Crear una Regla de Firewall**
```bash
gcloud compute firewall-rules create [NOMBRE_REGLA] \
  --network=[NOMBRE_VPC] \
  --allow=[PROTOCOLO:PUERTO] \
  --source-ranges=[RANGOS_IP]
```
- Ejemplo:
  ```bash
  gcloud compute firewall-rules allow-http \
    --network=mi-vpc \
    --allow=tcp:80 \
    --source-ranges=0.0.0.0/0
  ```

#### **Listar Reglas de Firewall**
```bash
gcloud compute firewall-rules list
```

#### **Eliminar una Regla de Firewall**
```bash
gcloud compute firewall-rules delete [NOMBRE_REGLA]
```

---

### **4. Cloud Load Balancing**

#### **Crear un Balanceador de Carga HTTP(S)**
```bash
gcloud compute url-maps create [NOMBRE_MAPA_URL] \
  --default-service=[NOMBRE_SERVICIO_BACKEND]
gcloud compute target-http-proxies create [NOMBRE_PROXY] \
  --url-map=[NOMBRE_MAPA_URL]
gcloud compute forwarding-rules create [NOMBRE_REGLA] \
  --target-http-proxy=[NOMBRE_PROXY] \
  --ports=[PUERTO]
```

#### **Crear un Balanceador de Carga TCP/UDP**
```bash
gcloud compute forwarding-rules create [NOMBRE_REGLA] \
  --load-balancing-scheme=EXTERNAL \
  --ports=[PUERTO] \
  --target-pool=[NOMBRE_POOL]
```

---

### **5. Cloud DNS**

#### **Crear una Zona DNS**
```bash
gcloud dns managed-zones create [NOMBRE_ZONA] \
  --dns-name=[DOMINIO] \
  --description="Descripción de la zona"
```

#### **Agregar Registros DNS**
```bash
gcloud dns record-sets transaction start --zone=[NOMBRE_ZONA]
gcloud dns record-sets transaction add [IP] \
  --name=[NOMBRE_DOMINIO] \
  --ttl=[TTL] \
  --type=A \
  --zone=[NOMBRE_ZONA]
gcloud dns record-sets transaction execute --zone=[NOMBRE_ZONA]
```

#### **Listar Zonas DNS**
```bash
gcloud dns managed-zones list
```

---

### **6. Cloud Interconnect**

#### **Crear una Interconexión Dedicada**
```bash
gcloud compute interconnects create [NOMBRE_INTERCONEXION] \
  --location=[LOCATION] \
  --admin-enabled \
  --customer-name=[NOMBRE_CLIENTE] \
  --link-type=[TIPO_ENLACE]
```

#### **Crear un VPN Gateway**
```bash
gcloud compute target-vpn-gateways create [NOMBRE_GATEWAY] \
  --network=[NOMBRE_VPC] \
  --region=[REGION]
```

---

### **7. Cloud VPN**

#### **Crear un Túnel VPN**
```bash
gcloud compute vpn-tunnels create [NOMBRE_TUNEL] \
  --peer-address=[IP_PEER] \
  --shared-secret=[SECRETO] \
  --target-vpn-gateway=[NOMBRE_GATEWAY] \
  --local-traffic-selector=[RANGO_IP_LOCAL] \
  --remote-traffic-selector=[RANGO_IP_REMOTO] \
  --region=[REGION]
```

#### **Listar Túneles VPN**
```bash
gcloud compute vpn-tunnels list
```

---

### **8. Cloud CDN**

#### **Habilitar CDN en un Balanceador de Carga**
```bash
gcloud compute backend-services update [NOMBRE_SERVICIO_BACKEND] \
  --enable-cdn
```

#### **Invalidar Caché CDN**
```bash
gcloud compute url-maps invalidate-cdn-cache [NOMBRE_MAPA_URL] \
  --path="/*"
```

---

### **9. Buenas Prácticas**

- Usa **VPC compartidas** para proyectos relacionados.
- Aplica **reglas de firewall mínimas** (principio de menor privilegio).
- Usa **Cloud Load Balancing** para alta disponibilidad y escalabilidad.
- Implementa **Cloud CDN** para mejorar el rendimiento de aplicaciones web.
- Usa **Cloud DNS** para gestionar dominios de manera centralizada.
- Considera **Cloud Interconnect** o **Cloud VPN** para conexiones híbridas.

---

Este cheatsheet cubre los aspectos más importantes de redes en Google Cloud. Para más detalles, consulta la [documentación oficial de Google Cloud Networking](https://cloud.google.com/network).
