# Modelo de permisos

## Línea base local

- `CONFIRMED` — Los permisos por rol se almacenan como DocPerm y cubren Read, Write, Create, Delete, Submit, Cancel, Amend, Print, Email, Report, Import, Export y Share.
- `CONFIRMED` — User Permission y Sharing son mecanismos distintos de DocPerm, representados por DocTypes nativos.
- `CONFIRMED` — User Permission permite `allow` sobre un DocType y `for_value` sobre su registro; los registros LAB creados para ventas restringen Company y Warehouse.
- `CONFIRMED` — El código local consulta `System Settings.disable_document_sharing`; el ajuste fue identificado pero no modificado.
- `CONFIRMED` — Administrator no equivale a un User con System Manager: `frappe.permissions.has_permission` permite todo explícitamente a Administrator y `get_roles` le devuelve todas las roles disponibles. System Manager obtiene permisos por sus DocPerm, incluido mantenimiento de User, pero no ese bypass.
- `CONFIRMED` — User Permission por Company restringió list, read efectivo y create de Sales Order: Comercio fue permitido y Distribución denegado. Sharing nativo sobre el documento de Distribución cambió temporalmente read de denegado a permitido y la revocación restauró la denegación. `disable_document_sharing` sigue desactivado.
- `HYPOTHESIS` — Roles, Role Profiles, Role Permissions, User Permissions y Sharing son suficientes como base nativa del MVP si se aplica hardening explícito al directorio User y se gobierna el uso de Sharing; no se demostró necesidad de un módulo custom de gestión de usuarios.
- `CONFIRMED` — Con sesión HTTP temporal de Sales User, el recurso REST y la carga de formulario directa para `SAL-ORD-2026-00002` (Distribución) devolvieron HTTP 403 por falta de permiso de lectura. Conocer el URL/nombre no eludió User Permission.
- `PENDING` — Validar documentos enlazados de Distribución. Company no se ha evaluado como frontera de tenant.
