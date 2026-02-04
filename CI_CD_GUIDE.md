# CI/CD Setup Guide

## ✅ Configuración Completada

Se han creado los siguientes archivos para CI/CD:

```
.github/
├── workflows/
│   └── ci.yml              # Workflow principal de CI
└── dependabot.yml          # Actualizaciones automáticas de dependencias
```

## 🚀 Próximos Pasos para Activar CI/CD

### 1. Crear Repositorio en GitHub

Si aún no tienes un repositorio en GitHub:

1. Ve a https://github.com/new
2. Crea un nuevo repositorio
3. **No** inicialices con README, .gitignore o licencia

### 2. Conectar tu Proyecto Local con GitHub

```bash
# Inicializar git (si no está inicializado)
git init

# Agregar todos los archivos
git add .

# Hacer el primer commit
git commit -m "feat: initial commit with CI/CD setup"

# Agregar el remote (reemplaza con tu URL)
git remote add origin https://github.com/TU-USUARIO/TU-REPOSITORIO.git

# Crear y subir la rama main
git branch -M main
git push -u origin main
```

### 3. Verificar que CI/CD Funciona

1. Ve a tu repositorio en GitHub
2. Navega a la pestaña **"Actions"**
3. Deberías ver el workflow "CI" ejecutándose
4. Espera a que todos los jobs terminen (Tests, Code Style, Build Assets)

### 4. Actualizar el Badge de CI

En `README.md`, reemplaza `YOUR-USERNAME/YOUR-REPO` con tu información:

```markdown
<a href="https://github.com/tu-usuario/tu-repo/actions">
  <img src="https://github.com/tu-usuario/tu-repo/workflows/CI/badge.svg" alt="CI Status">
</a>
```

### 5. Configurar Dependabot (Opcional)

En `.github/dependabot.yml`, actualiza el campo `reviewers`:

```yaml
reviewers:
  - "tu-usuario-github"  # Tu usuario de GitHub
```

## 🔍 ¿Qué Hace el CI?

El workflow de CI se ejecuta automáticamente en:
- ✅ Push a las ramas `main` y `develop`
- ✅ Pull Requests hacia `main` y `develop`

### Jobs que se ejecutan:

1. **Tests (PHP 8.2 y 8.3)**
   - Instala dependencias de Composer
   - Configura base de datos SQLite
   - Ejecuta migraciones
   - Corre todos los tests con Pest

2. **Code Style (Laravel Pint)**
   - Verifica que el código sigue el estándar de Laravel
   - Falla si hay código mal formateado

3. **Build Assets (Vite)**
   - Instala dependencias de npm/yarn
   - Compila assets de producción
   - Verifica que el manifest de Vite se genera

## 🛠️ Testing Local

Antes de hacer push, puedes ejecutar los mismos checks localmente:

```bash
# Tests
php artisan test

# Code Style Check
./vendor/bin/pint --test

# Fix Code Style
./vendor/bin/pint

# Build Assets
yarn build
```

## 🔒 Proteger la Rama Main (Recomendado)

1. Ve a tu repositorio en GitHub
2. Settings → Branches
3. Add branch protection rule
4. Branch name pattern: `main`
5. ✅ Require status checks to pass before merging
6. Selecciona los checks:
   - `Tests (PHP 8.2)`
   - `Tests (PHP 8.3)`
   - `Code Style (Pint)`
   - `Build Assets (Vite)`
7. ✅ Require branches to be up to date before merging
8. Save changes

Esto asegura que ningún código que falle los tests o el linting pueda mergearse a `main`.

## 📦 Deployment (No Configurado Aún)

Actualmente, el CI solo ejecuta tests y verifica el código. Si necesitas deployment automático:

### Opciones de Deployment:

- **Laravel Forge**: Deployment automático con webhooks
- **Laravel Envoyer**: Zero-downtime deployment
- **GitHub Actions → VPS**: SSH deployment con actions
- **Plataformas Cloud**:
  - Laravel Vapor (AWS)
  - DigitalOcean App Platform
  - Heroku
  - Railway

Avísame si necesitas configurar deployment automático.

## 🐛 Troubleshooting

### El workflow falla en Tests

```bash
# Ejecuta localmente para ver el error
php artisan test
```

### El workflow falla en Code Style

```bash
# Ve qué archivos necesitan corrección
./vendor/bin/pint --test

# Corrige automáticamente
./vendor/bin/pint
```

### El workflow falla en Build Assets

```bash
# Ejecuta el build localmente
yarn build

# Verifica que vite.config.js esté correcto
```

## 📝 Notas Importantes

- El CI usa **SQLite in-memory** para tests (rápido y ligero)
- Los assets compilados se cachean como artifacts
- Dependabot creará PRs automáticos semanalmente
- Los workflows se ejecutan en paralelo para velocidad

## ✨ Mejoras Futuras Sugeridas

- [ ] Agregar PHPStan/Larastan para análisis estático
- [ ] Configurar code coverage con Codecov
- [ ] Agregar tests de navegador con Laravel Dusk
- [ ] Implementar deployment automático
- [ ] Agregar security scanning con `composer audit`
