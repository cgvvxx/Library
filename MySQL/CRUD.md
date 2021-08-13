# MySQL - CRUD 명령어



## 1. CRUD(Create, Read, Update, Delete)

### 1.1 Create - INSERT

- DB에 새로운 값을 넣는 작업

```mysql
# INSERT INTO [테이블명] ([열이름1], [열이름2], ..) VALUES ('var1', 'var2', ...)

# EX.
INSERT INTO users  VALUES ('KIM', 23, 0, 'hi');
# INSERT INTO [테이블명] VALUES (var1, var2, ...) ; column 지정안할 시 차례대로, 전체 열의 개수와 인풋 개수가 같아야함

# EX.
INSERT INTO users (name, age, married) VALUES ('LEE', 18, 0);
# 열이름 지정시 지정된 열에만, 지정되지 않은 열은 NULL 값 입력

# EX.
INSERT INTO users  
VALUES ('KIM', 23, 0, 'hi'),
	('PARK', 62, 1, 'hello'),
	('CHOI', 45, 1, 'bye');
# 복수개의 행 입력
```

- INSERT ~ ON DUPLICATE KEY
  - 입력하는 테이블에 해당 키에 해당하는 데이터가 없으면 INSERT문을 실행하여 입력, 해당 키가 이미 테이블에 있으면 UPDATE

```mysql
# EX.
INSERT INTO insert_test VALUES (10, 'hi', 'kim', 010-1234-5678, now()) 
ON DUPLICATE KEY UPDATE 
	cont = '사회적 거리두기를 실천 합시다.', 
	name = 'choi', 
	tel_num = 010-9876-5432, 
	input_date = now();
```



### 1.2 Read - SELECT 

- select 명령어 참고



### 1.3 Update - UPDATE

- 기존 DB에 존재하는 데이터의 값을 바꾸는 작업

```mysql
# UPDATE [테이블명] SET [열이름1]='var1', [열이름2]='var2', ... WHERE [조건식]

# EX.
UPDATE users SET age = 30 WHERE id = 2;
```



### 1.4 Delete

- 기존 DB에 존재하는 데이터의 삭제

```mysql
# DELETE FROM [테이블명] WHERE [조건식]

# EX.
DELETE FROM users WHERE id = 2;

# EX.
DELETE p1 FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND p1.Id > p2.Id
# contacts 테이블에서 email이 중복된 항 제거
```


