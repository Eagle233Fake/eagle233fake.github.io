---
title: CS50x - Week 7 SQL
date: 2024-11-22 21:55:29
tags:
categories: 
- CS50x
---

---

- **THE STRUCTURED QUERY LANGUAGE**

- **SQLite**

- **phpMyAdmin: GUI**

Click here to know more: **[Introduction to Databases with SQL](https://cs50.harvard.edu/sql)**

---

## Data Types

| 数据类型     | 描述                                       |
|--------------|--------------------------------------------|
| **INT**      | 整数类型，通常用于存储整数值。            |
| SMALLINT     | 较小的整数类型，范围比 INT 小。           |
| TINYINT      | 非常小的整数类型，通常用于存储小范围的整数。|
| MEDIUMINT    | 中等大小的整数类型，范围介于 SMALLINT 和 INT 之间。|
| BIGINT       | 大整数类型，适用于存储非常大的整数值。    |
| DECIMAL      | 精确的小数类型，适用于财务计算。          |
| FLOAT        | 单精度浮点数类型，适用于存储近似值。      |
| BIT          | 位类型，通常用于存储布尔值（0 或 1）。    |
| DATE         | 日期类型，格式为 'YYYY-MM-DD'。           |
| TIME         | 时间类型，格式为 'HH:MM:SS'。             |
| DATETIME     | 日期和时间类型，格式为 'YYYY-MM-DD HH:MM:SS'。|
| TIMESTAMP    | 时间戳类型，记录自1970年1月1日以来的秒数。 |
| CHAR         | 固定长度字符串类型。e.g. CHAR(10)          |
| VARCHAR      | 可变长度字符串类型。e.g. VARCHAR(99)       |
| BINARY       | 固定长度的二进制数据类型。                 |
| **BLOB**     | 二进制大对象，适用于存储大量二进制数据。   |
| **TEXT**     | 文本类型，适用于存储长文本。               |
| ENUM         | 枚举类型，允许从预定义的值中选择。         |
| GEOMETRY     | 用于存储几何数据的类型。                   |
| LINESTRING   | 用于存储线段的几何数据类型。               |

更简单的：

| 数据类型 | 描述                                       |
|----------|--------------------------------------------|
| NULL     | 表示缺失或未知的值。                       |
| INTEGER  | 整数类型，通常用于存储整数值。            |
| REAL     | 浮点数类型，通常用于存储近似值。          |
| TEXT     | 文本类型，适用于存储长文本。               |
| BLOB     | 二进制大对象，适用于存储大量二进制数据。   |

---

## Design Principles

- Create one table for each **entity** in your dataset.

- All tables should have a **primary key**.

- The information in the table should depend on the primary key ***only***.

---

## Syntax

### Open Files

`sqlite3 reads.db`

### Create Tables

```sql
CREATE TABLE table_name (
    column0 INTEGER,
    column1 TEXT,
    column2 NUMERIC, -- DATES
    colunm3 REAL,
    PRIMARY KEY(column0)
);
```

### Check Tables

`.schema books`

### Type Affinities

- 在 SQL 中，Type Affinities 是 SQLite 中的一个概念，用于定义列的数据类型和它们的优先级。SQLite 允许你在创建表时指定列的数据类型，但它并不严格遵循这些类型，而是根据类型亲和性（type affinity）来处理数据。

- 数据在插入时，若不是同类型的数据，也不会报错。

```sql
CREATE TABLE example (
    id INTEGER,
    name TEXT
);

INSERT INTO example (id, name) VALUES ('usbw', 'Test User');
```

|  id | name      |
|-----|-----------|
|**0**| Test User |

```sql
CREATE TABLE example (
    id INTEGER,
    name TEXT
);

INSERT INTO example (id, name) VALUES (4.5, 'Test User');
```

|  id | name      |
|-----|-----------|
|**4**| Test User |

### Quit SQL

`.quit`

### Run `.sql`

`.read 1.sql`

### Show All Tables

`.table`

### Keep Track of the Time

`.timer ON`

### Indexes

```sql
CREATE INDEX name ON table (column, ...);
```

### Primary Key

- **Joint Primary Key**: a combination of two columns that is always guaranteed to be unique.

    ```sql
    CREATE TABLE authored (
        author_id INTEGER,
        book_id INTEGER,
        PRIMARY KEY(author_id, book_id)
    )
    ```

### Foreign Key

```sql
CREATE TABLE authored (
    author_id INTEGER,
    book_id INTEGER,
    PRIMARY KEY(author_id, book_id), -- Order has some slight underlying effects on your database.

    FOREIGN KEY(author_id) REFERENCES authors(id),
    FOREIGN KEY(book_id) REFERENCES books(id)
)
```

### 实现

| SQL 操作  | 描述                                       |
|-----------|--------------------------------------------|
| INSERT    | 向数据库表中插入新记录。                  |
| SELECT    | 从数据库表中查询数据。                    |
| UPDATE    | 更新数据库表中现有记录的值。              |
| DELETE    | 从数据库表中删除现有记录。                |

users:

| idnum | username | password | fullname          |
|-------|----------|----------|--------------------|
| 10    | jerry    | fus!ll!  | Jerry Seinfeld      |
| 11    | gcostanza| b0sc0    | George Costanza     |

moms:

| username  | mother              |
|-----------|---------------------|
| jerry     | Helen Seinfeld      |
| gcostanza | Estelle Costanza    |

### `INSERT`

```sql
INSERT INTO
<table>
(<columns>)
VALUES
(<values>)
```

```sql
INSERT INTO
users
(username, password, fullname)
VALUES
(‘newman’, ‘USMAIL’, ‘Newman’)
```

| idnum | username  | password | fullname          |
|-------|-----------|----------|--------------------|
| 10    | jerry     | fus!ll!  | Jerry Seinfeld      |
| 11    | gcostanza | b0sc0    | George Costanza     |
| **12**| newman    | USMAIL   | Newman              |

- Usually a good idea to have the colume that ultimately ends up being your table's primary key be an interger.

- Automatically add by one when you configure that column to **autoincrement**.

```sql
INSERT INTO
moms
(username, mother)
VALUES
(‘kramer’, ‘Babs Kramer’)
```

| username  | mother              |
|-----------|---------------------|
| jerry     | Helen Seinfeld      |
| gcostanza | Estelle Costanza    |
| kramer    | Babs Kramer         |

### `SELECT`

```sql
SELECT
<columns>
FROM
<table>
WHERE
<condition>
ORDER BY
<column>
```

```sql
SELECT
idnum, fullname
FROM
users
```

```sql
SELECT
password
FROM
users
WHERE
idnum < 12
```

```sql
SELECT
* -- ALL
FROM
moms
WHERE
username = ‘jerry’
```

### `SELECT (JOIN)`

- What if we now find ourselves in a situation where we need to get a user’s full name (from the users table) and their mother’s name (from the mother table)?

```sql
SELECT
users.fullname, moms.mother
FROM
users
JOIN
moms
ON
users.username = moms.username
```

| username  | mother              |
|-----------|---------------------|
| jerry     | Helen Seinfeld      |
| gcostanza | Estelle Costanza    |
| kramer    | Babs Kramer         |

ALL JOIN TOGETHER:

| users.idnum | users.username moms.username | users.password | users.fullname     | moms.mother         |
|--------------|--------------------------------|----------------|---------------------|---------------------|
| 10           | jerry                          | fus!ll!        | Jerry Seinfeld       | Helen Seinfeld      |
| 11           | gcostanza                      | b0sc0          | George Costanza      | Estelle Costanza    |

### `UPDATE`

```sql
UPDATE
<table>
SET
<column> = <value>
WHERE
<predicate>
```

```sql
UPDATE
users
SET
password = ‘yadayada’
WHERE
idnum = 10
```

| idnum | username    | password | fullname         |
|-------|-------------|----------|-------------------|
| 10    | jerry       | **yadayada** | Jerry Seinfeld     |
| 11    | gcostanza   | b0sc0    | George Costanza    |
| 12    | newman      | USMAIL   | Newman             |

### `DELETE`

```sql
DELETE FROM
<table>
WHERE
<predicate>
```

```sql
DELETE FROM
users
WHERE
username = ‘newman’
```

| idnum | username    | password | fullname         |
|-------|-------------|----------|-------------------|
| 10    | jerry       | yadayada | Jerry Seinfeld     |
| 11    | gcostanza   | b0sc0    | George Costanza    |

### `ORDER BY`

- End of the sentences.

```sql
SELECT name, tempo
FROM songs
ORDER BY tempo;
```
  
- `ASC`: **Ascending 升序**

- `DESC`: **Descending 降序**

    ```sql
    SELECT name
    FROM songs
    ORDER BY diration_ms DESC
    ```

### Syntax Order

- `SELECT FROM` - `WHERE` - `ORDER BY` - `LIMIT`

### `AND` `OR`

```sql
SELECT name
FROM songs
WHERE danceability > 0.75
AND energy > 0.75
AND valence > 0.75
```

### Aggregate Functions

```sql
SELECT COUNT(column)
```

```sql
SELECT ROUND(AVG(column), 2) -- `ROUND()` is not an aggregate function.
```

```sql
SELECT MIN(column)
```

```sql
SELECT MAX(column)
```

### Combining Tables

#### Subqueries

```sql
SELECT rating
FROM ratings
WHERE movie_id = (
    SELECT id
    FROM movies
    WHERE title = 'Cars 3'
);
```

- **ALWAYS USE SINGLE QUOTES!!!**

- **USING DOUBLE QUOTES FOR IDENTIFIERS**

#### Joins

`JOIN ON`

```sql
SELECT songs.name, energy, artists.name
FROM songs
JOIN artists
ON artists.id = songs.artists_id
WHERE artists.name = 'Drake';
```

#### 区别

- 一个结果使用 Subqueries ，多个结果使用 Joins ，这在两个作业当中会体现中区别。

  - The subquery might return multiple rows, which isn't allowed in this context.

### *`EXPLAIN QUERY PLAN`

```sql
EXPLAIN QUERY PLAN
SELECT songs.name, artists.name
FROM songs 
JOIN artists
ON artists.id = songs.artists_id;
```

### `LIKE`

```sql
SELECT name
FROM songs
WHERE name LIKE '%feat.%';
```

### `GROUP BY`

```sql
SELECT artists.name, COUNT(artists.name)
FROM songs
JOIN artists ON artists.id = songs.artists_id
GROUP BY artists.name;
```

### Comments

```sql
SELECT 'It''s a sunny day';  -- 输出: It's a sunny day
```

### Escape Character

```sql
SELECT 'It''s a sunny day';  -- 输出: It's a sunny day
```

### `HAVING`

```sql
SELECT title
FROM movies
JOIN stars
ON stars.movie_id = movies.id
JOIN people
ON people.id = stars.person_id
WHERE people.name
IN ('Bradley Cooper', 'Jennifer Lawrence')
GROUP BY movies.title
HAVING COUNT(DISTINCT people.name) = 2; -- 确保每部电影都有两人. (From "Movies" 12.sql)
```

---

## SQL with Python

```python
from cs50 import SQL
```

### Race Conditions

```python
db.execute("BEGIN TRANSACTION")
rows = db.execute("SELECT likes FROM posts WHERE id = ?", id)
likes = rows[0]["likes"]
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, id)
db.execute("COMMIT")
```

### SQL Injection Attacks

```python
rows = db.execute(f"SELECT * FROM users WHERE username = 'malan@harvard.edu'--' AND password = '{password}'")
```

- So don't use `format`.

    ```python
    rows = db.execute("SELECT * FROM users WHERE username = ? AND password = ?", username, password)
    ```

---
