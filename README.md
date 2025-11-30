# Briefly Monorepo

Монорепозиторий для **Briefly**, содержащий фронтенд (Next.js) и бэкенд (Actix) как Git submodules, с готовым `docker-compose` для локальной разработки и деплоя.

---

## Структура

```
briefly-monorepo/
├─ backend/      # Git submodule: Actix backend
├─ frontend/     # Git submodule: Next.js frontend
├─ docker-compose.yml
└─ README.md
```

* `backend/` — ссылка на репозиторий с Rust Actix сервером.
* `frontend/` — ссылка на репозиторий с Next.js фронтендом.
* `docker-compose.yml` поднимает оба сервиса и настраивает сетевое взаимодействие.

---

## Подключение submodules

При первом клоне:

```bash
git clone --recurse-submodules <URL_монорепо>
```

Если репо уже склонировано:

```bash
git submodule update --init --recursive
```

Для обновления submodules:

```bash
git submodule update --remote --merge
```

---

## Docker Compose

Сервисы:

* **backend**: слушает на порту `8080`, поднимается из папки `backend`.
* **frontend**: слушает на порту `3000`, зависит от backend, переменная окружения `NEXT_PUBLIC_API_URL=http://backend:8080`.

Запуск:

```bash
docker compose up --build
```

После запуска:

* Фронт: [http://localhost:3000](http://localhost:3000)
* API: [http://localhost:8080](http://localhost:8080)

---

## Переменные окружения

* `.env` для бэкенда хранится в `backend/.env`.
* Фронтенд получает URL бэка через `NEXT_PUBLIC_API_URL` в `docker-compose.yml`.

---

## Разработка

1. Вносишь изменения в `frontend` или `backend` как обычно в их репозиториях.
2. Пересобираешь контейнеры при необходимости:

```bash
docker compose up --build
```

3. Логи:

```bash
docker compose logs -f frontend
docker compose logs -f backend
```

