
---

## **DevOps en AWS Cheatsheet**

### **Conceptos Básicos**
- **DevOps**: Cultura y prácticas que combinan desarrollo de software (Dev) y operaciones de TI (Ops) para acelerar la entrega de aplicaciones.
- **CI/CD**: Integración Continua (CI) y Entrega Continua (CD) para automatizar la construcción, prueba y despliegue de aplicaciones.
- **Infraestructura como Código (IaC)**: Gestión de infraestructura mediante código en lugar de procesos manuales.
- **Monitoreo y Observabilidad**: Supervisión del rendimiento y salud de las aplicaciones en tiempo real.

---

### **Servicios y Herramientas de AWS para DevOps**

#### **CI/CD**
1. **AWS CodePipeline**:
   - Servicio de entrega continua para automatizar pipelines de lanzamiento.
   - Ejemplo de configuración en `pipeline.json`:
     ```json
     {
       "pipeline": {
         "name": "my-pipeline",
         "roleArn": "arn:aws:iam::123456789012:role/AWS-CodePipeline-Service",
         "stages": [
           {
             "name": "Source",
             "actions": [
               {
                 "name": "SourceAction",
                 "actionTypeId": {
                   "category": "Source",
                   "owner": "ThirdParty",
                   "provider": "GitHub",
                   "version": "1"
                 },
                 "configuration": {
                   "Owner": "my-github-username",
                   "Repo": "my-repo",
                   "Branch": "main",
                   "OAuthToken": "my-github-token"
                 },
                 "outputArtifacts": [
                   {
                     "name": "SourceOutput"
                   }
                 ]
               }
             ]
           }
         ]
       }
     }
     ```

2. **AWS CodeBuild**:
   - Servicio de integración continua para compilar y probar código.
   - Ejemplo de configuración en `buildspec.yml`:
     ```yaml
     version: 0.2

     phases:
       install:
         commands:
           - echo "Installing dependencies..."
       build:
         commands:
           - echo "Building the application..."
       post_build:
         commands:
           - echo "Running tests..."
     ```

3. **AWS CodeDeploy**:
   - Servicio para automatizar despliegues de aplicaciones.
   - Ejemplo de configuración en `appspec.yml`:
     ```yaml
     version: 0.0
     os: linux
     files:
       - source: /
         destination: /var/www/html
     hooks:
       BeforeInstall:
         - location: scripts/install_dependencies.sh
           timeout: 300
           runas: root
     ```

#### **Gestión de Infraestructura**
1. **AWS CloudFormation**:
   - Herramienta de IaC para gestionar recursos de AWS.
   - Ejemplo de configuración en YAML:
     ```yaml
     Resources:
       MyEC2Instance:
         Type: AWS::EC2::Instance
         Properties:
           InstanceType: t2.micro
           ImageId: ami-0abcdef1234567890
           KeyName: my-key-pair
     ```

2. **Terraform con AWS**:
   - Herramienta de IaC para gestionar recursos en múltiples proveedores.
   - Ejemplo de configuración en Terraform:
     ```hcl
     provider "aws" {
       region = "us-west-2"
     }

     resource "aws_instance" "example" {
       ami           = "ami-0abcdef1234567890"
       instance_type = "t2.micro"
     }
     ```

3. **Elastic Kubernetes Service (EKS)**:
   - Servicio gestionado de Kubernetes para orquestar contenedores.
   - Ejemplo de despliegue en EKS:
     ```bash
     eksctl create cluster --name my-cluster --region us-west-2 --nodegroup-name my-nodes --node-type t2.micro --nodes 3
     kubectl create deployment my-app --image=my-repo/my-image:tag
     kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080
     ```

#### **Monitoreo y Observabilidad**
1. **Amazon CloudWatch**:
   - Supervisión de métricas, logs y alertas.
   - Ejemplo de creación de una alerta:
     ```bash
     aws cloudwatch put-metric-alarm --alarm-name "CPUAlarm" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 80 --comparison-operator GreaterThanOrEqualToThreshold --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --evaluation-periods 2 --alarm-actions arn:aws:sns:us-west-2:123456789012:my-sns-topic
     ```

