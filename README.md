# Traefik Docker Stack

Traefik 3.5 configuration with Docker Compose following best practices, Prometheus metrics, logs, and a secure dashboard.

## Main features
- **Traefik 3.5** as a reverse proxy and load balancer
- Web admin dashboard
- Configurable access logs
- Prometheus metrics ready for monitoring
- Docker integration (service autodiscovery)
- Environment variables for ports and routes

## File structure
- `docker-compose.yml`: Main service orchestration
- `config/traefik.yml`: Traefik static configuration (optional, you can use only flags)
- `.env.example`: Example environment variables
- `logs/`: Suggested folder for Traefik logs

## Quick start
1. Copy `.env.example` to `.env` and adjust values if desired:
	```sh
	cp .env.example .env
	```
2. (Optional) Create the logs folder:
	```sh
	mkdir -p logs/traefik
	```
3. Bring the stack up:
	```sh
	docker compose up -d
	```
4. Access the dashboard at: [http://localhost:8080](http://localhost:8080)

## Metrics and monitoring
- Prometheus metrics are exposed at the `/prometheus/metrics` path on the same Traefik HTTP port (by default `:80`).
- You can configure Prometheus to scrape from `http://localhost/prometheus/metrics`.
- This avoids opening an additional port and allows protecting the path with middlewares if desired.

## Security
- The dashboard is enabled only on the internal network and requires additional configuration to expose it safely in production.
- Review and adjust entrypoints, rules, and certificates as needed.

## How to enable on the external container

Add the following labels to the container you want to expose through Traefik, adjusting the values as needed:

```properties
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.yourms.rule=PathPrefix(`/yourms`)"
  - "traefik.http.routers.yourms.entrypoints=web"
  - "traefik.http.middlewares.yourms-stripprefix.stripprefix.prefixes=/yourms"
  - "traefik.http.routers.yourms.middlewares=yourms-stripprefix,retry@file"
  - "traefik.http.services.yourms.loadbalancer.server.port=8012"
  - "traefik.http.services.yourms.loadbalancer.healthcheck.path=/management/health"
  - "traefik.http.services.yourms.loadbalancer.healthcheck.interval=5s"
  - "traefik.http.services.yourms.loadbalancer.healthcheck.timeout=2s"
```

Note: Replace `yourms` with your service name and adjust the port and health check path according to your configuration. Use `retry@file` to enable retries on failures.

## Author

<div align="center">
  <img src="https://github.com/villcabo.png" width="100" height="100" style="border-radius: 50%;" alt="villcabo">
  <br/>
  <strong>Bismarck Villca</strong>
  <br/>
  <a href="https://github.com/villcabo">
    <img src="https://img.shields.io/badge/GitHub-villcabo-blue?style=flat-square&logo=github" alt="GitHub Profile">
  </a>
  <br/>
  <a href="https://github.com/villcabo/traefik-docker">
    <img src="https://img.shields.io/badge/Repository-traefik--docker-green?style=flat-square&logo=github" alt="Repository">
  </a>
</div>
