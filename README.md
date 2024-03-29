# Курс по SQL: конспект

SQL-команды пишутся в верхнем регистре (написание в нижнем регистре не вызовет ошибок, но так делает только быдло); Названия - в произвольном регистре

## Создание базы данных

Синтаксис:

```
CREATE DATABASE название;
```

## Удаление базы данных

Синтаксис:

```
DROP DATABASE название;
```

(PhpMyAdmin не будет спрашивать, уверены ли вы, а сразу удалит)

## Создание таблиц

Перейдём в базу данных, внутри которой будем создавать таблицу: тыкнем на нужную базу в списке (на название, а не плюсик) и уже находясь здесь, тыкнем на вкладку SQL.

##### Про некоторые типы данных:

VARCHAR - небольшая строка (масимум 255 символов)

TEXT - большой объём текста (максимум 65К символов)


Синтаксис:

```
CREATE TABLE название(
    здесь описываются поля таблицы; 
    создать пустую таблицу без полей невозможно;
);
```

Как описываются поля таблицы:

```
CREATE TABLE название(
    название_поля ТИП_ДАННЫХ 
);
```

Пример:

```
CREATE TABLE название(
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(10),
    ... ещё всякие поля ...
    PRIMARY KEY(id)
);
```

Здесь:

+ id - обязательное поле для любой таблицы; это уникальный идентификатор, представляющий собой порядковый номер записи в таблице. id НЕ потворяется => по id всегда можно будет получить конкретную запись
+ NOT NULL - поле не может быть пустым
+ AUTO_INCREMENT - автоматическое увеличение значения на 1
+ в скобках у VARCHAR указано максимальное чило символов, которое можно будет вписать в это поле
+ PRIMARY KEY - значение поля не повторяется. Мы можем написать это, как здесь, после описания всех полей, а можем использовать другой способ добавления первичного ключа:

```
CREATE TABLE название(
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(10),
    ... ещё всякие поля ...
);
```

Результат обоих способов добавления первичного ключа будет одинаковым.

После последнего поля запятая не нужна.

Также можно написать названия полей в `кавычках` (так лучше):

```
...
`id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
`name` VARCHAR(10),
...
```

    Чтобы присвоить полю в качестве значению по умолчанию текущее время, используется:

    ```
    `название_поля` DATETIME DEFAULT CURRENT_TIMESTAMP
    ```

    (делается это при создании таблицы)

## Удаление таблиц

Находясь внутри базы данных (НЕ внутри таблицы), выбираем вкладку SQL.

Синтаксис:

```
DROP TABLE название;
```

### Добавление полей

Чтобы добавить поле в уже существующую таблицу, используем:

```
ALTER TABLE название_таблицы ADD название_поля ТИП_ДАННЫХ(ограничение);
```

### Удаление полей

Чтобы удалить поле из уже существующей таблицы, используем:

```
ALTER TABLE название_таблицы DROP COLUMN название_поля;
```

## Добавление записей в таблицу

1 способ - через кнопку "Вставить" в PhpMyAdmin (тут есть интерактивное окно для даты).

2 способ - через SQL.

### Добавление записей в таблицу через SQL.

Синтаксис:

```
INSERT INTO название_таблицы (название_поля(полей)) VALUES (значение записей);
```

Можно указать значения как для нескольких полей одной строки:

```
INSERT INTO person (`name`, `bio`, `birthday`) VALUES ('серега', 'жил был серега', '1999-05-07')
```

Так и для нескольких строк одного поля:

```
INSERT INTO person (`age`) VALUES (10),(30),(100)
```

Можно также прописывать не все поля в одной строке, а что-то оставить пустым, и т.д.

### Множественное добавление

Пример добавления сразу нескольких записей:

```
INSERT INTO `person` (`name`, `bio`, `birthday`, `vozrast`) 
VALUES ('не серега', 'это анастасия',      '2019-08-03', 45),
     ('не серега', 'это анастасия', '2019-08-03', 45),
     ('не серега', 'бебебе', '2019-08-07', 500);
