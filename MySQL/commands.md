# MySQL - 명령어



## 1. SELECT

- SELECT [컬럼명] FROM [테이블명] WHERE [조건식]

```mysql
SELECT CustomerID AS ID FROM Customers 
WHERE Address LIKE '%57' ORDER BY CustomerID DESC;

SELECT CustomerID, 'Default' FROM Customers;
# Default 이름의 열이 생기고 모든 항은 Default라는 문자열을 가짐

SELECT DISTINCT Country FROM Customers;
# Country 열에서 중복된 값 제외하고 출력

SELECT Country FROM Customers LIMIT 3;
# 주어진 결과에서 위에서 3개만 출력
# LIMIT num ; num 개수의 행 출력
# LIMIT num1 OFFSET num2 ; num2 번째 행부터 num1 개수의 행 출력
# LIMIT num1 num2 ; num1 번째 행부터 num2 개의 행 출력

SELECT Name, Math + English as Score FROM Exam_results;
# 산술 연산자
# Math 열과 English 열의 합을 Score 열에 저장하고 출력

SELECT * FROM Customers ORDER BY 3;
# Customers의 실행결과에서 3번째 열을 기준으로 오름차순 정렬

SELECT * FROM Customers WHERE Country = 'Germany'
UNION
SELECT * FROM Customers WHERE Country = 'Mexico'
# 집합 연산자
# UNION ; 중복값 제거한 합집합, UNION ALL ; 중복값 제거X 합집합
# INTERSECT ; 교집합

```



## 2. 단일행 함수

- 결과 레코드를 찾아서 출력할 때마다 각 행에 함수 적용

### 2.1 문자열

```mysql
SELECT UPPER(CustomerName), LOWER(Country) FROM Customers
# UPPER ; 대문자로, LOWER ; 소문자로, LENGTH ; 길이
# TRIM, LTRIM, RTRIM ; 공백 없애기

SELECT LPAD(Country, 20, '-') FROM Customers
# LPAD([컬럼명], 자리수, 채워넣을 값) ; 왼쪽으로 padding, RPAD

SELECT SUBSTR('America', 1, 3) FROM Customers; >> Ame
# SUBSTR('문자열', 시작위치, 리턴 길이) ; 주어진 열에서 시작위치부터 길이만큼의 문자열 반환 (시작 index = 1), SUBSTRING이랑 기능 동일

SELECT CONCAT('My country is ', Country) AS Country FROM Customers;
# 기존의 열의 값을 붙여서 새로운 열 생성, 열 또는 문자열이 여러개 있어도 가능

SELECT INSTR('MySQL', 'SQL'); >> 3
# INSTR('문자열', '찾는 문자') ; 문자열에서 찾는 문자의 인덱스, 없으면 0
# 대소문자 구분 X

SELECT REPLACE(Continent, 'a', '*') FROM Country
# REPLACE('문자열', '찾는 문자', '치환할 문자') ; 찾는 문자가 있으면 치환
# 대소문자 구분 O

```



### 2.2 숫자

```mysql
SELECT ROUND(123.34147, 2); >> 123.32
SELECT ROUND(1465127.124, -1); >> 1465130
# ROUND(num, '표현할 소수 자리수'), TRUNCATE와 기능 동일

# MOD(num, divisor) ; 나머지
# CEIL(num) ; num보다 크면서 가장 작은 정수
# FLOOR(num) ; num보다 작으면서 가장 큰 정수
# POWER(num, power)
```



### 2.3 날짜, 시간

```mysql
SELECT NOW(); >> 2021-08-13T04:37:44Z # SYSDATE()와 기능 동일
SELECT CURDATE(); >> 2021-08-13 # CURRENT_DATE와 기능 동일
SELECT CURRENT_TIME(); >> 04:38:02

SELECT MONTH('[날짜명]'), YEER, HOUR, MINUTE ...

SELECT DATE_ADD('2020-12-31 23:59:59', INTERVAL 2 day);
>> 2021-01-02 23:59:59
# DATE_ADD('날짜', INTERVAL 기간 type) ; 타입만큼의 기간을 주어진 날짜에 더해서 리턴, DATE_SUB
# type = second, minute, hour, day, month, year ...

SELECT DATE_FORMAT('2020-12-31 23:59:59', '%m-%d'); >> 12-31
# DATE_FORMAT('날짜', 'type') ; 주어진 날짜를 type형태로
# 'type' = '%y', '%d', '%h', '%l' ...
```



