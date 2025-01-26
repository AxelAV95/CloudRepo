

---

## **1. Google Cloud SQL**
- **Tipo**: Base de datos relacional totalmente administrada.
- **Motores compatibles**:
  - MySQL
  - PostgreSQL
  - SQL Server
- **Casos de uso**:
  - Aplicaciones web y móviles.
  - Sistemas de gestión de contenido (CMS).
  - Bases de datos transaccionales.
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas y restauración.
  - Alta disponibilidad con replicación.
  - Integración con otros servicios de GCP (Cloud Storage, BigQuery, etc.).
- **Ventajas**:
  - Sin gestión de infraestructura.
  - Compatible con aplicaciones existentes que usan MySQL, PostgreSQL o SQL Server.
- **Limitaciones**:
  - No es ideal para cargas de trabajo NoSQL o de gran escala.

---

## **2. Google Cloud Spanner**
- **Tipo**: Base de datos relacional distribuida y globalmente escalable.
- **Casos de uso**:
  - Aplicaciones que requieren consistencia global y alta disponibilidad.
  - Sistemas financieros, inventarios globales, etc.
- **Características**:
  - Escalabilidad horizontal ilimitada.
  - Consistencia transaccional fuerte.
  - Replicación global automática.
  - Soporte para SQL estándar.
- **Ventajas**:
  - Combina la escalabilidad de NoSQL con la consistencia de SQL.
  - Ideal para aplicaciones críticas que requieren baja latencia y alta disponibilidad.
- **Limitaciones**:
  - Costo más elevado en comparación con otras opciones.
  - Complejidad en la optimización de consultas.

---

## **3. Google Firestore**
- **Tipo**: Base de datos NoSQL orientada a documentos.
- **Casos de uso**:
  - Aplicaciones móviles y web en tiempo real.
  - Almacenamiento de datos semi-estructurados.
  - Sistemas de chat, juegos en línea, etc.
- **Características**:
  - Escalabilidad automática.
  - Sincronización de datos en tiempo real.
  - Consultas flexibles y indexación automática.
  - Integración con Firebase.
- **Ventajas**:
  - Fácil de usar para desarrolladores.
  - Ideal para aplicaciones que requieren baja latencia.
- **Limitaciones**:
  - No es adecuado para consultas complejas o transaccionales.

---

## **4. Google Bigtable**
- **Tipo**: Base de datos NoSQL de gran escala y baja latencia.
- **Casos de uso**:
  - Análisis de big data.
  - Almacenamiento de series temporales (IoT, métricas, etc.).
  - Aplicaciones que requieren alta escalabilidad.
- **Características**:
  - Escalabilidad horizontal.
  - Baja latencia para lecturas/escrituras.
  - Integración con Hadoop, Dataflow y BigQuery.
- **Ventajas**:
  - Ideal para cargas de trabajo masivas.
  - Alto rendimiento y bajo costo por TB.
- **Limitaciones**:
  - No es adecuado para consultas complejas o transaccionales.
  - Sin soporte para SQL.

---

## **5. Google BigQuery**
- **Tipo**: Almacén de datos analítico totalmente administrado.
- **Casos de uso**:
  - Análisis de grandes volúmenes de datos.
  - Business Intelligence (BI).
  - Machine Learning con datos masivos.
- **Características**:
  - Consultas SQL estándar.
  - Escalabilidad automática.
  - Integración con herramientas de visualización (Data Studio, Tableau, etc.).
  - Soporte para datos estructurados y semi-estructurados.
- **Ventajas**:
  - Ideal para análisis en tiempo real.
  - Sin gestión de infraestructura.
- **Limitaciones**:
  - No es una base de datos transaccional.
  - Costos pueden aumentar con consultas complejas.

---

## **6. Google Memorystore**
- **Tipo**: Servicio de caché en memoria totalmente administrado.
- **Motores compatibles**:
  - Redis
  - Memcached
- **Casos de uso**:
  - Almacenamiento en caché para aplicaciones web.
  - Sesiones de usuario.
  - Mejora del rendimiento de consultas frecuentes.
- **Características**:
  - Alta disponibilidad y replicación.
  - Escalabilidad automática.
  - Integración con otros servicios de GCP.
- **Ventajas**:
  - Baja latencia para aplicaciones sensibles al tiempo.
  - Sin gestión de infraestructura.
- **Limitaciones**:
  - No es un almacenamiento persistente.

---

## **7. Google Cloud Datastore**
- **Tipo**: Base de datos NoSQL orientada a documentos (legado, migrar a Firestore).
- **Casos de uso**:
  - Aplicaciones web y móviles.
  - Almacenamiento de datos semi-estructurados.
- **Características**:
  - Escalabilidad automática.
  - Consultas flexibles.
  - Integración con App Engine.
- **Ventajas**:
  - Fácil de usar para desarrolladores.
- **Limitaciones**:
  - En proceso de migración a Firestore.

---

## **Comparativa Rápida**

| **Servicio**       | **Tipo**         | **Escalabilidad** | **Consistencia** | **Casos de Uso**                  |
|---------------------|------------------|-------------------|------------------|-----------------------------------|
| **Cloud SQL**       | Relacional       | Vertical          | Fuerte           | Apps web, CMS, transacciones      |
| **Cloud Spanner**   | Relacional       | Horizontal        | Fuerte global    | Apps globales, financieras        |
| **Firestore**       | NoSQL (Documentos)| Horizontal        | Eventual         | Apps en tiempo real, móviles      |
| **Bigtable**        | NoSQL (Wide-column)| Horizontal        | Eventual         | Big data, IoT, series temporales  |
| **BigQuery**        | Analítico        | Horizontal        | Eventual         | BI, análisis masivo               |
| **Memorystore**     | Caché en memoria | Horizontal        | Depende          | Caché, sesiones                   |
| **Datastore**       | NoSQL (Documentos)| Horizontal        | Eventual         | Apps web (legado)                 |

---

## **Consejos de Elección**
1. **Relacional y pequeña escala**: Cloud SQL.
2. **Relacional y global**: Cloud Spanner.
3. **NoSQL y tiempo real**: Firestore.
4. **Big data y análisis**: BigQuery.
5. **Caché en memoria**: Memorystore.
6. **Series temporales o IoT**: Bigtable.

---

Este cheatsheet te ayudará a elegir la base de datos adecuada en función de tus necesidades en Google Cloud. ¡Espero que sea útil! 🚀
