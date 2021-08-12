# MySQL



## 1. 기본 명령어

```mysql
SHOW DATABASES;
SHOW TABLES;
DESC _tablename;	>> describe 테이블
```



## 2. 데이터베이스 생성

```mysql
CREATE TABLE _tablename(
	_col1 _datatype NOT NULL AUTO_INCREMENT,   >> 값이 없으면 안됨, 자동으로 1씩 증가
    _col2 _datatype,
    PRIMARY KEY(_col1)   >> 식별자 column 지정
    
    
ex.

CREATE TABLE topic(
	id INT(11) NOT NULL AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    description TEXT NULL,
    created DATETIME NOT NULL,
    PRIMARY KEY(id)
)
```



## 3. CRUD(Create, Read, Update, Delete)

```mysql
INSERT INTO _tablename (_col1, col2, ..) VALUES ('_content1', 'content2', ...);
	>> 주어진 테이블에 행 추가
 == INSERT INTO _tablename VALUES ('_content1', ..);	>> 전체 col 개수만큼 맞춰서 넣을때
ALTER TABLE _tablename ADD _col1 datatype;	>> 주어진 테이블에 열 추가

SELECT * FROM _tablename;	>> 주어진 테이블의 모든 항 읽기
SELECT _col1, _col2 FROM _tablename WHERE _col1='_content1' ORDER BY id DESC LIMIT 2;		>> _tablename에서 _col1, _col2항만 출력, 주어진 col1이 지정한 값을 가지는 행만, id 순으로 내림차순, 위에서부터 2개만

UPDATE _tablename SET _col1='_content1', _col2='_content2' WHERE id=2;
	>> id가 2인 행에서의 _col1과 _col2의 내용을 수정

DELETE FROM topic WHERE id=5;	>> topic 테이블에서 id=5인 행 제거
```



## 4. JOIN

```mysql
SELECT * FROM topic LEFT JOIN author on topic.author_id = author.id;
	>> topic 테이블에서 author 테이블을 left join, topic의 author_id col이 author의 id 값과 같을 때

SELECT topic.id AS topic_id, title, name, profile FROM topic LEFT JOIN author on topic.author_id = author.id;
	>> topic에 있는 id col의 이름을 topic_id로 바꾸고 선택한 col만 출력
```


