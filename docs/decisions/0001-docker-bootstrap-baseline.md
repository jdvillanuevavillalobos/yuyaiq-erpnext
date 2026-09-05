# ADR 0001: Baseline de imagen para docker-bootstrap

## Estado

`YUYAIQ DECISION` — aprobada para `feature/docker-bootstrap`.

## Decisión

El laboratorio Docker usará como baseline exacto la imagen `frappe/erpnext:v16.31.1`.

- ERPNext esperado: `v16.31.1`.
- Frappe: no se fijará ni inferirá una versión separada. Se usará la versión incluida oficialmente en la imagen y se verificará mediante `bench version` una vez levantado el entorno.

## Alcance

Esta decisión aplica únicamente al laboratorio de `docker-bootstrap`. No congela la versión de producción de YuyaIQ.

## Evidencia relacionada

- [Conocimiento Docker](../knowledge/platform/docker.md)
- [Fuentes de la investigación Docker](../knowledge/sources.md)

## Pendiente

`PENDING` — decidir y registrar por separado la versión definitiva de producción de YuyaIQ.
