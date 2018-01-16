---
layout: post
title: Note-SQL
category: note
toc: true
---

# 基础操作

```sql
CREATE DATABASE # 创建新数据库
ALTER DATABASE  # 修改数据库
CREATE TABLE    # 创建新表
ALTER TABLE # 变更（改变）数据库表
DROP TABLE  # 删除表
CREATE INDEX    # 创建索引（搜索键）
DROP INDEX  # 删除索引
```

# Example: table "Persons"

| Id   | LastName | FirstName | Address        | City     | Age  |
| ---- | -------- | --------- | -------------- | -------- | ---- |
| 1    | Adams    | John      | Oxford Street  | London   | 23   |
| 2    | Bush     | George    | Fifth Avenue   | New York | 56   |
| 3    | Carter   | Thomas    | Changan Street | Beijing  | 36   |

# Select

```sql
SELECT * FROM Persons   # 选择全部数据
SELECT LastName FROM Persons    # 选择某一列
SELECT LastName, FirstName FROM Persons # 选择多列数据
SELECT DISTINCT LastName FROM Persons   # 选择不同的值
SELECT * FROM Persons WHERE City="Beijing"
SELECT * FROM Persons WHERE Age BETWEEN 18 AND 35
SELECT * FROM Persons WHERE Address LIKE "%Street"
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
SELECT * FROM Persons
    WHERE (FirstName='Thomas' OR FirstName='William') AND LastName='Carter'
```

# Insert

```sql
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing', 60)
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')
```

# Update

```sql
UPDATE Persons SET FirstName = 'Fred' WHERE LastName = 'Wilson'
```

# Delete

```sql
DELETE FROM Persons WHERE LastName = 'Wilson'
DELETE * FROM Persons   # 删除所有列
```

# Sort

```sql
SELECT LastName FROM Persons ORDER BY City
SELECT * FROM Persons ORDER BY City DESC, Age ASC
```
