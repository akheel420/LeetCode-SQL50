### SELECT-QUESTIONS AND SOLUTIONS

**1.Write an SQL query to find the ids of products that are both low fat and recyclable.
Return the result table in any order.**

```sql
SELECT product_id 
FROM Products
WHERE low_fats="Y" AND recyclable="Y"; 
```

**2.Write an SQL query to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.
Return the result table in any order.**

```sql
with ctc as
(select tweet_id,length(content) as l from tweets)

select tweet_id from ctc
where l>15
```

**3.Write an SQL query to find all the authors that viewed at least one of their own articles.
Return the result table sorted by id in ascending order.**

```sql
select distinct(author_id) as id from views
where author_id=viewer_id
order by 1 
```

**4.A country is big if:
it has an area of at least three million (i.e., 3000000 km2), or
it has a population of at least twenty-five million (i.e., 25000000).
Write an SQL query to report the name, population, and area of the big countries.
Return the result table in any order.**

```sql
/* Write your T-SQL query statement below */
select name,population,area from World
where area>=3000000 or population>=25000000;
```

**5.Write an SQL query to report the names of the customer that are not referred by the customer with id = 2.
Return the result table in any order.**
```sql
/* Write your T-SQL query statement below */
select name from Customer
where referee_id!=2 or referee_id is null;
```


