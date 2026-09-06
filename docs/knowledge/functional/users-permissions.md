# Usuarios y permisos

## Línea base local

- `CONFIRMED` — User, Role, Role Profile y User Permission son DocTypes nativos; User, Role Profile y User Permission tienen `track_changes` activo.
- `CONFIRMED` — Los roles relevantes presentes incluyen Sales User/Manager, Purchase User/Manager, Stock User/Manager, Accounts User/Manager y System Manager.
- `CONFIRMED` — Un DocType admite múltiples permisos por rol y nivel mediante DocPerm. En Sales Order, Sales User tiene create/write/submit/cancel/amend; Accounts User tiene acceso de lectura; Sales Manager tiene permisos más amplios.
- `CONFIRMED` — Role Profile agrupa roles y su código local sincroniza perfiles asignados hacia usuarios. User Permission es un DocType distinto del modelo de permisos por rol.
- `CONFIRMED` — Se crearon cuatro usuarios ficticios habilitados, con idioma `es` y zona horaria `America/Lima`: ventas, compras, inventario y administración. Sus perfiles LAB usan exclusivamente roles nativos y ninguno recibió Administrator.
- `CONFIRMED` — En v16, `role_profiles` es una tabla multiselección: al añadir `LAB - Inventario` al usuario de ventas que ya tenía `LAB - Ventas`, ambos perfiles y sus roles se combinaron. Añadir un rol manual mientras existía el perfil no lo conservó al guardar; este comportamiento se observó localmente y requiere una prueba de sesión separada antes de generalizarlo.
- `CONFIRMED` — El usuario de ventas tiene User Permission nativo para `Company = YuyaIQ Comercio LAB` (por defecto) y `Warehouse = Stores - YCL`.
- `CONFIRMED` — En el contexto autenticado de `lab.ventas@example.invalid`, Customer, Item y Sales Order se pudieron listar; Purchase Order y Purchase Invoice devolvieron PermissionError y System Settings no tuvo permiso de lectura.
- `CONFIRMED` — Sales User pudo listar User y recibió nombre, email, full name, enabled, user type y role profile de cuatro usuarios LAB. La API de cliente denegó abrir otro User y Administrator; crear, borrar o modificar otro User también fue denegado.
- `CONFIRMED` — El intento de cambiar el `role_profile_name` propio devolvió éxito, pero la inspección posterior confirmó que `role_profiles` y el rol efectivo siguieron siendo `LAB - Ventas` / Sales User. No se demostró escalamiento de privilegios.
- `CONFIRMED` — El código local da `select` de User a Desk User y el hook permite consultar cuentas no estándar; excluye Administrator y Guest. System Manager es quien tiene Read/Write/Create sobre User. La exposición del directorio básico requiere hardening si la política MVP no permite mostrar emails o perfiles a usuarios de ventas.
- `CONFIRMED` — Sales User listó Sales Orders de Comercio y creó `SAL-ORD-2026-00003`; no listó, no tuvo read efectivo y no pudo crear Sales Orders de Distribución. La lectura directa mediante `frappe.get_doc` desde consola no equivale a una operación del cliente y no debe tomarse como bypass HTTP.
- `CONFIRMED` — Sharing otorgó temporalmente read sobre una Sales Order de Distribución y, tras revocarlo, la lectura volvió a denegarse.
- `CONFIRMED` — Con sesión HTTP temporal de Sales User, `GET /api/resource/Sales%20Order/SAL-ORD-2026-00002` y `GET /api/method/frappe.desk.form.load.getdoc?...` devolvieron HTTP 403 con mensaje de falta de permiso de lectura. Conocer el nombre no permitió acceder al documento de Distribución.
- `PENDING` — La ruta SPA `/app/sales-order/...` redirigió (HTTP 301) y no constituye evidencia de autorización por sí misma; la autorización efectiva se confirmó en los endpoints de recurso/carga. Company no se debe tratar como frontera de tenant.
