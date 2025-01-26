

---

## **AWS Lambda: Conceptos Clave**
- **Descripción**: Servicio de computación sin servidor para ejecutar código en respuesta a eventos.
- **Lenguajes soportados**: Node.js, Python, Java, Go, Ruby, .NET, PowerShell, Rust, y más (mediante Runtime API).
- **Casos de uso**:
  - Procesamiento de eventos en tiempo real.
  - Integración con servicios de AWS (S3, DynamoDB, API Gateway, etc.).
  - Backends para aplicaciones móviles y web.
  - Automatización de tareas.

---

## **Características Principales**
1. **Escalabilidad automática**: Escala a cero cuando no hay solicitudes y escala horizontalmente bajo carga.
2. **Pago por uso**: Solo pagas por el tiempo de ejecución y los recursos utilizados.
3. **Integración nativa**: Conecta fácilmente con otros servicios de AWS como S3, DynamoDB, API Gateway, etc.
4. **Triggers (Disparadores)**: Ejecuta funciones en respuesta a eventos como la carga de un archivo en S3 o un mensaje en SQS.
5. **Capas (Layers)**: Permite compartir código y dependencias entre múltiples funciones.
6. **Concurrencia**: Soporta ejecución concurrente de múltiples instancias de una función.

---

## **Tipos de Triggers**
1. **API Gateway**:
   - Invoca una función mediante una solicitud HTTP.
   - Ejemplo: Crear una API REST.

2. **S3**:
   - Invoca una función cuando se sube o elimina un archivo en un bucket.

3. **DynamoDB**:
   - Invoca una función cuando se crea, actualiza o elimina un ítem en una tabla.

4. **SQS**:
   - Invoca una función cuando llega un mensaje a una cola.

5. **EventBridge**:
   - Invoca una función en respuesta a eventos personalizados o de otros servicios de AWS.

---

## **Comandos Útiles (AWS CLI)**
1. **Crear una función**:
   ```bash
   aws lambda create-function --function-name [FUNCTION_NAME] --runtime [RUNTIME] --handler [HANDLER] --role [ROLE_ARN] --zip-file fileb://[ZIP_FILE]
   ```
   - Ejemplo para Node.js:
     ```bash
     aws lambda create-function --function-name myFunction --runtime nodejs14.x --handler index.handler --role arn:aws:iam::123456789012:role/lambda-execution-role --zip-file fileb://function.zip
     ```

2. **Actualizar una función**:
   ```bash
   aws lambda update-function-code --function-name [FUNCTION_NAME] --zip-file fileb://[ZIP_FILE]
   ```

3. **Listar funciones**:
   ```bash
   aws lambda list-functions
   ```

4. **Obtener detalles de una función**:
   ```bash
   aws lambda get-function --function-name [FUNCTION_NAME]
   ```

5. **Invocar una función**:
   ```bash
   aws lambda invoke --function-name [FUNCTION_NAME] output.txt
   ```

6. **Eliminar una función**:
   ```bash
   aws lambda delete-function --function-name [FUNCTION_NAME]
   ```

7. **Ver logs de una función**:
   ```bash
   aws logs tail /aws/lambda/[FUNCTION_NAME] --follow
   ```

---

## **Estructura de una Función**
### Ejemplo en Node.js:
```javascript
exports.handler = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify('Hello, World!'),
  };
};
```

### Ejemplo en Python:
```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello, World!'
    }
```

### Ejemplo en Go:
```go
package main

import (
    "github.com/aws/aws-lambda-go/lambda"
)

func handler() (string, error) {
    return "Hello, World!", nil
}

func main() {
    lambda.Start(handler)
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
     aws lambda update-function-configuration --function-name [FUNCTION_NAME] --environment Variables={KEY=VALUE}
     ```

3. **Manejo de errores**:
   - Implementa un manejo robusto de errores para evitar fallos inesperados.

4. **Optimiza el tiempo de ejecución**:
   - Reduce el tiempo de ejecución para minimizar costos.
   - Usa caching cuando sea posible.

5. **Monitorea y registra**:
   - Usa CloudWatch para supervisar el rendimiento y depurar errores.

6. **Seguridad**:
   - Usa IAM para controlar el acceso a las funciones.
   - Valida y sanitiza las entradas HTTP para evitar vulnerabilidades.

---

## **Integraciones Comunes**
1. **S3**:
   - Procesar archivos subidos a un bucket.
   - Ejemplo de trigger:
     ```bash
     aws s3api put-bucket-notification-configuration --bucket [BUCKET_NAME] --notification-configuration file://notification.json
     ```
     ```json
     {
       "LambdaFunctionConfigurations": [
         {
           "Id": "ProcessFile",
           "LambdaFunctionArn": "arn:aws:lambda:[REGION]:[ACCOUNT_ID]:function:[FUNCTION_NAME]",
           "Events": ["s3:ObjectCreated:*"]
         }
       ]
     }
     ```

2. **DynamoDB**:
   - Reaccionar a cambios en una tabla.
   - Ejemplo de trigger:
     ```bash
     aws dynamodbstreams describe-stream --table-name [TABLE_NAME]
     ```

3. **API Gateway**:
   - Crear una API REST.
   - Ejemplo de integración:
     ```bash
     aws apigateway create-rest-api --name [API_NAME]
     ```

---

## **Precios y Límites**
1. **Precios**:
   - **Tiempo de ejecución**: Se cobra por el tiempo de ejecución en incrementos de 1 ms.
   - **Invocaciones**: Las primeras 1 millón de invocaciones son gratuitas cada mes.
   - **Recursos**: Se cobra por el uso de memoria y CPU.

2. **Límites**:
   - **Tiempo máximo de ejecución**: 15 minutos (900 segundos).
   - **Memoria**: Hasta 10 GB por función.
   - **Tamaño del código**: Hasta 50 MB (comprimido) o 250 MB (sin comprimir).

---

## **Ejemplo de Caso de Uso**
### Procesamiento de Imágenes en S3:
1. **Trigger**: Cuando se sube una imagen a un bucket de S3.
2. **Función**: Redimensionar la imagen y guardarla en otro bucket.
3. **Implementación**:
   ```javascript
   const AWS = require('aws-sdk');
   const sharp = require('sharp');

   const s3 = new AWS.S3();

   exports.handler = async (event) => {
     const sourceBucket = event.Records[0].s3.bucket.name;
     const sourceKey = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, ' '));
     const destinationBucket = 'resized-images';

     const image = await s3.getObject({ Bucket: sourceBucket, Key: sourceKey }).promise();
     const resizedImage = await sharp(image.Body).resize(200, 200).toBuffer();

     await s3.putObject({
       Bucket: destinationBucket,
       Key: sourceKey,
       Body: resizedImage,
     }).promise();

     return {
       statusCode: 200,
       body: 'Image resized and saved!',
     };
   };
   ```

---

Este cheatsheet te proporciona una visión completa de AWS Lambda, desde conceptos básicos hasta implementaciones avanzadas. ¡Espero que te sea útil! 🚀
