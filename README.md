MySQL Note
==========
MySQL 사용법과 팁을 정리한 문서입니다.


## Database

연결:

```
mysql -h localhost -u root -p
```

비밀번호 변경:

```
grant all privileges on *.* to 'root'@'%' identified by 'newpassword';
```

문자셋 변경:

```
ALTER DATABASE databaseName CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

타임존 조회:

```
SELECT @@global.time_zone;
```

테이블 정보 조회:

```
desc tableName;
```

컬럼 속성 변경:

```
ALTER TABLE tableName MODIFY attr BIGINT NOT NULL DEFAULT 100;
```

버전체크:

```
SHOW VARIABLES LIKE "%version%";
```

인코딩:

```
set names 'utf8mb4'
```

테이블 용량 체크:

```
SELECT table_schema "DatabaseName", sum( data_length + index_length ) / 1024 / 1024 "Data Base Size in MB"
FROM information_schema.TABLES GROUP BY table_shcema;
```

기존 디비 복제:

```
mysqldump db1 > dump.sql
mysqladmin create db2 
mysql db2 < dump.sql
```

## Table

테이블 컬럼 추가:

```
ALTER TABLE tableName ADD attr varchar(10);
```

테이블 속성 수정:

```
ALTER TABLE tableName MODIFY attr INT NOT NULL AUTO_INCREMENT PRIMARY KEY;
```

데이터 업데이트:

```
Update tableName set attr = value where id = 1;
```

테이블 비우기:

```
truncate tableName;
```

데이터 입력:

```
INSERT INTO databaseName.tableName (no, name, level, amount)
VALUES ('1002', 'chris', 'user', '10000');
```

## Functions

CURDATE():  오늘 날짜

CURTIME(): 지금 시간

CONCAT(): 텍스트 합치기

```
SELECT CONCAT(attr1, '-', attr2) FROM tableName;
```

MD5(): 해쉬태그 생성

IFNULL(): Null 값 체크

JOIN: [참고](http://stackoverflow.com/questions/17542431/less-number-of-records-for-left-join-vs-inner-join)

비트연산: [참고](http://www.phpschool.com/gnuboard4/bbs/board.php?bo_table=tipntech&wr_id=77064)



## Pivot table

```
select left(datetime, 10) as date,
     sum(case when type=300 then 1 else 0 end) as type_300,
     sum(case when type=301 then 1 else 0 end) as type_301,
     sum(case when type=302 then 1 else 0 end) as type_302,
     sum(case when type=303 then 1 else 0 end) as type_303,
     sum(case when type=310 then 1 else 0 end) as type_310,
     sum(case when type=311 then 1 else 0 end) as type_311,
     sum(case when type=312 then 1 else 0 end) as type_312,
     sum(case when type=313 then 1 else 0 end) as type_313
from step_web.log
where type > 299 and type < 314
group by date order by date desc;
```

## 기존에 업는 데이터만 추가하는 쿼리

```
INSERT INTO tableName (attr1, attr2)
SELECT ?, ?, NOW() FROM DUAL
WHERE NOT EXISTS(
    SELECT 1
    FROM tableName
    WHERE attr1 = ?
  )
LIMIT 1;
```

## 시간 비교

```
SELECT *
FROM table
WHERE start < CAST('05:00:00' AS time)
AND end > CAST('05:00:00' AS time)
```

## Multiple INSERT

```mysql
INSERT INTO tableName
  (example_id, name, value, other_value)
VALUES
  (100, 'Name 1', 'Value 1', 'Other 1'),
  (101, 'Name 2', 'Value 2', 'Other 2'),
  (102, 'Name 3', 'Value 3', 'Other 3'),
  (103, 'Name 4', 'Value 4', 'Other 4');
```


## MySQL on OSX

설치:

```
brew install mysql
```

시작/종료/재시작:

```
mysql.server start
mysql.server stop
mysql.server restart
```