2. **AWS X-Ray**:
   - Rastreo de solicitudes y análisis de rendimiento.
   - Ejemplo de configuración en `xray.json`:
     ```json
     {
       "SamplingRule": {
         "RuleName": "MySamplingRule",
         "ResourceARN": "*",
         "Priority": 1000,
         "FixedRate": 0.1,
         "ReservoirSize": 10,
         "ServiceName": "my-service",
         "ServiceType": "*",
         "Host": "*",
         "HTTPMethod": "*",
         "URLPath": "*",
         "Version": 1
       }
     }
     ```

#### **Seguridad**
1. **AWS Identity and Access Management (IAM)**:
   - Gestión de identidades y acceso.
   - Ejemplo de asignación de roles:
     ```bash
     aws iam attach-role-policy --role-name MyRole --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
     ```

2. **AWS Security Hub**:
   - Supervisión de la seguridad y detección de amenazas.

---

### **Mejores Prácticas**
1. **Automatización**: Automatiza todo lo posible, desde pruebas hasta despliegues.
2. **Infraestructura como Código**: Usa herramientas como CloudFormation o Terraform.
3. **Monitoreo Continuo**: Implementa alertas y dashboards para supervisar el rendimiento.
4. **Seguridad Integrada**: Asegura cada etapa del pipeline CI/CD.
5. **Escalabilidad**: Diseña aplicaciones y pipelines para ser escalables.

---

### **Ejemplo de Pipeline CI/CD en AWS**
1. **Código en GitHub**:
   - Los desarrolladores envían código a un repositorio de GitHub.

2. **AWS CodePipeline**:
   - CodePipeline detecta cambios y ejecuta pruebas y construcción de imágenes Docker.
   - Configuración en `pipeline.json`:
     ```json
     {
       "pipeline": {
         "name": "my-pipeline",
         "roleArn": "arn:aws:iam::123456789012:role/AWS-CodePipeline-Service",
         "stages": [
           {
             "name": "Source",
             "actions": [
               {
                 "name": "SourceAction",
                 "actionTypeId": {
                   "category": "Source",
                   "owner": "ThirdParty",
                   "provider": "GitHub",
                   "version": "1"
                 },
                 "configuration": {
                   "Owner": "my-github-username",
                   "Repo": "my-repo",
                   "Branch": "main",
                   "OAuthToken": "my-github-token"
                 },
                 "outputArtifacts": [
                   {
                     "name": "SourceOutput"
                   }
                 ]
               }
             ]
           }
         ]
       }
     }
     ```

3. **AWS CodeBuild**:
   - CodeBuild compila y prueba el código.
   - Configuración en `buildspec.yml`:
     ```yaml
     version: 0.2

     phases:
       install:
         commands:
           - echo "Installing dependencies..."
       build:
         commands:
           - echo "Building the application..."
       post_build:
         commands:
           - echo "Running tests..."
     ```

4. **AWS CodeDeploy**:
   - CodeDeploy despliega la aplicación en EC2 o ECS.
   - Configuración en `appspec.yml`:
     ```yaml
     version: 0.0
     os: linux
     files:
       - source: /
         destination: /var/www/html
     hooks:
       BeforeInstall:
         - location: scripts/install_dependencies.sh
           timeout: 300
           runas: root
     ```

5. **Monitoreo**:
   - CloudWatch y X-Ray supervisan el rendimiento y la salud de la aplicación.

---

### **Recursos Adicionales**
- [Documentación Oficial de AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/)
- [Documentación Oficial de AWS CodeBuild](https://docs.aws.amazon.com/codebuild/)
- [Documentación Oficial de AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/)
- [Documentación Oficial de AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)
- [Precios de AWS DevOps](https://aws.amazon.com/pricing/)

---

Esta cheatsheet te proporciona una referencia rápida y completa para implementar **DevOps en AWS**. ¡Espero que te sea útil! 😊
