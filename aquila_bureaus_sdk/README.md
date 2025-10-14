# Aquila Bureaus SDK

Sistema modular de consulta a bureaus de crédito brasileiros, diseñado como **piezas de Lego** intercambiables.

## 🎯 Características Principales

- ✅ **Modularidad Total**: Intercambia providers de APIs sin modificar código
- ✅ **Reutilizable**: Mismo SDK en múltiples proyectos (Aquila + Reanalizador)
- ✅ **Extensible**: Agrega nuevos providers sin modificar existentes
- ✅ **Testeable**: Mock providers fácilmente para testing
- ✅ **Configuración Externa**: Define providers via YAML/JSON

## 📦 Instalación

### Para Usuarios del SDK

```bash
# Instalación básica (cuando esté en PyPI)
pip install aquila-bureaus-sdk

# Con providers específicos
pip install aquila-bureaus-sdk[credithub,bigdatacorp,bmp]
```

### Para Desarrollo Local

```bash
# Clonar repositorio
git clone https://github.com/tu-org/aquila_project.git
cd aquila_project

# Instalar dependencias (incluye el SDK)
pip install -r requirements.txt

# O si usas Poetry (recomendado)
poetry install

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales reales
```

## 🚀 Uso Rápido

### Uso Directo de Providers

```python
from aquila_bureaus_sdk import (
    CreditHubProtestsAdapter,
    BigDataCorpLawsuitsAdapter,
    BMPSCRAdapter
)

# Protests provider (CreditHub)
async with CreditHubProtestsAdapter(api_key="xxx") as protests_provider:
    protests = await protests_provider.get_protests_history(cnpj="12345678000190")
    print(f"Total protestos: {protests.data['total_protests']}")
    print(f"Vigentes: {protests.data['vigentes_protests']}")

# Lawsuits provider (BigDataCorp)
async with BigDataCorpLawsuitsAdapter(token="xxx", token_id="yyy") as lawsuits_provider:
    lawsuits = await lawsuits_provider.get_lawsuits(cnpj="12345678000190")
    print(f"Processos passivos: {lawsuits.data['total_lawsuits_passive']}")

# SCR provider (BMP)
async with BMPSCRAdapter(api_key="xxx") as scr_provider:
    scr = await scr_provider.get_scr_data(cnpj="12345678000190")
    print(f"Dívida total: R$ {scr.data['total_debt']:,.2f}")
```

### Uso con Manager (Próximamente)

```python
from aquila_bureaus_sdk import BureauManager

# Configuración externa (YAML/JSON)
config = {
    'protests': {
        'provider': 'credithub',  # Intercambiable: credithub, serasa, etc.
        'api_key': 'xxx'
    },
    'lawsuits': {
        'provider': 'bigdatacorp',
        'token': 'xxx',
        'token_id': 'yyy'
    },
    'scr': {
        'provider': 'bmp',
        'api_key': 'xxx'
    }
}

# Manager orquesta todos los providers
manager = BureauManager(config)
bureau_data = await manager.get_complete_bureau_data(cnpj="12345678000190")

print(f"Protestos: {bureau_data['protests']['total']}")
print(f"Processos: {bureau_data['lawsuits']['total_passive']}")
print(f"SCR: R$ {bureau_data['scr']['total_debt']:,.2f}")
```

## 🏗️ Arquitectura

### Patrón Strategy + Adapter

```
BaseProvider (ABC)
    ├── ProtestsProvider (ABC)
    │   └── CreditHubProtestsAdapter
    │   └── SerasaProtestsAdapter (futuro)
    │
    ├── LawsuitsProvider (ABC)
    │   └── BigDataCorpLawsuitsAdapter
    │
    ├── SCRProvider (ABC)
    │   └── BMPSCRAdapter
    │   └── CreditHubSCRAdapter (futuro)
    │
    ├── CompanyDataProvider (ABC)
    │   └── CreditHubCompanyDataAdapter
    │
    └── RiskAnalysisProvider (ABC)
        └── BigDataCorpRiskAdapter
```

### Respuesta Estandarizada

Todos los providers retornan `BureauResponse`:

```python
@dataclass
class BureauResponse:
    success: bool
    data: Dict[str, Any]
    provider_type: ProviderType
    data_source: DataSource
    cnpj: str
    consultation_date: str
    metadata: Dict[str, Any]
    errors: Optional[List[str]]
    cached: bool
```

## 🔌 Providers Disponibles

| Provider | Interface | Descripción |
|----------|-----------|-------------|
| **CreditHubProtestsAdapter** | ProtestsProvider | Protestos (vigentes + histórico) |
| **CreditHubCompanyDataAdapter** | CompanyDataProvider | Dados RFB |
| **BigDataCorpLawsuitsAdapter** | LawsuitsProvider | Processos judiciais |
| **BigDataCorpRiskAdapter** | RiskAnalysisProvider | Análise de risco + fraude |
| **BMPSCRAdapter** | SCRProvider | Dados SCR oficiais Bacen |

