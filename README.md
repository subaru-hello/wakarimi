# wakarimi

### 1. image作成
```
$ docker pull postgres:11
```

### 2. docker image 起動
```
$ docker run --name posgre11 -d -e POSTGRES_PASSWORD=pass1 postgres:11
```

### 3. docker containerに入る
```
$ docker exec -it posgre11 bash
```

### 4. sql操作
```
### パスの確認
root@34550af05c12:/# which psql createuser createdb
/usr/bin/psql
/usr/bin/createuser
/usr/bin/createdb

### ユーザー作成
root@34550af05c12:/# createuser -U postgres subaru

### 作成できているか確認
root@34550af05c12:/# psql -U postgres -c '\du'
                                   List of roles
 Role name |                         Attributes                         | Member
 of 
-----------+------------------------------------------------------------+-------
----
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 subaru    |                                                            | {}

### DB作成
createdb -U postgres -O subaru -E UTF8 --locale=C -T  template0 titan

### DB作成確認
 psql -U postgres -l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
   
-----------+----------+----------+------------+------------+--------------------
---
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres        
  +
           |          |          |            |            | postgres=CTc/postgr
es
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres        
  +
           |          |          |            |            | postgres=CTc/postgr
es
 titan     | subaru   | UTF8     | C          | C          | 
(4 rows)

```

### 5. テーブル作成(コマンドライン操作でsqlファイルを作成)
```
# cat >> create-titan1.sql << EOF
 create table titan1 (
 id integer primary key,
 name text not null unique,
 age integer
 )
 EOF

# cat create-titan1.sql 
create table titan1 (
id integer primary key,
name text not null unique,
age integer
)

# psql -U subaru titan < create-titan1.sql 
CREATE TABLE

```

### 6. 作成を確認
```
# psql -U subaru titan
psql (11.16 (Debian 11.16-1.pgdg90+1))
Type "help" for help.

titan=> \dt
        List of relations
 Schema |  Name  | Type  | Owner  
--------+--------+-------+--------
 public | titan1 | table | subaru
 ```
 
 ### 7. データインサート
 ```
 titan=> insert into titan1(id, name, age) values (101, 'Alice', 20);
INSERT 0 1
titan=> insert into titan1(id, name, age) values (102, 'Bob', 25);
INSERT 0 1
titan=> insert into titan1(id, name, age) values (103, 'Cathy', 23);
INSERT 0 1
titan=> select * from titan1;
 id  | name  | age 
-----+-------+-----
 101 | Alice |  20
 102 | Bob   |  25
 103 | Cathy |  23
(3 rows)
```

### 備考
titanテーブル
```
create table titans(
id integer primary key,
name text not null unique,
height integer,
gender text)
;

 insert into titans(id, name, height, gender)
 values (101, 'エレン', 170, 'M')
, (102, 'ミカサ', 170, 'F') 
, (103, 'アルミン', 163, 'M')
, (104, 'ジャン', 175, 'M')
,(105, 'サシャ', 168, 'F')
, (106, 'コニー',  158, 'M');
```


moviesテーブル
```
 create table movies (
  movie_id    integer    primary key
 , title       text       not null unique
 );
```

```
insert into movies (movie_id, title) values (93, '風の谷のナウシカ')
, (94, '天空の城ラピュタ') , (95, 'となりのトトロ')
, (96, '崖の上のポニョ')
;
```

characters テーブル
```
 create table characters ( id integer primary key,
 movie_id integer references movies(movie_id),
 name text not null,
 gender char(1) not null);

 insert into characters (id, movie_id, name, gender)
  values (401, 93, 'ナウシカ', 'F'),
 (402, 94, 'パズー' , 'M')
 , (403,94, 'シータ' , 'F'),
 (404,94, 'ムスカ' , 'M'),
 (405,95, 'さつき' , 'F'),
 (406, 95, 'メイ' , 'F'),
 (407, null, 'クラリス', 'F');
```
