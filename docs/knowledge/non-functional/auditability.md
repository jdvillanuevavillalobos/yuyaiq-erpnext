# Auditabilidad

## Línea base local

- `CONFIRMED` — Version, Activity Log, Access Log y Error Log existen como DocTypes nativos. Company, User, Role Profile, User Permission y los documentos transaccionales principales consultados tienen seguimiento de cambios en metadata.
- `CONFIRMED` — El site contiene registros Version y Activity Log. En Accounts Settings, `enable_immutable_ledger` está actualmente desactivado; no se cambió esta configuración.
- `CONFIRMED` — Version registró el envío y la cancelación de `MAT-PRE-2026-00002`; el documento amended conserva `amended_from`. Esto permite reconstruir el cambio de estado y la cadena original→amendment, pero no demuestra Audit Trail como auditoría universal.
- `CONFIRMED` — GL Entry responde por movimientos contables; Stock Ledger Entry por cantidades y valoración; Version por cambios documentales; Activity Log por actividad registrada; Error Log y logs de bench por eventos técnicos.
- `PENDING` — Access Log tiene código local para exportación, archivos, backups y PDF. Una sesión HTTP temporal de Sales User ejecutó `frappe.utils.print_format.report_to_pdf` con HTTP 200, pero no materializó una fila en Access Log al consultar el site. No se demostró cobertura de lecturas normales, de esa operación ni de todas las acciones autenticadas.

## Evidencia local de reconstrucción

- `CONFIRMED` — Para Sales Invoice `ACC-SINV-2026-00001`, los campos de documento conservan Company, owner, creación, última modificación, modificador, estado y `docstatus`. La factura enlaza Sales Order `SAL-ORD-2026-00001`; el pago `ACC-PAY-2026-00001` y Payment Ledger Entry permiten demostrar la liquidación total de 100 PEN.
- `CONFIRMED` — Version conserva el cambio de la factura de borrador a enviada y el del Payment Entry de borrador a validado. La factura no tiene `amended_from`; por lo tanto, no hay cadena cancel/amend aplicable a este caso.
- `CONFIRMED` — Audit Trail existe como DocType single nativo, restringido a System Manager. Su código compara documentos a través de la cadena `amended_from` y Version; no crea por sí mismo una bitácora independiente de toda actividad.
- `CONFIRMED` — El Error Log `ncu2qlrnbe` conserva método, fecha, usuario owner, traceback, fingerprint y metadata. No contiene referencia documental ni trace ID, de modo que no puede vincularse de forma demostrable con la factura seleccionada.
- `PENDING` — Definir requisitos de retención, búsqueda y correlación entre documentos de negocio, Error Log y logs de contenedor. La evidencia actual permite reconstruir la transacción contable elegida, pero no afirmar una auditoría técnica completa de cada operación.
