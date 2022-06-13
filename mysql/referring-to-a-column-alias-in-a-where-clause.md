# Referring to a Column Alias in a WHERE Clause

만약 다음과 같이 SELECT 절에 ALIAS를 사용하여 COLUMN을 생성하여 WHRER 절에서 사용하고자 하는 경우 오류를 발생시킨다.

```mysql
SELECT StudentID as ID, StudentScore as SCORE
FROM STUDENTS
WHERE SCORE >= 80;
```

이는 SQL의 문법 작성 순서와 실제 실행되는 순서가 다르기 때문이다. 기본적인 SQL의 문법 순서와 실행 순서는 다음과 같다.

- 문법 작성 순서 : **SELECT**  >  **FROM**  >  **WHERE**  >  **GROUP BY**  >  **HAVING**  >  **ORDER BY**
- 실행 순서 : **FROM**  >  (ON  >  JOIN)  >  **WHERE**  >  **GROUP BY**  >  **HAVING**  >  **SELECT**  >  (DISTINCT)  >  **ORDER BY**  >  (LIMIT | OFFSET)

WHERE 절 같은 경우 SELECT 절 보다 빨리 실행되기 때문에 SELECT 절에서 사용한 ALIAS는 WHERE 절에서 사용할 수 없다. 이것을 기존에 원했던 방식으로 작동하기 위하여는 다음과 같이 서브쿼리를 사용하여야 한다.

```mysql
SELECT *
FROM (
	SELECT StudentID as ID, StudentScore as SCORE
    FROM STUDENTS
    WHERE SCORE >= 80
    ) AS T
WHERE T.SCORE >= 80;
```

다만 MySQL의 경우 GROUP BY, ORDER BY 및 HAVING 절에서는 SELECT 절의 ALIAS를 사용할 수 있다. GROUP BY와 HAVING 절 같은 경우 기본적인 실행 순서는 SELECT 절의 ALIAS보다 앞에 있지만 MySQL에서는 이를 DBMS 자체에서 인식하게 처리해주기 때문이다.  



**Reference**

- [Learn SQL - Order of execution of a Query](https://sqlbolt.com/lesson/select_queries_order_of_execution)

- [MySQL 8.0 Reference Manual - Problems with Column Aliases](https://dev.mysql.com/doc/refman/8.0/en/problems-with-alias.html)
