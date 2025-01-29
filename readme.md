# MotiveYou 3.0

## Tech Stack

### Backend
- Node.js with NestJS framework
  - TypeScript for type safety
  - PostgreSQL with Prisma ORM
  - GraphQL API with Apollo Server
  - JWT authentication
  - Redis for caching
  - WebSocket support for real-time features

### Frontend
- Next.js 14
  - React Server Components
  - TypeScript
  - Tailwind CSS
  - Apollo Client for GraphQL
  - Zustand for state management
  - React Hook Form for forms
  - Zod for validation

### DevOps
- Docker & Docker Compose
- GitHub Actions for CI/CD
- AWS or Vercel for hosting

## Data Schema

### User
```typescript
model User {
  id            String    @id @default(uuid())
  email         String    @unique
  passwordHash  String
  name          String
  bio           String?
  avatar        String?
  role          Role      @default(USER)
  goals         Goal[]
  habits        Habit[]
  books         Book[]
  insights      Insight[]
  reviews       Review[]
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

enum Role {
  USER
  ADMIN
}
```

### Goal
```typescript
model Goal {
  id          String    @id @default(uuid())
  title       String
  description String?
  deadline    DateTime?
  status      Status    @default(IN_PROGRESS)
  priority    Priority  @default(MEDIUM)
  userId      String
  user        User      @relation(fields: [userId], references: [id])
  habits      Habit[]
  tasks       Task[]
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}

enum Status {
  IN_PROGRESS
  COMPLETED
  ABANDONED
}

enum Priority {
  LOW
  MEDIUM
  HIGH
}
```

### Habit
```typescript
model Habit {
  id          String       @id @default(uuid())
  title       String
  description String?
  frequency   Frequency    @default(DAILY)
  goalId      String?
  goal        Goal?        @relation(fields: [goalId], references: [id])
  userId      String
  user        User         @relation(fields: [userId], references: [id])
  streaks     Int         @default(0)
  completions Completion[]
  createdAt   DateTime    @default(now())
  updatedAt   DateTime    @updatedAt
}

enum Frequency {
  DAILY
  WEEKLY
  MONTHLY
}
```

### Task
```typescript
model Task {
  id          String    @id @default(uuid())
  title       String
  description String?
  dueDate     DateTime?
  completed   Boolean   @default(false)
  goalId      String
  goal        Goal      @relation(fields: [goalId], references: [id])
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}
```

### Completion
```typescript
model Completion {
  id        String   @id @default(uuid())
  date      DateTime @default(now())
  habitId   String
  habit     Habit    @relation(fields: [habitId], references: [id])
  createdAt DateTime @default(now())
}
```

### Book
```typescript
model Book {
  id          String    @id @default(uuid())
  title       String
  authors     String[]
  cover       String?
  description String?
  tags        String[]
  rating      Float?
  isPrivate   Boolean   @default(false)
  userId      String
  user        User      @relation(fields: [userId], references: [id])
  insights    Insight[]
  reviews     Review[]
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}
```

### Insight
```typescript
model Insight {
  id          String    @id @default(uuid())
  text        String
  page        Int?
  chapter     String?
  bookId      String
  book        Book      @relation(fields: [bookId], references: [id])
  userId      String
  user        User      @relation(fields: [userId], references: [id])
  likes       Int       @default(0)
  comments    Comment[]
  isPrivate   Boolean   @default(false)
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}
```

### Review
```typescript
model Review {
  id          String    @id @default(uuid())
  text        String
  rating      Float
  bookId      String
  book        Book      @relation(fields: [bookId], references: [id])
  userId      String
  user        User      @relation(fields: [userId], references: [id])
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}
```

### Comment
```typescript
model Comment {
  id        String   @id @default(uuid())
  text      String
  insightId String
  insight   Insight  @relation(fields: [insightId], references: [id])
  userId    String
  user      User     @relation(fields: [userId], references: [id])
  createdAt DateTime @default(now())
}
```

## Словарь терминов

### Основные понятия
- **Book (Книга)** - бизнес-книга или книга по саморазвитию, которая содержит полезные идеи и практики
- **Insight (Инсайт)** - ключевая мысль или цитата из книги, которая несет практическую ценность
- **Habit (Привычка)** - регулярное действие, которое пользователь хочет развить
- **Goal (Цель)** - конкретная измеримая цель, которую пользователь хочет достичь
- **Task (Задача)** - конкретный шаг для достижения цели
- **Review (Обзор)** - пользовательская рецензия на книгу с оценкой и комментарием
- **Streak (Серия)** - количество дней подряд, когда привычка выполнялась
- **Comment (Комментарий)** - обсуждение инсайта или обзора другими пользователями

### Функциональные термины
- **Private/Public** - режим видимости контента (приватный/публичный)
- **Rating (Рейтинг)** - оценка книги по 5-балльной шкале
- **Tags (Теги)** - категории или темы, к которым относится книга
- **Completion (Выполнение)** - отметка о выполнении привычки в конкретный день

## Key Features
1. Библиотека бизнес-книг и книг по саморазвитию
2. Система сбора и организации инсайтов из книг
3. Цели и задачи с отслеживанием прогресса
4. Формирование привычек с системой streak
5. Социальные функции (комментарии, лайки, шеринг)
6. Аналитика и визуализация прогресса
7. Геймификация достижений

## Особенности работы с книгами и инсайтами
- Возможность добавления книг в личную библиотеку
- Выделение и сохранение ключевых цитат (инсайтов)
- Категоризация книг по тегам
- Система рейтингов и обзоров
- Приватные и публичные инсайты
- Социальное взаимодействие через комментарии и лайки
- Поиск и фильтрация по книгам и инсайтам

