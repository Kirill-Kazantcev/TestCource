

Проект: [«На страже Волшебной школы»](https://github.com/Kirill-Kazantcev/higher-web-practice-school)

### Сводка по статусам

| Статус | Количество | Процент |
|--------|------------|---------|
| Выполнено | 38 | 76% |
| Частично | 6 | 12% |
| Не выполнено | 6 | 12% |

**Условные обозначения:**
- **Выполнено** – тест уже реализован в коде проекта (JUnit)
- **Частично** – покрытие есть, но не полностью соответствует кейсу
- **Не выполнено** – тест отсутствует или требует доработки

---

### 1. Сьют: Аутентификация и вход в систему (6 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-1 | Вход существующего пользователя (pooh) | Система запущена, пользователь не авторизован | 1. Ввести логин `pooh`<br>2. Ввести пароль `1!` | Вход выполнен, текущий пользователь – Винни Пух (GUARD) | Выполнено |
| TC-2 | Вход с неверным паролем | Система запущена | 1. Ввести логин `owl`<br>2. Ввести неверный пароль `wrong` | Сообщение об ошибке аутентификации | Выполнено |
| TC-3 | Вход несуществующего пользователя | – | 1. Ввести логин `unknown`<br>2. Любой пароль | Ошибка: «Пользователь не найден» | Выполнено |
| TC-4 | Смена текущего пользователя без выхода | Авторизован `pooh` | 1. Выполнить команду `login roo`<br>2. Ввести пароль `5%` | Текущий пользователь сменился на Крошку Ру (STUDENT) | Частично |
| TC-5 | Выход из системы (logout) | Авторизован любой пользователь | 1. Выполнить команду `logout` | Сессия завершена, запрос логина и пароля | Выполнено |
| TC-6 | Повторный вход после выхода | Выполнен logout | 1. Войти снова (`rabbit` / `6^`) | Вход успешен, история действий сохранилась | Частично |

---

### 2. Сьют: Ролевая модель и базовые права (5 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-7 | ADMIN имеет полный доступ к помещениям | Авторизован `robin` (ADMIN) | 1. Попытаться войти в `School`, `Cabin`, `Room`, `ClassA` | Все попытки успешны | Выполнено |
| TC-8 | GUARD может войти только в школу | Авторизован `pooh` (GUARD) | 1. Войти в `School` – успех<br>2. Войти в `TeachersRoom` – ошибка | Доступ в школу разрешён | Выполнено |
| TC-9 | STUDENT может войти в класс только с учителем | Авторизован `roo`, учитель `piglet` НЕ в классе | 1. Попытаться войти в `ClassB` | Отказано из-за `TeacherPresentCondition` | Выполнено |
| TC-10 | TEACHER имеет доступ в учительскую и классы | Авторизован `piglet` | 1. Войти в `TeachersRoom`<br>2. Войти в `ClassC` | Оба входа разрешены | Выполнено |
| TC-11 | CHIEF может войти в кабинет директора всегда | Авторизован `owl` | 1. Войти в `DirectorCabin` | Успешно | Выполнено |

---

### 3. Сьют: Доступ к помещениям (локациям) (6 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-12 | Вход в школу для всех ролей | Все роли поочерёдно | 1. Каждый пробует войти в `School` | Все успешно входят | Выполнено |
| TC-13 | Вход учителя в кабинет директора при отсутствии директора | Директор не в кабинете, авторизован `tigger` | 1. Войти в `DirectorCabin` | Успешно (`DirectorAbsentCondition`) | Выполнено |
| TC-14 | Вход учителя в кабинет директора при присутствии директора | Директор в кабинете, авторизован `eeyore` | 1. Войти в `DirectorCabin` | Отказано | Выполнено |
| TC-15 | Вход студента в класс без учителя | В `ClassA` нет учителя, авторизован `roo` | 1. Войти в `ClassA` | Отказано | Выполнено |
| TC-16 | Вход студента в класс с учителем | Учитель `rabbit` вошёл в `ClassD` | 1. `roo` входит в `ClassD` | Успешно | Выполнено |
| TC-17 | Вход родителя в учительскую | Авторизована `kanga` | 1. Попытаться войти в `TeachersRoom` | Отказано | Выполнено |

---

### 4. Сьют: Журнал оценок (7 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-18 | Учитель просматривает журнал | Авторизован `piglet` | 1. `watch journal` | Отображаются оценки всех учеников | Выполнено |
| TC-19 | Учитель редактирует журнал | Авторизован `piglet` | 1. Выставить оценку `roo` – 5 | Оценка сохранена | Выполнено |
| TC-20 | Студент просматривает журнал (с учителем) | Учитель в классе, авторизован `roo` | 1. `watch journal` | Студент видит оценки | Выполнено |
| TC-21 | Студент пытается редактировать журнал | Авторизован `roo` | 1. `edit journal` | Отказано | Выполнено |
| TC-22 | Родитель смотрит журнал – только своего ребёнка | Авторизована `kanga` | 1. `watch journal` | Видит только оценки `roo` | Частично |
| TC-23 | Родитель пытается редактировать журнал | Авторизована `kanga` | 1. `edit journal` | Отказано | Выполнено |
| TC-24 | Директор просматривает журнал | Авторизован `owl` | 1. `watch journal` | Видит оценки всех учеников | Выполнено |

---

### 5. Сьют: Условные разрешения (Conditions) (5 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-25 | DirectorAbsentCondition – учитель в кабинет, директор отсутствует | Директор не в кабинете, авторизован `rabbit` | 1. Войти в `DirectorCabin` | Успешно | Выполнено |
| TC-26 | DirectorAbsentCondition – учитель в кабинет, директор присутствует | Директор в кабинете, учитель пытается войти | 1. `owl` в `Cabin`<br>2. `rabbit` пытается войти | Отказано | Выполнено |
| TC-27 | TeacherPresentCondition – студент в класс, учитель есть | Учитель в классе, авторизован `roo` | 1. `roo` входит в класс | Успешно | Выполнено |
| TC-28 | TeacherPresentCondition – студент в класс, учителя нет | Учителя нет в классе | 1. `roo` пытается войти | Отказано | Выполнено |
| TC-29 | OwnChildOnlyCondition – родитель видит только своего ребёнка | `kanga` | 1. `watch journal` | Только оценки `roo` | Частично |

---

### 6. Сьют: История действий (Accounting) (4 кейса)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-30 | Запись входа в систему | – | 1. Войти как `pooh` | В истории: «pooh вошёл в систему» | Выполнено |
| TC-31 | Запись перемещения между локациями | Авторизован `tigger` | 1. Войти в `School`<br>2. Перейти в `TeachersRoom` | История содержит оба входа | Выполнено |
| TC-32 | Запись действий с журналом | Авторизован `rabbit` | 1. `watch journal`<br>2. `edit journal` | История: просмотр, редактирование | Выполнено |
| TC-33 | Просмотр истории командой | Любой пользователь | 1. `history` | Выводится список всех действий | Выполнено |

---

### 7. Сьют: Многопользовательский режим и состояние (5 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-34 | Сохранение местоположения пользователей | `piglet` в `ClassA`, `roo` в `School` | 1. `logout`<br>2. Войти как `owl` | Состояние сохранилось | Не выполнено |
| TC-35 | Два пользователя в одной локации | `piglet` в `ClassA` | 1. `tigger` входит в `ClassA` | Оба в `ClassA` | Выполнено |
| TC-36 | Пользователь покидает локацию | `piglet` в `ClassA` | 1. `leave ClassA` | Пользователь покинул класс | Частично |
| TC-37 | Проверка присутствия для условия | Учитель `piglet` в `ClassA` | 1. `who is in ClassA` | Возвращает `piglet` | Не выполнено |
| TC-38 | Смена пользователя без потери состояния | `piglet` в `ClassA` | 1. `login roo` | `roo` текущий, `piglet` остался в классе | Не выполнено |

---

### 8. Сьют: Граничные случаи и ошибки (Edge Cases) (5 кейсов)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-39 | Вход в несуществующую локацию | Авторизован любой | 1. `enter Dungeon` | «Локация не найдена» | Выполнено |
| TC-40 | Редактирование журнала без права | Авторизован `roo` | 1. `edit journal` | Отказ | Выполнено |
| TC-41 | Пустой ввод команды | – | 1. Нажать Enter | Приглашение повторяется | Не выполнено |
| TC-42 | Выход из локации, где пользователя нет | `pooh` в `School` | 1. `leave ClassA` | «Вы не находитесь в ClassA» | Частично |
| TC-43 | Повторный вход в ту же локацию | `pooh` в `School` | 1. `enter School` | «Вы уже в школе» | Не выполнено |

---

### 9. Сьют: Интеграционные сценарии (4 кейса)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-44 | Учитель ставит оценку, студент видит | Авторизованы `piglet`, затем `roo` | 1. `piglet` в `ClassA`<br>2. Ставит 5 `roo`<br>3. `roo` в `ClassA`<br>4. `roo` смотрит журнал | Видит 5 | Выполнено |
| TC-45 | Директор в кабинете, учитель не входит | `owl` в `Cabin` | 1. `rabbit` пытается войти | Отказ | Выполнено |
| TC-46 | Родитель смотрит оценки | Учитель выставил оценки | 1. `kanga` → `watch journal` | Только оценки `roo` | Частично |
| TC-47 | Администратор смотрит историю | Несколько пользователей действовали | 1. `robin` → `history` | Видит всю историю | Выполнено |

---

### 10. Сьют: Проверка данных пользователей по умолчанию (3 кейса)

| ID | Название | Предусловия | Шаги | Ожидаемый результат | Статус |
|:----|:----------|:-------------|:------|:----------------------|:--------|
| TC-48 | Привязка родителя к ребёнку | `kanga` | 1. Посмотреть `childrenNames` | Список содержит `roo` | Не выполнено |
| TC-49 | Все 9 пользователей загружены | Система запущена | 1. Войти под каждым из списка | Каждый успешно входит | Выполнено |
| TC-50 | Роли соответствуют описанию | Все пользователи поочерёдно | 1. Проверить разрешения согласно документации | Поведение соответствует ролевой модели | Выполнено |

---

### Подробные ссылки на ключевые тесты

#### Выполненные (примеры):

1. **Аутентификация** – [`SecureStateImplTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/impl/SecureStateImplTest.java)
   - Строки ~45-70: `testAuthenticationSuccess()`, `testAuthenticationFailure()`

2. **Ролевая модель** – [`test/ru/yandex/practicum/model/role/`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/tree/main/test/ru/yandex/practicum/model/role)
   - Например, [`AdminTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/role/AdminTest.java)
   - [`GuardTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/role/GuardTest.java)
   - [`StudentTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/role/StudentTest.java)

3. **Условные разрешения** – [`test/ru/yandex/practicum/model/condition/`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/tree/main/test/ru/yandex/practicum/model/condition)
   - [`DirectorAbsentConditionTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/condition/DirectorAbsentConditionTest.java)
   - [`TeacherPresentConditionTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/condition/TeacherPresentConditionTest.java)
   - [`OwnChildOnlyConditionTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/condition/OwnChildOnlyConditionTest.java)

4. **Разрешения (Permissions)** – [`test/ru/yandex/practicum/model/permission/`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/tree/main/test/ru/yandex/practicum/model/permission)
   - [`EnterSchoolTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/permission/EnterSchoolTest.java)
   - [`WatchJournalTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/permission/WatchJournalTest.java)

5. **Журнал оценок** – [`test/ru/yandex/practicum/model/journal/SchoolJournalTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/journal/SchoolJournalTest.java)

6. **История действий** – [`SecureStateImplTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/impl/SecureStateImplTest.java#L200-L230)
   - Метод `testHistoryLogging()`

7. **Интеграционные сценарии** – [`ScenarioTests.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/integration/ScenarioTests.java)

8. **Локации** – [`test/ru/yandex/practicum/model/location/`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/tree/main/test/ru/yandex/practicum/model/location)
   - [`SchoolTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/location/SchoolTest.java)
   - [`ClassroomTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/location/ClassroomTest.java)

9. **Пользователи** – [`test/ru/yandex/practicum/model/user/UserTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/user/UserTest.java)

#### Частично выполненные:

1. **TC-4 (Смена пользователя без выхода)** – тест есть, но не проверяет сохранение истории
   - [`SecureStateImplTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/impl/SecureStateImplTest.java#L100-L115) – метод `testSwitchUser()`

2. **TC-22, TC-29, TC-46 (Родитель и журнал)** – фильтрация работает, но нет теста на чужого ребёнка
   - [`test/ru/yandex/practicum/model/condition/OwnChildOnlyConditionTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/condition/OwnChildOnlyConditionTest.java)
   - [`test/ru/yandex/practicum/model/journal/SchoolJournalTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/journal/SchoolJournalTest.java#L60-L80)

3. **TC-36 (Пользователь покидает локацию)** – тест есть, но без проверки ошибок
   - [`test/ru/yandex/practicum/model/location/LocationTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/tree/main/test/ru/yandex/practicum/model/location)

#### Не выполненные: 

1. **TC-34** – [`SecureStateImplTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/impl/SecureStateImplTest.java) – добавить метод `testStatePreservedAfterLogoutLogin()`
2. **TC-37** – создать новый тест `testWhoCommand()` в [`SecureStateImplTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/impl/SecureStateImplTest.java) (команда `who` не реализована в консоли)
3. **TC-41** – [`EdgeCaseTests.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/EdgeCaseTests.java) – добавить `testEmptyInput()`
4. **TC-43** – добавить `testPreventDoubleEnterLocation()` в [`EdgeCaseTests.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/EdgeCaseTests.java)
5. **TC-48** – [`test/ru/yandex/practicum/model/user/UserTest.java`](https://github.com/Kirill-Kazantcev/higher-web-practice-school/blob/main/test/ru/yandex/practicum/model/user/UserTest.java) – добавить `testParentChildBinding()`

---

