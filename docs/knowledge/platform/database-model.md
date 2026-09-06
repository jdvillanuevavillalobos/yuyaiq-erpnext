# Modelo de datos de ERPNext/Frappe

## Alcance de esta línea base

- `CONFIRMED` — Esta evidencia proviene exclusivamente del site LAB `erpnext.localhost`, inspeccionado sin cambios de datos ni schema. No describe un dimensionamiento de producción.
- `CONFIRMED` — El site usa la base `_be1ae98086729644` en MariaDB `11.8.9-MariaDB-ubu2404`, con `utf8mb4` y `utf8mb4_unicode_ci`.
- `CONFIRMED` — El snapshot contiene 737 tablas base, aproximadamente 49,634,284 bytes (47.3 MiB): 20,358,124 bytes de datos y 29,276,160 bytes de índices.
- `CONFIRMED` — 734 tablas son InnoDB. `tabError Log`, `tabData Import Log` y `__global_search` son MyISAM; por tanto, no todas las tablas del site comparten semántica transaccional InnoDB.
- `CONFIRMED` — 734 tablas tienen prefijo `tab`, patrón que representa tablas de DocType; existen además tres tablas internas.

## Inventario resumido

- `CONFIRMED` — `information_schema.TABLES.table_rows` es una estimación para tablas InnoDB. En este LAB, las mayores estimaciones son `tabDocField` (13,875), `tabDocPerm` (1,008), `tabSingles` (1,005), `tabDocType` (811), `tabPatch Log` (760) y `tabHas Role` (680).
- `CONFIRMED` — Por tamaño, `tabDocField` domina el snapshot con 12,140,544 bytes. Le siguen `tabPrint Format` (507,904), `tabDocType` (376,832), `tabPatch Log` (311,296), `tabDocPerm` (278,528), `tabSingles` (262,144) y los ledgers/tablas de ítems con 131,072–245,760 bytes por sus índices.
- `CONFIRMED` — Los conteos exactos actuales de interés funcional son: GL Entry 22, Stock Ledger Entry 8, Payment Ledger Entry 5, Version 34, Activity Log 8, Error Log 1 y Access Log 0. El LAB aún no permite inferir volúmenes operativos futuros.
- `CONFIRMED` — Las tablas más indexadas del snapshot son GL Entry (15 índices), Purchase Receipt Item (15), Payment Ledger Entry (14), Purchase Invoice Item (13), Delivery Note Item (12) y Sales Invoice Item (12).

## Mapa por dominio

- `CONFIRMED` — Core/framework: `tabUser`, `tabRole`, `tabHas Role`, `tabDocPerm`, `tabUser Permission`, `tabVersion`, `tabError Log`, `tabActivity Log` y `tabAccess Log` existen como DocTypes nativos.
- `CONFIRMED` — Sales: `tabCustomer`, `tabSales Order`, `tabSales Order Item`, `tabDelivery Note`, `tabDelivery Note Item`, `tabSales Invoice` y `tabSales Invoice Item` existen. Sales Order y Sales Invoice son documentos enviados; sus Item son child tables.
- `CONFIRMED` — Purchasing: `tabSupplier`, `tabPurchase Order`, `tabPurchase Order Item`, `tabPurchase Receipt`, `tabPurchase Receipt Item`, `tabPurchase Invoice` y `tabPurchase Invoice Item` existen.
- `CONFIRMED` — Inventory: `tabItem`, `tabWarehouse`, `tabBin`, `tabStock Entry`, `tabStock Entry Detail` y `tabStock Ledger Entry` existen.
- `CONFIRMED` — Accounting: `tabCompany`, `tabAccount`, `tabGL Entry`, `tabPayment Entry`, `tabPayment Ledger Entry` y `tabJournal Entry` existen.
- `CONFIRMED` — POS: `tabPOS Profile` y `tabPOS Opening Entry` tienen una fila LAB; `tabPOS Invoice` existe pero no tiene filas porque el flujo LAB utilizó Sales Invoice en modo POS.

## Patrón padre / hijo

- `CONFIRMED` — Las child tables usan su propio `name` y almacenan `parent`, `parenttype`, `parentfield` e `idx`; `parent` está indexado en las tablas de ítems inspeccionadas. `idx` ordena las filas dentro del documento.
- `CONFIRMED` — En `tabSales Invoice Item`, la fila `s6u6i9d32s` tiene `parent=ACC-SINV-2026-00001`, `parenttype=Sales Invoice`, `parentfield=items` e `idx=1`.
- `CONFIRMED` — En `tabSales Order Item`, `rtajaiiqcl` usa el mismo patrón con `parent=SAL-ORD-2026-00001`.
- `CONFIRMED` — En `tabStock Entry Detail`, `sfrqu9krfo` usa `parent=MAT-STE-2026-00002`, `parenttype=Stock Entry`, `parentfield=items` e `idx=1`.

## Claves y relaciones