```

(Это всё можно было написать и в одну строчку, просто так красивее)

Записи разделены запятыми, после последней записи ставится ;

### Изменение записей, уже добавленных в таблицу

###### !
если мы вдруг захотим сделать так, чтобы какое-то поле было NOT NULL, нам нужно сначала внести туда какое-нибудь значение, а иначе будет ошибка.

#### Синтаксис изменения записи

Пример:

```
ALTER TABLE `person` CHANGE `age` `vozrast` INT NOT NULL 
```

Синтаксис:

```
ALTER TABLE название_таблицы CHANGE название_поля новые_свойства
```
```
новое_название новый_тип_данных ещё_какие-нибудь_новые_свойства
```

То есть, мы пишем команду изменения, потом пишем, что именно мы меняем, а потом, как если бы мы создавали это поле, указываем для него свойства. Если какое-то свойство мы хотим оставить таким, каким оно было раньше, просто пишем то же, что и было до этого.

### Обновление данных

```
UPDATE название_таблицы SET название_поля = новое_значение_этого_поля WHERE название_поля = старое_значение_поля;
```

SET - то есть установить значение таким-то

Другой вариант:

Указать, какое именно поле нам необходимо поменять, можно также через id:

```
UPDATE название_таблицы SET название_поля = новое_значение_этого_поля WHERE id = какое-то;
```

И через все остальные поля тоже.

Также можно использовать не равенство, а, например:

```
UPDATE название_таблицы SET название_поля = новое_значение_этого_поля WHERE id > какого-то числа;
```

Указать несколько условий для WHERE можно через AND:

```
UPDATE название_таблицы SET название_поля = новое_значение_этого_поля WHERE id = какое-то AND name = какое-то;
```

#### Множественное обновление

Пример:

```
UPDATE название_таблицы SET название_поля1 = новое_значение_поля1, название_поля2 = новое_значение_поля2, название_поля3 = новое_значение_поля3 WHERE что-то = какое-то;
```

## Удаление данных

```
DELETE FROM название_таблицы WHERE какое-то_условие
```

Если не написать условие, то из указанной таблицы будут удалены все записи.

Также можно использовать:

```
TRUNCATE название_таблицы;
```

Тогда из таблицы будут удалены все записи. Останется пустая таблица.

## Выборка данных из базы

Выбрать все записи:

```
SELECT * FROM название_таблицы;
```

Выбрать определённые поля:

```
SELECT `поле_1`, `поле_2`, `поле_n` FROM `название_таблицы`;
```

Добавить условие:

```
SELECT `поле_1`, `поле_2`, `поле_n` FROM `название_таблицы` WHERE условие;
```

## Индексы

Индексы - это дополнительные характеристики к различным полям в таблице, помогают ускорять поиск.

Индексы не видны пользователю.

Синтаксис добавления индекса:

```
CREATE INDEX название_индекса ON название_таблицы(название_поля);
```

После добавления индекса мы сможем увидеть его только в структуре, он обозначается ключом.

Теперь, если мы будем делать обычный запрос, выводящий строки этого поля, мы получим результат поиска быстрее, чем если бы у этого поля не было индекса (мы нигде не обращаемся к индексу, он просто существует, а ищем мы по полю).

Удалить индекс:

```
DROP INDEX название_индекса ON название_табицы;
```

## Объединение данных (связь между таблицами)

Для того, чтобы какие-то поля могли быть ссылкой на поле другой таблицы, нам нужно сделать их внешними ключами (FOREIGHN KEY):

```
FOREIGN KEY(поле_этой_таблицы) REFERENCES название_другой_таблицы(поле_другой_таблицы)
```

(Это обычно пишется при создании таблицы, после описания всех её полей)

##### Пример использования внешних ключей: 
если внешние ключи были установлены на поля shopID и personID, то мы можем сделать следующее:

```
INSERT INTO `orders`(`orderNumber`, `shopID`, `personID`) 
VALUES (0001, 2, 4)
```

Здесь shopID указывает на поле ID таблицы shop, и таким образом в таблицу orders мы добавили запись, содержащую ссылку на поле ID другой таблицы. Аналогично для personID - это связь с таблицей person.

Теперь при наведении на содержимое поля, содержащего ссылку на поле другой таблицы, мы увидим название этого поля в той таблице.

##### Пример, показывающий, как использовать INNER JOIN

```
SELECT orders.orderNumber, person.name, person.vozrast FROM person INNER JOIN orders ON person.id = orders.personID
```

Данным запросом мы выбрали поля из двух таблиц и сказали о том, что это должно быть выведено только для тех полей, у которых id в таблице person равно тому id, который в таблице orders является ссылкой на id в таблице person.

###### оффтоп: пример сортировки:

сортировка (упорядочивание) по полю orderNumber таблицы orders:

```
SELECT orders.orderNumber, person.name, person.vozrast FROM person INNER JOIN orders ON person.id = orders.personID ORDER BY orders.orderNumber DESC;
```

DESC - в порядке убывания.

### Таким образом, INNER JOIN - 

это когда мы берём данные, общие для двух таблиц. Существует также LEFT JOIN (берём данные из первой таблицы и сравниваем совторой таблицей) и RIGHT JOIN (берём данные из второй таблицы и сравниваем с первой таблицей). 

Пример:

```
SELECT person.name, orders.orderNumber 
FROM person 
LEFT JOIN orders on person.id = orders.personID
ORDER BY person.name DESC;
```

LEFT JOIN будет брать данные из таблицы, указанной после FROM; RIGHT JOIN - из другой таблицы