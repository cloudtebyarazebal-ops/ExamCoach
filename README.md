# ExamCoach — тренажёр и адаптер для демоэкзамена КОД

Офлайн HTML-приложение, **WPF desktop-приложение** и **CLI AdaptTest** для автоматической подгонки ASP.NET Core проекта под текст ТЗ (PDF/TXT).

## Требования

- .NET 8 SDK
- Windows (WPF desktop)
- Visual Studio 2022 (опционально, для пошагового режима)

## Быстрый старт

### 1. Сборка desktop-приложения

```powershell
cd ExamCoach
powershell -ExecutionPolicy Bypass -File .\Build-ExamCoachDesktop.ps1
.\Start-ExamCoachDesktop.bat
```

### 2. HTML-тренажёр (офлайн)

```powershell
powershell -ExecutionPolicy Bypass -File .\Generate-ExamCoach.ps1
```

Откройте **`index.html`** в браузере.

### 3. Адаптация проекта под ТЗ (AdaptTest)

Сверка без изменений:

```cmd
dotnet run --project tools\AdaptTest\AdaptTest.csproj -- "C:\path\to\tz.txt"
```

Применить к проекту (закройте запущенное приложение перед `--apply`):

```cmd
cd /d "C:\path\to\YourMvcProject"
dotnet run --project "C:\path\to\ExamCoach\tools\AdaptTest\AdaptTest.csproj" -- "C:\path\to\tz.txt" --apply
dotnet build && dotnet run
```

AdaptTest:

- распознаёт вариант ПУ/БУ и домен (книги, комплектующие, настольные игры, смартфоны и др.);
- подставляет текстовые замены, сид БД, Author/Manufacturer, пороги скидок;
- пересоздаёт `Entities.cs`, `DbSeeder.cs`, сбрасывает SQLite при смене ТЗ.

### 4. Пошаговый режим в Visual Studio

1. **File → New → Project → ASP.NET Core Web App (MVC)**, .NET 8.
2. Запустите ExamCoachDesktop → **Подключить Visual Studio** → **Из VS — найти проект**.
3. Идите по шагам — файлы создаются в вашем проекте, код вставляется в редактор VS.

## Обновление manifest из эталонного проекта

```powershell
powershell -ExecutionPolicy Bypass -File .\Generate-ExamCoach.ps1 -UpdateManifest -ProjectRoot "C:\path\to\KodShopWeb"
```

Скрипт **`Update-Manifest.ps1`** сканирует MVC-проект: `Models/`, `Data/`, `Services/`, `Controllers/`, `Views/`, `wwwroot/`.

## Структура репозитория

| Путь | Назначение |
|------|------------|
| `Desktop/` | WPF-приложение (парсер ТЗ, трансформации, UI) |
| `tools/AdaptTest/` | CLI: сверка и `--apply` к целевому проекту |
| `tools/PdfExtract/` | Утилита извлечения текста из PDF |
| `steps-data.json` | Шаги для HTML и Desktop |
| `assignment-transform-rules.json` | Правила текстовых замен |
| `index.html` | Офлайн-тренажёр в браузере |

## Таймеры модулей (4 часа)

| Модуль | Время | Содержание |
|--------|-------|------------|
| **М1 — БД** | 50 мин | Models + Data + DbSeeder + ER + импорт |
| **М2 — Вход** | 40 мин | Program + Account + Products Index + Login |
| **М3 — Товары** | 1ч 30 | ProductService + Edit view + products.js |
| **М4 — Заказы** | 1ч | Orders (вариант ПУ) |

## NuGet целевого MVC-проекта (8.x)

```
Microsoft.EntityFrameworkCore.Sqlite 8.0.11
Microsoft.EntityFrameworkCore.Design 8.0.11
ClosedXML 0.105.0
```

## Сборка solution

```cmd
dotnet build ExamCoach.sln
```

Не переписывайте всё по буквам — используйте **«Построчно»** и **✓ Готово** как чеклист.
