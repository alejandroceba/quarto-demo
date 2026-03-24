# Mi Primer Dashboard

Este repositorio contiene un dashboard construido con [Quarto](https://quarto.org/) y Python, configurado para desplegarse en GitHub Pages desde la carpeta `docs/`.

## Prerrequisitos 📧:
- Quarto (para desarrollo local)
- Python 3.10+
- Dependencias definidas en `requirements.txt` (`pandas`, `numpy`, `plotly`, `jupyter`)

---

## Cómo iniciar tu repositorio y hacer el Deploy

Sigue estos pasos detallados para subir tu código a GitHub y que se publique tu Dashboard.

### 1. Iniciar el repositorio localmente

Abre tu terminal en la carpeta principal del proyecto (`mi-primer-dashboard`) y ejecuta los siguientes comandos:

```bash
# 1. Iniciar git en esta carpeta
git init

# 2. Agregar todos los archivos para el primer guardado
git add .

# 3. Crear el primer commit con un mensaje descriptivo
git commit -m "feat: configuración inicial del dashboard"

# 4. Asegurarse de que la rama principal se llama 'main'
git branch -M main
```

### 2. Crear el repositorio en GitHub y enlazarlo

1. Ve a [GitHub](https://github.com/) e inicia sesión en tu cuenta.
2. Crea un **Nuevo Repositorio** pulsando el botón de **+** en la esquina superior derecha o ve a [Crear Repositorio](https://github.com/new).
3. Ponle un nombre (ej. `mi-primer-dashboard`), déjalo como **Público** y **NO marques** la opción de agregar README (ya que ya tenemos uno).
4. Copia el enlace de tu nuevo repositorio y ejecuta en tu terminal:

```bash
# Conectar tu carpeta local con el repositorio de GitHub (reemplaza con tu enlace)
git remote add origin https://github.com/TU-USUARIO/mi-primer-dashboard.git

# Subir tu código a GitHub
git push -u origin main
```

### 3. Configurar GitHub Pages

1. Ve a la pestaña **Settings** (Configuración) de tu repositorio en GitHub.
2. En el menú lateral izquierdo, haz clic en **Pages**.
3. En la sección **Build and deployment**, asegúrate de que **Source** esté configurado como **Deploy from a branch**.
4. En **Branch**, selecciona la rama `main` y la carpeta `/docs`. Haz clic en **Save**.
5. En unos minutos aparecerá el enlace a tu dashboard publicado (por ejemplo: `https://TU-USUARIO.github.io/mi-primer-dashboard/`).

---

## Desarrollo Local

Para hacer cambios y previsualizarlos antes de subirlos a GitHub:

```bash
# Activar el entorno virtual
source .venv/bin/activate

# Instalar las librerías necesarias de Python (si no lo has hecho)
pip install -r requirements.txt

# Previsualizar el dashboard en el navegador
quarto preview mi-primer-dashboard.qmd

# Cuando estés listo para publicar, renderiza el dashboard
quarto render mi-primer-dashboard.qmd

# Haz commit y push (incluyendo la carpeta docs/)
git add .
git commit -m "update: actualizar dashboard"
git push
```

---

## Alternativa: Deploy automático con GitHub Actions

Si prefieres que el dashboard se renderice automáticamente en cada push (sin necesidad de correr `quarto render` localmente), puedes usar un workflow de GitHub Actions.

Crea el archivo `.github/workflows/deploy.yml` con el siguiente contenido:

```yaml
on:
  push:
    branches: main
  workflow_dispatch:

name: Quarto Publish

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
      id-token: write
    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'
          cache: 'pip'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Render and Publish
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> **Nota:** Si usas este método, configura GitHub Pages con la rama `gh-pages` y carpeta `/ (root)` en lugar de `main` y `/docs`.
