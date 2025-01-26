

---

## **Azure Service Bus y Queue Storage Cheatsheet**

### **Conceptos Básicos**
- **Azure Service Bus**: Servicio de mensajería empresarial para enviar y recibir mensajes entre aplicaciones.
- **Azure Queue Storage**: Servicio de colas simple para almacenar grandes cantidades de mensajes.
- **Mensaje**: Unidad de datos que se envía a través de Service Bus o Queue Storage.
- **Cola (Queue)**: Un recurso que almacena mensajes enviados por los productores y los entrega a los consumidores.
- **Tópico (Topic)**: Un recurso en Service Bus que permite la publicación/suscripción de mensajes.
- **Suscripción (Subscription)**: Un recurso en Service Bus que recibe mensajes de un tópico.

---

### **Tipos de Colas**
1. **Azure Service Bus Queues**:
   - Mensajería empresarial con características avanzadas como sesiones, ordenamiento y duplicación.
   - Ideal para escenarios complejos de mensajería.

2. **Azure Queue Storage**:
   - Colas simples para almacenar grandes cantidades de mensajes.
   - Ideal para escenarios simples y de bajo costo.

---

### **Flujo de Trabajo**
1. **Crear una Cola o Tópico**: Define una cola o tópico para que los productores envíen mensajes.
2. **Enviar Mensajes**: Los productores envían mensajes a la cola o tópico.
3. **Recibir Mensajes**: Los consumidores reciben mensajes de la cola o suscripción.
4. **Procesar y Eliminar Mensajes**: Los consumidores procesan los mensajes y los eliminan de la cola o suscripción.

---

### **Comandos de Azure CLI**

#### **Azure Service Bus**
- **Crear un Namespace**:
  ```bash
  az servicebus namespace create --name NOMBRE_NAMESPACE --resource-group GRUPO_RECURSOS --location REGION
  ```
- **Crear una Cola**:
  ```bash
  az servicebus queue create --name NOMBRE_COLA --namespace-name NOMBRE_NAMESPACE --resource-group GRUPO_RECURSOS
  ```
- **Enviar un Mensaje**:
  ```bash
  az servicebus queue send --name NOMBRE_COLA --namespace-name NOMBRE_NAMESPACE --resource-group GRUPO_RECURSOS --message-body "MENSAJE"
  ```
- **Recibir Mensajes**:
  ```bash
  az servicebus queue receive --name NOMBRE_COLA --namespace-name NOMBRE_NAMESPACE --resource-group GRUPO_RECURSOS
  ```

#### **Azure Queue Storage**
- **Crear una Cola**:
  ```bash
  az storage queue create --name NOMBRE_COLA --account-name NOMBRE_CUENTA
  ```
- **Enviar un Mensaje**:
  ```bash
  az storage message put --content "MENSAJE" --queue-name NOMBRE_COLA --account-name NOMBRE_CUENTA
  ```
- **Recibir Mensajes**:
  ```bash
  az storage message get --queue-name NOMBRE_COLA --account-name NOMBRE_CUENTA
  ```

---

### **Configuraciones Avanzadas**
- **Dead Letter Queue (DLQ)**: Cola para manejar mensajes que no se procesan correctamente después de varios intentos.
- **Visibilidad de Mensajes**: Tiempo que un mensaje está oculto después de ser recibido por un consumidor.
- **Retención de Mensajes**: Tiempo máximo que un mensaje se retiene en la cola (hasta 7 días en Queue Storage, configurable en Service Bus).
- **Sesiones**: Permite el procesamiento ordenado de mensajes en Service Bus.

---

### **Integraciones Comunes**
- **Azure Functions**: Ejecuta funciones en respuesta a mensajes de Service Bus o Queue Storage.
- **Logic Apps**: Automatiza flujos de trabajo que incluyen Service Bus.
- **Event Grid**: Envía eventos basados en mensajes de Service Bus.
- **Storage Explorer**: Herramienta para gestionar Queue Storage.

---

### **Mejores Prácticas**
1. **Manejo de Errores**: Configura Dead Letter Queues para manejar mensajes fallidos.
2. **Escalabilidad**: Aprovecha la escalabilidad automática de Service Bus y Queue Storage.
3. **Seguridad**: Usa RBAC (Role-Based Access Control) para controlar el acceso a las colas y tópicos.
4. **Monitoreo**: Utiliza Azure Monitor para supervisar el rendimiento y la salud de Service Bus y Queue Storage.

---

### **Ejemplo de Uso**

#### **Azure Service Bus**
- **Crear una Cola**:
  ```bash
  az servicebus queue create --name mi-cola --namespace-name mi-namespace --resource-group mi-grupo
  ```
- **Enviar un Mensaje**:
  ```bash
  az servicebus queue send --name mi-cola --namespace-name mi-namespace --resource-group mi-grupo --message-body "Hola, mundo!"
  ```
- **Recibir Mensajes**:
  ```bash
  az servicebus queue receive --name mi-cola --namespace-name mi-namespace --resource-group mi-grupo
  ```

#### **Azure Queue Storage**
- **Crear una Cola**:
  ```bash
  az storage queue create --name mi-cola --account-name mi-cuenta
  ```
- **Enviar un Mensaje**:
  ```bash
  az storage message put --content "Hola, mundo!" --queue-name mi-cola --account-name mi-cuenta
  ```
- **Recibir Mensajes**:
  ```bash
  az storage message get --queue-name mi-cola --account-name mi-cuenta
  ```

---

### **Recursos Adicionales**
- [Documentación Oficial de Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/)
- [Documentación Oficial de Azure Queue Storage](https://docs.microsoft.com/azure/storage/queues/)
- [Precios de Azure Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)
- [Precios de Azure Queue Storage](https://azure.microsoft.com/pricing/details/storage/queues/)

---

Esta cheatsheet te proporciona una referencia rápida para trabajar con **Azure Service Bus** y **Queue Storage** en **Microsoft Azure**. ¡Espero que te sea útil! 😊
