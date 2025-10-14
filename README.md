# Aquila Credit System 🦅

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)
[![Code Style](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)
[![GitHub Issues](https://img.shields.io/github/issues/elgomy/aquila_project)](https://github.com/elgomy/aquila_project/issues)
[![GitHub Stars](https://img.shields.io/github/stars/elgomy/aquila_project)](https://github.com/elgomy/aquila_project/stargazers)
[![Maintained](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/elgomy/aquila_project/graphs/commit-activity)

Sistema de análisis de crédito automatizado pionero que implementa una arquitectura híbrida única donde la IA actúa como orquestador coordinando herramientas determinísticas.

## 🎯 Características Principales

- **🤖 IA como Orquestador**: Claude AI coordina el flujo de trabajo completo
- **🔧 Herramientas Determinísticas**: Lógica de negocio encapsulada en tools especializadas
- **📝 Lógica Externa en YAML**: Todas las reglas y umbrales configurables sin código
- **🔍 Análisis en Dos Fases**: Preliminar (rápido) y Completo (profundo)
- **🏗️ Arquitectura Modular**: Cadastro y Crédito completamente independientes
- **🔌 Multi-Provider**: SDK intercambiable para bureaus de crédito

## 📦 Arquitectura

```
aquila_project/
├── aquila_app/              # Aplicación principal
│   ├── orchestrator/        # Orquestador central IA
│   ├── cadastro/           # Validación documental
│   ├── credit/             # Análisis de crédito
│   │   ├── financial_analysis/  # Submódulo análisis financiero
│   │   ├── tools/          # 12+ tools especializadas
│   │   ├── rules/          # Reglas de negocio YAML
│   │   └── scoring/        # Motor de scoring
│   ├── core/               # Componentes compartidos
│   └── integrations/       # APIs externas
│
├── aquila_bureaus_sdk/     # SDK modular de bureaus
│   ├── providers/          # Abstracciones por tipo
│   └── adapters/           # Implementaciones específicas
│
└── docs/                   # Documentación completa
    ├── architecture/
    ├── standards/
    └── DevOps/
```

## 🚀 Instalación Rápida

### Requisitos

- Python 3.10+
- Redis (opcional, para cache)
- PostgreSQL vía Supabase

### Setup

```bash
# Clonar repositorio
git clone https://github.com/elgomy/aquila_project.git
cd aquila_project

# Instalar dependencias
pip install -r requirements.txt

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales

# Ejecutar tests
pytest tests/ -v
```

## 🔑 Variables de Entorno Requeridas

Ver `.env.example` para la lista completa. Las críticas son:

```bash
# LLM Providers
ANTHROPIC_API_KEY=tu_claude_api_key
OPENAI_API_KEY=tu_openai_api_key  # Fallback

# Bureau APIs
CREDITHUB_API_KEY=tu_credithub_key
BIGDATACORP_TOKEN=tu_bigdatacorp_token
BMP_API_KEY=tu_bmp_key

# Database
SUPABASE_URL=tu_supabase_url
SUPABASE_KEY=tu_supabase_key
```

## 📊 Stack Tecnológico

### Core
- **Python 3.10+**: Lenguaje principal
- **FastAPI**: Framework API REST asíncrono
- **PostgreSQL**: Base de datos vía Supabase
- **Redis**: Cache distribuido

### IA/ML
- **Anthropic Claude**: Orquestador principal
- **OpenAI GPT**: Fallback
- **Google Gemini**: Parsing de documentos

### Bureaus de Crédito
- **CreditHub**: Protestos, RFB, SCR
- **BigDataCorp**: Procesos judiciales, análisis de riesgo
- **BMP Digital**: SCR Banco Central

### DevOps
- **GitHub Actions**: CI/CD automatizado
- **Pre-commit**: Hooks de calidad de código
- **pytest**: Framework de testing
- **black, flake8, isort**: Code quality
- **bandit, safety**: Security scanning

## 🧪 Testing

```bash
# Ejecutar todos los tests
pytest tests/ -v

# Con coverage
pytest tests/ --cov=aquila_app --cov-report=html

# Tests de integración específicos
python test_bureau_tools.py
```

## 📚 Documentación

- **[Arquitectura](docs/architecture.txt)**: Patrones y decisiones de diseño
- **[Standards](docs/standards/)**: Guías de código y mejores prácticas
- **[API Specs](docs/specs/)**: Documentación de APIs externas
- **[Flows](docs/flows/)**: Diagramas de flujos de trabajo
- **[DevOps](docs/DevOps/)**: Guías de CI/CD y deployment

## 🛠️ Desarrollo

### Code Quality

```bash
# Formatear código
black aquila_app/

# Ordenar imports
isort aquila_app/

# Linting
flake8 aquila_app/

# Type checking
mypy aquila_app/ --ignore-missing-imports
```

### Pre-commit Hooks

```bash
# Instalar hooks
pre-commit install

# Ejecutar en todos los archivos
pre-commit run --all-files
```

## 🔒 Seguridad

- ✅ Credenciales protegidas con `.gitignore`
- ✅ Secret scanning en CI/CD
- ✅ Security scanning con Bandit
- ✅ Dependency vulnerability checks con Safety
- ✅ GitHub Push Protection habilitada

**⚠️ IMPORTANTE**: Después de clonar, debes rotar las credenciales documentadas en `docs/security/CREDENTIALS_TO_ROTATE.md`

## 📈 Estado del Proyecto

### ✅ Completado
- [x] Arquitectura Orquestador + Tools
- [x] Módulo de Cadastro (validación documental)
- [x] Módulo de Crédito (análisis preliminar y completo)
- [x] SDK modular de bureaus
- [x] Sistema de scoring configurable (YAML)
- [x] Análisis financiero con detección de informalidad
- [x] CI/CD con GitHub Actions
- [x] Pre-commit hooks
- [x] Security hardening

### 🚧 En Progreso
- [ ] Tests unitarios completos
- [ ] Integración con Pipefy
- [ ] Generación de reportes PDF
- [ ] Dashboard administrativo

### 📋 Roadmap
- [ ] Migración a Poetry
- [ ] Multi-tenancy
- [ ] API pública
- [ ] Mobile app

## 🤝 Contribuir

1. Fork el proyecto
2. Crea tu feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push al branch (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

Ver [CONTRIBUTING.md](CONTRIBUTING.md) para más detalles.

## 📄 Licencia

Proprietary - Capital Financas

## 👥 Equipo

- **Igor Gomez Cabrera** - Tech Lead
- **Capital Financas** - Product Owner

## 🔗 Links

- [Repositorio GitHub](https://github.com/elgomy/aquila_project)
- [Documentación Completa](docs/)
- [SDK README](aquila_bureaus_sdk/README.md)

---

**Versión**: 1.0.0-alpha
**Última actualización**: 2025-10-14
🤖 Built with Claude Code