### 2.4 일반 함수

```mysql
SELECT IF(Salary > 500, '부자', '서민') FROM Salaries
# IF([조건식], '참인 경우', '거짓인 경우') 
# IFNULL([컬럼명], NULL 아니면 리턴할 값) 

# CASE
#    WHEN 조건 THEN '반환 값'
#    WHEN 조건 THEN '반환 값'
#    ELSE 'WHEN 조건에 해당 안되는 경우 반환 값'
# END

# EX.
SELECT name,
	CASE 
		WHEN dept = 'A' THEN '경영지원부'
		WHEN dept = 'B' THEN '영업부' 
	END AS dept,
    IF(salary >= 300 , '고액연봉', '일반') AS salary_type, 
    IFNULL(bonus, 0)
FROM salary;

SELECT COALESCE(dept1, dept2, dept3) FROM salary;
# SELECT COALESCE([열이름1], [열이름2], ...) FORM [테이블명];
# 지정한 표현식들 중 첫번째로 NULL이 아닌 값 반환
```



## 3. 다중행 함수

- 조건 절을 만족하는 모든 행을 다 찾고나서 모든 레코드를 한번에 연산
- 수행 순서 
  - FROM > WHERE > GROUP BY > HAVING > ORDER BY > SELECT > LIMIT

```mysql
# SUM, AVG, MAX, MIN, STDDEV, VARIANCE

SELECT COUNT(*) FROM SALARY
# COUNT(*) ; 모든 행의 개수를 count, 만약 컬럼명을 넣으면 NULL값을 제외한 개수를 count

SELECT Do, SUM(Budget_value) AS 'Budget_sum' FROM BUDGET GROUP BY Do;
# GROUP BY [컬럼명] ; 주어진 열을 기준으로 Grouping하여 출력
# SELECT [열이름] FROM [테이블명] WHERE 조건식1 GROUP BY [열이름] HAVING 조건식2 ORDER BY [열이름] ; 
# 조건1 처리 후, 그룹화 후, 조건2 처리 후, 순서 나열

# EX.
SELECT 
IF(Do IN ('서울특별시','경기도'), '수도권','지방') AS '지역구분', AVG(Budget_value) AS '예산평균', 
SUM(Budget_value) AS '예산합계' 
FROM Budget 
GROUP BY IF(Do IN ('서울특별시','경기도'), '수도권','지방') AS '지역구분';
# 열에 조건절을 사용하는 경우 SELECT 절과 GROUP BY 절에 같은 형태로 작성해야함
```



## 4. Join

![](./IMAGE/mysql_join.png)

```mysql
# EX.
SELECT c.name AS 고객명, c.point AS 고객_point, g.name AS 상품명 
FROM customer c 
JOIN gift g ON c.point BETWEEN g.point_s AND g.point_e;
# customer 테이블의 point라는 열 값이 gift 테이블의 point_s와 point_e 사이에 있는 경우에 대해서 join

# FULL OUTER JOIN ~ UNION
```



## 5. etc

- 포함

```mysql
# [컬럼명] LIKE '특정문자열%' ; 특정문자열로 시작하는
# [컬럼명] LIKE '%특정문자열%' ; 특정문자열을 포함하는
# [컬럼명] LIKE '특정문자열?' ; 특정문자열 + 한글자
```

- 사용자 정의 변수

```sql
SET @hour := -1;

# EX.
SET @start := 15, @finish := 20;
SELECT * FROM employee WHERE id BETWEEN @start AND @finish;
```

- EXISTS
  - EXISTS , NOT EXISTS
  - IN이나 JOIN으로 표현할 수도 있음
    - 일반적으로 JOIN 연산이 더 빠르고 IN 연산은 느림

```mysql
# EX.
SELECT * 
FROM customers # 내가 결과를 원하는 테이블(customers)
WHERE EXISTS ( 
    SELECT * FROM orders WHERE orders.c_id = customers.c_id
    # 존재 여부를 확인하고 싶은 테이블(orders)
);

# 같은 문장을 'IN'으로
SELECT * 
FROM customers 
WHERE c_id IN ( SELECT DISTINCT c_id FROM orders);

# 같은 문장을 'JOIN'으로
SELECT DISTINCT c.*
FROM customers c 
JOIN orders o
ON o.c_id = c.c_id;
```

