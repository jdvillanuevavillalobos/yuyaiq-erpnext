# Seguridad

## Investigación Docker realizada

- `CONFIRMED` — Frappe Docker admite `DB_PASSWORD_SECRETS_FILE` para no establecer la contraseña de base de datos directamente como variable.
- `CONFIRMED` — La configuración de site contiene credenciales y no debe versionarse.
- `HYPOTHESIS` — Mantener secretos fuera de Git y usar contraseñas únicas, sin valores de demostración, podría ser necesario para el entorno futuro.
- `HYPOTHESIS` — Exigir HTTPS en staging y producción y exponer únicamente el proxy podría ser apropiado.
- `HYPOTHESIS` — Restringir CORS y referrers, sin usar comodines en producción, podría ser apropiado.

Estado de implementación: `PENDING`.
