# Actualizaciones

## Investigación Docker realizada

- `CONFIRMED` — Frappe Docker separa desarrollo, exploración y producción; para despliegues reales recomienda imágenes `custom` o `layered` cuando se requiere flexibilidad.
- `CONFIRMED` — Después de actualizar imágenes, la operación de site requiere `bench --site <site> migrate`.
- `HYPOTHESIS` — Fijar versiones exactas de imágenes, ensayar actualizaciones en staging, respaldar antes de migrar y conservar una estrategia de rollback validada podría reducir el riesgo de actualización.

Estado de implementación: `PENDING`.
