
---

## **Google Compute Engine (GCE) - Cheatsheet de Instancias**

### **1. Tipos de Instancias**
Google Cloud ofrece varios tipos de instancias optimizadas para diferentes cargas de trabajo:

- **General Purpose (E2, N2, N2D, N1)**: Equilibrio entre CPU y memoria.
  - Ejemplo: `e2-medium`, `n1-standard-1`
- **Compute Optimized (C2)**: Alto rendimiento de CPU.
  - Ejemplo: `c2-standard-4`
- **Memory Optimized (M2, M1)**: Alta relación memoria/CPU.
  - Ejemplo: `m2-ultramem-160`
- **Accelerator Optimized (A2)**: Para cargas de trabajo de GPU.
  - Ejemplo: `a2-highgpu-1g`
- **Storage Optimized**: Alto rendimiento de disco.
  - Ejemplo: `n2-highdisk-8`

---

### **2. Comandos Básicos de `gcloud` para Instancias**

#### **Crear una Instancia**
```bash
gcloud compute instances create [NOMBRE_INSTANCIA] \
  --machine-type=[TIPO_MÁQUINA] \
  --zone=[ZONA] \
  --image-family=[FAMILIA_IMAGEN] \
  --image-project=[PROYECTO_IMAGEN] \
  --tags=[ETIQUETAS] \
  --boot-disk-size=[TAMAÑO_DISCO] \
  --boot-disk-type=[TIPO_DISCO]
```
- Ejemplo:
  ```bash
  gcloud compute instances create mi-instancia \
    --machine-type=e2-medium \
    --zone=us-central1-a \
    --image-family=debian-10 \
    --image-project=debian-cloud
  ```

#### **Listar Instancias**
```bash
gcloud compute instances list
```

#### **Detener una Instancia**
```bash
gcloud compute instances stop [NOMBRE_INSTANCIA] --zone=[ZONA]
```

#### **Iniciar una Instancia**
```bash
gcloud compute instances start [NOMBRE_INSTANCIA] --zone=[ZONA]
```

#### **Eliminar una Instancia**
```bash
gcloud compute instances delete [NOMBRE_INSTANCIA] --zone=[ZONA]
```

#### **Conectar a una Instancia (SSH)**
```bash
gcloud compute ssh [NOMBRE_INSTANCIA] --zone=[ZONA]
```

#### **Cambiar Tipo de Máquina**
```bash
gcloud compute instances set-machine-type [NOMBRE_INSTANCIA] \
  --machine-type=[NUEVO_TIPO] \
  --zone=[ZONA]
```

---

### **3. Gestión de Discos**

#### **Crear un Disco Persistente**
```bash
gcloud compute disks create [NOMBRE_DISCO] \
  --size=[TAMAÑO] \
  --zone=[ZONA] \
  --type=[TIPO_DISCO]
```

#### **Adjuntar un Disco a una Instancia**
```bash
gcloud compute instances attach-disk [NOMBRE_INSTANCIA] \
  --disk=[NOMBRE_DISCO] \
  --zone=[ZONA]
```

#### **Desadjuntar un Disco**
```bash
gcloud compute instances detach-disk [NOMBRE_INSTANCIA] \
  --disk=[NOMBRE_DISCO] \
  --zone=[ZONA]
```

---

### **4. Redes y Firewalls**

#### **Asignar una IP Estática**
```bash
gcloud compute addresses create [NOMBRE_IP] --region=[REGION]
gcloud compute instances add-access-config [NOMBRE_INSTANCIA] \
  --address=[IP_ESTÁTICA] \
  --zone=[ZONA]
```

#### **Crear una Regla de Firewall**
```bash
gcloud compute firewall-rules create [NOMBRE_REGLA] \
  --allow=[PROTOCOLO:PUERTO] \
  --source-ranges=[RANGOS_IP]
```
- Ejemplo:
  ```bash
  gcloud compute firewall-rules allow-http \
    --allow=tcp:80 \
    --source-ranges=0.0.0.0/0
  ```

---

### **5. Imágenes y Snapshots**

#### **Crear una Imagen Personalizada**
```bash
gcloud compute images create [NOMBRE_IMAGEN] \
  --source-disk=[NOMBRE_DISCO] \
  --source-disk-zone=[ZONA]
```

#### **Crear un Snapshot de un Disco**
```bash
gcloud compute disks snapshot [NOMBRE_DISCO] \
  --snapshot-names=[NOMBRE_SNAPSHOT] \
  --zone=[ZONA]
```

#### **Restaurar un Snapshot**
```bash
gcloud compute disks create [NOMBRE_DISCO] \
  --source-snapshot=[NOMBRE_SNAPSHOT] \
  --zone=[ZONA]
```

---

### **6. Autoscaling y Grupos de Instancias**

#### **Crear un Grupo de Instancias**
```bash
gcloud compute instance-groups managed create [NOMBRE_GRUPO] \
  --template=[NOMBRE_TEMPLATE] \
  --size=[TAMAÑO] \
  --zone=[ZONA]
```

#### **Configurar Autoscaling**
```bash
gcloud compute instance-groups managed set-autoscaling [NOMBRE_GRUPO] \
  --max-num-replicas=[MAX_REPLICAS] \
  --min-num-replicas=[MIN_REPLICAS] \
  --target-cpu-utilization=[UTILIZACIÓN_CPU] \
  --zone=[ZONA]
```

---

### **7. Precios y Ahorro de Costos**

- **Instancias de Preemptibles**: Hasta un 80% más baratas, pero pueden ser detenidas por Google en cualquier momento.
  ```bash
  gcloud compute instances create [NOMBRE_INSTANCIA] \
    --preemptible \
    --machine-type=[TIPO_MÁQUINA] \
    --zone=[ZONA]
  ```
- **Commited Use Discounts**: Descuentos por compromiso de uso a largo plazo (1 o 3 años).
- **Sustained Use Discounts**: Descuentos automáticos por uso continuo.

---

### **8. Monitoreo y Logs**

#### **Ver Métricas de una Instancia**
```bash
gcloud compute instances describe [NOMBRE_INSTANCIA] --zone=[ZONA]
```

#### **Ver Logs de una Instancia**
```bash
gcloud logging read "resource.type=gce_instance" --limit=10
```

---

### **9. Buenas Prácticas**
- Usa **etiquetas** para organizar instancias.
- Aprovecha **imágenes personalizadas** para configuraciones repetibles.
- Usa **snapshots** para backups regulares.
- Considera **preemptibles** para cargas de trabajo tolerantes a fallos.

---

Este cheatsheet cubre los aspectos más importantes de las instancias en Google Cloud. Para más detalles, consulta la [documentación oficial de Google Cloud](https://cloud.google.com/compute/docs).
