

---

## **Azure Functions: Conceptos Clave**
- **Descripción**: Servicio de computación sin servidor para ejecutar código en respuesta a eventos.
- **Lenguajes soportados**: C#, JavaScript, Python, Java, PowerShell, TypeScript, y más (mediante custom handlers).
- **Casos de uso**:
  - Procesamiento de eventos en tiempo real.
  - Integración con servicios de Azure (Blob Storage, Event Hubs, Cosmos DB, etc.).
  - Backends para aplicaciones móviles y web.
  - Automatización de tareas.

---

## **Características Principales**
1. **Escalabilidad automática**: Escala a cero cuando no hay solicitudes y escala horizontalmente bajo carga.
2. **Pago por uso**: Solo pagas por el tiempo de ejecución y los recursos utilizados.
3. **Integración nativa**: Conecta fácilmente con otros servicios de Azure como Blob Storage, Event Hubs, Cosmos DB, etc.
4. **Triggers (Disparadores)**: Ejecuta funciones en respuesta a eventos como la carga de un archivo en Blob Storage o un mensaje en Event Hubs.
5. **Bindings (Enlaces)**: Simplifica la conexión con servicios de entrada y salida.
6. **Durable Functions**: Extensión para crear flujos de trabajo con estado.

---

## **Tipos de Triggers**
1. **HTTP Trigger**:
   - Invoca una función mediante una solicitud HTTP.
   - Ejemplo: Crear una API REST.

2. **Blob Storage Trigger**:
   - Invoca una función cuando se sube o elimina un archivo en un contenedor de Blob Storage.

3. **Event Hubs Trigger**:
   - Invoca una función cuando llega un mensaje a un Event Hub.

4. **Cosmos DB Trigger**:
   - Invoca una función cuando se crea, actualiza o elimina un documento en Cosmos DB.

5. **Timer Trigger**:
   - Invoca una función en un horario programado.

---

## **Comandos Útiles (Azure CLI)**
1. **Crear una función**:
   ```bash
   az functionapp create --resource-group [RESOURCE_GROUP] --consumption-plan-location [LOCATION] --runtime [RUNTIME] --functions-version [VERSION] --name [APP_NAME] --storage-account [STORAGE_ACCOUNT]
   ```
   - Ejemplo para Node.js:
     ```bash
     az functionapp create --resource-group myResourceGroup --consumption-plan-location eastus --runtime node --functions-version 3 --name myFunctionApp --storage-account mystorageaccount
     ```

2. **Implementar código**:
   ```bash
   func azure functionapp publish [APP_NAME]
   ```

3. **Listar funciones**:
   ```bash
   az functionapp list --resource-group [RESOURCE_GROUP]
   ```

4. **Obtener detalles de una función**:
   ```bash
   az functionapp show --resource-group [RESOURCE_GROUP] --name [APP_NAME]
   ```

5. **Invocar una función HTTP**:
   ```bash
   curl https://[APP_NAME].azurewebsites.net/api/[FUNCTION_NAME]?code=[FUNCTION_KEY]
   ```

6. **Eliminar una función**:
   ```bash
   az functionapp delete --resource-group [RESOURCE_GROUP] --name [APP_NAME]
   ```

7. **Ver logs de una función**:
   ```bash
   az webapp log tail --resource-group [RESOURCE_GROUP] --name [APP_NAME]
   ```

---

## **Estructura de una Función**
### Ejemplo en C#:
```csharp
using System.IO;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

public static class HttpTriggerExample
{
    [FunctionName("HttpTriggerExample")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        name = name ?? data?.name;

        return name != null
            ? (ActionResult)new OkObjectResult($"Hello, {name}")
            : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
    }
}
```

### Ejemplo en JavaScript:
```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name
        : "Please pass a name on the query string or in the request body";

    context.res = {
        status: 200,
        body: responseMessage
    };
};
```

