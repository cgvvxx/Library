# In vs. Or Operator

sql문에서 where절의 조건들을 OR 연산자로 연결하여 사용하거나 IN 연산자를 사용한다. 예를 들어 다음과 같이  비교하고자 하는 칼럼이 특정한 값들 중 하나에 해당하는 조건을 조회하고자 하는 경우, 두 sql문은 동일한 결과를 보여준다.

```sql
SELECT *
FROM TABLE
WHERE COL = '1' OR COL = '2' OR COL = '3' OR COL = '4'
```

```sql
SELECT *
FROM TABLE
WHERE COL IN ('1', '2', '3', '4')
```

위와 같이 하나의 컬럼에 대하여 여러 개의 '=' 조건으로 묶이는 경우 IN 연산자를 사용할 수 있다. IN 연산자는 '=' 조건으로 묶이는 경우에만 사용 가능하므로 IN 연산자로 표현할 수 있으면 OR 연산자로 항상 표현할 수 있지만, OR 연산자로 표현 가능하다고 항상 IN 연산자로 대체할 수 있지는 않다.

```sql
-- 다음과 같은 OR 연산자의 경우 IN 연산자로 대체 불가능하다
SELECT *
FROM TABLE
WHERE (COR = '1' OR COL LIKE '2%')
```

결론부터 보자면 일반적으로 IN 연산자를 사용할 수 있다면, OR 연산자보다는 IN 연산자를 사용하는 것이 좋다.

1. 가장 먼저 IN 연산자가 OR 연산자에 비하여 사용하기 편리하고 가독성이 좋다.

2. IN 연산자의 경우 경우에 따라 서브쿼리를 사용할 수 있다.

   ```sql
   -- IN 연산자 내부에서 서브쿼리를 사용할 수 있다
   SELECT *
   FROM TABLE1
   WHERE COL1 IN (SELECT *
   	    		FROM TABLE2 
       			WHERE COL2 = '1')
   ```

3. IN 연산자가 OR 연산자에 비해 성능이 좋다.

   - MySQL의 경우 IN 연산자를 사용할 때, 해당 리스트 안의 값을 정렬하고, 해당 값이 배열 안에 있는지 탐색하는 이진 배열을 사용하기 때문에 시간복잡도 O(logn)으로, 시간복잡도 O(n)인 OR 연산자보다 일반적으로 빠르다.



**Reference**

- [MySQL :: MySQL 8.0 Reference Manual :: 12.4.2 Comparison Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html#function_in)

- [MySQL OR vs IN performance - Stack Overflow](https://stackoverflow.com/questions/782915/mysql-or-vs-in-performance)

- [IN vs OR in the SQL WHERE clause - Stack Overflow](https://stackoverflow.com/questions/3074713/in-vs-or-in-the-sql-where-clause)
