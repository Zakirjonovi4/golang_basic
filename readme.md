# Activity Tracker Platform

## Цель

Создать платформу, позволяющую пользователям отслеживать историю своей активности по Telegram ID через Telegram-бота и Web UI. Приложение должно быть расширяемым: DnD-модуль является лишь одним из возможных типов активности.

---

Telegram      Browser
   │              │
   ▼              ▼
┌────────────── API Gateway ─────────────┐
│                                        │
│       ┌────────────┐    ┌────────────┐ │
│       │  Auth Svc  │    │ Profile Svc│ │
│       └────────────┘    └────────────┘ │
│                                        │
│     ┌────────────┬─────────────┐       │
│     ▼            ▼             ▼       │
│ DnD Module   Meeting Module  Stats Svc │
│     │            │             │       │
│     └────────────┴─────────────┘       │
│              PostgreSQL                |
└────────────────────────────────────────┘


---

## Роли пользователей
- **Обычный юзер**: видит свою историю активности
- **Организатор**: добавляет активности, заполняет сводки, помечает участников

---

## Типы активности
- **DnD-походы**: получение данных с DnD Beyond, запись снепшотов персонажей, опрос кто будет, диф
- **Встречи**: запись кто был, тема встречи, заметки
- **Проекты**: отслеживание участия в задачах, списки событий

---

## Интерфейсы
- **Telegram Bot**: команды, опросы, срезы игр
- **Web UI**: профиль юзера, история, отчеты, организация событий

---

## Базовая модель данных (Go)

```go
type User struct {
    ID          string
    TelegramID  string
    Username    string
    Role        string
}

type Activity struct {
    ID          string
    Type        string
    Title       string
    Description string
    Date        time.Time
    OrganizerID string
    Participants []Participant
}

type Participant struct {
    UserID     string
    ActivityID string
    Status     string // e.g. "present", "absent"
}

type DnDCharacterSnapshot struct {
    ID           string
    CharacterID  string
    ActivityID   string
    StatsJSON    string
    Timestamp    time.Time
}
```

---

## Первый этап MVP
- Telegram-бот с основными командами
- DnD Parser для снепшотов
- PostgreSQL + Docker Compose
- UI с историей игр и отчётом
- API Gateway

---

## План на будущее
- Telegram OAuth для Web UI
- Другие типы активности (onboarding, собрания, team events)
- Роли, права доступа
- Статистика, графики, отчёты
- CI/CD, мониторинг, бэкапы
```

