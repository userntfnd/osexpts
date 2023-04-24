```
CREATE TABLE books (
   id INT PRIMARY KEY,
   book_name VARCHAR(255),
   author_name VARCHAR(255)
);

```

```
INSERT INTO books (id, book_name, author_name)
VALUES
  (1, 'To Kill a Mockingbird', 'Harper Lee'),
  (2, 'The Great Gatsby', 'F. Scott Fitzgerald'),
  (3, '1984', 'George Orwell'),
  (4, 'Pride and Prejudice', 'Jane Austen');

```

```
DELETE FROM books WHERE id = 3 AND book_name = '1984' AND author_name = 'George Orwell';

```

```
SELECT * FROM books ORDER BY id ASC;
```

```
INSERT INTO books (id, book_name, author_name)
VALUES (5, 'Rich Dad Poor Dad', 'Robert Kiyosaki');
```

```
CREATE TABLE magazines (
   id INT PRIMARY KEY,
   magazine_name VARCHAR(255),
   publisher_name VARCHAR(255)
);

```


```
INSERT INTO magazines (id, magazine_name, publisher_name)
VALUES (1, 'National Geographic', 'National Geographic Society');

```

```
SELECT * FROM books INNER JOIN magazines ON books.id = magazines.id;

```

```

SELECT * FROM books LEFT JOIN magazines ON books.id = magazines.id;

```

```
CREATE TABLE employees (
   empid INT PRIMARY KEY,
   name VARCHAR(255),
   salary DECIMAL(10, 2),
   email VARCHAR(255),
   hire_date DATE
);

```


```
INSERT INTO employees (empid, name, salary, email, hire_date)
VALUES
  (1, 'John Smith', 50000.00, 'john.smith@example.com', '2022-01-01'),
  (2, 'Jane Doe', 75000.00, 'jane.doe@example.com', '2021-03-15'),
  (3, 'Bob Johnson', 60000.00, 'bob.johnson@example.com', '2022-02-28'),
  (4, 'Alice Kim', 80000.00, 'alice.kim@example.com', '2020-12-01'),
  (5, 'Tom Lee', 65000.00, 'tom.lee@example.com', '2021-07-10');
```


```
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

```


```
SELECT name
FROM employees
WHERE YEAR(hire_date) = (SELECT YEAR(hire_date) FROM employees WHERE name = 'John Smith');
```

```
SELECT name, salary
FROM employees
WHERE YEAR(hire_date) = (SELECT YEAR(hire_date) FROM employees WHERE name = 'Jane Doe')
  AND salary = (SELECT salary FROM employees WHERE name = 'Jane Doe');

```


```
SELECT e1.name, e1.salary
FROM employees e1
WHERE salary = (
  SELECT MAX(salary)
  FROM employees e2
  WHERE e1.department = e2.department
);

```

```

```




