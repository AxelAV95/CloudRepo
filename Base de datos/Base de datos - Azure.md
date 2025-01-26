

---

## **1. Azure SQL Database**
- **Tipo**: Base de datos relacional totalmente administrada.
- **Motores compatibles**:
  - SQL Server
- **Casos de uso**:
  - Aplicaciones web y móviles.
  - Sistemas de gestión de contenido (CMS).
  - Bases de datos transaccionales.
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas y restauración.
  - Alta disponibilidad con replicación.
  - Integración con otros servicios de Azure (Azure Functions, Logic Apps, etc.).
- **Ventajas**:
  - Sin gestión de infraestructura.
  - Compatible con aplicaciones existentes que usan SQL Server.
- **Limitaciones**:
  - No es ideal para cargas de trabajo NoSQL o de gran escala.

---

## **2. Azure Cosmos DB**
- **Tipo**: Base de datos NoSQL multi-modelo.
- **Modelos compatibles**:
  - Documentos (JSON)
  - Clave-valor
  - Grafos
  - Columnas anchas
  - Tablas
- **Casos de uso**:
  - Aplicaciones globales que requieren baja latencia y alta escalabilidad.
  - Sistemas de IoT, juegos, comercio electrónico, etc.
- **Características**:
  - Escalabilidad global automática.
  - Consistencia ajustable (5 niveles).
  - Latencia de milisegundos en todo el mundo.
  - SLA del 99.999% de disponibilidad.
- **Ventajas**:
  - Ideal para aplicaciones globales y distribuidas.
  - Soporte para múltiples modelos de datos.
- **Limitaciones**:
  - Costo más elevado en comparación con otras opciones.

---

## **3. Azure Database for MySQL**
- **Tipo**: Base de datos relacional totalmente administrada.
- **Motores compatibles**:
  - MySQL
- **Casos de uso**:
  - Aplicaciones web y móviles.
  - Sistemas de gestión de contenido (CMS).
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas y restauración.
  - Alta disponibilidad con replicación.
- **Ventajas**:
  - Sin gestión de infraestructura.
  - Compatible con aplicaciones existentes que usan MySQL.
- **Limitaciones**:
  - No es ideal para cargas de trabajo NoSQL o de gran escala.

---

## **4. Azure Database for PostgreSQL**
- **Tipo**: Base de datos relacional totalmente administrada.
- **Motores compatibles**:
  - PostgreSQL
- **Casos de uso**:
  - Aplicaciones web y móviles.
  - Sistemas de gestión de contenido (CMS).
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas y restauración.
  - Alta disponibilidad con replicación.
- **Ventajas**:
  - Sin gestión de infraestructura.
  - Compatible con aplicaciones existentes que usan PostgreSQL.
- **Limitaciones**:
  - No es ideal para cargas de trabajo NoSQL o de gran escala.

---

## **5. Azure Synapse Analytics**
- **Tipo**: Almacén de datos analítico.
- **Casos de uso**:
  - Análisis de grandes volúmenes de datos.
  - Business Intelligence (BI).
  - Machine Learning con datos masivos.
- **Características**:
  - Consultas SQL estándar.
  - Escalabilidad automática.
  - Integración con herramientas de visualización (Power BI, Tableau, etc.).
  - Soporte para datos estructurados y semi-estructurados.
- **Ventajas**:
  - Ideal para análisis en tiempo real.
  - Sin gestión de infraestructura.
- **Limitaciones**:
  - No es una base de datos transaccional.
  - Costos pueden aumentar con consultas complejas.

---

## **6. Azure Cache for Redis**
- **Tipo**: Servicio de caché en memoria.
- **Casos de uso**:
  - Almacenamiento en caché para aplicaciones web.
  - Sesiones de usuario.
  - Mejora del rendimiento de consultas frecuentes.
- **Características**:
  - Alta disponibilidad y replicación.
  - Escalabilidad automática.
  - Integración con otros servicios de Azure.
- **Ventajas**:
  - Baja latencia para aplicaciones sensibles al tiempo.
  - Sin gestión de infraestructura.
- **Limitaciones**:
  - No es un almacenamiento persistente.

---

## **7. Azure Table Storage**
- **Tipo**: Base de datos NoSQL clave-valor.
- **Casos de uso**:
  - Almacenamiento de datos semi-estructurados.
  - Sistemas de registro, métricas, etc.
- **Características**:
  - Escalabilidad automática.
  - Bajo costo por GB.
  - Integración con otros servicios de Azure.
- **Ventajas**:
  - Ideal para cargas de trabajo simples y escalables.
  - Sin gestión de infraestructura.
- **Limitaciones**:
  - No es adecuado para consultas complejas o transaccionales.

---

## **8. Azure Database for MariaDB**
- **Tipo**: Base de datos relacional totalmente administrada.
- **Motores compatibles**:
  - MariaDB
- **Casos de uso**:
  - Aplicaciones web y móviles.
  - Sistemas de gestión de contenido (CMS).
- **Características**:
  - Escalabilidad automática.
  - Copias de seguridad automáticas y restauración.
  - Alta disponibilidad con replicación.
- **Ventajas**:
  - Sin gestión de infraestructura.
  - Compatible con aplicaciones existentes que usan MariaDB.
- **Limitaciones**:
  - No es ideal para cargas de trabajo NoSQL o de gran escala.

---

## **Comparativa Rápida**

| **Servicio**               | **Tipo**         | **Escalabilidad** | **Consistencia** | **Casos de Uso**                  |
|-----------------------------|------------------|-------------------|------------------|-----------------------------------|
| **Azure SQL Database**     | Relacional       | Vertical          | Fuerte           | Apps web, CMS, transacciones      |
| **Azure Cosmos DB**        | NoSQL (Multi-modelo)| Horizontal        | Ajustable        | Apps globales, baja latencia      |
| **Azure Database for MySQL**| Relacional       | Vertical          | Fuerte           | Apps web, CMS                     |
| **Azure Database for PostgreSQL**| Relacional | Vertical          | Fuerte           | Apps web, CMS                     |
| **Azure Synapse Analytics**| Analítico        | Horizontal        | Eventual         | BI, análisis masivo               |
| **Azure Cache for Redis**  | Caché en memoria | Horizontal        | Depende          | Caché, sesiones                   |
| **Azure Table Storage**    | NoSQL (Clave-Valor)| Horizontal        | Eventual         | Datos semi-estructurados          |
| **Azure Database for MariaDB**| Relacional    | Vertical          | Fuerte           | Apps web, CMS                     |

---

## **Consejos de Elección**
1. **Relacional y pequeña escala**: Azure SQL Database.
2. **NoSQL y global**: Azure Cosmos DB.
3. **MySQL compatible**: Azure Database for MySQL.
4. **PostgreSQL compatible**: Azure Database for PostgreSQL.
5. **Análisis de datos**: Azure Synapse Analytics.
6. **Caché en memoria**: Azure Cache for Redis.
7. **Clave-valor simple**: Azure Table Storage.
8. **MariaDB compatible**: Azure Database for MariaDB.

---

Este cheatsheet te ayudará a elegir la base de datos adecuada en función de tus necesidades en Microsoft Azure. ¡Espero que sea útil! 🚀
