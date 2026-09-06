# Auditabilidad

## Línea base local

- `CONFIRMED` — Version, Activity Log, Access Log y Error Log existen como DocTypes nativos. Company, User, Role Profile, User Permission y los documentos transaccionales principales consultados tienen seguimiento de cambios en metadata.
- `CONFIRMED` — El site contiene registros Version y Activity Log. En Accounts Settings, `enable_immutable_ledger` está actualmente desactivado; no se cambió esta configuración.
- `CONFIRMED` — Version registró el envío y la cancelación de `MAT-PRE-2026-00002`; el documento amended conserva `amended_from`. Esto permite reconstruir el cambio de estado y la cadena original→amendment, pero no demuestra Audit Trail como auditoría universal.
- `CONFIRMED` — GL Entry responde por movimientos contables; Stock Ledger Entry por cantidades y valoración; Version por cambios documentales; Activity Log por actividad registrada; Error Log y logs de bench por eventos técnicos.
- `PENDING` — Access Log tiene código local para exportación, archivos, backups y PDF. Una sesión HTTP temporal de Sales User ejecutó `frappe.utils.print_format.report_to_pdf` con HTTP 200, pero no materializó una fila en Access Log al consultar el site. No se demostró cobertura de lecturas normales, de esa operación ni de todas las acciones autenticadas.
