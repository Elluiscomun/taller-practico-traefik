# Uso de Traefik con Docker Compose

Este documento describe cómo utilizar y probar el entorno basado en Traefik configurado en este proyecto.

## Cómo iniciar los servicios

Ejecuta el siguiente comando para levantar los contenedores en segundo plano:
```sh
docker-compose up -d
```
Para verificar que los servicios están en ejecución:
```sh
docker ps
```

## Acceso a los servicios
- **Panel de Traefik:** `http://localhost:8080`
- **Servicio Nginx:** `http://localhost/nginx/`
- **Servicio Whoami:** `http://localhost/whoami/`

## Preguntas 

### 1. ¿Cómo detecta Traefik los servicios configurados en Docker Compose?
Traefik detecta los servicios mediante su integración con Docker. Los contenedores etiquetados con `traefik.enable=true` son automáticamente gestionados por Traefik.

### 2. ¿Qué rol juegan los middlewares en la seguridad y gestión del tráfico?
Los middlewares permiten modificar las solicitudes antes de que lleguen a los servicios, aplicando autenticación, limitación de tasa de peticiones (`rate-limit`), redirecciones, entre otros.

### 3. ¿Cómo se define un router en Traefik y qué parámetros son esenciales?
Un router se configura mediante etiquetas en `docker-compose.yml` o en `traefik.yml`. Parámetros clave incluyen:
   - `rule`: Define cómo se enruta el tráfico (ej. `PathPrefix` o `Host`).
   - `entrypoints`: Determina por qué puerto se accede.
   - `service`: Especifica el backend al que se dirige el tráfico.
   - `tls.certresolver`: Indica el mecanismo de generación de certificados SSL.

### 4. ¿Cuál es la diferencia entre un router y un servicio en Traefik?
   - **Router:** Define las reglas de enrutamiento y decide a qué servicio enviar la solicitud.
   - **Servicio:** Es el backend real al que Traefik dirige el tráfico, como Nginx o Whoami.

### 5. ¿Cómo se pueden agregar más reglas de enrutamiento para diferentes rutas?
Para agregar nuevas rutas, se pueden definir más routers en `docker-compose.yml` o `traefik.yml`, usando `PathPrefix` u otras reglas.

Ejemplo de configuración para un nuevo servicio:
```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.miapp.rule=PathPrefix(`/miapp`)"
  - "traefik.http.routers.miapp.entrypoints=websecure"
  - "traefik.http.routers.miapp.tls.certresolver=letsencrypt"
```
