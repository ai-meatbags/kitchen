# Linear Issue Maker

Alfred workflow для быстрого создания задач в [Linear](https://linear.app) прямо из Alfred.

## Установка

1. Скачайте `Linear Issue Maker.alfredworkflow`
2. Дважды кликните для установки в Alfred
3. Настройте обязательные переменные (см. ниже)

## Настройка

### Обязательные переменные

В настройках workflow (Alfred → Workflows → Linear Issue Maker → ⓘ) укажите:

| Переменная | Описание |
|------------|----------|
| `API_KEY` | API-ключ Linear. [Получить здесь](https://linear.app/settings/api) |
| `TEAM_ID` | UUID команды в Linear |

Как получить UUID команды:
1. Откройте Linear
2. Нажмите Cmd+K, введите `UUID`
3. Выберите `Developer: Copy Model UUID...`
4. Найдите и скопируйте вашу команду

### Опциональные переменные

Дополнительные настройки можно указать по желанию — workflow будет работать и без них.

| Переменная | Описание |
|------------|----------|
| `PROJECT_ID` | UUID проекта по умолчанию |
| `STATE_ID` | UUID статуса по умолчанию |
| `ASSIGNEE_ID` | UUID ответственного по умолчанию |
| `LABEL_IDS` | UUID меток по умолчанию (через запятую) |
| `DEFAULT_PRIORITY` | Приоритет по умолчанию: `u/h/m/l` или `0-4` |
| `API_URL` | Переопределить URL API (по умолчанию `https://api.linear.app/graphql`) |
| `PYTHON_BIN` | Путь к python, если Alfred не видит `python3` |
| `DEBUG` | `true/1` для подробных ошибок |

## Использование

Вызовите Alfred и введите ключевое слово workflow (по умолчанию `li`), затем текст задачи.

Примеры:
- `li Fix login bug`
- `li Fix login bug -u`
- `li Add docs -l`

На ошибке покажет уведомление с текстом, подробности доступны в Alfred Debugger.

## Требования

- [Alfred](https://www.alfredapp.com/) с Powerpack
- Аккаунт в [Linear](https://linear.app)
- Python 3 (если Alfred не видит `python3`, укажите `PYTHON_BIN`)

## Лицензия

MIT
