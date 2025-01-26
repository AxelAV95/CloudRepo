

---

## **Amazon SQS Cheatsheet**

### **Conceptos Básicos**
- **SQS**: Servicio de colas de mensajes escalable y gestionado para enviar y recibir mensajes entre aplicaciones.
- **Mensaje**: Unidad de datos que se envía a través de SQS.
- **Cola (Queue)**: Un recurso que almacena mensajes enviados por los productores y los entrega a los consumidores.
- **Productor (Producer)**: Aplicación o servicio que envía mensajes a una cola.
- **Consumidor (Consumer)**: Aplicación o servicio que recibe y procesa mensajes de una cola.

---

### **Tipos de Colas**
1. **Colas Estándar**:
   - Entrega de mensajes de alta throughput.
   - Mensajes pueden entregarse en un orden diferente al enviado.
   - Pueden entregarse duplicados.

2. **Colas FIFO (First-In-First-Out)**:
   - Mensajes se entregan en el orden exacto en que se enviaron.
   - No se permiten duplicados.
   - Throughput limitado a 300 mensajes por segundo (sin batching) o 3000 con batching.

---

### **Flujo de Trabajo**
1. **Crear una Cola**: Define una cola para que los productores envíen mensajes.
2. **Enviar Mensajes**: Los productores envían mensajes a la cola.
3. **Recibir Mensajes**: Los consumidores reciben mensajes de la cola.
4. **Procesar y Eliminar Mensajes**: Los consumidores procesan los mensajes y los eliminan de la cola.

---

### **Comandos de AWS CLI**

#### **Colas**
- **Crear una cola**:
  ```bash
  aws sqs create-queue --queue-name NOMBRE_COLA
  ```
- **Listar colas**:
  ```bash
  aws sqs list-queues
  ```
- **Eliminar una cola**:
  ```bash
  aws sqs delete-queue --queue-url URL_COLA
  ```

#### **Enviar Mensajes**
- **Enviar un mensaje**:
  ```bash
  aws sqs send-message --queue-url URL_COLA --message-body "MENSAJE"
  ```

#### **Recibir Mensajes**
- **Recibir mensajes**:
  ```bash
  aws sqs receive-message --queue-url URL_COLA
  ```

#### **Eliminar Mensajes**
- **Eliminar un mensaje**:
  ```bash
  aws sqs delete-message --queue-url URL_COLA --receipt-handle HANDLE
  ```

---

### **Configuraciones Avanzadas**
- **Visibilidad de Mensajes**: Tiempo que un mensaje está oculto después de ser recibido por un consumidor.
- **Dead Letter Queue (DLQ)**: Cola para manejar mensajes que no se procesan correctamente después de varios intentos.
- **Retención de Mensajes**: Tiempo máximo que un mensaje se retiene en la cola (hasta 14 días).
- **Delay de Mensajes**: Retraso opcional antes de que un mensaje esté disponible para ser recibido.

---

### **Integraciones Comunes**
- **Lambda**: Ejecuta funciones en respuesta a mensajes de SQS.
- **SNS (Simple Notification Service)**: Envía mensajes a múltiples colas o suscriptores.
- **EC2**: Instancias de EC2 que consumen mensajes de SQS.
- **Step Functions**: Orquesta flujos de trabajo que incluyen SQS.

---

### **Mejores Prácticas**
1. **Manejo de Errores**: Configura Dead Letter Queues para manejar mensajes fallidos.
2. **Escalabilidad**: Aprovecha la escalabilidad automática de SQS para manejar grandes volúmenes de mensajes.
3. **Seguridad**: Usa IAM para controlar el acceso a las colas.
4. **Monitoreo**: Utiliza CloudWatch para supervisar el rendimiento y la salud de SQS.

---

### **Ejemplo de Uso**

#### **Crear una Cola**
```bash
aws sqs create-queue --queue-name mi-cola
```

#### **Enviar un Mensaje**
```bash
aws sqs send-message --queue-url https://sqs.region.amazonaws.com/123456789012/mi-cola --message-body "Hola, mundo!"
```

#### **Recibir Mensajes**
```bash
aws sqs receive-message --queue-url https://sqs.region.amazonaws.com/123456789012/mi-cola
```

#### **Eliminar un Mensaje**
```bash
aws sqs delete-message --queue-url https://sqs.region.amazonaws.com/123456789012/mi-cola --receipt-handle "ReceiptHandle"
```

---

### **Recursos Adicionales**
- [Documentación Oficial de Amazon SQS](https://aws.amazon.com/sqs/documentation/)
- [Precios de Amazon SQS](https://aws.amazon.com/sqs/pricing/)
- [Ejemplos de Código en GitHub](https://github.com/aws-samples/aws-sqs-sample)

---

Esta cheatsheet te proporciona una referencia rápida para trabajar con **Amazon SQS** en **AWS**. ¡Espero que te sea útil! 😊
