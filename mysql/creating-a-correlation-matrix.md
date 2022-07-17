# Creating a Correlation Matrix

만약 다음과 같이 각 학생별 성적 테이블에 대하여 수학 성적과 과학 성적의 상관계수(Correlation) 또는 수학 성적과 영어 성적의 상관계수등 각 성적별 상관계수를 구하기 위하여 상관계수 행렬을 구하고 싶다고 하자.

| NAME   | MATH | SCIENCE | ENGLISH |
| ------ | ---- | ------- | ------- |
| DAVID  | 76   | 85      | 56      |
| JOSEPH | 91   | 95      | 75      |
| SARAH  | 56   | 75      | 60      |
| NANCY  | 75   | 82      | 64      |

먼저 위의 테이블을 다음과 같이 생성한다.

```sql
CREATE TABLE SCORE (NAME, MATH, SCIENCE, ENGLISH) AS
SELECT 'DAVID', 76, 85, 56 FROM DUAL UNION ALL
SELECT 'JOSEPH', 91, 95, 75 FROM DUAL UNION ALL
SELECT 'SARAH', 56, 75, 60 FROM DUAL UNION ALL
SELECT 'NANCY', 75, 82, 64 FROM DUAL;
```

그리고 다음과 같은 SQL 문을 통해 상관계수 행렬을 구할 수 있다.

```sql
WITH TEMP AS (
    SELECT ROWNUM AS RN, S.*
    FROM SCORE S
)
SELECT KEY,
       CORR(MATH, value) AS MATH,
       CORR(SCIENCE, value) AS SCIENCE,
       CORR(ENGLISH, value) AS ENGLISH
FROM TEMP T
    INNER JOIN (
        SELECT *
        FROM TEMP
        UNPIVOT (
            VALUE FOR KEY IN (MATH, SCIENCE, ENGLISH)
        ) 
    ) P
    ON T.RN = P.RN
GROUP BY KEY
```

| KEY     |                                      MATH |                                   SCIENCE |                                   ENGLISH |
| :------ | ----------------------------------------: | ----------------------------------------: | ----------------------------------------: |
| MATH    |                                         1 | .9757474866833620268116053973359468470468 | .6918765426586298809495346822543809197424 |
| SCIENCE | .9757474866833620268116053973359468470468 |                                         1 | .7325947528041109150090074211719818002762 |
| ENGLISH | .6918765426586298809495346822543809197424 | .7325947528041109150090074211719818002762 |                                         1 |

<br>

해당 상관계수 행렬을 구하는 과정을 살펴보자

**1. ROWNUM**

Oracle에서는 ROWNUM 키워드를 이용하여 조회되는 순번을 매길 수 있다.

```sql
SELECT ROWNUM AS RN, MATH, SIENCE, ENGLISH
FROM SCORE S
```

 ROWNUM | MATH | SCIENCE | ENGLISH
 -----: | ---: | ------: | ------:
   1 |   76 |      85 |      56
   2 |   91 |      95 |      75
   3 |   56 |      75 |      60
   4 |   75 |      82 |      64

**2. UNPIVOT**

UNPIVOT 함수를 통해 열을 행으로 바꿀 수 있다. (PIVOT 함수를 통해 행을 열로 바꿀수도 있다)

```sql
/* SELECT *
FROM (대상테이블)
UNPIVOT (컬럼별칭(값) FOR 컬럼별칭(키) IN (UNPIVOT할대상컬럼, ...)) AS UNPVT */

SELECT *
FROM (
    SELECT ROWNUM, MATH, SCIENCE, ENGLISH
    FROM SCORE
)
UNPIVOT (
    VALUE FOR KEY IN (MATH, SCIENCE, ENGLISH)
)
```

