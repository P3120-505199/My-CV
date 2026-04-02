# Отчёт по лабораторной работе 3 семестра 2
## "CI/CD для статического сайта в SourceCraft"
**Выполнил:** Богун Андрей Витальевич  
**Группа:** P3120  

---

## Содержание

1. [Цель работы](#цель-работы)
2. [Задачи](#задачи)
3. [Структура проекта](#структура-проекта)
4. [Реализация автоматического деплоя](#реализация-автоматического-деплоя)
   - [GitHub Actions](#github-actions)
   - [SourceCraft CI/CD](#sourcecraft-cicd)
5. [Результаты](#результаты)
6. [Выводы](#выводы)

---

## Цель работы

Научиться настраивать автоматическое развертывание статического сайта, построенного на движке MkDocs, с использованием двух независимых платформ: GitHub Pages (через GitHub Actions) и SourceCraft Sites (через встроенный CI/CD).

---

## Задачи

1. Создать локальный репозиторий с сайтом на MkDocs.
2. Создать два удалённых репозитория: на GitHub и на SourceCraft.
3. Настроить GitHub Actions для автоматической сборки и публикации сайта на GitHub Pages.
4. Настроить SourceCraft Sites.
5. Проверить работоспособность обоих сайтов.

---

## Структура проекта
Sourcecraft/ 
├── .github/
│ └── workflows/
│ └── gh-pages.yml 
├── .sourcecraft/
│ ├── sites.yaml 
│ └── ci.yaml 
├── docs/
│ ├── index.md 
│ └── about.md 
├── mkdocs.yml 
├── README.md
└── site/ 


---

## Реализация автоматического деплоя

### GitHub Actions

Файл `.github/workflows/gh-pages.yml`:

```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - run: pip install mkdocs mkdocs-material
      - run: mkdocs build
      - uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

### SourceCraft CI/CD

Файл `.sourcecraft/sites.yaml`:
```yaml
site:
  root: ./site
  ref: main
```

### Настройки в Sourcecraft
Создан персональный токен доступа (PAT)
После пуша папки site сайт автоматически стал доступен по адресу https://endryu-bogun.sourcecraft.site/my-site.

## Результаты

### GitHub Pages
ссылка на сайт:https://p3120-505199.github.io/Sourcecraft/	
ссылка на репозиторий: https://github.com/P3120-505199/Sourcecraft

### SourceCraft Sites
ссылка на сайт: https://endryu-bogun.sourcecraft.site/my-site	
ссылка на репозиторий: https://sourcecraft.dev/endryu-bogun/my-site

## Выводы

В ходе выполнения лабораторной работы были:
1. Освоены принципы работы GitHub Actions (сборка MkDocs, деплой на GitHub Pages).
2. Изучена конфигурация SourceCraft Sites через sites.yaml (декларативный подход).
3. Приобретён опыт работы с двумя независимыми удалёнными репозиториями (origin и sourcecraft)