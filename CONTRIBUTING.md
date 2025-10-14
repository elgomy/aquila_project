# Guía de Contribución - Aquila Credit System

¡Gracias por tu interés en contribuir a Aquila! Este documento te guiará en el proceso de contribución.

## 📋 Tabla de Contenidos

- [Código de Conducta](#código-de-conducta)
- [Primeros Pasos](#primeros-pasos)
- [Flujo de Trabajo](#flujo-de-trabajo)
- [Estándares de Código](#estándares-de-código)
- [Commits y Pull Requests](#commits-y-pull-requests)
- [Testing](#testing)
- [Documentación](#documentación)

## 🤝 Código de Conducta

Este proyecto adhiere a principios de respeto, colaboración y profesionalismo. Esperamos que todos los contribuidores:

- Sean respetuosos y constructivos en sus comentarios
- Acepten críticas constructivas
- Se enfoquen en lo mejor para el proyecto y la comunidad
- Demuestren empatía hacia otros contribuidores

## 🚀 Primeros Pasos

### 1. Fork y Clone

```bash
# Fork el repositorio en GitHub, luego:
git clone https://github.com/TU_USUARIO/aquila_project.git
cd aquila_project

# Agrega el repositorio original como upstream
git remote add upstream https://github.com/elgomy/aquila_project.git
```

### 2. Configurar Entorno de Desarrollo

```bash
# Instalar dependencias
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Configurar pre-commit hooks
pre-commit install

# Copiar y configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales de desarrollo
```

### 3. Verificar Setup

```bash
# Ejecutar tests
pytest tests/ -v

# Ejecutar linters
pre-commit run --all-files
```

## 🔄 Flujo de Trabajo

### 1. Sincronizar con Upstream

Antes de empezar a trabajar, asegúrate de tener la última versión:

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

### 2. Crear Feature Branch

```bash
# Nombra tu branch descriptivamente
git checkout -b feature/nombre-descriptivo
# o
git checkout -b fix/descripcion-del-bug
# o
git checkout -b docs/mejora-documentacion
```

### 3. Hacer Cambios

- Escribe código limpio y bien documentado
- Sigue los estándares de código del proyecto
- Agrega tests para nueva funcionalidad
- Actualiza documentación según sea necesario

### 4. Commit Frecuente

```bash
git add .
git commit -m "tipo: descripción breve

Descripción más detallada si es necesario.
"
```

### 5. Mantener Branch Actualizado

```bash
git fetch upstream
git rebase upstream/main
```

### 6. Push y Pull Request

```bash
git push origin feature/nombre-descriptivo
```

Luego crea un Pull Request en GitHub.

## 📝 Estándares de Código

### Python Style Guide

Seguimos **PEP 8** estrictamente. Nuestras herramientas automatizan esto:

#### Formateo Automático

```bash
# Black (formatter)
black aquila_app/

# isort (import sorting)
isort aquila_app/
```

#### Linting

```bash
# Flake8
flake8 aquila_app/ --max-line-length=120

# Pylint (opcional, más estricto)
pylint aquila_app/
```

#### Type Hints

Usamos type hints extensivamente:

```python
from typing import List, Dict, Optional

def process_data(
    data: List[Dict[str, Any]],
    config: Optional[Dict[str, str]] = None
) -> Dict[str, Any]:
    """
    Procesa datos según configuración.

    Args:
        data: Lista de diccionarios con datos a procesar
        config: Configuración opcional

    Returns:
        Diccionario con resultados procesados
    """
    pass
```

#### Docstrings

Usa formato Google:

```python
def calculate_score(financial_data: Dict[str, Any]) -> float:
    """
    Calcula score financiero basado en datos.

    Args:
        financial_data: Diccionario con métricas financieras
            - revenue: float, ingresos anuales
            - debt: float, deuda total
            - assets: float, activos totales

    Returns:
        Score entre 0 y 9

    Raises:
        ValueError: Si los datos están incompletos

    Example:
        >>> data = {'revenue': 1000000, 'debt': 200000, 'assets': 800000}
        >>> calculate_score(data)
        7.5
    """
    pass
```

### Principios de Diseño

1. **KISS (Keep It Simple, Stupid)**: La simplicidad siempre gana
2. **DRY (Don't Repeat Yourself)**: Evita duplicación
3. **SOLID**: Especialmente Single Responsibility
4. **Separation of Concerns**: Módulos independientes
5. **Fail Fast**: Validaciones tempranas

### Arquitectura

- **Orquestador Central**: IA coordina, no ejecuta
- **Tools Determinísticas**: Lógica de negocio en código Python explícito
- **Configuración Externa**: Reglas y umbrales en YAML
- **Inmutabilidad**: Preferir estructuras de datos inmutables

## 💬 Commits y Pull Requests

### Convención de Commits

Seguimos [Conventional Commits](https://www.conventionalcommits.org/):

```
tipo(scope): descripción breve

Descripción más detallada si es necesaria.

Fixes #123
```

#### Tipos de Commit

- **feat**: Nueva funcionalidad
- **fix**: Corrección de bugs
- **docs**: Cambios en documentación
- **style**: Cambios de formato (no afectan funcionalidad)
- **refactor**: Refactorización de código
- **perf**: Mejoras de performance
- **test**: Agregar o corregir tests
- **chore**: Tareas de mantenimiento
- **ci**: Cambios en CI/CD
- **build**: Cambios en dependencias o build

#### Ejemplos

```bash
# Nueva funcionalidad
git commit -m "feat(credit): agregar validación de CNPJ duplicado"

# Bug fix
git commit -m "fix(scoring): corregir cálculo de score preliminar

El cálculo estaba dividiendo por cero cuando no había deuda.
Ahora retorna score máximo en ese caso.

Fixes #42"

# Documentación
git commit -m "docs(readme): actualizar instrucciones de instalación"

# Refactoring
git commit -m "refactor(bureau): extraer lógica de retry a utility function"
```

### Pull Request Template

Tu PR debe incluir:

```markdown
## Descripción
[Descripción clara de los cambios]

## Tipo de Cambio
- [ ] Bug fix (non-breaking change)
- [ ] Nueva funcionalidad (non-breaking change)
- [ ] Breaking change (funcionalidad existente afectada)
- [ ] Documentación

## Testing
- [ ] Tests unitarios agregados/actualizados
- [ ] Tests de integración agregados/actualizados
- [ ] Tests manuales realizados

## Checklist
- [ ] Código sigue estándares del proyecto
- [ ] Self-review realizado
- [ ] Código comentado en áreas complejas
- [ ] Documentación actualizada
- [ ] No genera warnings
- [ ] Tests agregados y pasan
- [ ] Pre-commit hooks pasan
```

### Code Review

Cuando hagas code review:

- **Sé constructivo**: Sugiere mejoras, no solo critiques
- **Explica el "por qué"**: No solo qué cambiar, sino por qué
- **Aprende**: Las reviews son oportunidades de aprendizaje mutuo
- **Sé específico**: Referencia líneas de código específicas
- **Prioriza**: Distingue entre issues críticos y nits

## 🧪 Testing

### Ejecutar Tests

```bash
# Todos los tests
pytest tests/ -v

# Tests específicos
pytest tests/test_scoring.py -v

# Con coverage
pytest tests/ --cov=aquila_app --cov-report=html

# Tests marcados
pytest -m "not slow"
```

### Escribir Tests

```python
import pytest
from aquila_app.credit.tools.scoring_tool import ScoringTool

class TestScoringTool:
    """Tests for ScoringTool"""

    @pytest.fixture
    def scoring_tool(self):
        """Fixture que retorna instancia de ScoringTool"""
        return ScoringTool()

    def test_calculate_preliminary_score_high(self, scoring_tool):
        """Test score alto con buenos indicadores"""
        data = {
            'revenue': 5000000,
            'debt': 100000,
            'protests': 0
        }

        result = scoring_tool.calculate_preliminary_score(data)

        assert result['score'] >= 7.0
        assert result['classification'] == 'HIGH'

    @pytest.mark.parametrize("revenue,expected_score", [
        (1000000, 5.0),
        (5000000, 7.0),
        (10000000, 8.5)
    ])
    def test_score_by_revenue(self, scoring_tool, revenue, expected_score):
        """Test score varía con revenue"""
        data = {'revenue': revenue, 'debt': 0, 'protests': 0}
        result = scoring_tool.calculate_preliminary_score(data)
        assert abs(result['score'] - expected_score) < 1.0
```

### Cobertura de Tests

Objetivo: **> 80% coverage** para código crítico

```bash
# Generar reporte HTML
pytest --cov=aquila_app --cov-report=html

# Ver en navegador
open htmlcov/index.html
```

## 📚 Documentación

### Documentar Código

- **Docstrings**: Todas las funciones públicas
- **Type hints**: Todas las funciones
- **Comentarios inline**: Solo para lógica compleja
- **README**: Actualizar si cambias comportamiento público

### Actualizar Docs

Si tus cambios afectan:

- **API pública**: Actualizar `README.md`
- **Arquitectura**: Actualizar `docs/architecture.txt`
- **Flujos**: Actualizar diagramas en `docs/flows/`
- **Configuración**: Actualizar `.env.example`

### Documentación en Español

La documentación del proyecto está en **español** para facilitar el uso por el equipo brasileño/latinoamericano.

## ❓ Preguntas Frecuentes

### ¿Cómo agrego una nueva tool al orquestador?

1. Crear clase en `aquila_app/credit/tools/`
2. Heredar de `BaseTool`
3. Implementar método `execute()`
4. Registrar en tool registry
5. Agregar schema de la tool
6. Agregar tests

### ¿Cómo agrego nuevas reglas de scoring?

Editar archivos YAML en `aquila_app/credit/rules/`:
- `preliminary_scoring.yaml` - Scoring preliminar
- `complete_scoring.yaml` - Scoring completo

### ¿Cómo agrego un nuevo provider de bureau?

Ver guía en `aquila_bureaus_sdk/README.md` sección "Agregar Nuevo Provider"

## 🆘 Necesitas Ayuda?

- **Issues**: Abre un issue en GitHub
- **Discusiones**: Usa GitHub Discussions
- **Email**: tech@capitalfinancas.com.br

## 📄 Licencia

Al contribuir, aceptas que tus contribuciones se licencien bajo la misma licencia del proyecto (Proprietary - Capital Financas).

---

¡Gracias por contribuir a Aquila! 🦅
