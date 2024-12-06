# SQL50
Leetcode SQL50 Answers


---

[1757 - Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
```sql
SELECT product_id FROM products
WHERE low_fats="Y" AND recyclable = "Y";
```

[584 - Find Customer Referee ](https://leetcode.com/problems/find-customer-referee/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT name FROM Customer
WHERE referee_id != 2 OR referee_id IS null;
```

[595 - Big Countries ](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT name , area , population FROM World 
WHERE area > 2999999 OR population > 24999999;
```

[1148 - Artical Views I ](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT DISTINCT author_id AS id FROM Views
WHERE author_id = viewer_id 
ORDER BY id ASC;
```

[1683 - Invalid Tweets ](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT tweet_id FROM Tweets 
WHERE CHAR_LENGTH(content) > 15;
```

[1378 - Replace Employee ID with Unique Identifiers ](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT EmployeeUNI.unique_id , name
FROM Employees
LEFT JOIN EmployeeUNI 
ON Employees.id = EmployeeUNI.id;
```



