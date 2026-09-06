# Contabilidad

## Línea base local

- `CONFIRMED` — Account, Journal Entry, GL Entry, Payment Entry, Sales Invoice y Purchase Invoice existen nativamente. Payment Entry, Sales Invoice y Purchase Invoice son submittable; GL Entry no lo es.
- `CONFIRMED` — Cada Company LAB generó su propio Chart of Accounts de plantilla estándar y tiene un ejercicio fiscal activo LAB 2026 asociado.
- `CONFIRMED` — Los flujos LAB produjeron GL Entries reales para Purchase Receipt, Purchase Invoice, Delivery Note y Sales Invoice, con Company `YuyaIQ Comercio LAB` y débitos/créditos balanceados.
- `CONFIRMED` — `ACC-PAY-2026-00001` liquidó `ACC-SINV-2026-00001`: Company Comercio, Party Type Customer, Party Customer LAB, paid/received 100 PEN, referencia asignada por 100 PEN y saldo 100→0. Sus GL Entries balancean Cash - YCL (debe 100) y Debtors - YCL (haber 100).
- `CONFIRMED` — Con Immutable Ledger desactivado en este site, cancelar una Purchase Receipt LAB produjo reversos GL dentro del mismo voucher: los cuatro registros quedaron marcados `is_cancelled=1`, incluyendo Stock In Hand y Stock Received But Not Billed por 60 PEN en sentidos opuestos.
- `PENDING` — Validar Journal Entry y el comportamiento equivalente cuando Immutable Ledger esté activado; este resultado no se extrapola a otras configuraciones.
