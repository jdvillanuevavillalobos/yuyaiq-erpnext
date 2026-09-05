# Arquitectura

## Investigación Docker realizada

- `CONFIRMED` — Frappe Docker define servicios para configurador, backend, frontend, WebSocket, workers de cola y scheduler. MariaDB y Redis se añaden mediante overrides oficiales.
- `HYPOTHESIS` — Una futura implementación podría usar una configuración propia derivada de `compose.yaml` y overrides oficiales, en vez de `pwd.yml` como base persistente.
- `HYPOTHESIS` — Una topología con proxy HTTPS, frontend, backend, WebSocket, workers, scheduler, MariaDB y Redis, manteniendo los servicios de datos sin exposición pública, podría ser apropiada.

Estado de implementación: `PENDING` — no hay Docker ni ERPNext instalados.