- `CONFIRMED` — En Sales Invoice, Sales Invoice Item, GL Entry, Stock Ledger Entry, Payment Entry, User y Company, `name` es la primary key física: `varchar(140) NOT NULL`.
- `CONFIRMED` — `information_schema.KEY_COLUMN_USAGE` no reportó foreign keys físicas en la base del site. Los Link fields de DocType, los campos de child tables y los pares voucher/documento son relaciones lógicas, no FKs declaradas.
- `CONFIRMED` — La ausencia de FKs físicas no equivale a ausencia de integridad: el schema observado aporta tipos, PKs, índices y unicidades, mientras Frappe aplica validaciones y comportamiento documental en la capa de aplicación.
- `CONFIRMED` — Ejemplos de relaciones lógicas observadas: Sales Invoice Item.`sales_order` → Sales Order; Sales Invoice Item.`so_detail` → Sales Order Item; Payment Entry Reference.`reference_doctype` + `reference_name` → Sales Invoice; Payment Ledger Entry.`against_voucher_type` + `against_voucher_no` → Sales Invoice; GL Entry.`voucher_type` + `voucher_no` → voucher; Stock Ledger Entry.`voucher_type` + `voucher_no` + `voucher_detail_no` → Stock Entry y su Detail.
- `CONFIRMED` — `tabBin` tiene la restricción única física `unique_item_warehouse(item_code, warehouse)`. User tiene PK `name` e índices únicos sobre `api_key`, `mobile_no` y `username` en este schema.

## Índices críticos observados

- `CONFIRMED` — Sales Invoice tiene PK `name` e índices por `creation`, `customer`, `debit_to`, `posting_date`, `project`, `return_against` e `inter_company_invoice_reference`. Sales Order tiene PK `name` e índices por `creation`, `customer`, `status`, `transaction_date`, `project` e intercompany reference.
- `CONFIRMED` — Sales Invoice Item tiene PK `name`, índice `parent` y enlaces indexados como `item_code`, `sales_order`, `delivery_note`, `dn_detail` y `so_detail`.
- `CONFIRMED` — GL Entry tiene 15 índices, incluyendo PK, `account`, `company`, `party`, `posting_date`, compuesto `(posting_date, company)`, `voucher_no` y compuesto `(voucher_type, voucher_no)`.
- `CONFIRMED` — Stock Ledger Entry tiene PK, índices por `voucher_type`, compuesto `(voucher_no, voucher_type)`, `voucher_detail_no` y el compuesto `(item_code, warehouse, posting_datetime, creation)`.
- `CONFIRMED` — Payment Ledger Entry tiene PK, índices por `company`, `party`, `posting_date`, `voucher_type`, `voucher_no`, `(voucher_no, voucher_type)`, `against_voucher_type`, `against_voucher_no` y `(against_voucher_no, against_voucher_type)`.
- `CONFIRMED` — Version tiene PK, `creation` y compuesto `(ref_doctype, docname)`. Activity Log tiene PK, `creation` y compuestos para `(reference_doctype, reference_name)` y timeline. Error Log tiene PK y `creation`.
- `PENDING` — No se evaluaron planes de ejecución ni carga concurrente; no hay evidencia para proponer índices adicionales.

## Trazabilidad LAB desde relaciones SQL/ORM

- `CONFIRMED` — `ACC-SINV-2026-00001` se une a su Item por Sales Invoice Item.`parent`. La fila Item enlaza `sales_order=SAL-ORD-2026-00001` y `so_detail=rtajaiiqcl`, que corresponde a Sales Order Item.`name`.
- `CONFIRMED` — Payment Entry Reference enlaza la factura mediante `reference_doctype=Sales Invoice` y `reference_name=ACC-SINV-2026-00001`; su `parent` identifica Payment Entry `ACC-PAY-2026-00001`.
- `CONFIRMED` — Payment Ledger Entry conserva dos relaciones hacia la factura: el voucher de Sales Invoice por 100 y el Payment Entry por -100, ambos mediante `against_voucher_type` y `against_voucher_no`.
- `CONFIRMED` — GL Entry se recupera con `voucher_type=Sales Invoice` y `voucher_no=ACC-SINV-2026-00001`; Version con `(ref_doctype, docname)`. La reconstrucción requiere al menos seis relaciones lógicas/tablas, no una sola FK navegable.
- `CONFIRMED` — `MAT-STE-2026-00002` se une a Stock Entry Detail por `parent`; su Detail `sfrqu9krfo` enlaza Item, origen `Stores - YCL` y destino `Finished Goods - YCL`.
- `CONFIRMED` — Stock Ledger Entry usa `voucher_type=Stock Entry`, `voucher_no=MAT-STE-2026-00002` y `voucher_detail_no=sfrqu9krfo`: produjo -1 en Stores y +1 en Finished Goods. Bin se consulta mediante `item_code` + `warehouse`, respaldado por su índice único.

## Ledgers y crecimiento

