# Plan de Migración a Poetry

**Fecha de creación**: 2025-10-14
**Prioridad**: 🟡 Media (recomendado para Fase 2 del proyecto)
**Esfuerzo estimado**: 2-3 horas

---

## Contexto

Actualmente el proyecto usa **pip + requirements.txt** para gestión de dependencias. Si bien esto funciona, presenta limitaciones:

### Problemas Actuales
- ❌ **Resolución manual de conflictos**: pip no resuelve dependencias automáticamente
- ❌ **Lock file separado**: requirements-frozen.txt es un snapshot, no un lock file verdadero
- ❌ **Múltiples archivos**: requirements.txt, requirements-dev.txt, requirements-frozen.txt
- ❌ **Sin gestión de proyectos**: No hay metadata del proyecto centralizada
- ❌ **Virtualenv manual**: Requiere creación y activación manual de entornos virtuales

### Beneficios de Poetry
- ✅ **Resolución automática de dependencias**: Poetry resuelve conflictos automáticamente
- ✅ **Lock file determinista**: `poetry.lock` garantiza builds reproducibles
- ✅ **Un solo archivo**: `pyproject.toml` centraliza todo (deps, metadata, config)
- ✅ **Gestión de entornos integrada**: Poetry crea y gestiona virtualenvs automáticamente
- ✅ **Compatibilidad con PEP 517/518**: Estándar moderno de empaquetado Python
- ✅ **Comandos simplificados**: `poetry add`, `poetry install`, `poetry run`

---

## Plan de Migración

### Fase 1: Instalación y Setup (30 min)

#### 1.1 Instalar Poetry
```bash
# Linux/macOS/WSL
curl -sSL https://install.python-poetry.org | python3 -

# Verificar instalación
poetry --version
```

#### 1.2 Configurar Poetry
```bash
# Crear virtualenvs dentro del proyecto (recomendado)
poetry config virtualenvs.in-project true

# Verificar configuración
poetry config --list
```

### Fase 2: Conversión del Proyecto (1-1.5 horas)

#### 2.1 Inicializar Poetry en el Proyecto
```bash
cd /path/to/aquila_project
poetry init --no-interaction
```

Esto creará `pyproject.toml` básico. Luego editar manualmente:

```toml
[tool.poetry]
name = "aquila"
version = "0.1.0"
description = "Sistema de análisis de crédito automatizado con IA"
authors = ["Capital Financas <tech@capitalfinancas.com.br>"]
readme = "README.md"
packages = [{include = "aquila_app"}, {include = "aquila_bureaus_sdk"}]

[tool.poetry.dependencies]
python = "^3.10"
# Copiar de requirements.txt (ver sección 2.2)

[tool.poetry.group.dev.dependencies]
# Copiar de requirements-dev.txt (ver sección 2.3)

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

#### 2.2 Migrar Dependencias de Producción
Convertir `requirements.txt` a Poetry:

```bash
# Opción 1: Manual (recomendado para control)
poetry add fastapi uvicorn anthropic openai google-generativeai
poetry add aiohttp httpx aioredis supabase
poetry add twilio pyyaml PyJWT cryptography

# Opción 2: Semi-automático
cat requirements.txt | grep -v "^#" | grep -v "^$" | xargs poetry add
```

#### 2.3 Migrar Dependencias de Desarrollo
Convertir `requirements-dev.txt`:

```bash
poetry add --group dev pytest pytest-asyncio pytest-cov pytest-mock
poetry add --group dev black flake8 isort mypy
poetry add --group dev pre-commit bandit safety
poetry add --group dev ipython ipdb python-dotenv
```

#### 2.4 Generar Lock File
```bash
# Resolver y crear poetry.lock
poetry lock

# Verificar que todo está correcto
poetry check
```

### Fase 3: Actualizar Workflows (30 min)

#### 3.1 Actualizar GitHub Actions
Modificar `.github/workflows/ci.yml`:

```yaml
- name: Install dependencies
  run: |
    pip install poetry
    poetry install
```

#### 3.2 Actualizar Pre-commit
Agregar a `.pre-commit-config.yaml`:

```yaml
- repo: https://github.com/python-poetry/poetry
  rev: 1.7.0
  hooks:
    - id: poetry-check
    - id: poetry-lock
      args: ['--no-update']
```

#### 3.3 Actualizar Documentación
Crear `docs/DevOps/POETRY_USAGE.md` con guía rápida:

```markdown
# Uso de Poetry en Aquila

