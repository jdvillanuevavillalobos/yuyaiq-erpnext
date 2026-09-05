# Backups y recuperación ante desastre

## Investigación Docker realizada

- `CONFIRMED` — `bench --site <site> backup` crea respaldos en el site; la documentación oficial contempla envío del último backup a almacenamiento S3-compatible y restore manual.
- `CONFIRMED` — Las claves de cifrado de site y backup son necesarias para recuperar contraseñas y backups cifrados.
- `HYPOTHESIS` — Respaldar datos de base de datos, archivos privados/públicos y configuración, con copia cifrada fuera del host y pruebas periódicas de restore en un entorno aislado, podría ser apropiado.

Estado de implementación: `PENDING`.
