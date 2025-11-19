# Лабораторна робота: Неперервна Інтеграція та Доставка (CI/CD)

Виконав: Лапін Микита ІПЗ-3.03  
Hello GitHub Actions - https://github.com/ni-cookie/skills-hello-github-actions  
Publish Packages - https://github.com/ni-cookie/skills-publish-packages.git   

## Огляд проєкту

Цей репозиторій створено для виконання практичного завдання з автоматизації процесів **Неперервної Інтеграції та Доставки (CI/CD)** з використанням GitHub Actions.

Мета роботи: створити робочий процес, який виконує збірку (build) простого front-end проєкту та публікує його як Docker-образ у **GitHub Container Registry (GHCR)**.

## Використані технології

* **Система автоматизації:** GitHub Actions
* **Контейнеризація:** Docker
* **Реєстр образів:** GitHub Container Registry (GHCR)
* **Середовище збірки:** Node.js (для `npm install` та `npm run build`)
* **Сервер:** Nginx (для обслуговування статичних файлів)

---

## ⚙️ Структура проєкту та файли

| Файл / Директорія | Призначення |
| :--- | :--- |
| `index.html` | Простий front-end файл, який обслуговується контейнером. |
| `package.json` | Містить скрипт `build`, що імітує збірку front-end проєкту. |
| `Dockerfile` | Багатоетапний файл Docker, який збирає проєкт і створює фінальний образ на базі Nginx. |
| `.github/workflows/ci-cd.yml` | Визначає логіку CI/CD робочого процесу (Workflow). |

---

## GitHub Actions Workflow (`.github/workflows/ci-cd.yml`)

Робочий процес `ci-cd.yml` відповідає вимогам завдання.

### Тригери (Запуск)

Workflow запускається автоматично за двома умовами:
1.  **Ручний запуск:** Через інтерфейс GitHub (`workflow_dispatch`).
2.  **Автоматичний запуск:** При пуші комітів у гілки `main` або будь-яку гілку, що починається з `feature/*`.

### Етапи виконання (`build-and-push`)

Робочий процес складається з однієї Job (`build-and-push`), що виконується на `ubuntu-latest` і містить такі кроки:

1.  **Checkout repository:** Клонування коду.
2.  **Install dependencies and build:** Виконання команд `npm install` та `npm run build` (згідно з `package.json`).
3.  **Log in to the Container registry:** Авторизація в `ghcr.io` з використанням вбудованих змінних контексту: `username: ${{ github.actor }}` та `password: ${{ secrets.GITHUB_TOKEN }}`.
4.  **Build and push Docker image:** Збірка образу (контекст `.`) та його публікація до GHCR. Образ отримує тег у форматі: `ghcr.io/${{ github.repository_owner }}/${{ github.event.repository.name }}:latest`.