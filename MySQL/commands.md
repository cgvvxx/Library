# MySQL



## 1. 명령어

- or
  - SELECT [컬럼명] FROM [테이블명] WHERE [컬럼명2] IN ('조건1', '조건2');
    - or 조건을 여러개 할 때 > In
- 포함
  - SELECT [컬럼명] FROM [테이블명] WHERE [컬럼명] LIKE '특정문자열%';
    - 특정문자열로 시작하는
  - SELECT [컬럼명] FROM [테이블명] WHERE [컬럼명] LIKE '%특정문자열%';
    - 특정문자열을 포함하는
  - SELECT [컬럼명] FROM [테이블명] WHERE [컬럼명] LIKE '특정문자열?';
    - 특정문자열 + 한글자
- count / sum(max, min, avg)
  - SELECT COUNT(*) / COUNT([열이름]) FROM [테이블명] WHERE 조건식;
    - 행 계수는 *, 특정 열의 개수는 열이름
    - COUNT(*) NULL 값 포함, COUNT([열 이름]) NULL 값 포함 X
  - SELECT COUNT(DISTINCT [열이름]) FROM [테이블명] WHERE 조건식;
    - 중복 제외한 개수
  - SELECT SUM([열이름]) FROM [테이블명] WHERE 조건식;
- group by
  - SELECT [열이름] FROM [테이블명] WHERE 조건식1 GROUP BY [열이름] HAVING 조건식2 ORDER BY [열이름];
    - 조건1 처리 후, 그룹화 후, 조건2 처리 후, 순서 나열

- null

  - [열이름] IS NULL
    - 열이 NULL 인지 체크
  - SELECT IFNULL([열이름], '대체값') FROM [테이블명];
    - 열에서 NULL 값을 '대체값'으로 대체

- if, case, coalesce

  - SELECT IF([조건식], 'True일 경우', 'False일 경우') FROM [테이블명];

  - CASE

    ​	WHEN 조건식1 THEN 식1

    ​	WHEN 조건식2 THEN 식2

    ​	ELSE 식3

    END

  - SELECT COALESCE([열이름1], [열이름2], ...) FORM [테이블명];

    - 지정한 표현식들 중 첫번째로 NULL이 아닌 값 반환

- 사용자 정의 변수

  - ```sql
    SET @hour := -1;
    
    # EX1.
    SET @start := 15, @finish := 20;
    SELECT * FROM employee WHERE id BETWEEN @start AND @finish;
    ```

- join

  ![](./IMAGE/mysql_join.png)

- 날짜

  - DATEDIFF([date1], [date2])  ;  date1과 date2의 차이
  - SELECT **date_format(**datetime, **'%Y-%m-%d'****)** FROM `sandbox2` WHERE id=1;

- etc

  - 가장 위 몇가지 항만
    - LIMIT [num]

