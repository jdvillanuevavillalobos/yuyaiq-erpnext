# Ventas

## Línea base local

- `CONFIRMED` — Customer, Sales Order, Delivery Note y Sales Invoice existen nativamente; los tres documentos transaccionales están marcados como submittable y con seguimiento de cambios.
- `CONFIRMED` — Se creó `Customer LAB`, `ITEM-LAB-001` y la UOM `Nos` mediante ORM nativo. Para el item se creó el grupo raíz LAB mínimo `Items LAB`, porque el site no tenía grupos de ítems.
- `CONFIRMED` — La creación de Sales Order fue rechazada nativamente hasta existir un Fiscal Year activo para la compañía. Se creó el ejercicio LAB `2026` (2026-01-01 a 2026-12-31) asociado a ambas compañías.
- `CONFIRMED` — El Setup Wizard creó `Standard Selling` en PEN. Con ella se creó y envió `SAL-ORD-2026-00001`, se entregó mediante `MAT-DN-2026-00001` y se facturó mediante `ACC-SINV-2026-00001` (estado Unpaid).
- `CONFIRMED` — La Delivery Note produjo la salida de 1 unidad y asientos por 50 PEN en Stock In Hand/Cost of Goods Sold. La Sales Invoice produjo Debtors 100 PEN (debe) y Sales 100 PEN (haber).
