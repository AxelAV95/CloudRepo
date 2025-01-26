

---

## **Google Cloud Pub/Sub Cheatsheet**

### **Conceptos Básicos**
- **Pub/Sub**: Servicio de mensajería escalable y gestionado para enviar y recibir mensajes entre aplicaciones.
- **Mensaje**: Unidad de datos que se envía a través de Pub/Sub.
- **Tópico (Topic)**: Un recurso al que los publicadores envían mensajes.
- **Suscripción (Subscription)**: Un recurso que recibe mensajes de un tópico y los entrega a los suscriptores.
- **Publicador (Publisher)**: Aplicación o servicio que envía mensajes a un tópico.
- **Suscriptor (Subscriber)**: Aplicación o servicio que recibe mensajes de una suscripción.

---

### **Flujo de Trabajo**
1. **Crear un Tópico**: Define un tópico para que los publicadores envíen mensajes.
2. **Crear una Suscripción**: Crea una suscripción asociada al tópico para que los suscriptores reciban mensajes.
3. **Publicar Mensajes**: Los publicadores envían mensajes al tópico.
4. **Recibir Mensajes**: Los suscriptores reciben mensajes de la suscripción.
5. **Confirmar Recepción (Ack)**: Los suscriptores confirman la recepción de mensajes para evitar reprocesamiento.

---

### **Comandos de Google Cloud SDK (gcloud)**

#### **Tópicos**
- **Crear un tópico**:
  ```bash
  gcloud pubsub topics create NOMBRE_TOPICO
  ```
- **Listar tópicos**:
  ```bash
  gcloud pubsub topics list
  ```
- **Eliminar un tópico**:
  ```bash
  gcloud pubsub topics delete NOMBRE_TOPICO
  ```

#### **Suscripciones**
- **Crear una suscripción**:
  ```bash
  gcloud pubsub subscriptions create NOMBRE_SUSCRIPCION --topic=NOMBRE_TOPICO
  ```
- **Listar suscripciones**:
  ```bash
  gcloud pubsub subscriptions list
  ```
- **Eliminar una suscripción**:
  ```bash
  gcloud pubsub subscriptions delete NOMBRE_SUSCRIPCION
  ```

#### **Publicar Mensajes**
- **Publicar un mensaje**:
  ```bash
  gcloud pubsub topics publish NOMBRE_TOPICO --message="MENSAJE"
  ```

#### **Recibir Mensajes**
- **Recibir mensajes (pull)**:
  ```bash
  gcloud pubsub subscriptions pull NOMBRE_SUSCRIPCION --auto-ack
  ```

---

### **Configuraciones Avanzadas**
- **Retención de Mensajes**: Los mensajes se retienen en una suscripción durante un máximo de 7 días (configurable).
- **Dead Letter Topics**: Configura un tópico para manejar mensajes fallidos después de varios reintentos.
- **Orden de Mensajes**: Opcionalmente, habilita el ordenamiento de mensajes basado en una clave.
- **Push vs Pull**:
  - **Push**: Pub/Sub envía mensajes a un endpoint HTTP configurado.
  - **Pull**: El suscriptor solicita mensajes manualmente.

---

### **Integraciones Comunes**
- **Cloud Functions**: Ejecuta funciones en respuesta a mensajes de Pub/Sub.
- **Dataflow**: Procesa flujos de datos en tiempo real.
- **BigQuery**: Almacena datos de mensajes para análisis.
- **Cloud Storage**: Almacena mensajes en buckets de GCS.

---

### **Mejores Prácticas**
1. **Manejo de Errores**: Configura Dead Letter Topics para manejar mensajes fallidos.
2. **Escalabilidad**: Aprovecha la escalabilidad automática de Pub/Sub para manejar grandes volúmenes de mensajes.
3. **Seguridad**: Usa IAM para controlar el acceso a tópicos y suscripciones.
4. **Monitoreo**: Utiliza Cloud Monitoring y Cloud Logging para supervisar el rendimiento y la salud de Pub/Sub.

---

### **Ejemplo de Uso**

#### **Publicar un Mensaje**
```bash
gcloud pubsub topics publish mi-topico --message="Hola, mundo!"
```

#### **Recibir Mensajes**
```bash
gcloud pubsub subscriptions pull mi-suscripcion --limit=5 --auto-ack
```

#### **Crear una Suscripción con Dead Letter Topic**
```bash
gcloud pubsub subscriptions create mi-suscripcion \
  --topic=mi-topico \
  --dead-letter-topic=mi-dead-letter-topic \
  --max-delivery-attempts=5
```

---

### **Recursos Adicionales**
- [Documentación Oficial de Cloud Pub/Sub](https://cloud.google.com/pubsub/docs)
- [Precios de Cloud Pub/Sub](https://cloud.google.com/pubsub/pricing)
- [Ejemplos de Código en GitHub](https://github.com/googleapis/python-pubsub)

---

Esta cheatsheet te proporciona una referencia rápida para trabajar con **Cloud Pub/Sub** en **Google Cloud**. ¡Espero que te sea útil! 😊
