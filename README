# Авторецензент: извлечение требований из Excel

## Описание

Проект представляет собой прототип скрипта для автоматического извлечения требований из Excel-документа с помощью LLM.

Скрипт читает таблицы с рисками, вопросами к документу, рекомендациями и примерами формулировок, после чего формирует структурированный JSON с требованиями.

## Цель

Автоматически получить из исходного Excel-файла список требований в JSON-формате с сохранением ссылки на исходный лист и чанк.

## Основной пайплайн

Проект состоит из двух этапов:

```text
Excel-файл
↓
разбиение на чанки
↓
обработка чанков через DeepSeek API
↓
итоговый JSON
```

## Файлы проекта

```text
01_prepare_chunks_by_sheets.ipynb
```

Подготавливает данные:

- читает Excel;
- разбивает документ на чанки по листам;
- сохраняет результат в `chunks.json`.

```text
02_extract_with_deepseek_final_json.ipynb
```

Извлекает требования:

- читает `chunks.json`;
- отправляет каждый чанк в DeepSeek API;
- получает JSON-ответ;
- проверяет и нормализует поля;
- сохраняет итоговый файл `requirements_output_deepseek.json`.

## Настройка API-ключа

В папке проекта нужно создать файл `.env`:

```env
DEEPSEEK_API_KEY=your_api_key_here
```

Файл `.env` не нужно публиковать или отправлять вместе с проектом.

## Установка зависимостей

```python
%pip install pandas openpyxl requests python-dotenv
```

## Порядок запуска

1. Запустить ноутбук:

```text
01_prepare_chunks_by_sheets.ipynb
```

После выполнения появится файл:

```text
chunks.json
```

2. Запустить ноутбук:

```text
02_extract_with_deepseek_final_json.ipynb
```

После выполнения появится итоговый файл:

```text
requirements_output_deepseek.json
```

## Структура результата

Итоговый JSON содержит:

```json
{
  "metadata": {
    "model": "deepseek-v4-flash",
    "provider": "deepseek",
    "chunks_total": 12,
    "requirements_total": 213,
    "errors_total": 0
  },
  "requirements": []
}
```

Каждое требование имеет поля:

```json
{
  "name": "...",
  "requirements_brief": "...",
  "check_prompt": "...",
  "document_category_id": 6,
  "system_prompt_id": 1,
  "version": 1,
  "status": "published",
  "risks": "...",
  "severity": "HIGH",
  "check_result_example": "...",
  "suggested_solutions": "...",
  "source": {
    "chunk_id": 1,
    "source_section": "...",
    "sheet_name": "...",
    "chunking_strategy": "one_sheet_one_chunk"
  }
}
```

## Fallback-обработка

Основная стратегия:

```text
один лист Excel = один чанк
```

Если модель не смогла корректно обработать лист целиком, используется резервная стратегия:

```text
лист делится на подчанки по 5 строк
```

Это помогает избежать ошибок при длинных таблицах и получить валидный JSON.

## Результат

В результате работы проекта был получен JSON-файл со структурированными требованиями.  
Каждое требование содержит основные поля для проверки документа и ссылку на исходный фрагмент Excel-файла.
