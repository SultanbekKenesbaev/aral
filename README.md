# LoveFrame

Платформа для создания поздравительных mini-sites внутри одного домена: лендинг, Google OAuth, личный кабинет, история открыток, редактор, публикация по уникальному subdomain и хранение медиа в S3-compatible storage.

## Stack

- `web`: Next.js 16, TypeScript, Tailwind CSS 4, App Router
- `api`: FastAPI, SQLAlchemy async, PostgreSQL, Redis
- `storage`: S3-compatible object storage (`MinIO` в локальной среде)
- `infra`: Docker Compose для локальной сборки и пример wildcard ingress через Caddy

## Structure

- [`web`](/Users/sultanbekkenesbaev/Desktop/gift/web) — лендинг, кабинет, редактор и публичный рендер открытки
- [`api`](/Users/sultanbekkenesbaev/Desktop/gift/api) — OAuth, сессии, шаблоны, CRUD mini-sites, publish и presigned uploads
- [`infra/Caddyfile`](/Users/sultanbekkenesbaev/Desktop/gift/infra/Caddyfile) — пример маршрутизации `mydomen.uz`, `app.mydomen.uz`, `api.mydomen.uz`, `*.mydomen.uz`
- [`docker-compose.yml`](/Users/sultanbekkenesbaev/Desktop/gift/docker-compose.yml) — локальный контейнерный запуск

## Local setup

1. Скопируйте переменные окружения:
   `cp .env.example .env`
2. Заполните `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GOOGLE_REDIRECT_URI`.
3. Поднимите всё через Docker:
   `docker compose up --build`

### AI configuration

- Set `OPENAI_API_KEY` in `.env`
- Use `OPENAI_BASE_URL=https://openrouter.ai/api/v1`
- Use `OPENAI_MODEL=openai/gpt-5.4`

После запуска:

- Frontend: `http://localhost:3000`
- API: `http://localhost:8000`
- MinIO API: `http://localhost:9000`
- MinIO console: `http://localhost:9001`

## Local development without Docker

### API

```bash
cd api
python3 -m venv .venv
. .venv/bin/activate
pip install -e '.[dev]'
uvicorn app.main:app --reload
```

### Web

```bash
cd web
npm install
npm run dev
```

## What is implemented

- Светлый premium landing с RU/UZ переключателем
- Вход только через Google OAuth
- Серверные cookie-сессии и Redis-backed cache
- Каталог шаблонов из кода с `Wedding 3D Invite` и `Luxury Wedding 3D Invite`
- Dashboard с историей mini-sites
- Editor с autosave draft, загрузкой фона и музыки через presigned uploads
- Publish flow с отдельным published snapshot
- Public rendering по host/subdomain

## Verification

- Backend tests:
  `cd api && . .venv/bin/activate && pytest -q`
- Frontend lint:
  `cd web && npm run lint`
- Frontend build:
  `cd web && npm run build`
