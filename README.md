# Traefik Docker Stack

Configuración de Traefik 3.5 con Docker Compose siguiendo buenas prácticas, métricas Prometheus, logs y dashboard seguro.

## Características principales
- **Traefik 3.5** como reverse proxy y balanceador de carga
- Dashboard web de administración
- Logs de acceso configurables
- Métricas Prometheus listas para monitoreo
- Integración con Docker (autodiscovery de servicios)
- Variables de entorno para puertos y rutas

## Estructura de archivos
- `docker-compose.yml`: Orquestación principal de servicios
- `config/traefik.yml`: Configuración estática de Traefik (opcional, puedes usar solo flags)
- `.env.example`: Variables de entorno de ejemplo
- `logs/`: Carpeta sugerida para logs de Traefik

## Uso rápido
1. Copia `.env.example` a `.env` y ajusta los valores si lo deseas:
	```sh
	cp .env.example .env
	```
2. (Opcional) Crea la carpeta de logs:
	```sh
	mkdir -p logs/traefik
	```
3. Levanta el stack:
	```sh
	docker compose up -d
	```
4. Accede al dashboard en: [http://localhost:8080](http://localhost:8080)

## Métricas y monitoreo
- Las métricas Prometheus están expuestas en la ruta `/prometheus/metrics` sobre el mismo puerto HTTP de Traefik (por defecto `:80`).
- Puedes configurar Prometheus para recolectar métricas desde `http://localhost/prometheus/metrics`.
- Esto evita abrir un puerto adicional y permite proteger la ruta con middlewares si lo deseas.

## Seguridad
- El dashboard está habilitado solo en la red interna y requiere configuración adicional para exponerlo de forma segura en producción.
- Revisa y ajusta los entrypoints, reglas y certificados según tus necesidades.

## Autor
Bismarck Villca <bismarck.villca@gmail.com>