## API Endpoints

### GraphQL Schemas будут включать:
- Mutations для CRUD операций с целями, задачами, привычками, книгами и инсайтами
- Queries для получения данных с фильтрацией и пагинацией
- Subscriptions для real-time уведомлений

## Безопасность
- JWT авторизация
- Rate limiting
- Input validation
- CORS настройки
- Encryption at rest
- XSS protection

## Масштабирование
- Микросервисная архитектура (опционально)
- Кэширование с Redis
- Очереди задач для обработки уведомлений
- CDN для статических ресурсов

## Анализ конкурентов

### Трекеры привычек и целей
1. **Habitify**
   - Сильные стороны: Красивый UI, детальная аналитика, интеграция с Apple Health
   - Слабые стороны: Нет работы с книгами и инсайтами, нет социального взаимодействия

2. **Strides**
   - Сильные стороны: Гибкая настройка трекинга, множество типов целей
   - Слабые стороны: Сложный интерфейс, нет работы с контентом

3. **Way of Life**
   - Сильные стороны: Простой интерфейс, хорошая визуализация
   - Слабые стороны: Ограниченный функционал, нет социальных функций

### Приложения для работы с книгами
1. **Goodreads**
   - Сильные стороны: Большое сообщество, обширная база книг, интеграция с Amazon
   - Слабые стороны: Устаревший интерфейс, нет трекинга привычек и целей

2. **Readwise**
   - Сильные стороны: Отличная работа с хайлайтами, интеграция с Kindle
   - Слабые стороны: Высокая цена, нет трекинга привычек

3. **Blinkist**
   - Сильные стороны: Краткое содержание книг, аудиоформат
   - Слабые стороны: Нет работы с полными текстами, нет социальных функций

### Приложения для заметок и инсайтов
1. **Notion**
   - Сильные стороны: Гибкость, мощные инструменты организации
   - Слабые стороны: Сложность для начинающих, нет специализированных функций для книг

2. **Evernote**
   - Сильные стороны: Удобный веб-клиппер, распознавание текста
   - Слабые стороны: Ограниченная бесплатная версия, нет специализации на книгах

### Наши преимущества
1. **Интеграция функционала**
   - Объединение работы с книгами, инсайтами и трекингом привычек
   - Связь между теорией (книги) и практикой (привычки, цели)

2. **Фокус на развитии**
   - Специализация на бизнес-литературе и саморазвитии
   - Структурированный подход к работе с инсайтами

3. **Социальное взаимодействие**
   - Обмен инсайтами и опытом
   - Совместная мотивация и поддержка

4. **Аналитика и геймификация**
   - Детальная статистика прогресса
   - Система достижений и наград

### Ценовая политика конкурентов
- Habitify: $5.99/месяц, $39.99/год
- Readwise: $7.99/месяц, $79.99/год
- Blinkist: $15.99/месяц, $99.99/год

## Уникальные фичи конкурентов

### Habitify
- Интеграция с Apple Health и Google Fit
- Виджеты для iOS и Android
- Экспорт данных в CSV/Excel
- Настраиваемые напоминания по геолокации

### Strides
- Гибкие типы целей (Да/Нет, Число, Среднее или Проект)
- SMART-цели с пошаговыми инструкциями
- Пользовательские графики и диаграммы
- Пакетное редактирование привычек

### Goodreads
- Сканирование ISBN книг
- Ежегодный reading challenge
- Рекомендации на основе прочитанных книг
- Группы по интересам и книжные клубы
- Интеграция с Amazon для покупки книг

### Readwise
- Автоматический импорт хайлайтов из Kindle
- Система интервального повторения (spaced repetition)
- Интеграция с Notion, Evernote, Roam
- OCR для сканирования физических книг
- Daily review с избранными цитатами

### Blinkist
- Профессиональные саммари книг
- Аудиоверсии саммари
- Подкасты с авторами книг
- Тематические плейлисты книг
- Оффлайн доступ к контенту

### Notion
- Настраиваемые базы данных
- Коллаборативное редактирование
- API для интеграций
- Встроенные таблицы и канбан-доски
- Версионирование страниц

## Потенциальные направления развития
На основе анализа конкурентов, следующие фичи могут быть добавлены в наш проект:

### Интеграции
- [ ] Импорт хайлайтов из Kindle
- [ ] Интеграция с фитнес-трекерами
- [ ] API для сторонних приложений
- [ ] Экспорт данных в популярные форматы

### Работа с контентом
- [ ] OCR для сканирования физических книг
- [ ] Аудиоверсии инсайтов
- [ ] Система интервального повторения
- [ ] Тематические подборки книг
- [ ] Профессиональные саммари (как опция)

### Социальные функции
- [ ] Книжные клубы
- [ ] Групповые челленджи
- [ ] Менторство и коучинг
- [ ] Геймификация социального взаимодействия

### Мобильный опыт
- [ ] Виджеты для быстрого доступа
- [ ] Оффлайн режим
- [ ] Геолокационные напоминания
- [ ] Сканирование ISBN

### Аналитика и персонализация
- [ ] Рекомендательная система книг
- [ ] Персонализированные челленджи
- [ ] Расширенная визуализация прогресса
- [ ] AI-ассистент для анализа целей

### Продвинутые инструменты
- [ ] Пакетное редактирование
- [ ] Настраиваемые дашборды
- [ ] Шаблоны целей и привычек
- [ ] Версионирование заметок