# Docker y Frappe Docker

## Confirmed Frappe/ERPNext facts

- `CONFIRMED` — La investigación Docker realizada usa las fuentes oficiales listadas en [sources.md](../sources.md). No existe implementación local todavía.
- `CONFIRMED` — `pwd.yml` es para exploración desechable; no es una base de desarrollo, producción ni migración.
- `CONFIRMED` — Desarrollo local se realiza con Devcontainers; producción manual se realiza con `compose.yaml` y overrides.
- `CONFIRMED` — Base: `configurator`, `backend`, `frontend`, `websocket`, `queue-short`, `queue-long` y `scheduler`.
- `CONFIRMED` — MariaDB y Redis se agregan con overrides; el configurador escribe la configuración común y los servicios dependientes esperan su finalización correcta.
- `CONFIRMED` — La línea ERPNext v16 tiene soporte planificado hasta fines de 2029; v15 hasta fines de 2027.
- `CONFIRMED` — El volumen `sites` incluye configuración del site, archivos privados/públicos, backups y logs de site; la base necesita su propio volumen persistente.
- `CONFIRMED` — `FRAPPE_PATH`, `FRAPPE_BRANCH`, `ERPNEXT_VERSION`, `DB_PASSWORD` o `DB_PASSWORD_SECRETS_FILE`, `DB_HOST`, `DB_PORT`, `REDIS_CACHE`, `REDIS_QUEUE`, variables Gunicorn, `FRAPPE_SITE_NAME_HEADER`, `SITES_RULE` y `LETSENCRYPT_EMAIL` están documentadas oficialmente.
- `CONFIRMED` — Tras DB saludable y la salida correcta de `configurator`, se crea un site mediante `bench new-site`; ERPNext puede instalarse con `--install-app erpnext`.
- `CONFIRMED` — Bench ofrece `backup`, `restore` y `migrate`; Frappe Docker documenta envío a S3-compatible y aclara que el restore es manual.
- `CONFIRMED` — DB debe estar saludable; Gunicorn/Socket.IO se comprueban por conectividad y workers/scheduler con `healthcheck.sh`.
- `CONFIRMED` — `pwd.yml` utiliza una configuración de demo y es desechable.

## Proposed approach for YuyaIQ

- `HYPOTHESIS` — Una configuración propia basada en `compose.yaml` y overrides oficiales podría ser preferible a operar una copia de `pwd.yml` para un entorno persistente.
- `HYPOTHESIS` — Una topología con proxy HTTPS como único punto público y servicios de datos en red interna podría ser apropiada para YuyaIQ.
- `HYPOTHESIS` — Frappe `version-16` y ERPNext con etiqueta exacta `v16.30.0` podrían servir como punto de partida, tras validar compatibilidad al implementar.
- `HYPOTHESIS` — Fijar la revisión de `frappe_docker` e imágenes, en vez de usar etiquetas flotantes, podría mejorar la reproducibilidad.
- `HYPOTHESIS` — Persistir `sites`, datos de base y estado TLS fuera de Git, manteniendo secretos fuera del repositorio, podría satisfacer requisitos operativos.
- `HYPOTHESIS` — Las plantillas podrían documentar nombres de variables sin versionar secretos.
- `HYPOTHESIS` — El bootstrap podría crear un site con dominio, contraseña de administrador y secretos reales en el momento aprobado para implementar.
- `HYPOTHESIS` — Un backup cifrado externo, restores probados y actualizaciones ensayadas en staging podrían reducir riesgo operativo.
- `HYPOTHESIS` — La recolección de logs de contenedor y site, con alertas de disponibilidad, colas, scheduler, recursos y backups, podría ser parte de la operación.

## Pending validations

- `PENDING` — Validar recursos, dominio, proveedor de TLS, almacenamiento de backups y requisitos de disponibilidad antes de implementación.
- `PENDING` — Seleccionar versiones concretas y comprobar su compatibilidad en el momento de implementar.
