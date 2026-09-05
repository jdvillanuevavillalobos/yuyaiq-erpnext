# YuyaIQ — reglas de trabajo

YuyaIQ es una plataforma empresarial para PYMEs peruanas. ERPNext/Frappe está siendo evaluado como plataforma base.

## Principios obligatorios

- Priorizar configuración nativa antes que personalización.
- Priorizar extensiones soportadas antes que modificar core.
- Nunca modificar el core de Frappe ni de ERPNext.
- Nunca trabajar directamente sobre `main`; usar ramas `feature/*`.
- Seguridad, auditoría, observabilidad, backups y capacidad de actualización son requisitos de arquitectura, no tareas opcionales posteriores.
- No afirmar que SUNAT funciona hasta contar con validación E2E.
- No inventar conocimiento faltante.

## Estado del conocimiento

Todo hecho, hipótesis o decisión debe identificarse como uno de los siguientes estados:

- `CONFIRMED`: confirmado por evidencia documentada.
- `HYPOTHESIS`: supuesto pendiente de validación.
- `YUYAIQ DECISION`: decisión tomada explícitamente para YuyaIQ.
- `PENDING`: información, decisión o validación pendiente.

## Flujo de trabajo

- Antes de implementar, leer los documentos relevantes de `docs/knowledge/` y `docs/decisions/`.
- Si el código o la base de datos contradicen la documentación, reportar el conflicto antes de asumir cuál es correcto.
- No investigar por Internet salvo instrucción explícita.
