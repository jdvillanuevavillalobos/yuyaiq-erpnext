# Observabilidad

## Investigación Docker realizada

- `CONFIRMED` — Frappe documenta comprobación por host/puerto para Gunicorn y Socket.IO, y `healthcheck.sh` para workers y scheduler.
- `HYPOTHESIS` — La futura operación podría recolectar logs de contenedor y de site, con rotación y alertas de disponibilidad, colas, scheduler, recursos y backups.

Estado de implementación: `PENDING`.

## Validación de laboratorio

- `CONFIRMED` — Los logs de contenedor se observaron mediante `docker compose logs`. El entorno también expone logs de Bench y logs específicos del site en el volumen `sites`.
- `CONFIRMED` — Los logs verificaron workers escuchando sus colas y WebSocket escuchando en el puerto interno `9000`; `bench doctor` confirmó dos workers online y el scheduler fue habilitado para `erpnext.localhost`.
