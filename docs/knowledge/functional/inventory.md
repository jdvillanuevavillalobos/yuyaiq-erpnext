# Inventario

## Línea base local

- `CONFIRMED` — Item, Warehouse, Stock Entry y Stock Ledger Entry existen nativamente. Stock Entry es submittable y tiene seguimiento de cambios; Stock Ledger Entry es un ledger no submittable.
- `CONFIRMED` — Cada Company LAB creó sus propios almacenes nativos: All Warehouses, Stores, Work In Progress, Finished Goods y Goods In Transit; este último enlaza `Warehouse Type: Transit`.
- `CONFIRMED` — La recepción registró 2 unidades en `Stores - YCL`; la entrega de venta retiró 1. El Stock Entry `MAT-STE-2026-00002` transfirió la unidad restante desde Stores a Finished Goods. Stock Ledger Entry registró las cuatro variaciones observadas y el saldo de Stores quedó en 0.
- `CONFIRMED` — La Purchase Receipt aislada `MAT-PRE-2026-00002` recibió 1 unidad en Work In Progress y luego fue cancelada. Stock Ledger Entry registró +1 y el reverso -1 en el mismo voucher, ambos `is_cancelled=1` tras la cancelación.
- `CONFIRMED` — El amendment nativo `MAT-PRE-2026-00002-1` enlaza `amended_from = MAT-PRE-2026-00002`, fue enviado con rate LAB 61 y conserva el documento cancelado como antecedente.
