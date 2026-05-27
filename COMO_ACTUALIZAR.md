# Cómo publicar actualizaciones de LookOut Modern

## Estructura del repositorio GitHub recomendada

```
lookout-modern/           ← repositorio público en GitHub
├── lookout-modern-3.0.0.xpi   ← el .xpi empaquetado y firmado
├── updates.json               ← manifiesto de versiones
└── (código fuente si quieres)
```

## Pasos para lanzar una nueva versión

1. **Editar `manifest.json`**: incrementa `"version"` (ej. `"3.1.0"`).

2. **Empaquetar el `.xpi`** (es un ZIP con extensión `.xpi`):
   ```bash
   cd lookout-modern-3/
   zip -r ../lookout-modern-3.1.0.xpi . -x "*.DS_Store" -x "__MACOSX/*"
   ```

3. **Calcular el SHA256** del nuevo `.xpi`:
   ```bash
   sha256sum lookout-modern-3.1.0.xpi
   ```

4. **Actualizar `updates.json`** — añade una nueva entrada (o reemplaza la existente):
   ```json
   {
     "version": "3.1.0",
     "update_link": "https://raw.githubusercontent.com/TU_USUARIO/lookout-modern/main/lookout-modern-3.1.0.xpi",
     "update_hash": "sha256:RESULTADO_DEL_PASO_3",
     "browser_specific_settings": {
       "gecko": { "strict_min_version": "115.0" }
     }
   }
   ```

5. **Sube ambos archivos** (`lookout-modern-3.1.0.xpi` y `updates.json`) a GitHub.

6. Thunderbird revisará `updates.json` automáticamente y notificará al usuario.

## Notas sobre firma

- Si instalas el .xpi en modo desarrollador (`about:debugging`) no necesitas firma.
- Para distribución general sin AMO, necesitas firmar con tu propia clave o usar AMO.
- Para uso personal/empresa: activar `xpinstall.signatures.required = false` en `about:config` de Thunderbird.

## Cambiar la URL de actualizaciones

En `manifest.json` → `browser_specific_settings.gecko.update_url` apunta a la URL pública de `updates.json`.
Asegúrate de que sea accesible via HTTPS.
