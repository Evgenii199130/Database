# Database

Задание 1
Опишите не менее семи таблиц, из которых состоит база данных:

какие данные хранятся в этих таблицах;
какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
Приведите решение к следующему виду:

Сотрудники (

идентификатор, первичный ключ, serial,
фамилия varchar(50),
...
идентификатор структурного подразделения, внешний ключ, integer).

Ответ:

**1. Сотрудники**

| Столбец             | Тип данных     | Описание                                                                           | Пример                         |
|----------------------|----------------|------------------------------------------------------------------------------------|----------------------------------|
| id_сотрудника       | serial          | Идентификатор сотрудника (первичный ключ)                                        | 1                               |
| фамилия            | varchar(50)    | Фамилия сотрудника                                                                | Суханова                      |
| имя                  | varchar(50)    | Имя сотрудника                                                                   | Арина                         |
| отчество            | varchar(50)    | Отчество сотрудника                                                               | Руслановна                     |
| оклад               | numeric(10, 2) | Оклад сотрудника                                                                 | 103333.00                     |
| дата_найма          | date           | Дата найма сотрудника                                                              | 200113                         |
| id_подразделения     | integer         | Идентификатор подразделения, к которому относится сотрудник (внешний ключ)       | 1                               |
| id_должности        | integer         | Идентификатор должности (внешний ключ)                                         | 1                               |

**2. Подразделения**

| Столбец               | Тип данных     | Описание                                                                           | Пример                         |
|------------------------|----------------|------------------------------------------------------------------------------------|----------------------------------|
| id_подразделения     | serial          | Идентификатор подразделения (первичный ключ)                                       | 1                               |
| название               | varchar(100)   | Название подразделения                                                              | Центр компетенций QA Москва   |
| id_типа              | integer         | Идентификатор типа подразделения (внешний ключ)                                  | 1                               |
| id_филиала           | integer         | Идентификатор филиала, к которому относится подразделение (внешний ключ)         | 1                               |

**3. Филиалы**

| Столбец               | Тип данных     | Описание                                                                           | Пример                                                       |
|------------------------|----------------|------------------------------------------------------------------------------------|---------------------------------------------------------------|
| id_филиала           | serial          | Идентификатор филиала (первичный ключ)                                           | 1                                                           |
| адрес                 | varchar(255)   | Адрес филиала                                                                   | Приморский край, г. Владивосток, ул Нижнепортовая, д. 1     |

**4. Проекты**

| Столбец               | Тип данных     | Описание                                                                           | Пример                         |
|------------------------|----------------|------------------------------------------------------------------------------------|----------------------------------|
| id_проекта            | serial          | Идентификатор проекта (первичный ключ)                                           | 1                               |
| название               | varchar(100)   | Название проекта                                                              | Итэлма Инженерный корпус     |

**5. Типы подразделений**

| Столбец               | Тип данных     | Описание                                                                           | Пример                         |
|------------------------|----------------|------------------------------------------------------------------------------------|----------------------------------|
| id_типа              | serial          | Идентификатор типа подразделения (первичный ключ)                                  | 1                               |
| название               | varchar(50)    | Название типа подразделения                                                       | Отдел                         |

**6. Назначения**

| Столбец               | Тип данных     | Описание                                                                           | Пример                         |
|------------------------|----------------|------------------------------------------------------------------------------------|----------------------------------|
| id_сотрудника          | integer         | Идентификатор сотрудника (внешний ключ)                                         | 1                               |
| id_проекта            | integer         | Идентификатор проекта (внешний ключ)                                           | 1                               |

**7. Должности**

| Столбец               | Тип данных     | Описание                                                                           | Пример                         |
|------------------------|----------------|------------------------------------------------------------------------------------|----------------------------------|
| id_должности          | serial          | Идентификатор должности (первичный ключ)                                           | 1                               |
| название               | varchar(100)   | Название должности                                                              | Ведущий QA инженер              |

Задание 2*
Перечислите, какие, на ваш взгляд, в этой денормализованной таблице встречаются функциональные зависимости и какие правила вывода нужно применить, чтобы нормализовать данные.

Ответ:

Функциональные зависимости:

* ФИО сотрудника → Оклад, Должность, Тип подразделения, Структурное подразделение, Дата найма
  *  Каждый сотрудник имеет уникальный набор атрибутов: оклад, должность, тип подразделения, структурное подразделение и дату найма.
* Структурное подразделение → Тип подразделения, Адрес филиала 
  *  Структурное подразделение однозначно определяет тип подразделения и адрес филиала.
* Проект на который назначен →  ? 
  *  В предоставленной таблице мы не видим однозначного определения атрибутов по проекту. Возможно,  необходимо добавить ключ для проектов.

Правила нормализации:

* 1NF (Первая нормальная форма):
    * Устранение повторяющихся групп данных. В данной таблице нет повторяющихся групп, поэтому она уже находится в 1NF.
* 2NF (Вторая нормальная форма):
    * Устранение частичных функциональных зависимостей.  В таблице есть частичная зависимость: 
        * Структурное подразделение → Тип подразделения, Адрес филиала 
        *  Чтобы перейти в 2NF, необходимо выделить отдельную таблицу "Подразделения" с атрибутами id_подразделения, название, тип, id_филиала.
* 3NF (Третья нормальная форма):
    * Устранение переходных функциональных зависимостей.  Переходных зависимостей в данной таблице нет.