## Instalación de Dependencias
\`\`\`bash
poetry install              # Instala todas las deps (prod + dev)
poetry install --no-dev     # Solo producción
\`\`\`

## Agregar Nueva Dependencia
\`\`\`bash
poetry add requests         # Producción
poetry add --group dev pytest  # Desarrollo
\`\`\`

## Ejecutar Comandos
\`\`\`bash
poetry run pytest           # Ejecutar tests
poetry run python script.py # Ejecutar script
poetry shell                # Activar virtualenv
\`\`\`

## Actualizar Dependencias
\`\`\`bash
poetry update               # Actualizar todas
poetry update requests      # Actualizar específica
\`\`\`
\`\`\`

### Fase 4: Testing y Validación (30-45 min)

#### 4.1 Crear Entorno Limpio
```bash
# Eliminar virtualenv anterior
rm -rf venv/ .venv/

# Instalar desde cero con Poetry
poetry install

# Verificar que funciona
poetry run python -c "import aquila_app; print('✅ OK')"
```

#### 4.2 Ejecutar Tests
```bash
# Tests con Poetry
poetry run pytest tests/ -v

# Pre-commit hooks
poetry run pre-commit run --all-files
```

#### 4.3 Verificar CI/CD
```bash
# Commit cambios y verificar que CI pasa
git add pyproject.toml poetry.lock
git commit -m "chore: migrate to Poetry for dependency management"
git push
```

### Fase 5: Limpieza (15 min)

#### 5.1 Deprecar Archivos Antiguos
```bash
# Mover a directorio legacy
mkdir -p legacy/
mv requirements.txt requirements-dev.txt requirements-frozen.txt legacy/

# Actualizar .gitignore
echo "legacy/" >> .gitignore
```

#### 5.2 Actualizar README.md
```markdown
## Instalación

### Requisitos
- Python 3.10+
- Poetry 1.7+

### Setup
\`\`\`bash
# Instalar dependencias
poetry install

# Copiar variables de entorno
cp .env.example .env

# Ejecutar tests
poetry run pytest
\`\`\`
```

---

## Checklist de Migración

- [ ] Poetry instalado y configurado
- [ ] `pyproject.toml` creado con metadata correcta
- [ ] Dependencias de producción migradas
- [ ] Dependencias de desarrollo migradas
- [ ] `poetry.lock` generado y versionado
- [ ] GitHub Actions actualizado
- [ ] Pre-commit hooks actualizados
- [ ] Tests pasando con Poetry
- [ ] CI/CD pasando
- [ ] Documentación actualizada
- [ ] Archivos legacy movidos
- [ ] README.md actualizado

---

## Comandos de Referencia Rápida

### Gestión de Dependencias
```bash
poetry add package           # Agregar dep de producción
poetry add --group dev pkg   # Agregar dep de desarrollo
poetry remove package        # Eliminar dependencia
poetry update                # Actualizar deps
poetry show                  # Listar deps instaladas
poetry show --tree           # Árbol de dependencias
```

### Entorno Virtual
```bash
poetry shell                 # Activar virtualenv
poetry env info              # Info del entorno
poetry env list              # Listar entornos
poetry env remove python     # Eliminar entorno
```

### Ejecución
```bash
poetry run python script.py  # Ejecutar script
poetry run pytest            # Ejecutar tests
poetry run black .           # Ejecutar formatter
```

### Lock File
```bash
poetry lock                  # Actualizar poetry.lock
poetry lock --no-update      # Regenerar sin actualizar
poetry install --sync        # Sincronizar con lock
```

---

## Troubleshooting

### Conflictos de Dependencias
```bash
# Ver detalle del conflicto
poetry add package --dry-run

# Resolver manualmente editando pyproject.toml
poetry lock --no-update
```

### Limpiar Cache
```bash
poetry cache clear pypi --all
poetry cache clear _default_cache --all
```

### Recrear Entorno
```bash
poetry env remove python
poetry install
```

---

## Recursos

- **Documentación oficial**: https://python-poetry.org/docs/
- **Migración desde requirements.txt**: https://python-poetry.org/docs/managing-dependencies/
- **Guía de pyproject.toml**: https://peps.python.org/pep-0518/
- **Poetry en CI/CD**: https://python-poetry.org/docs/ci/

---

*Documento generado: 2025-10-14*
*Última actualización: 2025-10-14*
