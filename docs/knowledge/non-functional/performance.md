# Rendimiento

## Línea base del schema LAB

- `CONFIRMED` — El snapshot de `erpnext.localhost` ocupa aproximadamente 47.3 MiB: 19.4 MiB de datos y 27.9 MiB de índices. `tabDocField` ocupa 11.6 MiB y es la tabla mayor en este estado inicial.
- `CONFIRMED` — Los ledgers LAB aún son pequeños: GL Entry 22 filas, Stock Ledger Entry 8 y Payment Ledger Entry 5. Sus tablas tienen 9–15 índices; este snapshot solo describe su estructura y tamaño actual.
- `HYPOTHESIS` — Los ledgers, Version y logs podrían dominar crecimiento y coste de consulta conforme aumenten transacciones y retención. Requiere métricas de uso, planes de ejecución y carga real.

Estado: `PENDING` — pendiente de carga esperada y pruebas.