- `CONFIRMED` — GL Entry tiene 22 filas y registra Company, cuenta, voucher, fecha de contabilización, party, débito y crédito. Las transacciones LAB generan filas nuevas; no se demostró inmutabilidad universal.
- `CONFIRMED` — Stock Ledger Entry tiene 8 filas y registra Company, item, warehouse, voucher, detalle, fecha/hora, cantidad, valoración y cancelación. Las transacciones LAB generan filas nuevas; no se demostró inmutabilidad universal.
- `CONFIRMED` — Payment Ledger Entry tiene 5 filas y registra Company, party, account, voucher/against voucher, fecha y monto. El pago LAB genera la pareja factura/pago observada.
- `HYPOTHESIS` — GL Entry, Stock Ledger Entry, Payment Ledger Entry, Version, Error Log, Activity Log y tablas de jobs son candidatas a crecimiento sostenido en una operación real por su función histórica o de eventos. Se requiere medición de carga y retención para cuantificarlo.

## Integridad y transacciones

- `CONFIRMED` — **DB ENFORCED:** MariaDB aplica los tipos, PKs, índices y restricciones únicas observadas, como `tabBin(item_code, warehouse)`. No se observaron FKs físicas para imponer los links funcionales.
- `CONFIRMED` — **APPLICATION ENFORCED:** DocType metadata declara Link fields; Link validation, permisos, lifecycle, validación de negocio y comportamiento documental residen en Frappe. El código de request hace rollback ante excepción; para métodos HTTP mutables realiza commit y para los demás rollback.
- `CONFIRMED` — **APPLICATION ENFORCED:** Los background jobs hacen rollback ante excepción, registran Error Log y hacen commit de ese registro; en éxito hacen commit. `frappe.db.savepoint` y rollback a savepoint existen como mecanismos locales.
- `CONFIRMED` — **LOGICAL CONVENTION:** `parent`/`parenttype`/`parentfield`, `voucher_type` + `voucher_no`, `ref_doctype` + `docname`, `against_voucher_type` + `against_voucher_no` y los Link fields conectan dominios sin FK física.
- `PENDING` — La cobertura completa y la resistencia de las validaciones de aplicación no se evaluaron exhaustivamente; esta línea base no afirma que sean infalibles.

## Reporting, BI y robustez

- `CONFIRMED` — Consultar con ORM/DocType metadata permite conocer child tables y Link fields; las consultas de negocio requieren varios joins o filtros por pares lógicos.
- `CONFIRMED` — Las reglas de permisos, validaciones y ciclo de vida están en Frappe, no en foreign keys físicas. Una consulta SQL directa no ejecuta esas comprobaciones de aplicación.
- `HYPOTHESIS` — Reporting operativo debería preferir APIs/ORM o mecanismos soportados que preserven contexto y permisos; una capa de lectura externa requeriría control de acceso y definición explícita de joins, actualización y retención.
- `HYPOTHESIS` — Para escritura, debería preferirse el ORM de Frappe, las APIs soportadas, los métodos de DocType y los hooks soportados, ya que preservan permisos, validaciones y lifecycle; escribir por SQL directo omitiría esas comprobaciones.
- `CONFIRMED` — **A1. Integridad SQL/database-level: MEDIA.** Hay tipos, PKs, índices y algunas unicidades, pero no FKs físicas para los enlaces funcionales.
- `CONFIRMED` — **A2. Integridad application-level Frappe: MEDIA.** Se observaron Link fields, permisos, lifecycle y validaciones documentales; su cobertura completa y comportamiento ante todos los casos límite permanecen PENDING.
- `CONFIRMED` — **B. Trazabilidad: MEDIA.** Voucher, child rows, Version y ledgers permiten reconstrucciones, pero el enlace documento ↔ Error Log no está garantizado.
- `CONFIRMED` — **C. Separación por dominio: FUERTE.** Los módulos inspeccionados separan Core, Selling, Buying, Stock, Accounts y Setup en DocTypes/tablas.
- `CONFIRMED` — **D. Consistencia de nombres: FUERTE.** El prefijo `tab`, PK `name`, campos estándar y patrón parent-child fueron consistentes en las tablas inspeccionadas.
- `CONFIRMED` — **E. Ledgers: FUERTE.** Hay tres ledgers especializados con claves de voucher y Company, además de índices orientados a consulta.
- `CONFIRMED` — **F. Extensibilidad: FUERTE.** DocType y DocField metadata hacen explícito el modelo y los Link fields sin requerir FKs físicas para cada relación.
- `CONFIRMED` — **G. Reporting: MEDIA.** Es consultable, pero requiere comprender metadata, child tables, pares lógicos y límites de permisos.
- `CONFIRMED` — **H. Mantenibilidad: MEDIA.** El modelo es uniforme, aunque las relaciones cruzadas y ausencia de FKs exigen disciplina y conocimiento de Frappe.
- `PENDING` — **I. Performance potencial:** no hay pruebas de carga ni planes de ejecución.
- `CONFIRMED` — **J. Riesgo de SQL directo: MEDIA.** El riesgo operativo proviene de que puede leer relaciones sin ejecutar permisos, validaciones ni lifecycle de Frappe; debe tratarse como acceso privilegiado, no como una vulnerabilidad intrínseca de ERPNext.
