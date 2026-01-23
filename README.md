# lg-traefik

Traefik v3.0 reverse proxy for LicenseGuard development environment.

## Features

- HTTP/HTTPS routing
- Proxy domains (*.lg.local)
- Dashboard (traefik.lg.local)
- Auto-discovery via Docker labels
- External networks (lg-public, lg-internal)

## Configuration

### traefik.yml
Main Traefik configuration:
- Entry points (web, websecure, traefik)
- API dashboard
- Docker provider

### dynamic.yml
Dynamic routing configuration:
- Host-based routing rules
- Service definitions
- Middleware

## Environment Variables

- `TRAEFIK_HTTP_PORT` - HTTP port (default: 80)
- `TRAEFIK_HTTPS_PORT` - HTTPS port (default: 443)
- `TRAEFIK_DASHBOARD_PORT` - Dashboard port (default: 8080)

## Standalone Usage

```bash
# Create networks first
docker network create lg-public
docker network create lg-internal

# Start Traefik
docker-compose up -d

# Access dashboard
open http://localhost:8080
# Or with proxy domain:
open http://traefik.lg.local
```

## Integration with lg-development

This repository is designed to be cloned by the [lg-development](https://github.com/WolfgangM81/lg-development) orchestrator.

```bash
cd lg-development
make prepare   # Clones this repo to repos/lg-traefik
make start     # Starts Traefik with other services
```

## Dashboard Access

- **Local:** http://localhost:8080
- **Proxy Domain:** http://traefik.lg.local (requires /etc/hosts entry)

## Service Routing

Services register themselves via Docker labels:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.myservice.rule=PathPrefix(`/api/myservice`)"
  - "traefik.http.services.myservice.loadbalancer.server.port=3000"
```

## License

MIT
