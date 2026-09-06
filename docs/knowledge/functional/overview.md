# Visión funcional

## Línea base local

- `CONFIRMED` — La instalación local incluye DocTypes nativos para Company, Customer, Supplier, Item, Sales Order, Sales Invoice, Purchase Order, Purchase Invoice, Stock Entry, Delivery Note, Payment Entry, GL Entry y Stock Ledger Entry.
- `CONFIRMED` — Sales Order, Sales Invoice, Purchase Order, Purchase Invoice, Stock Entry, Delivery Note y Payment Entry son submittable; sus metadatos también indican seguimiento de cambios.
- `CONFIRMED` — En este site, la creación nativa de Company requirió `Warehouse Type: Transit`; el código local lo instala como fixture del Setup Wizard. Se creó únicamente ese registro estándar para continuar el laboratorio.
- `CONFIRMED` — Se crearon las compañías LAB `YuyaIQ Comercio LAB` y `YuyaIQ Distribución LAB`, con país Perú, moneda PEN y plantilla contable estándar. Son datos de laboratorio, no decisiones de producto.
- `CONFIRMED` — Tras completar manualmente el Setup Wizard, el site contiene UOM, Territory, Customer Group, Supplier Group, Item Group, Mode of Payment y las listas de precios Standard Selling/Buying. Currency y Country ya estaban cargados. Estos prerequisitos ya no están pendientes.
- `CONFIRMED` — El Setup Wizard creó la Company adicional `Ferreteria San Martin`; no fue editada ni eliminada durante el laboratorio.
- `CONFIRMED` — Los flujos LAB de compra, recepción, venta, entrega, facturación y POS se ejecutaron con documentos enviados. Persisten validaciones específicas de permisos por URL directa y auditoría avanzada.
