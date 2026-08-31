# 2026-08-28 CyberLens v0.6.0

## Características públicas

### Variant Publish ELITE
- **Upsert por SHA256**: Implementación de upsert (insert or update) usando SHA256 como clave primaria para evitar duplicados
- **Metadatos de variante**: Almacena `parent_primitive`, `variant_metadata` con `xor_key`, `spray_count`, `groom_order`
- **SIGMA mapping estable**: Mapeo a técnicas MITRE ATT&CK (T1210, T1203) con IDs estables por SHA256
- **Evasion Score**: Cálculo automático de `detection_evasion_score` (0-99) basado en diferencia de bytes vs variante base
- **Status ELITE**: Clasificación automática cuando `evasion_score >= 85` → status ELITE, sino STANDARD
- **API REST**: Endpoints `/api/variants/list`, `/api/findings/upsert`, `/api/findings/{id}/patch`, `/api/findings?sha256={sha}`

### C2DECT Adapter Upsert
- **Sincronización de hallazgos**: Sincroniza exploits y findings del motor de weaponización hacia CyberLens
- **Upsert por SHA256**: Evita duplicados usando SHA256 del binario como clave primaria
- **Mapeo MITRE automático**: Mapeo automático de técnicas explotadas a IDs MITRE ATT&CK (T1210, T1203)
- **Feed de hallazgos**: Endpoint `/api/findings` con paginación y filtros por `sha256`, `technique_id`, `status`

### Dashboard Cobertura
- **Matriz MITRE ATT&CK v16**: Visualización de 46 técnicas (38 ATT&CK v16 + 8 ATLAS)
- **Cobertura por equipo**: Red team (ejecución exitosa), Blue team (detección MTTA, falsos positivos), Purple team (cobertura, gaps)
- **Cobertura por fase**: Acceso inicial → LPE → Credenciales → Movimiento lateral
- **Métricas por equipo**: Red (éxito/fallo, duración, nodos), Blue (MTTA, falsos positivos), Purple (cobertura, gaps, alertas abiertas)
- **SIGMA YAML**: Exportación/importación de reglas SIGMA v2 con validación YAML

### Mejoras internas (no expuestas públicamente)
- **Upsert por SHA256**: Eliminación de duplicados en hallazgos y reglas SIGMA
- **SIGMA YAML estable**: IDs determinísticos basados en SHA256 para reglas
- **Upsert por finding**: `PATCH /api/findings/{id}` o `POST /api/finding-add` con idempotencia
- **Dashboard**: Métricas por equipo (Red/Blue/Purple) con gráficos de tendencia