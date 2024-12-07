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

[1068 - Product Sales Analysis I ](https://leetcode.com/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT Product.product_name , year , price FROM Sales
LEFT JOIN Product 
ON Sales.product_id = Product.product_id ;
```

[1581 - Customer who visited but did not make any transactions ](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT customer_id , COUNT(customer_id) AS count_no_trans
FROM Visits
LEFT JOIN Transactions 
ON Visits.visit_id = Transactions.visit_id
WHERE Transactions.transaction_id IS NULL
GROUP BY customer_id;
```

[197 - Rising Temperature ](https://leetcode.com/problems/rising-temperature/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT W1.id FROM Weather W1 
JOIN Weather W2
ON DATEDIFF(W1.recordDate,W2.recordDate) = 1
WHERE W1.temperature >  W2.temperature;
```

[1661 - Average time of process per machine ](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT A1.machine_id , 
ROUND(AVG(A2.timestamp - A1.timestamp) , 3) AS processing_time
FROM Activity A1
JOIN Activity A2
ON A1.machine_id = A2.machine_id
   AND A1.process_id = A2.process_id
   AND A1.activity_type = "start"
   AND A2.activity_type = "end"
GROUP BY A1.machine_id;   
```

[577 - Employee Bonus ](https://leetcode.com/problems/employee-bonus/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT name , Bonus.bonus FROM Employee 
LEFT JOIN Bonus 
ON Employee.empId = Bonus.empId 
WHERE Bonus.bonus < 1000 OR Bonus.bonus IS NULL;   
```

[1280 - Students and Examinations ](https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT Students.student_id , 
       Students.student_name , 
       Subjects.subject_name ,
       COALESCE(COUNT(Examinations.student_id),0)  AS attended_exams
FROM Students
CROSS JOIN Subjects
LEFT JOIN Examinations
ON Students.student_id = Examinations.student_id 
   AND Subjects.subject_name  = Examinations.subject_name
GROUP BY Students.student_id ,
         Students.student_name ,
         Subjects.subject_name   
ORDER BY 
    Students.student_id, Subjects.subject_name;          
```



