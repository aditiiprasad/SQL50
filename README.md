# SQL50
Leetcode SQL50 Answers


---


## Select

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


## Basic Joins

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


[570 - Manager with atleast 5 direct reports ](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT E2.name 
FROM Employee E2
JOIN Employee E1 
ON E1.managerId = E2.id
GROUP BY E2.id, E2.name
HAVING COUNT(E1.id) >= 5; 
```

[1934 - Confirmation Rate](https://leetcode.com/problems/confirmation-rate/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT Signups.user_id, 
ROUND(
    CASE 
    WHEN COUNT(Confirmations.user_id) = 0 THEN 0
    ELSE SUM(CASE WHEN Confirmations.action = 'confirmed' THEN 1 ELSE 0 END) / COUNT(Confirmations.user_id) END ,2) AS confirmation_rate 
FROM Signups
LEFT JOIN Confirmations
ON Signups.user_id = Confirmations.user_id
GROUP BY Signups.user_id;
```


## Basic Aggregate Functions

[620 - Not Boring Movies](https://leetcode.com/problems/not-boring-movies/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT id, movie, description, rating
FROM Cinema 
WHERE id%2 = 1
      AND description != "boring"
ORDER BY rating DESC; 
```

[1251 - Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT P.product_id ,
 COALESCE(ROUND(SUM(P.price * U.units) / NULLIF(SUM(U.units), 0), 2), 0) AS average_price
FROM Prices P
LEFT JOIN UnitsSold U
ON P.product_id = U.product_id
   AND U.purchase_date BETWEEN P. start_date AND P.end_date
GROUP BY P.product_id;  
```

[1075 - Project Employess I](https://leetcode.com/problems/project-employees-i/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT P.project_id  , 
ROUND(SUM(E.experience_years)/COUNT(P.employee_id),2)
AS average_years
FROM Project P
JOIN Employee E
ON P.employee_id = E.employee_id
GROUP BY P.project_id ;
```

[1633 - Percentage of users attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT R.contest_id ,
ROUND(COUNT(R.user_id)*100 / (SELECT COUNT(user_id) FROM Users),2)
AS percentage 
FROM Register R
GROUP BY R.contest_id
ORDER BY percentage DESC,
         R.contest_id ASC;

```





[1211 - Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT query_name ,
ROUND(AVG(rating/position),2)
AS quality, 
ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END)*100 / COUNT(rating),2)
AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name;
```

[1193 - Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT
DATE_FORMAT(trans_date, '%Y-%m') AS month,
country,
COUNT(id) AS trans_count ,
COUNT(CASE WHEN state = "approved" THEN 1 END) AS approved_count ,
SUM(amount) AS trans_total_amount ,
SUM(CASE WHEN state="approved" THEN amount ELSE 0 END) AS approved_total_amount
FROM  transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;
```

[1174 - Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
ROUND(COUNT(CASE WHEN  order_date=customer_pref_delivery_date THEN 1 END)*100/COUNT(customer_id),2)
AS immediate_percentage 
FROM Delivery
WHERE 
(customer_id , order_date ) IN (
    SELECT customer_id, MIN(order_date)
    FROM Delivery
    GROUP BY customer_id
);

```

[550 - Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/?envType=study-plan-v2&envId=top-sql-50)
```sql
WITH FirstLogin AS (

    SELECT
        player_id,
        MIN(event_date) AS first_login_date
    FROM
        Activity
    GROUP BY
        player_id
),
NextDayLogin AS (
    
    SELECT
        f.player_id
    FROM
        FirstLogin f
    JOIN
        Activity a
    ON
        f.player_id = a.player_id
        AND a.event_date = DATE_ADD(f.first_login_date, INTERVAL 1 DAY)
)

SELECT
    ROUND(COUNT(DISTINCT n.player_id) * 1.0 / COUNT(DISTINCT f.player_id), 2) AS fraction
FROM
    FirstLogin f
LEFT JOIN
    NextDayLogin n
ON
    f.player_id = n.player_id;
```


## Sorting and Grouping

[2356 - Number of unique subjects tought by each teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT teacher_id,
       COUNT(DISTINCT subject_id ) as cnt 
FROM Teacher
GROUP BY   teacher_id;

```

[1141 - User Activity for the past 30 days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT activity_date AS day ,
       COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE
      activity_date BETWEEN DATE_SUB('2019-07-27', INTERVAL 29 DAY )   AND '2019-07-27'
GROUP BY
    activity_date;   
```

[1070 - Prodyct Sales analysis III](https://leetcode.com/problems/product-sales-analysis-iii/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    s.product_id,
    s.year AS first_year,
    s.quantity,
    s.price
FROM 
    Sales s
JOIN 
    (SELECT product_id, MIN(year) AS first_year
     FROM Sales
     GROUP BY product_id) first_year_table
ON 
    s.product_id = first_year_table.product_id 
    AND s.year = first_year_table.first_year;

```

[596 - Class more than 5 students](https://leetcode.com/problems/classes-more-than-5-students/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

[1729 - Find Followers Count](https://leetcode.com/problems/find-followers-count/submissions/1473520338/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT DISTINCT user_id,
       COUNT(DISTINCT follower_id) AS followers_count
 FROM Followers
 GROUP BY user_id;      
```

[619 - Biggest Single Number](https://leetcode.com/problems/biggest-single-number/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    MAX(num) AS num
FROM (
    SELECT num 
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(*) = 1
) AS single_numbers;   
```

[1045 - Customers who bought all products](https://leetcode.com/problems/customers-who-bought-all-products/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(DISTINCT product_key) FROM Product);
 
```










## Advanced Select and Joins

[1731 - The number of employee which report to each employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    e.employee_id, 
    e.name, 
    COUNT(r.employee_id) AS reports_count, 
    ROUND(AVG(r.age)) AS average_age
FROM 
    Employees e
LEFT JOIN 
    Employees r
ON 
    e.employee_id = r.reports_to
WHERE 
    r.employee_id IS NOT NULL
GROUP BY 
    e.employee_id, e.name
ORDER BY 
    e.employee_id;
 
```

[1789 - Primary department of each employee](https://leetcode.com/problems/primary-department-for-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    employee_id, 
    department_id
FROM 
    Employee
WHERE 
    primary_flag = 'Y'
UNION
SELECT 
    employee_id, 
    MIN(department_id) AS department_id
FROM 
    Employee
GROUP BY 
    employee_id
HAVING 
    COUNT(*) = 1;

```

[610 - Triangle Judgement](https://leetcode.com/problems/triangle-judgement/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    x, 
    y, 
    z, 
    CASE 
        WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM 
    Triangle;

 
```

[180 - Consecutive Number](https://leetcode.com/problems/consecutive-numbers/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l1.id = l2.id - 1
JOIN Logs l3 ON l1.id = l3.id - 2
WHERE l1.num = l2.num AND l2.num = l3.num;
```

[1164 - Product Price at a given date](https://leetcode.com/problems/product-price-at-a-given-date/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    p.product_id,
    COALESCE(
        (SELECT new_price 
         FROM Products 
         WHERE product_id = p.product_id 
           AND change_date <= '2019-08-16' 
         ORDER BY change_date DESC 
         LIMIT 1), 10) AS price
FROM 
    (SELECT DISTINCT product_id FROM Products) p;

```

[1204 - Last person to fit in the bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT person_name
FROM Queue
WHERE (SELECT SUM(weight) 
       FROM Queue AS q2 
       WHERE q2.turn <= Queue.turn) <= 1000
ORDER BY turn DESC
LIMIT 1;
```

[1907 - Count Salary categories](https://leetcode.com/problems/count-salary-categories/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    'Low Salary' AS category, 
    COUNT(*) AS accounts_count 
FROM Accounts 
WHERE income < 20000
UNION ALL
SELECT 
    'Average Salary' AS category, 
    COUNT(*) AS accounts_count 
FROM Accounts 
WHERE income BETWEEN 20000 AND 50000
UNION ALL
SELECT 
    'High Salary' AS category, 
    COUNT(*) AS accounts_count 
FROM Accounts 
WHERE income > 50000;
```


## Subqueries

[1978 - Employees whose manager left the company](https://leetcode.com/problems/employees-whose-manager-left-the-company/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT e1.employee_id
FROM Employees e1
LEFT JOIN Employees e2 ON e1.manager_id = e2.employee_id
WHERE e1.salary < 30000
  AND e1.manager_id IS NOT NULL
  AND e2.employee_id IS NULL
ORDER BY e1.employee_id;
```

[626 - Exchange Seats](https://leetcode.com/problems/exchange-seats/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT id, 
CASE WHEN MOD(id,2)=0 THEN (LAG(student) OVER (ORDER BY id))
ELSE (LEAD(student, 1, student) OVER (ORDER BY id))
END AS 'Student'
FROM Seat


```

[1341 - Movie Rating](https://leetcode.com/problems/movie-rating/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
(SELECT name AS results
FROM Users u
LEFT JOIN MovieRating mr
ON u.user_id = mr.user_id
GROUP BY name
ORDER BY COUNT(rating) DESC, name ASC
LIMIT 1)

UNION ALL

(SELECT title
FROM Movies m
LEFT JOIN MovieRating mr
ON m.movie_id = mr.movie_id
WHERE DATE_FORMAT(created_at, '%Y-%m') = '2020-02'
GROUP BY title
ORDER BY AVG(rating) DESC, title ASC
LIMIT 1 
)
```

[1321 - Restaurant Growth](https://leetcode.com/problems/restaurant-growth/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT visited_on, amount, ROUND(amount/7, 2) AS average_amount
FROM (
    SELECT DISTINCT visited_on,
    SUM(amount) OVER(ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW) AS amount,
    MIN(visited_on) OVER() day_1
    FROM Customer
) t
WHERE visited_on >= day_1+6;
```

[602 - Friend Request II - Who has the most friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/?envType=study-plan-v2&envId=top-sql-50)
```sql
WITH CTE AS (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
)

SELECT id, COUNT(id) AS num
FROM CTE
GROUP BY id
ORDER BY num DESC
LIMIT 1
```

[585 - Investments in 2016](https://leetcode.com/problems/investments-in-2016/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT
    ROUND(SUM(tiv_2016),2) AS tiv_2016
FROM insurance
WHERE tiv_2015 IN (SELECT tiv_2015 FROM insurance GROUP BY tiv_2015 HAVING COUNT(*) > 1)
AND (lat,lon) IN (SELECT lat,lon FROM insurance GROUP BY lat,lon HAVING COUNT(*) = 1)
```

[185 - Departments top 3 salaries](https://leetcode.com/problems/department-top-three-salaries/description/?envType=study-plan-v2&envId=top-sql-50)
```sql

WITH RankedSalaries AS 
(SELECT 
    e.Id AS employee_id,
    e.name AS employee,
    e.salary,
    e.departmentId,
    DENSE_RANK() OVER (PARTITION BY e.departmentId ORDER BY e.salary DESC) AS salary_rank 
FROM Employee e)
SELECT d.name AS Department,
r.employee,
r.salary
FROM Department d
JOIN RankedSalaries r ON r.departmentId = d.id
WHERE r.salary_rank <=3;

```


















## Advanced String Functions / Regex / Clause

[1667 - Fix Names in Table](https://leetcode.com/problems/fix-names-in-a-table/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    user_id, 
    CONCAT(UPPER(SUBSTRING(name, 1, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM 
    Users
ORDER BY 
    user_id;
```



[1527 - Patients with a Condition](https://leetcode.com/problems/patients-with-a-condition/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT patient_id,
       patient_name,
       conditions
FROM Patients
WHERE conditions REGEXP '(^| )DIAB1';
```





[196 - Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/?envType=study-plan-v2&envId=top-sql-50)
```sql
DELETE FROM Person
WHERE id NOT IN (
    SELECT id
    FROM (
        SELECT MIN(id) AS id
        FROM Person
        GROUP BY email
    ) AS temp
);
```



[176 - Second Highest Salary](https://leetcode.com/problems/second-highest-salary/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT 
    MAX(salary) AS SecondHighestSalary
FROM 
    Employee
WHERE 
    salary < (SELECT MAX(salary) FROM Employee);
 
```





[1484 - Group Sold Products by date](https://leetcode.com/problems/group-sold-products-by-the-date/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT sell_date,
       COUNT(DISTINCT product ) AS num_sold,
       GROUP_CONCAT(DISTINCT product ORDER BY PRODUCT) AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date; 
```


      

[1327 - List of products ordered in a period ](https://leetcode.com/problems/list-the-products-ordered-in-a-period/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT P.product_name, SUM(O.unit) AS unit
FROM Orders O
JOIN Products P ON O.product_id = P.product_id
WHERE O.order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY O.product_id
HAVING SUM(O.unit) >= 100;

```

[1517 - Find users with valid emails](https://leetcode.com/problems/find-users-with-valid-e-mails/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT user_id , name , mail
FROM Users 
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$';
```
 

 