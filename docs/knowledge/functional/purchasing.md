# Compras

## Línea base local

- `CONFIRMED` — Supplier, Purchase Order y Purchase Invoice existen nativamente; Purchase Order y Purchase Invoice son submittable y tienen seguimiento de cambios.
- `CONFIRMED` — El Setup Wizard creó `Standard Buying` en PEN. Con `Supplier LAB` se creó y envió `PUR-ORD-2026-00001`, se recibió mediante `MAT-PRE-2026-00001` y se facturó mediante `ACC-PINV-2026-00001` (estado Unpaid).
- `CONFIRMED` — La Purchase Receipt ingresó 2 unidades valorizadas en 50 PEN y produjo Stock In Hand 100 PEN (debe) / Stock Received But Not Billed 100 PEN (haber). La Purchase Invoice compensó Stock Received But Not Billed contra Creditors por 100 PEN.