## 📝 Agregar Nuevo Provider

1. **Crear adapter** implementando interface correspondiente:

```python
from aquila_bureaus_sdk.providers import ProtestsProvider, BureauResponse, DataSource

class SerasaProtestsAdapter(ProtestsProvider):
    def __init__(self, api_key: str):
        super().__init__("Serasa", DataSource.SERASA)
        self.api_key = api_key

    async def get_protests(self, cnpj: str) -> BureauResponse:
        # Implementar lógica específica de Serasa
        pass
```

2. **Registrar en Factory** (próximamente):

```python
BureauProviderFactory.register_protests_provider(
    'serasa',
    SerasaProtestsAdapter
)
```

3. **Usar vía configuración**:

```yaml
bureaus:
  protests:
    provider: serasa  # ✅ Nuevo provider sin modificar código
    api_key: ${SERASA_API_KEY}
```

## 🧪 Testing

### Ejecutar Tests

```bash
# Instalar dependencias de desarrollo
pip install -r requirements-dev.txt

# Ejecutar todos los tests
pytest tests/ -v

# Ejecutar con coverage
pytest tests/ -v --cov=aquila_bureaus_sdk --cov-report=html

# Ejecutar tests específicos
pytest tests/test_credithub_adapter.py -v
```

### Mock de Providers para Testing

```python
# Mock fácil de providers para testing
class MockProtestsProvider(ProtestsProvider):
    async def get_protests(self, cnpj: str) -> BureauResponse:
        return BureauResponse(
            success=True,
            data={'total_protests': 5},
            provider_type=ProviderType.PROTESTS,
            data_source=DataSource.CUSTOM,
            cnpj=cnpj
        )

# Usar en tests
async with MockProtestsProvider() as provider:
    result = await provider.get_protests("12345678000190")
    assert result.success
    assert result.data['total_protests'] == 5
```

### Code Quality Checks

```bash
# Formatear código con Black
black aquila_bureaus_sdk/

# Ordenar imports con isort
isort aquila_bureaus_sdk/

# Lint con flake8
flake8 aquila_bureaus_sdk/

# Type checking con mypy
mypy aquila_bureaus_sdk/ --ignore-missing-imports

# Security scan con bandit
bandit -r aquila_bureaus_sdk/
```

### Pre-commit Hooks

```bash
# Instalar pre-commit hooks
pre-commit install

# Ejecutar manualmente en todos los archivos
pre-commit run --all-files
```

## 📊 Estado del Proyecto

### ✅ Completado (Fase 1)
- [x] Abstracciones base (ProtestsProvider, LawsuitsProvider, SCRProvider, etc.)
- [x] Adapters para APIs existentes (CreditHub, BigDataCorp, BMP)
- [x] Estructura de paquete independiente
- [x] BureauResponse estandarizado
- [x] Documentación básica

### 🚧 En Progreso (Fase 2)
- [ ] Factory Pattern para crear providers
- [ ] BureauManager para orquestar múltiples providers
- [ ] Configuración externa via YAML/JSON
- [ ] Sistema de cache integrado

### 📋 Pendiente (Fase 3)
- [ ] Tests unitarios completos
- [ ] setup.py y empaquetamiento PyPI
- [ ] Integración en bureau_tools.py
- [ ] CI/CD pipeline
- [ ] Documentación completa

## 🤝 Contribuir

1. Fork el repositorio
2. Crea tu feature branch (`git checkout -b feature/nuevo-provider`)
3. Commit tus cambios (`git commit -am 'Add Serasa provider'`)
4. Push al branch (`git push origin feature/nuevo-provider`)
5. Crea un Pull Request

## 📄 Licencia

Proprietary - Aquila Credit System

## 🔗 Recursos

- [Documentación CreditHub](./docs/credithub-api-guide.md)
- [Documentación BigDataCorp](./docs/bigdatacorp-api-guide.md)
- [Documentación BMP](./docs/bmp-api-guide.md)

---

## 🛠️ Ambiente de Desarrollo

### Configuración Recomendada

```bash
# Usar Python 3.10+
python --version  # 3.10 o superior

# Instalar Poetry (opcional pero recomendado)
curl -sSL https://install.python-poetry.org | python3 -

# Configurar pre-commit hooks
pre-commit install

# Configurar editor con Black/isort
# VS Code: instalar extensiones Python, Black Formatter
# PyCharm: configurar Black como formatter
```

### Variables de Entorno

El SDK requiere las siguientes variables (ver `.env.example`):

```bash
# Bureau APIs
CREDITHUB_API_KEY=tu_credithub_key
BIGDATACORP_TOKEN=tu_bigdatacorp_token
BIGDATACORP_TOKEN_ID=tu_bigdatacorp_token_id
BMP_API_KEY=tu_bmp_key

# Cache (opcional)
REDIS_URL=redis://localhost:6379
```

---

**Versión**: 1.0.0-alpha
**Última actualización**: 2025-10-14
**Mantenido por**: Capital Financas Tech Team
