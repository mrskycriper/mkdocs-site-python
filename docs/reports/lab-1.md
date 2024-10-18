# Практика 1

Автор: Салимов Сергей

---

## Отчёт

### 1. Проверка версии pyhton

Установлен в системе по умолчанию.

```bash
python3 --version
```

> `Python 3.12.3`

### 2. Установка virtualenv

```bash
python3 -m pip install --user virtualenv
```

### 3. Создаём и активируем venv

``` bash
mkdir mkdocs-site-python
cd mkdocs-site-python
```

```bash
virtualenv venv
```

```bash
source venv/bin/activate
```

### 4. Устанавливаем MkDocs

```bash
pip install mkdocs
```

### 5. Генерируем проект

Используем `./` вместо названия для использования текущей директории.

```bash
mkdocs new ./
```

### 6. Редактируем конфиг

Файл `mkdocs.yml`.

```yml
site_name: My Docs
theme:
  name: mkdocs
  color_mode: auto
  user_color_mode_toggle: true
```

### 7. Редактируем содержимое

Файл `docs/index.md`.

```md
# Welcome to MkDocs

## Lab 1

Hello world!
```

### 8. Загружаем на GitHub

### 9. Создаём Action для сборки

Файл `.github/workflows/mkdocs.yml`.

```yml
name: Deploy MkDocs to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: 3.12
      - name: Setup MkDocs
        run: pip install mkdocs
      - name: Build
        run: mkdocs build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: 'site/'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 10. Результат

После успешного выполнения сценария получаем сайт по адресу [https://mrskycriper.github.io/mkdocs-site-python/](https://mrskycriper.github.io/mkdocs-site-python/)
