### BASIC JOINS QUESTIONS AND SOLUTIONS

**1.Write an SQL query to show the unique ID of each user, If a user does not have a unique ID replace just show null.
Return the result table in any order.**
```sql
select unique_id,name from EmployeeUNI e1
right join Employees e2
on e1.id=e2.id
order by name
```

**2.Write an SQL query that reports the product_name, year, and price for each sale_id in the Sales table.
Return the resulting table in any order.**
```sql
select  product_name,year,price
from Sales s join Product p
on s.product_id=p.product_id
```

**3.Write a SQL query to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.
Return the result table sorted in any order.**
```sql
select customer_id,count(customer_id) as count_no_trans from visits v
left join transactions t
on v.visit_id=t.visit_id
where t.transaction_id is null
group by customer_id
```

**4.Write an SQL query to find all dates' Id with higher temperatures compared to its previous dates (yesterday).
Return the result table in any order.**
```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2 ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature > w2.temperature
```

**6.Write an SQL query to report the name and bonus amount of each employee with a bonus less than 1000.
Return the result table in any order.**
```sql
select name,bonus from Employee e
left join Bonus b
on e.empID=b.empID
where bonus<1000 or bonus is null
```

**7.Write an SQL query to find the number of times each student attended each exam.
Return the result table ordered by student_id and subject_name.**
```sql
select s.student_id,s.student_name,t.subject_name,count(e.subject_name) attended_exams
from Students s
cross join Subjects t
left join Examinations e
on s.student_id=e.student_id and t.subject_name=e.subject_name
group by 3,2,1
order by student_id,subject_name
```

**8.Write an SQL query to report the managers with at least five direct reports.
Return the result table in any order.**
```sql
with c as 
(select managerId from employee
where managerId!='null'
group by 1
having count(managerId)>=5)

select name from Employee
where id in (select * from c)
```

**9.The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to two decimal places.
Write an SQL query to find the confirmation rate of each user.
Return the result table in any order.**
```sql
with c as
(select s.user_id,action from Signups s
left join Confirmations c
on s.user_id=c.user_id),

c1 as
(select user_id,sum(if(action="confirmed",1,0))/count(action) confirmation_rate  from c
group by 1)

select user_id,round(if(confirmation_rate is null,0,confirmation_rate),2) confirmation_rate  from c1
```
### Hope you like the solutions
