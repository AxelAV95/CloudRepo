

---

## **Google Cloud Functions: Conceptos Clave**
- **Descripción**: Servicio de computación sin servidor para ejecutar código en respuesta a eventos.
- **Lenguajes soportados**: Node.js, Python, Go, Java, .NET, Ruby, PHP.
- **Casos de uso**:
  - Procesamiento de eventos en tiempo real.
  - Integración con servicios de Google Cloud (Cloud Storage, Pub/Sub, Firestore, etc.).
  - Backends para aplicaciones móviles y web.
  - Automatización de tareas.

---

## **Características Principales**
1. **Escalabilidad automática**: Escala a cero cuando no hay solicitudes y escala horizontalmente bajo carga.
2. **Pago por uso**: Solo pagas por el tiempo de ejecución y los recursos utilizados.
3. **Integración nativa**: Conecta fácilmente con otros servicios de GCP como Cloud Storage, Pub/Sub, Firestore, etc.
4. **Triggers (Disparadores)**: Ejecuta funciones en respuesta a eventos como la carga de un archivo en Cloud Storage o un mensaje en Pub/Sub.
5. **Entornos de ejecución**: Soporta múltiples versiones de lenguajes y entornos personalizados.
6. **Concurrencia**: Soporta ejecución concurrente de múltiples instancias de una función.

---

## **Tipos de Triggers**
1. **HTTP Triggers**:
   - Invoca una función mediante una solicitud HTTP.
   - Ejemplo: Crear una API REST.
   - URL de invocación: `https://[REGION]-[PROJECT_ID].cloudfunctions.net/[FUNCTION_NAME]`.

2. **Event Triggers**:
   - Invoca una función en respuesta a eventos de servicios de GCP.
   - Ejemplos:
     - **Cloud Storage**: Cuando se sube o elimina un archivo.
     - **Pub/Sub**: Cuando llega un mensaje a un tema.
     - **Firestore**: Cuando se crea, actualiza o elimina un documento.

---

## **Comandos Útiles (gcloud CLI)**
1. **Crear una función**:
   ```bash
   gcloud functions deploy [FUNCTION_NAME] --runtime [RUNTIME] --trigger-http --entry-point [ENTRY_POINT]
   ```
   - Ejemplo para Node.js:
     ```bash
     gcloud functions deploy myFunction --runtime nodejs16 --trigger-http --entry-point helloWorld
     ```

2. **Listar funciones**:
   ```bash
   gcloud functions list
   ```

3. **Obtener detalles de una función**:
   ```bash
   gcloud functions describe [FUNCTION_NAME]
   ```

4. **Invocar una función HTTP**:
   ```bash
   curl https://[REGION]-[PROJECT_ID].cloudfunctions.net/[FUNCTION_NAME]
   ```

5. **Eliminar una función**:
   ```bash
   gcloud functions delete [FUNCTION_NAME]
   ```

6. **Ver logs de una función**:
   ```bash
   gcloud functions logs read [FUNCTION_NAME]
   ```

---

## **Estructura de una Función**
### Ejemplo en Node.js:
```javascript
exports.helloWorld = (req, res) => {
  res.send('Hello, World!');
};
```

### Ejemplo en Python:
```python
def hello_world(request):
    return 'Hello, World!'
```

### Ejemplo en Go:
```go
package p

import (
    "net/http"
)

func HelloWorld(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Hello, World!"))
}
```

---

## **Mejores Prácticas**
1. **Mantén las funciones pequeñas y específicas**:
   - Cada función debe realizar una única tarea.
   - Evita funciones monolíticas.

2. **Usa variables de entorno**:
   - Configura credenciales y parámetros mediante variables de entorno.
   - Ejemplo:
     ```bash
     gcloud functions deploy myFunction --set-env-vars KEY=VALUE
     ```

3. **Manejo de errores**:
   - Implementa un manejo robusto de errores para evitar fallos inesperados.

4. **Optimiza el tiempo de ejecución**:
   - Reduce el tiempo de ejecución para minimizar costos.
   - Usa caching cuando sea posible.

5. **Monitorea y registra**:
   - Usa Cloud Logging y Cloud Monitoring para supervisar el rendimiento y depurar errores.

6. **Seguridad**:
   - Usa IAM para controlar el acceso a las funciones.
   - Valida y sanitiza las entradas HTTP para evitar vulnerabilidades.

---

## **Integraciones Comunes**
1. **Cloud Storage**:
   - Procesar archivos subidos a un bucket.
   - Ejemplo de trigger:
     ```bash
     gcloud functions deploy processFile --runtime nodejs16 --trigger-bucket [BUCKET_NAME] --entry-point processFile
     ```

2. **Pub/Sub**:
   - Procesar mensajes de un tema.
   - Ejemplo de trigger:
     ```bash
     gcloud functions deploy processMessage --runtime nodejs16 --trigger-topic [TOPIC_NAME] --entry-point processMessage
     ```

3. **Firestore**:
   - Reaccionar a cambios en una colección de Firestore.
   - Ejemplo de trigger:
     ```bash
     gcloud functions deploy firestoreTrigger --runtime nodejs16 --trigger-event providers/cloud.firestore/eventTypes/document.write --trigger-resource projects/[PROJECT_ID]/databases/(default)/documents/[COLLECTION_NAME]/{documentId} --entry-point firestoreTrigger
     ```

---

## **Precios y Límites**
1. **Precios**:
   - **Tiempo de ejecución**: Se cobra por el tiempo de ejecución en incrementos de 100 ms.
   - **Invocaciones**: Las primeras 2 millones de invocaciones son gratuitas cada mes.
   - **Recursos**: Se cobra por el uso de memoria y CPU.

2. **Límites**:
   - **Tiempo máximo de ejecución**: 9 minutos (540 segundos).
   - **Memoria**: Hasta 8 GB por función.
   - **Tamaño del código**: Hasta 500 MB (comprimido) o 1 GB (sin comprimir).

---

## **Ejemplo de Caso de Uso**
### Procesamiento de Imágenes en Cloud Storage:
1. **Trigger**: Cuando se sube una imagen a un bucket de Cloud Storage.
2. **Función**: Redimensionar la imagen y guardarla en otro bucket.
3. **Implementación**:
   ```javascript
   const { Storage } = require('@google-cloud/storage');
   const sharp = require('sharp');

   exports.resizeImage = async (file) => {
     const storage = new Storage();
     const sourceBucket = storage.bucket(file.bucket);
     const destinationBucket = storage.bucket('resized-images');

     const image = sourceBucket.file(file.name);
     const resizedImage = destinationBucket.file(file.name);

     await image.download().then((data) => {
       return sharp(data[0])
         .resize(200, 200)
         .toBuffer()
         .then((buffer) => resizedImage.save(buffer));
     });
   };
   ```

---

Este cheatsheet te proporciona una visión completa de Google Cloud Functions, desde conceptos básicos hasta implementaciones avanzadas. ¡Espero que te sea útil! 🚀
