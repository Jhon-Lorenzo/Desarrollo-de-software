## Parte A: Infraestructura como Código (IaC)

### 1. ¿Qué es IaC?

Antes, si querías un servidor, tenías que:
- Pedirlo físicamente.
- O configurarlo manualmente, instalar Windows, Apache, etc.

Ahora, con IaC, escribes un archivo de código que dice:
> "Quiero un servidor Ubuntu con 4GB de RAM, Docker instalado y una IP".

Ejecutas el código usando como ejemplo Terraform y el servidor se crea solo.

**Beneficios:**
- **Consistencia:** Todos los servidores son iguales, ya no tendremos la excusa de que en mi pc funciona.
- **Control de versiones:** Si algo falla, vuelves a una versión anterior, como Git pero para servidores.
- **Automatización:** No más clicks eternos en AWS o Azure.

### 2. Herramientas de IaC

- **Terraform:** El más popular. Usa archivos `.tf` para definir infraestructura.
- **Ansible:** Para configurar servidores ya existentes (instalar software, etc.).
- **AWS CloudFormation:** Si usas AWS, pero es menos flexible.

### 3. Estructura de un Proyecto IaC

Imagina que vas a crear una app web. Tu proyecto en Terraform podría organizarse así:

```
mi-proyecto/  
├── modules/  
│   ├── network/ (configura la red)  
│   ├── database/ (crea la base de datos)  
│   └── app/ (servidores para la app)  
└── main.tf (une todos los módulos)  
```

¿Por qué? Para reutilizar código. ejemplo: El módulo database puede usarse en otros proyectos.

## Parte B: Contenedores y Kubernetes

### 1. Contenedores (Docker)

**¿Qué es?** Una "contenedor" (valga la redundancia)que guarda tu app y todo lo que necesita para funcionar como librerías, configs, etc.

**Diferencia con una VM:**
- **VM:** Es como una casa grande y ocupa mucho espacio.
- **Contenedor:** Es como una mini-casa de Lego ligera y portable.

**Dockerfile :**

```dockerfile
FROM node:14  # Imagen base  
WORKDIR /app  # Carpeta dentro del contenedor  
COPY . .      # Copia tu código  
RUN npm install  # Instala dependencias  
CMD ["npm", "start"]  # Comando para iniciar la app  
```

### 2. Kubernetes

**Problema:** Si tienes 100 contenedores, ¿cómo los administras? Ahí entra Kubernetes.

**Componentes clave:**
- **Pod:** El "envase" donde corre un contenedor o varios contenedores.
- **Deployment:** Define cuántas copias de tu app deben correr como ejemplo pueden ser 3 replicas.
- **Service:** Da una IP fija a tu app para que otros la encuentren.

**Ejemplo de YAML para un Deployment:**

```yaml
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: mi-app  
spec:  
  replicas: 3  # 3 copias de la app  
  template:  
    spec:  
      containers:  
      - name: mi-app  
        image: ghcr.io/usuario/app:latest  
```

### 3. Estrategias de Despliegue

- **Rolling Update:** Actualiza de a pocos contenedores .
- **Blue-Green:** Tienes dos versiones , azul = vieja, verde = nueva y cambias el tráfico de una a otra.

## Parte C: Observabilidad

### 1. ¿Para qué sirve?

Imagina que tu app se cae a medianoche. ¿Cómo sabes qué pasó? Con observabilidad:

- **Métricas:** CPU, memoria, etc.
- **Logs:** Mensajes de error (ELK Stack).
- **Trazas:** Rastrear una petición desde el frontend hasta la base de datos (Postgres).

### 2. Herramientas

**Prometheus + Grafana:**
- Prometheus recolecta datos.
- Grafana los muestra en gráficos bonitos.
- Alertas: Si el CPU llega al 90%, te llega un mensaje a Slack.

**Ejemplo de métricas importantes:**
- Latencia lo que es el tiempo de respuesta.
- Tasa de errores HTTP como por ejemplo un 500 que es in internal server error, etc.

## Parte D: CI/CD

### 1. ¿Qué es?

- **CI (Integración Continua):** Cada vez que haces un git push, se ejecutan pruebas automáticas.
- **CD (Despliegue Continuo):** Si las pruebas pasan, se despliega automáticamente en producción.

### 2. Describir la relevancia de implementar pruebas automáticas (unitarias, de integración, de seguridad) dentro del pipeline.
Las pruebas automáticas en el pipeline son esenciales porque atrapan errores rápido al detectar cualquier falla en cambios del código mediante pruebas unitarias y de integración antes de que lleguen a producción. Incorporan seguridad al escanear vulnerabilidades como dependencias inseguras y bloquean automáticamente despliegues riesgosos. Además, eliminan el clásico problema del "funciona en mi máquina" al garantizar que el código se ejecute correctamente en todos los entornos, manteniendo consistencia y confiabilidad en cada despliegue.

