# Comments Histogram

This question was asked by: Facebook.

Source: [Interview Query](https://www.interviewquery.com/)

## Description

`users` table

| columns   | type     |
| --------- | -------- |
| id        | int      |
| name      | varchar  |
| joined_at | datetime |
| city_id   | int      |
| device    | int      |

`user_comments` table

| columns    | type     |
| ---------- | -------- |
| user_id    | int      |
| body       | text     |
| created_at | datetime |

Write a SQL query to create a histogram of number of comments per user in the month of January 2019. Assume bin buckets class intervals of one.

## Solution 1

```sql
# MySql v5.7
CREATE TABLE users
  (
     id        INT,
     name      VARCHAR(20),
     joined_at DATETIME,
     city_id   INT,
     device    INT
  );

CREATE TABLE user_comments
  (
     user_id    INT,
     body       TEXT,
     created_at DATETIME
  );

INSERT INTO users
VALUES
      (1,'A', '2017-01-01', 100, 234),
      (2,'B', '2017-01-01', 100, 234),
      (3,'C', '2017-01-01', 100, 234),
      (4,'D', '2017-01-01', 100, 234),
      (5,'E', '2017-01-01', 100, 234),
      (6,'F', '2017-01-01', 100, 234),
      (7,'G', '2017-01-01', 100, 234);

INSERT INTO user_comments
VALUES
      (1,'A comment!', '2019-01-19'),
      (1,'A comment!', '2019-01-19'),
      (1,'A comment!', '2019-01-19'),
      (1,'A comment!', '2019-01-19'),
      (1,'A comment!', '2019-01-19'),
      (1,'A comment!', '2019-01-19'),
      (1,'A comment!', '2019-01-19'),
      (2,'A comment!', '2019-01-19'),
      (2,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (3,'A comment!', '2019-01-19'),
      (6,'A comment!', '2019-01-19'),
      (6,'A comment!', '2019-01-19'),
      (6,'A comment!', '2019-01-19'),
      (6,'A comment!', '2019-01-19'),
      (6,'A comment!', '2019-01-19'),
      (6,'A comment!', '2019-01-19');
```

```sql
# MySql v5.7
SELECT name,
             count(created_at)
FROM   users
       LEFT JOIN user_comments
              ON id = user_id
WHERE  ( Month(created_at) = 1
         AND Year(created_at) = 2019 )
        OR created_at IS NULL
GROUP  BY name;
```

## Solution 2

As provided by [Interview Query](https://www.interviewquery.com/)

```sql
WITH hist AS (
    SELECT users.id, COUNT(user_comments.user_id) AS comment_count
    FROM users
    LEFT JOIN user_comments
        ON users.id = user_comments.user_id
    WHERE created_at BETWEEN '2019-01-01' AND '2019-01-31'
    GROUP BY 1
)

SELECT comment_count, COUNT(*) AS frequency
FROM hist
GROUP BY 1
```
