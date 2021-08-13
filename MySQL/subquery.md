# MySQL - subquery

> 하나의 SQL 문 안에 포함되어 있는 또 다른 SQL문



## 1. 정의

- 서브쿼리란 부모 쿼리 안에 작성하는 내부의 SELECT 쿼리이며, 주로 부모 쿼리의 FROM과 WHERE의 조건으로 사용
- SELECT와 괄호 ()를 이용하여 작성
- 일반적으로 subquery나 IN 함수는 성능이 좋지 않아 잘 쓰이지 않음
  - JOIN으로 대체 가능한 경우 더 좋은 성능을 가짐



## 2. 분류

### 2.1 위치에 따른 분류

- SELECT 절 ; 스칼라 서브쿼리(Scalar Subquery)

```mysql
SELECT Player_name, Height, (
	SELECT AVG(Height)
	FROM Player p
	WHERE p.Team_ID = x.Team_ID) as Avg_height
FROM Player x;
```

- FROM 절 ; 인라인 뷰(Inline View)
  - 동적으로 생성된 테이블, 무조건 alias 지정해줘야 함

```mysql
SELECT t.Team_name, p.Player_name, p.Height
FROM Player t, (
	SELECT Team_ID, Player_name, Back_no
	FROM Player
	WHERE Postion = 'MF') p
WHERE p.Team_ID = t.Team_ID;
```

- WHERE 절 ; 중첩 서브쿼리(Nested Subquery)

```mysql
SELECT Player_name
FROM Player
WHERE Team_ID IN (
	SELECT Team_ID
	FROM Team
	WHERE Team_country = 'KOR');
```



### 2.2 반환 값에 따른 분류

- 단일 행 서브쿼리(Single Row Subquery) 
  - 하나의 열로 구성된 결과 행 하나를 반환

```mysql
SELECT agent_name, agent_code, phone_no
FROM agents
WHERE agent_code = (
    SELECT agent_code 
  	FROM agents
  	WHERE agent_name = 'Alex');
```

- 다중 행 서브쿼리(Multiple Row Subquery)
  - 서브쿼리 결과로 여러개의 행을 반환
  - IN, ANY, ALL, EXISTS 등의 연선자를 이용

```mysql
SELECT ord_num,ord_amount,ord_date, cust_code, agent_code
FROM orders
WHERE agent_code IN (
    SELECT agent_code FROM agents
 	WHERE working_area='Bangalore');
```

- 다중 컬럼 서브쿼리(Multiple Column Subquery)
  - 서브쿼리 결과 여러개의 행을 반환
  - 이 때 WHERE 또는 HAVING  연산자 사이에 컬럼 리스트가 괄호로 묶여 있어야 함

```mysql
SELECT ord_num, agent_code, ord_date, ord_amount
FROM orders
WHERE (agent_code, ord_amount) IN (
	SELECT agent_code, MIN(ord_amount)
	FROM orders 
	GROUP BY agent_code);
```

