# Система управления бронированием услуг в отелях 

Система предназначена для эффективного распределения нагрузки и управления бронированиями по временным слотам для специалистов сферы услуг.

## Установка и запуск

1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/Ganza6/Hotel-booking-management-System.git
   ```

2. Установите зависимости:
   ```bash
   npm install
   ```

3. Запустите приложение:
   ```bash
   npm start
   ```

## Тестирование

Запустите тесты командой:
```bash
   npm test
```

## Реализация требований

1. **Формат хранения расписания**
   - Реализована реляционная структура с таблицами Workers, Services, Schedules и BusySlots
   - В базе хранятся только занятые слоты, что упрощает модификацию расписания
   - Каждая смена содержит информацию о работнике, дате и времени работы

2. **Механизм учета слотов**
   - Занятые слоты хранятся в таблице BusySlots с указанием времени начала и продолжительности
   - Свободные слоты вычисляются динамически на основе графика работы и занятых слотов
   - Реализована функция getFreeTimeSlots для эффективного расчета доступных интервалов

3. **Функция getAvailableTimeSlots**
   ```typescript
   getAvailableTimeSlots({ 
     date: string;
     serviceId: string;
     workerId?: string;
   }): Promise<Slot[]>
   ```
   - Возвращает массив доступных слотов с указанием времени и их кол-ва
   - Учитывает длительность услуги и график работы специалистов
   - Поддерживает фильтрацию по конкретному работнику и по id услуги

4. **Плотная упаковка слотов**
   - Алгоритм учитывает занятость специалиста на всю длительность услуги. Такой подход обеспечивает максимально эффективное использование рабочего времени

5. **Выбор массажиста**
   - Реализована возможность получения слотов для конкретного специалиста

### Особенности реализации

1. **Динамический расчет свободных слотов**
   - В базе данных хранятся только занятые слоты
   - Свободные слоты вычисляются "на лету"
   - Учитывается график работы специалиста и его текущая загруженность
   - Позволяет максимально плотно упаковывать расписание

2. **Автоматическое создание смен**
   - Каждый день в 00:00 запускается задача `createNewDayShift`
   - Добавляет новую смену для всех работников, чтобы записаться к ним можно было заранее
   - Учитывает индивидуальный график каждого работника (кто-то работает полный день, а кто-то пол дня)

3. **Гибкий график работы**
   - Работники могут иметь разное время начала и окончания работы
   - Предусмотрена возможность административного интерфейса для управления расписанием
   - Текущая реализация позволяет легко добавлять, например, обеденные перерывы через создание занятых слотов

## База данных

Проект использует реляционную базу данных со следующими таблицами:
- `Workers` - информация о работниках
- `Services` - доступные услуги
- `WorkerServices` - связь работников с услугами
- `Schedules` - расписание работников
- `BusySlots` - занятые временные слоты

Полная схема базы данных доступна в файле `tables.sql`.

## Структура проекта

- `src/` – исходный код  
  - `functions/` – бизнес-логика  
  - `types/` – типы и интерфейсы  
  - `jobs/` – задачи (например, создание смен)
- `mock-db/` – эмуляция базы данных  
- `configs/` – конфигурационные файлы
- `tests/` - тесты

## Технический стек

- TypeScript для типизации
- date-fns для работы с датами
- ESLint + Prettier для форматирования кода
- jest для написания тестов
