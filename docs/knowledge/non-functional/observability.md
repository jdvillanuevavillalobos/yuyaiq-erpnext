# Observabilidad

## Investigación Docker realizada

- `CONFIRMED` — Frappe documenta comprobación por host/puerto para Gunicorn y Socket.IO, y `healthcheck.sh` para workers y scheduler.
- `HYPOTHESIS` — La futura operación podría recolectar logs de contenedor y de site, con rotación y alertas de disponibilidad, colas, scheduler, recursos y backups.

Estado de implementación: `PENDING`.
