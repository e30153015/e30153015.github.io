---
title: SQL基本語法
date: 2021-07-28 19:40:40
tags: SQL
categories: SQL
---

## SQL基本語法

### SELECT:查詢資料;
```sql
SELECT `欄位名稱` FROM `資料表名稱`;
```

### AS: 設定欄位別名
```sql
SELECT `欄位名稱` AS `欄位別名`, `欄位名稱` AS `欄位別名` FROM `資料表名稱`   ;
```
<!-- more -->
### SELECT DISTINCT: 去除重複資料只顯示一筆;
```sql
SELECT DISTINCT `欄位名稱` FROM `資料表名稱`   ;
```

### WHERE: 篩選條件
```sql
SELECT `欄位名稱` FROM `資料表名稱` WHERE `條件敘述句`;
```

### AND、OR、NOT: 連接多個條件式
ex:
```sql
SELECT * FROM `students` WHERE `cSex` = 'M';
```
```sql
SELECT * FROM `students` WHERE `cID` > 5 AND `cSex` = 'M';
```
```sql
SELECT * FROM `student` WHERE NOT (`cID` > 5 AND `cSex` = 'M');
```

### BETWEEN…AND: 設定篩選範圍
```sql
SELECT `欄位名稱` FROM `資料表名稱` WHERE `欄位名稱` BETWEEN `起始值` AND `結束值`;
```
ex:
```sql
SELECT * FROM `students` WHERE `cBirthday` BETWEEN `1988-01-01` AND `1988-12-31`;
```

### IN: 指定多個篩選值
```sql
SELECT `欄位名稱` FROM `資料表名稱` WHERE `欄位名稱` IN (`欄位值1`,`欄位值2`,...);
```
ex:
```sql
SELECT FROM `students` WHERE `cID` IN (1,3,5,7,9);
```

### LIKE: 設定字串比對的篩選值
#### 萬用字元的使用
* `_` 代表**單**個任意字元
* `%` 代表**零**或**多個**任意字元

ex:查出第二個字為"中"且為三個字的姓名
```sql
SELECT * FROM `students` WHERE `cName` LIKE '_中_';;
```

ex:查出第二個字為"中",長度不限的姓名
```sql
SELECT * FROM `students` WHERE `cName` LIKE '_中%';;
```

ex:查出號碼為0918開頭
```sql
SELECT * FROM `students` WHERE `cPhone` LIKE '0918%';;
```

### ORDER BY: 設定查詢結果的排序
#### 排序方式有兩種
1. ASC:遞增排序，由小到大，也是未指定時的預設排法。
2. DESC:遞減排序，由大到小。
```sql
SELECT `欄位名稱` FROM `資料表名稱` ORDER BY `指定排序的欄位名稱` 排序方式;
```
ex:生日遞減排序
```sql
SELECT * FROM `students` ORDER BY `cBirthday` DESC;;
```
#### 多欄位排序
依條件依序排列,條件與條件之間逗號分隔
```sql
ORDER BY `指定排序的欄位名稱1` 排序方式1, `指定排序的欄位名稱2` 排序方式2
```
ex:性別遞增排序後,遞減生日排序
```sql
SELECT * FROM `students` ORDER BY `cSex` ASC,`cBirthday` DESC
```

### LIMIT: 設定查詢顯示的筆數
可以設定查詢後由哪筆開始顯示,並顯示多少筆
```sql
SELECT `欄位名稱` FROM `資料表名稱` LIMIT 開始顯示的筆數,顯示多少筆
```
SQL中是由0開始,若從0筆開始
```sql
LIMIT 0,5 
```
若資料由第一筆開始, 0可直接省略如下
```sql
LIMIT 5 
```
* ORDER BY 搭配 LIMIT 可取得前X筆 或 後X筆 資料
