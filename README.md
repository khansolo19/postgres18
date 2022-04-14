# SLASH commands
* `\с` - показывает в какой бд мы находимся и через какого юзера

* `\с name_of_db` - переключается к этой бд

* `\l` - показывает все бд

* `\dt` - показывает все таблицы в бд

* `\du` - показывает всех юзеров

* `\q` - выход

# Creating db and table
```sql
CREATE DATABASE name_of_db; 
-- создает базу данных
```

```sql
CREATE TABLE name_of_table (
    name_of_column1 data_type constraint,
    name_of_column2 data_type constraint,
    ...
); 
-- создает таблицу с полями
```
# Fulling table
```sql
INSERT INTO name_of_table (name_of_column1, name_of_column2) 
VALUES (val1, val2);
-- добавляет запись в таблицу
```
# Вata output from tables
```sql
SELECT * FROM name_of_table; 
-- достает все поля и записи из таблицы

SELECT name_of_column1, name_of_column2 FROM name_of_table; 
-- достает только указанные столбцы из таблицы
```
# Nets
## primary key,foreugn key
> primery key (pk) - первичный ключ
> это ограничение, которое мы указываем на те поля, 
которые должны быть уникальными для того, чтобы потом их использовать в связях(например id)

> foreign key (fk) - внешний ключ 
> это ограничение, которое мы указываем на те поля,
которые будут ссылаться на pk в другой таблице, для создание связи

```sql
CREATE TABLE author (
    id serial primary key,
    first_name varchar(50),
    last_name varchar(50)
)

CREATE TABLE book (
    id serial,
    title varchar(100),
    published year,
    author_id int,
    constraint fk_author_book
    foreign key (author_id) references author (id)
)
```
## Виды связей
> One to one - (один к одному) 
* Один автор - одна биография 
* Один флаг - одна срана
* Один человек - одно сердце


> One to many - (один ко многим)
* Один  человек - много клеток, но у одной клетки только один человек
* Одни родители - много детей, но у одного ребенка только одни родители
* Один аккаунт - много постов, но у многих постов только один автор (аккаунт)

> Many to many - (Многие ко многим)
* У человека много друзей и у друга много дргуих друзей
* У доктора много пациентов и у пациента много докторов
* У пользователя много соц.сетей и у одной соц.сети много

## Виды связей
### One to one 
```sql
CREATE TABLE flag(
    id serial primary key,
    photo text
);
CREATE TABLE country(
    id serial primary key,
    title varchar(50)
    gimn text,
    flag_id int unique,

    constraint fk_country_flag 
    foreign key (flag_id) references flag(id)
);
```
### One to many
```sql
CREATE TABLE account(
    id serial primary key,
    nickname varchar(25) unique,
    upassword varchar(255)
);
CREATE TABLE post(
    id serial primary key,
    title varchar(100),
    body text,
    photo text,
    account_id int,

    constraint fk_acc_post
    foreign key (account_id) references account(id)
);
```
### Many to many
```sql
CREATE TABLE doctor(
    id serial primary key,
    first_name varchar(25),
    last_name varchar(50)
);
CREATE TABLE patient(
    id serial primary key,
    first_name varchar(25),
    last_name varchar(50)
);
CREATE TABLE doctor_patient(
    doctor_id int, 
    patient_id int,
    constraint fk_doctor
    foreign key (doctor_id) references doctor(id),
    constraint fk_patient
    foreign key (patient_id) references patient(id)
);
```


# Joins (теория)
> JOIN - это инструкция, которая позволяет в запросах SELECT извлекать данные из 
нескольких таблиц
> INNER JOIN - когда достаются только те записи у которых есть полная связь
> FULL JOIN -  когда достаются абсолютно все записи со всех таблиц
> RIGHT JOIN - когда достаются все записи с "правой" таблицы и так же те записи с полной связью
> LEFT JOIN - когда достаются все записи с "левой" таблицы и так же те записи с полной связью

## Joins (практика)
```sql
SELECT  author.first_name, book.title
FROM author 
JOIN book ON author.id = book.author_id
```
### One to one
```sql
SELECT country.title, country.gimn, flig.photo
FROM country
JOIN flag
ON country.flag_id = flag.id;
```
### One to many
```sql
SELECT account.nickname, post.title, post.photo
FROM account 
JOIN post
ON account.id = post.acoount_id;
```
### Many to many
```sql
SELECT doctor.first_name as Doctor, patient.first_name as Patient
FROM doctor
JOIN doctor_patient as dp ON dostor.id = dp.doctor_id
JOIN patient ON patient.id = dp.patient_id;
```
# Import/export data
write from file to database
```bash
psql db_name < file.sql
```
write from db to file
```bash
pg_dump db_name > file.sql
```