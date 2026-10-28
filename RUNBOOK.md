# RUNBOOK

## Сервис не отвечает

Симптом: / или /health не отвечают, пользователи видят ошибки.

1. Жив ли сервис: `curl -s http://localhost:8000/health`
2. Запущены ли контейнеры: `docker compose ps` (если app не running -> `docker compose up -d`)
3. Что в логах: `docker compose logs --tail=50 app`
4. Доступна ли база: `curl -s http://localhost:8000/messages`
5. Был недавно релиз? -> откат: сменить тег образа в compose.yaml на предыдущий, выполнить `docker compose up -d`
6. Не помогло за 15 минут -> эскалация: позвать владельца сервиса

## База недоступна

Симптом: сервис /health отвечает, но /messages выдает ошибку (не может достучаться до БД).

1. Статус контейнера с базой: `docker compose ps db` (должен быть healthy)
2. Логи базы: `docker compose logs --tail=50 db`
3. Проверить переменные окружения приложения: `docker compose exec app env | grep DATABASE_URL`
4. Перезапустить базу: `docker compose restart db`
5. Не помогло за 15 минут -> эскалация: позвать владельца сервиса