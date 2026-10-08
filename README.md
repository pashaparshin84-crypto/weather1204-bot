# Telegram Weather + AI Bot

Telegram-бот на Python, который:

- показывает текущую погоду;
- показывает прогноз на 7 дней;
- понимает простые запросы о погоде обычным текстом;
- отвечает на обычные вопросы через OpenAI API;
- хранит короткий контекст диалога в памяти процесса.

## 1. Что нужно установить

Нужен Python 3.10 или новее.

Проверить:

```bash
python --version
```

## 2. Создать Telegram-бота

1. Открой Telegram.
2. Найди `@BotFather`.
3. Выполни `/newbot`.
4. Придумай имя бота.
5. Придумай username, который заканчивается на `bot`.
6. BotFather выдаст токен.

Скопируй токен.

## 3. Получить OpenAI API Key

Создай API key в OpenAI Platform.

Важно: API key — секрет. Не отправляй его в Telegram, GitHub и не публикуй в интернете.

## 4. Установить зависимости

### Windows

Открой PowerShell в папке проекта:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Если PowerShell не разрешает активацию:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.venv\Scripts\Activate.ps1
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## 5. Создать .env

Скопируй `.env.example` в `.env`.

Windows:

```powershell
copy .env.example .env
```

macOS/Linux:

```bash
cp .env.example .env
```

Открой `.env` и вставь ключи:

```env
TELEGRAM_BOT_TOKEN=токен_от_BotFather
OPENAI_API_KEY=твой_OpenAI_API_ключ
OPENAI_MODEL=gpt-5-mini
```

## 6. Запустить

Windows:

```powershell
python bot.py
```

macOS/Linux:

```bash
python3 bot.py
```

Если всё правильно, в консоли появится:

```text
Bot started
```

После этого открой своего бота в Telegram и отправь:

```text
/start
```

## 7. Примеры

Текущая погода:

```text
/weather Москва
```

Прогноз:

```text
/forecast Москва
```

Обычным сообщением:

```text
Какая погода в Париже?
```

И обычные вопросы:

```text
Объясни простыми словами, что такое блокчейн
```

```text
Напиши мне короткое поздравление с днём рождения
```

## 8. Как работает погода

Для поиска города и прогноза используется Open-Meteo.

Отдельный ключ для Open-Meteo в этом проекте не нужен.

## 9. Запуск 24/7

Для постоянной работы бота лучше использовать VPS.

Подойдёт обычный Linux-сервер с Python. На VPS можно запустить:

```bash
python3 bot.py
```

Для настоящего продакшена лучше оформить бота как systemd-сервис или использовать Docker.

## 10. Важные замечания

### API-ключи

Не добавляй `.env` в Git:

```text
.env
```

уже находится в `.gitignore`.

### История чата

Сейчас история хранится только в оперативной памяти процесса. После перезапуска бота она исчезнет.

Если нужна постоянная история, можно добавить SQLite/PostgreSQL.

### Ограничение Telegram

Telegram имеет ограничения на размер сообщений. Если ответы ИИ будут очень длинными, в будущем можно добавить автоматическое разбиение ответа на несколько сообщений.

### Стоимость

Telegram Bot API и Open-Meteo не требуют оплаты за сам факт использования этого проекта. OpenAI API тарифицируется отдельно согласно выбранной модели и текущим тарифам OpenAI.

## Архитектура

```text
Telegram
   │
   ▼
python-telegram-bot
   │
   ├── /weather ───────► Open-Meteo Geocoding
   │                         │
   │                         ▼
   │                    Open-Meteo Forecast
   │
   ├── /forecast ──────► Open-Meteo
   │
   └── обычный текст ──► OpenAI API
```
