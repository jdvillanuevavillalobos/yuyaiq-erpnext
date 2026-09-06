# Observabilidad

## Investigación Docker realizada

- `CONFIRMED` — Frappe documenta comprobación por host/puerto para Gunicorn y Socket.IO, y `healthcheck.sh` para workers y scheduler.
- `HYPOTHESIS` — La futura operación podría recolectar logs de contenedor y de site, con rotación y alertas de disponibilidad, colas, scheduler, recursos y backups.

Estado de implementación: `PENDING`.

## Validación de laboratorio

- `CONFIRMED` — Los logs de contenedor se observaron mediante `docker compose logs`. El entorno también expone logs de Bench y logs específicos del site en el volumen `sites`.
- `CONFIRMED` — Los logs verificaron workers escuchando sus colas y WebSocket escuchando en el puerto interno `9000`; `bench doctor` confirmó dos workers online y el scheduler fue habilitado para `erpnext.localhost`.

## Evidencia local: trazabilidad de una transacción

- `CONFIRMED` — La transacción seleccionada fue Sales Invoice `ACC-SINV-2026-00001`: Company `YuyaIQ Comercio LAB`, owner y último modificador `Administrator`, creada el 2026-09-05 19:19:59, enviada (`docstatus=1`) y posteriormente pagada. Es una factura de 100 PEN para `Customer LAB`, con `update_stock=0`.
- `CONFIRMED` — La factura enlaza Sales Order `SAL-ORD-2026-00001`. Payment Entry `ACC-PAY-2026-00001` la asignó íntegramente por 100 PEN; Payment Ledger Entry conserva la pareja de movimientos de factura (+100) y pago (-100) para el cliente.
- `CONFIRMED` — GL Entry permite reconstruir los dos movimientos de la factura (Debtors +100 y Sales -100) y los dos del cobro (Cash +100 y Debtors -100). No existen Stock Ledger Entries para esta factura porque no actualiza stock.
- `CONFIRMED` — Version registra el envío de la factura y del pago. No se encontraron Activity Log ni Access Log referidos a esta factura durante la inspección; Access Log tenía cero filas en el site. La ausencia de esos registros no prueba que no hubiera actividad previa.

## Evidencia local: error técnico

- `CONFIRMED` — El único Error Log observado fue `ncu2qlrnbe`, creado el 2026-09-05 19:11:47 con método `Failed to create demo data`, antes de la transacción seleccionada. No tiene `reference_doctype`, `reference_name` ni `trace_id`.
- `CONFIRMED` — Su traceback ubica el flujo en ERPNext: `erpnext/setup/demo.py`, `setup_demo_data` línea 27 → `make_transactions` línea 136 → `create_transaction` línea 163. La validación terminó en `erpnext/accounts/utils.py`, `get_fiscal_years` línea 141, con `FiscalYearError` porque la Company demo no tenía ejercicio fiscal activo para su fecha de transacción.
- `CONFIRMED` — La clasificación provisional del error es `B. configuración faltante`: el traceback identifica la ausencia de un Fiscal Year activo para la Company demo. No hay evidencia de que afectara una transacción LAB ni de que sea un defecto del core.
- `CONFIRMED` — El Error Log corresponde a una solicitud HTTP de Setup Wizard procesada por la capa de aplicación Frappe/ERPNext. La salida actual de `backend`, `frontend`, `queue-short`, `queue-long`, `scheduler`, `websocket` y `mariadb` no conserva una coincidencia del incidente; por ello no se puede asociar el Error Log a una línea concreta de un contenedor, request ID o job ID.
- `CONFIRMED` — El volumen de logs contiene `bench.log`, `database.log`, `frappe.log`, `ipython.log`, `scheduler.log` y sus variantes rotadas en `/home/frappe/frappe-bench/logs/` y `/home/frappe/frappe-bench/sites/erpnext.localhost/logs/`. Ninguno aportó una coincidencia histórica segura para este Error Log.

## Gaps y propuesta mínima

- `PENDING` — No se demostró una correlación nativa y navegable documento ↔ Error Log: el registro analizado no incluye Company, DocType, documento ni trace ID, y la retención visible de contenedores no permitió correlacionarlo retrospectivamente.
- `PENDING` — No se demostró que Activity Log o Access Log cubran todas las lecturas, cambios o acciones de una transacción; Access Log sigue siendo un mecanismo específico, no una auditoría universal.
- `HYPOTHESIS` — Una arquitectura mínima futura podría registrar contexto estructurado de transacción: `correlation_id`/request ID, site, Company, usuario, DocType, nombre de documento, acción, servicio, `job_id` y `error_log_id`. Requiere diseño, validación y una decisión explícita antes de implementarse.
