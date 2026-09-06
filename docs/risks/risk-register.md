# Registro de riesgos

## Restore de laboratorio no reproducido

- `CONFIRMED` — Un primer intento de `bench restore` mostró `Unknown SEQUENCE: bisect_nodes_id_seq`.
- `CONFIRMED` — El fallo no volvió a reproducirse usando el mismo backup, imagen, MariaDB, `bench restore`, filtros y archivos.
- `CONFIRMED` — Un restore posterior completo en `erpnext-restore-debug.localhost` finalizó correctamente.
- `PENDING` — No hay causa raíz confirmada. Si el incidente reaparece, capturar stdout, stderr y el estado de la base destino inmediatamente después del error.

Estado: `PENDING` — pendiente de identificación, evaluación y responsables adicionales.

## Exposición del directorio User a Sales User

- `CONFIRMED` — En el LAB, Sales User puede listar usuarios no estándar y ver email, nombre, estado, tipo y Role Profile; no pudo abrir, crear, borrar ni modificar a otro usuario.
- `HYPOTHESIS` — Si el MVP no permite mostrar datos de contacto o perfiles internos a ventas, se requerirá endurecer los permisos/configuración de User antes de producción.
- `PENDING` — Definir la política de visibilidad del directorio de usuarios y validar la mitigación nativa elegida.
