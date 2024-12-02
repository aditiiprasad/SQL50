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


