# Punto de venta

## Línea base local

- `CONFIRMED` — POS Profile existe como DocType nativo en el módulo Accounts.
- `CONFIRMED` — Se creó `POS LAB - Comercio` para YuyaIQ Comercio LAB con Finished Goods, Standard Selling, PEN y Cash. Para ello fue necesario asociar nativamente Cash con `Cash - YCL` en Mode of Payment.
- `CONFIRMED` — El flujo POS exige un POS Opening Entry enviado. La instancia configurada usa el modo POS basado en Sales Invoice; el sistema rechazó POS Invoice e indicó explícitamente crear Sales Invoice.
- `CONFIRMED` — Tras abrir `POS-OPE-2026-00001`, se creó y envió `ACC-SINV-2026-00002` como venta POS al contado por 100 PEN, con actualización de stock y Cash como pago.
