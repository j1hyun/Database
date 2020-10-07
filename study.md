## 가상머신이 작동되기 전 설정
> 머신 > 설정 > 시스템 > 프로세서 >프로세서 개수: 4개로 늘리기  
> 네트워크 > 어댑터1 > 호스트 전용 어댑터, 어댑터2> NAT   
  
> 파일 > 호스트 네트워크 관리자 > DHCP 서버 사용 해제  
> IPv4 주소가 VirtualBox Host-Only Network의 IPv4 주소와 같은지 확인할 것

## 기본 인코딩 utf8로 DBname database  생성

mysql> CREATE DATABASE DBname DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
  
## 사용자 생성  
  
mysql> create user Username identified by 'Password'  
Username과 password는 각자 자유롭게 설정

​

## 생성한 사용자에게 권한 주기  

mysql> grant all on DBname.* to Username@﻿'localhost';  
'localhost' 대신 '%'로 적으면 외부에서도 접속 가능하다  

​

(+)처음에는

mysql> grant all on DBname.* to Username@'%' identified by 'Password'  
위 코드를 이용하였는데 Error 1064(42000) : you have an error in your SQL Syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near~ 과 같은 오류가 났다.  
identified~ 부분을 빼자 오류 없이 실행이 되었다.  
​

위 작업이 끝나면  

mysql> flush privileges;  
로 설정을 저장해 준다.  

​
mysql -u Username -p  
로 다음에 접속하면 특정 사용자로 로그인이 가능해진다.  
  
재로그인 후  use DBname;  
  
​
  
## 테이블 생성  
  
CREATE TABLE Tablename  
    -> (  
    ->  id INT PRIMARY KEY AUTO_INCREMENT,   
    ->  name VARCHAR(32) NOT NULL,  
    ->  test VARCHAR(32)  
    -> ) CHARSET=utf8;  
Tablename이라는 이름을 가지고 있는 TABLE을 생성한다.  
  
임의로 설정한 id, name, test가 filed name이다. INT, VARCHAR는 데이터 타입(자료형)을 의미한다.  
  
(+)https://devdhjo.github.io/mysql/2020/01/30/database-mysql-003.html
  
 
[MySQL] 003# MySQL 데이터 타입 (자료형) 정리  
  

에서 다른 자료형도 확인할 수 있다.  
  
PRIMARY KEY는 unique, not null의 제약 조건의 특징을 가지고 있는 키다. 이 키는 table 당 1개만 설정할 수 있다.  

auto_increment는 자동으로 숫자가 증가함을 의미한다.  

NOT NULL을 적어 주면 그 field에는 null값이 존재하면 안 된다.  

테이블 안의 데이터가 한글일 때 깨지지 않게 하기 위하여 charset=utf8로 설정해 준다.  
  
​
  
mysql> DESC Tablename;  
+-------+-------------+------+-----+---------+----------------+  
| Field | Type        | Null | Key | Default | Extra          |  
+-------+-------------+------+-----+---------+----------------+  
| id    | int         | NO   | PRI | NULL    | auto_increment |  
| name  | varchar(32) | NO   |     | NULL    |                |  
| test  | varchar(32) | YES  |     | NULL    |                |  
+-------+-------------+------+-----+---------+----------------+  
3 rows in set (0.01 sec)  
DESC Tablename을 하면 다음과 같은 결과를 확인할 수 있다.  
  
​
  
## TABLE에 데이터 삽입(INSERT INTO)  
  
테이블에 데이터 삽입하는 방법 세 가지를 살펴보자.  
  
​

1. insert into tablename values();  
  
mysql> insert into Tablename values('1','test', 'test1 sentence');  
다음과 같이 필드 이름을 지정해 주지 않고  values만 사용할 경우 순서에 맞춰 모든 키를 적어 주어야 에러가 나지 않는다.  
  
​
  
2.insert into tablename () values();  

mysql> insert into Tablename (name, test) values('test2', 'test2 sentence');  
tablename 뒤에 데이터를 삽입할 필드를 지정할 수 있다. id는 auto_increment로 지정해 주었으므로 나머지 두 필드만 적고 데이터를 삽입해도 에러가 나지 않는다.  
  
​
  
3.부분 삽입  
  
mysql> insert into Tablename (name) values('test3');  
위에서 보면 name field는 null값이 존재하면 안 되지만 test field는 생략 가능한 filed이다. 그러므로 name에만 데이터를 삽입해도 에러가 나지 않는다.  
  
mysql> insert into Tablename (name, test) values('test4', '한글 테스트');  
  
## 전체 TABLE 확인  
  
mysql> select *from Tablename;  
+----+-------+----------------+  
| id | name  | test           |  
+----+-------+----------------+  
|  1 | test  | test1 sentence |  
|  2 | test2 | test2 sentence |  
|  3 | test3 | NULL           |  
|  4 | test4 | 한글테스트      |  
+----+-------+----------------+  
3 rows in set (0.00 sec)  
select * from Tablename을 하면 전체 테이블 값을 조회할 수 있다. 앞서 삽입한 데이터들이 제대로 삽입되었음을 알 수 있다. 한글도 깨지지 않는다.  
  
​
  
select fieldname from Tablename을 하면 특정 filed값만 확인할 수도 있다.  
  
ex)  
  
mysql> select name from testtable;  
+-------+  
| name  |  
+-------+  
| test  |  
| test2 |  
| test3 |  
| test4 |  
+-------+  
3 rows in set (0.10 sec)  


 ## Table Column 확인
 > desc tablename
 
 ## Table Column 추가
 > alter table 테이블명 add 컬럼명 자료형 옵션;
 
 ## Table Column 위치 변경
 > alter table 테이블명 modify column 컬렴명 자료형 after 바꿀위치앞의컬럼명;  
 또는  
 > alter table 테이블명 modify column 컬럼명 자료형 first;  
   
 ## PRIMARY KEY 설정(기존 column을  primary key로 지정할 때)  
 > alter table 테이블명 modify column 컬럼명 자료형 primary key;  
  ex) alter table jongro modify column id char primary key;  
     
 ## FOREIGN KEY 설정(기존 column을 foreign key로 지정할 때)  
 > alter table 테이블명 add foreign key (컬럼명) regerence 연결할테이블명(컬렴명);  
  ex) alter power_table add foreign key (id) references jongro(id);   
 
