# Credenciales a Rotar - URGENTE

**Fecha de detección**: 2025-10-14
**Prioridad**: 🔴 CRÍTICA

## ⚠️ ADVERTENCIA
Las siguientes credenciales fueron encontradas expuestas en archivos del repositorio.
**DEBEN SER ROTADAS INMEDIATAMENTE** contactando a cada servicio.

---

## 1. Twilio (WhatsApp Notifications)

**Ubicación encontrada**: `docs/specs/APIs/Twilio/.env`

**Credenciales a rotar**:
- [ ] `TWILIO_ACCOUNT_SID`: AC********************************** (expuesto)
- [ ] `TWILIO_AUTH_TOKEN`: ******************************** (expuesto)
- [ ] `TWILIO_WHATSAPP_NUMBER`: +14155238886

**Acción requerida**:
1. Acceder a https://console.twilio.com
2. Ir a Account > API Keys & Tokens
3. Rotar Auth Token
4. Actualizar .env con nuevo valor
5. Verificar que .env NO se commitee

**Contacto**: support@twilio.com

---

## 2. Pipefy (Workflow Management)

**Ubicación encontrada**: `docs/specs/APIs/Pipefy/.env`

**Credenciales a rotar**:
- [ ] `PIPEFY_TOKEN`: eyJhbG******************************************[332 bytes] (expuesto)

**Acción requerida**:
1. Acceder a https://app.pipefy.com
2. Ir a Settings > Personal Access Tokens
3. Revocar token actual
4. Generar nuevo token
5. Actualizar .env con nuevo valor

**Contacto**: support@pipefy.com

---

## 3. Brave Search API

**Ubicación encontrada**: `.mcp.json`

**Credenciales a rotar**:
- [ ] `BRAVE_API_KEY`: BSAF**************************** (expuesto)

**Acción requerida**:
1. Acceder a https://api.search.brave.com/app/keys
2. Revocar API key actual
3. Generar nueva key
4. Actualizar variables de entorno (NO .mcp.json)

**Contacto**: api-support@brave.com

---

## 4. Firecrawl API

**Ubicación encontrada**: `.mcp.json`

**Credenciales a rotar**:
- [ ] `FIRECRAWL_API_KEY`: fc-********************************** (expuesto)

**Acción requerida**:
1. Acceder a https://firecrawl.dev/app/api-keys
2. Revocar API key actual
3. Generar nueva key
4. Actualizar variables de entorno

**Contacto**: support@firecrawl.dev

---

## 5. Supabase Access Token

**Ubicación encontrada**: `.mcp.json`

**Credenciales a rotar**:
- [ ] `SUPABASE_ACCESS_TOKEN`: sbp_**************************************** (expuesto)

**Acción requerida**:
1. Acceder a https://supabase.com/dashboard/account/tokens
2. Revocar token actual
3. Generar nuevo Personal Access Token
4. Actualizar variables de entorno

**Contacto**: support@supabase.com

---

## Checklist de Rotación

- [ ] Todas las credenciales rotadas
- [ ] Nuevos valores actualizados en .env (NO commiteado)
- [ ] Archivos originales con credenciales eliminados del historial Git
- [ ] .gitignore configurado correctamente
- [ ] Servicios funcionando con nuevas credenciales

---

## Después de Rotar

1. **Eliminar archivos con credenciales antiguas**:
```bash
rm docs/specs/APIs/Pipefy/.env
rm docs/specs/APIs/Twilio/.env
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch .mcp.json' \
  --prune-empty --tag-name-filter cat -- --all
```

2. **Actualizar .mcp.json sin credenciales**:
- Usar variables de entorno en lugar de valores hardcoded

3. **Verificar que .gitignore protege archivos**:
```bash
git status  # .env NO debe aparecer
```

---

*Documento generado por plan de seguridad - 2025-10-14*