### Ejemplo en Python:
```python
import logging

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello, {name}.")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
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
     az functionapp config appsettings set --name [APP_NAME] --resource-group [RESOURCE_GROUP] --settings KEY=VALUE
     ```

3. **Manejo de errores**:
   - Implementa un manejo robusto de errores para evitar fallos inesperados.

4. **Optimiza el tiempo de ejecución**:
   - Reduce el tiempo de ejecución para minimizar costos.
   - Usa caching cuando sea posible.

5. **Monitorea y registra**:
   - Usa Application Insights para supervisar el rendimiento y depurar errores.

6. **Seguridad**:
   - Usa RBAC para controlar el acceso a las funciones.
   - Valida y sanitiza las entradas HTTP para evitar vulnerabilidades.

---

## **Integraciones Comunes**
1. **Blob Storage**:
   - Procesar archivos subidos a un contenedor.
   - Ejemplo de trigger:
     ```csharp
     [FunctionName("BlobTriggerExample")]
     public static void Run([BlobTrigger("mycontainer/{name}", Connection = "AzureWebJobsStorage")] Stream myBlob, string name, ILogger log)
     {
         log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
     }
     ```

2. **Event Hubs**:
   - Procesar mensajes de un Event Hub.
   - Ejemplo de trigger:
     ```csharp
     [FunctionName("EventHubTriggerExample")]
     public static void Run([EventHubTrigger("myeventhub", Connection = "EventHubConnectionAppSetting")] string[] events, ILogger log)
     {
         foreach (var eventData in events)
         {
             log.LogInformation($"C# Event Hub trigger function processed a message: {eventData}");
         }
     }
     ```

3. **Cosmos DB**:
   - Reaccionar a cambios en una colección.
   - Ejemplo de trigger:
     ```csharp
     [FunctionName("CosmosDBTriggerExample")]
     public static void Run([CosmosDBTrigger(
         databaseName: "mydatabase",
         collectionName: "mycollection",
         ConnectionStringSetting = "CosmosDBConnection",
         LeaseCollectionName = "leases")] IReadOnlyList<Document> documents, ILogger log)
     {
         if (documents != null && documents.Count > 0)
         {
             log.LogInformation($"Documents modified: {documents.Count}");
         }
     }
     ```

---

## **Precios y Límites**
1. **Precios**:
   - **Tiempo de ejecución**: Se cobra por el tiempo de ejecución en incrementos de 1 ms.
   - **Invocaciones**: Las primeras 1 millón de invocaciones son gratuitas cada mes.
   - **Recursos**: Se cobra por el uso de memoria y CPU.

2. **Límites**:
   - **Tiempo máximo de ejecución**: 10 minutos (600 segundos) en el plan de consumo.
   - **Memoria**: Hasta 1.5 GB por función en el plan de consumo.
   - **Tamaño del código**: Hasta 1.5 GB por aplicación de funciones.

---

## **Ejemplo de Caso de Uso**
### Procesamiento de Imágenes en Blob Storage:
1. **Trigger**: Cuando se sube una imagen a un contenedor de Blob Storage.
2. **Función**: Redimensionar la imagen y guardarla en otro contenedor.
3. **Implementación**:
   ```csharp
   using System.IO;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Extensions.Logging;
   using SixLabors.ImageSharp;
   using SixLabors.ImageSharp.Processing;

   public static class BlobTriggerExample
   {
       [FunctionName("BlobTriggerExample")]
       public static void Run([BlobTrigger("mycontainer/{name}", Connection = "AzureWebJobsStorage")] Stream myBlob, string name, [Blob("resized-images/{name}", FileAccess.Write)] Stream outputBlob, ILogger log)
       {
           log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");

           using (var image = Image.Load(myBlob))
           {
               image.Mutate(x => x.Resize(200, 200));
               image.SaveAsJpeg(outputBlob);
           }
       }
   }
   ```

---

Este cheatsheet te proporciona una visión completa de Azure Functions, desde conceptos básicos hasta implementaciones avanzadas. ¡Espero que te sea útil! 🚀