| ROWNUM | KEY     | VALUE |
| -----: | :------ | ----: |
|      1 | MATH    |    76 |
|      1 | SCIENCE |    85 |
|      1 | ENGLISH |    56 |
|      2 | MATH    |    91 |
|      2 | SCIENCE |    95 |
|      2 | ENGLISH |    75 |
|      3 | MATH    |    56 |
|      3 | SCIENCE |    75 |
|      3 | ENGLISH |    60 |
|      4 | MATH    |    75 |
|      4 | SCIENCE |    82 |
|      4 | ENGLISH |    64 |

**3. INNER JOIN**

기존의 테이블과 UNPIVOT한 테이블을 INNER JOIN하여 각 과목별 성적이 다른 과목별 성적과 RN을 KEY로 조인되어 합쳐진 테이블을 얻을 수 있다.

```sql
WITH TEMP AS (
    SELECT ROWNUM AS RN, S.*
    FROM SCORE S
)
SELECT *
FROM TEMP T
    INNER JOIN (
        SELECT *
        FROM TEMP
        UNPIVOT (
            VALUE FOR KEY IN (MATH, SCIENCE, ENGLISH)
        ) 
    ) P
    ON T.RN = P.RN
```

|   RN | MATH | SCIENCE | ENGLISH |   RN | KEY     | VALUE |
| ---: | ---: | ------: | ------: | ---: | :------ | ----: |
|    1 |   76 |      85 |      56 |    1 | MATH    |    76 |
|    1 |   76 |      85 |      56 |    1 | SCIENCE |    85 |
|    1 |   76 |      85 |      56 |    1 | ENGLISH |    56 |
|    2 |   91 |      95 |      75 |    2 | MATH    |    91 |
|    2 |   91 |      95 |      75 |    2 | SCIENCE |    95 |
|    2 |   91 |      95 |      75 |    2 | ENGLISH |    75 |
|    3 |   56 |      75 |      60 |    3 | MATH    |    56 |
|    3 |   56 |      75 |      60 |    3 | SCIENCE |    75 |
|    3 |   56 |      75 |      60 |    3 | ENGLISH |    60 |
|    4 |   75 |      82 |      64 |    4 | MATH    |    75 |
|    4 |   75 |      82 |      64 |    4 | SCIENCE |    82 |
|    4 |   75 |      82 |      64 |    4 | ENGLISH |    64 |

**4. CORR**

숫자 쌍 칼럼에 대하여 상관계수를 구해주는 CORR 함수와 GROUP BY를 이용하여 각 KEY 별(과목 별) 상관계수를 구하여 준다. Oracle에서 CORR 함수는 피어슨 상관 계수를 계산한다.

```sql
WITH TEMP AS (
    SELECT ROWNUM AS RN, MATH, SCIENCE, ENGLISH
    FROM SCORE
)
SELECT KEY,
       CORR(MATH, value) AS MATH,
       CORR(SCIENCE, value) AS SCIENCE,
       CORR(ENGLISH, value) AS ENGLISH
FROM TEMP T
    INNER JOIN (
        SELECT *
        FROM TEMP
        UNPIVOT (
            VALUE FOR KEY IN (MATH, SCIENCE, ENGLISH)
        ) 
    ) P
    ON T.RN = P.RN
GROUP BY KEY
```

| KEY     |                                      MATH |                                   SCIENCE |                                   ENGLISH |
| :------ | ----------------------------------------: | ----------------------------------------: | ----------------------------------------: |
| MATH    |                                         1 | .9757474866833620268116053973359468470468 | .6918765426586298809495346822543809197424 |
| SCIENCE | .9757474866833620268116053973359468470468 |                                         1 | .7325947528041109150090074211719818002762 |
| ENGLISH | .6918765426586298809495346822543809197424 | .7325947528041109150090074211719818002762 |                                         1 |



**Reference**

- [[ORACLE] UNPIVOT](https://gent.tistory.com/382)

- [Calculate correlation matrix using SQL in Oracle - Stack Overflow](https://stackoverflow.com/questions/72264240/calculate-correlation-matrix-using-sql-in-oracle)
