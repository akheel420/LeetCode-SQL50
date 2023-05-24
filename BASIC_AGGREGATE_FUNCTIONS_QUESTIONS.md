## BASIC AGGREGATE FUNCTIONS QUESTIONS AND SOLUTIONS

**1.Write an SQL query to report the movies with an odd-numbered ID and a description that is not "boring".
Return the result table ordered by rating in descending order.**
```sql
select * from Cinema 
where id%2!=0 and description!='boring'
order by rating desc;
```

**2.Write an SQL query to find the average selling price for each product. average_price should be rounded to 2 decimal places.
Return the result table in any order.**
```sql
with c as
(select p.product_id,units,price,price*units c from prices p
join unitssold u
on p.product_id=u.product_id and u.purchase_date between p.start_date and p.end_date)

select product_id,round((sum(c)/sum(units)),2) average_price from c
group by 1
```

**3.Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.
Return the result table in any order.**
```sql
# Write your MySQL query statement below
with c as
(select project_id,experience_years,p.employee_id 
from project p join employee e
on p.employee_id=e.employee_id)

select project_id,round(sum(experience_years)/count(employee_id),2) average_years 
from c
group by 1
```

**4.Write an SQL query to find the percentage of the users registered in each contest rounded to two decimals.
Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.**
```sql
select contest_id,round((count(user_id)/(select count(*) from users))*100,2) percentage from Register
group by 1
order by percentage desc,contest_id 
```

**5.We define query quality as:
The average of the ratio between query rating and its position.
We also define poor query percentage as:
The percentage of all queries with rating less than 3.
Write an SQL query to find each query_name, the quality and poor_query_percentage.
Both quality and poor_query_percentage should be rounded to 2 decimal places.
Return the result table in any order.**
```sql
# Write your MySQL query statement below
select query_name,round((sum(if(rating<3,1,0))/count(query_name))*100,2) quality,
round(sum(rating/position)/count(query_name),2) poor_query_percentage
from Queries
group by 1
```

**6.Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.
Return the result table in any order.**
```sql
# Write your MySQL query statement below
select date_format(trans_date, '%Y-%m') month,country,count(trans_date) trans_count,sum(if(state='approved',1,0)) approved_count,sum(amount) trans_total_amount,sum(if(state='approved',amount,0)) approved_total_amount
from Transactions
group by 1,2
```

**7.If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.
The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.
Write an SQL query to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.**
```sql
with c as 
(select * from 
(select  customer_id,order_date,customer_pref_delivery_date,dense_rank() over(partition by customer_id order by order_date) r from Delivery) t
where r=1)

select round((sum(if(order_date=customer_pref_delivery_date,1,0))/count(*))*100,2) immediate_percentage from c
```

**8.Write an SQL query to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. 
In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.**
```sql
with cte1 as (
    select player_id, DATE_ADD(min(event_date), INTERVAL 1 DAY) tomorrow
    from Activity
    group by player_id)

select round(count(distinct a.player_id)/(select count(distinct player_id) from Activity),2) fraction
from Activity a
join cte1 c on c.player_id=a.player_id and c.tomorrow=a.event_date
```
