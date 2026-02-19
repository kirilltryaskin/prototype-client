# Fork Strategy — Prototype Client

## Архитектура

`prototype-client` — публичный форк [HKUDS/nanobot](https://github.com/HKUDS/nanobot) с кастомными расширениями для платформы Prototype.

## Что обновляется при sync с upstream

✅ **Обновляется:**
- `nanobot/` — ядро агента, инструменты, провайдеры, навыки
- `tests/`, `bridge/`, `pyproject.toml` — инфраструктура nanobot
- `Dockerfile`, `docker-compose.yml` — базовая конфигурация

❌ **Не обновляется (защищено через `.gitattributes` merge=ours):**
- `nanobot/channels/http.py` — HTTP канал для Prototype SaaS
- `workspace/` — шаблоны для новых клиентов (SOUL.md, AGENTS.md, конфиг)

## При обновлении клиентского сервера

✅ **Обновляется:**
- Ядро nanobot (`pip install` из prototype-client)

❌ **Не обновляется:**
- `~/.openclaw/workspace/` — живёт на persistent Fly volume
- Память, навыки, MCP конфиг клиента — всё на volume

## Два уровня защиты

1. **Git (merge)** — `.gitattributes` с `merge=ours` защищает наши файлы от upstream
2. **Docker volume** — workspace клиента на persistent volume, не перезаписывается при деплое

## Как синхронизировать с upstream

```bash
git fetch upstream
git merge upstream/main
# .gitattributes автоматически сохраняет наши файлы
git push origin main
```

## Как добавить новое расширение

1. Создать файл в нужной директории
2. Добавить строку в `.gitattributes`: `path/to/file merge=ours`
3. Закоммитить и запушить

## Связанные репозитории

- **prototype-client** (этот репо) — публичный форк nanobot для клиентских серверов
- **prototype-backend** (приватный) — роутер, Fly.io оркестрация, Telegram бот, Docker образ
