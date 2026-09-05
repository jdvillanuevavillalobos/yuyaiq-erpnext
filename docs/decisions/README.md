# Decisiones de arquitectura

Registrar aquí decisiones con contexto, alternativas, consecuencias y estado.

## Decision lifecycle

`HYPOTHESIS` → análisis / prueba / discusión → aprobación explícita → ADR en `docs/decisions/` → `YUYAIQ DECISION`

- Ningún agente puede crear una `YUYAIQ DECISION` por inferencia.
- Una recomendación técnica no equivale a una decisión.
- Toda decisión arquitectónica o funcional relevante debe quedar registrada como ADR.
- Si una decisión cambia, no se borra el historial: se crea una nueva ADR que supersede la anterior.
- Las decisiones deben enlazar la evidencia o conocimiento relevante de `docs/knowledge/`.
- Si existe conflicto entre una ADR y una hipótesis, prevalece la ADR vigente.
- Si no existe decisión, el agente debe tratar el tema como `HYPOTHESIS` o `PENDING`.

Estado: `PENDING` — aún no hay registros de decisión formales.
