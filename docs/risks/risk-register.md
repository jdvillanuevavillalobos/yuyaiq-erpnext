# Registro de riesgos

## Restore de laboratorio no reproducido

- `CONFIRMED` — Un primer intento de `bench restore` mostró `Unknown SEQUENCE: bisect_nodes_id_seq`.
- `CONFIRMED` — El fallo no volvió a reproducirse usando el mismo backup, imagen, MariaDB, `bench restore`, filtros y archivos.
- `CONFIRMED` — Un restore posterior completo en `erpnext-restore-debug.localhost` finalizó correctamente.
- `PENDING` — No hay causa raíz confirmada. Si el incidente reaparece, capturar stdout, stderr y el estado de la base destino inmediatamente después del error.

Estado: `PENDING` — pendiente de identificación, evaluación y responsables adicionales.
