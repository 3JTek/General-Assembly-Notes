# SQL Joins

There are several types of join we can make with data in a relational database:

* `INNER JOIN`
* `LEFT JOIN`
* `RIGHT JOIN`
* `FULL JOIN`

Before we look at them in detail, let's establish a data set that we'll be working with, and the relationships between them. For this example we'll use a book store:

* A book belongs to **one** author
* An author can have **many** books

```sql
-- create authors table
CREATE TABLE authors (
  id SERIAL PRIMARY KEY,
  firstname CHAR(15) NOT NULL,
  initials CHAR(15),
  lastname CHAR(15) NOT NULL
);

-- create books table
CREATE TABLE books (
  id SERIAL PRIMARY KEY,
  title CHAR(40) NOT NULL,
  author_id INT,
  genre CHAR(20)
);

-- add author data
INSERT INTO authors (firstname, initials, lastname) VALUES
  ('George', 'R. R.', 'Martin'),
  ('Terry', NULL, 'Pratchett'),
  ('J.', 'R. R', 'Tolkien'),
  ('Ursula', NULL, 'Le Guin');

-- add book data
INSERT INTO books (title, author_id, genre) VALUES
  ('Equal Rites', 2, 'Fantasy'),
  ('The Return of the King', 3, 'Fantasy'),
  ('A Storm of Swords 1: Steel & Snow', 1, 'Fantasy'),
  ('Tehanu', NULL, 'Fantasy'),
  ('Sorcery', 2, 'Fantasy'),
  ('The Tombs of Atuan', NULL, 'Fantasy'),
  ('A Clash of Kings', 1, 'Fantasy'),
  ('The Light Fantastic', 2, 'Fantasy'),
  ('The Fellowship of the Ring', 3, 'Fantasy');
```

You will notice that not all books have an author ID, this will be important as we look at the different joins available.

When we make a join, we use the `ON` clause of the SQL statement to indicate how to establish the join. Normally this means matching an ID in both tables. We can join the books and authors table via the author id like so:

```sql
SELECT books.id, title, genre, firstname, initials, lastname FROM books JOIN authors ON books.author_id = authors.id;
```

Which would return the following data:

```
 id |                  title                   |        genre         |    firstname    |    initials     |    lastname     
----+------------------------------------------+----------------------+-----------------+-----------------+-----------------
  1 | Equal Rites                              | Fantasy              | Terry           |                 | Pratchett      
  2 | The Return of the King                   | Fantasy              | J.              | R. R            | Tolkien        
  3 | A Storm of Swords 1: Steel & Snow        | Fantasy              | George          | R. R.           | Martin         
  5 | Sorcery                                  | Fantasy              | Terry           |                 | Pratchett      
  7 | A Clash of Kings                         | Fantasy              | George          | R. R.           | Martin         
  8 | The Light Fantastic                      | Fantasy              | Terry           |                 | Pratchett      
  9 | The Fellowship of the Ring               | Fantasy              | J.              | R. R            | Tolkien        
```

Looking at the data returned, we have joined together two tables. The `books` table can be referred to as the **left** table (because it appears first in the SQL statement), and the `authors` table can be referred to as the **right** table, because it appears second in the SQL statement.

You'll notice that there are two books and one author missing from the table. This is because the two books in question do not have an `author_id` so the join cannot be established for those books. The author is missing because there are no books with her `author_id` so again, the join cannot be established.

## `INNER JOIN`

![inner-join](https://media.git.generalassemb.ly/user/15120/files/ee41ef00-0838-11e9-84b7-11e8b67b08bd)

An `INNER JOIN` is the same as a `JOIN`. It will return all the data where the join can be established. Only data where the join can be made is returned.

## `LEFT JOIN`

![left-join](https://media.git.generalassemb.ly/user/15120/files/ee41ef00-0838-11e9-8bd5-bee03d12bc93)

A `LEFT JOIN` will return all the data from the **left** table (the first one in the SQL statement), and only the data that can be joined to it from the **right** table:

```sql
-- books is the left table, authors the right
SELECT books.id, title, genre, firstname, initials, lastname FROM books LEFT JOIN authors ON books.author_id = authors.id;
```

```
 id |                  title                   |        genre         |    firstname    |    initials     |    lastname     
----+------------------------------------------+----------------------+-----------------+-----------------+-----------------
  1 | Equal Rites                              | Fantasy              | Terry           |                 | Pratchett      
  2 | The Return of the King                   | Fantasy              | J.              | R. R            | Tolkien        
  3 | A Storm of Swords 1: Steel & Snow        | Fantasy              | George          | R. R.           | Martin         
  4 | Tehanu                                   | Fantasy              |                 |                 |
  5 | Sorcery                                  | Fantasy              | Terry           |                 | Pratchett      
  6 | The Tombs of Atuan                       | Fantasy              |                 |                 |
  7 | A Clash of Kings                         | Fantasy              | George          | R. R.           | Martin         
  8 | The Light Fantastic                      | Fantasy              | Terry           |                 | Pratchett      
  9 | The Fellowship of the Ring               | Fantasy              | J.              | R. R            | Tolkien        
```

Since the `books` table appears first in the SQL statement, it becomes the **left** table. Notice that all the books are present now, even though two are missing an author.

Let's make the `authors` table the left table, and the `books` the right one:

```sql
-- authors is now the left table because it appears first in the SQL statement
SELECT books.id, title, genre, firstname, initials, lastname FROM authors LEFT JOIN books ON books.author_id = authors.id;
```

```
 id |                  title                   |        genre         |    firstname    |    initials     |    lastname     
----+------------------------------------------+----------------------+-----------------+-----------------+-----------------
  1 | Equal Rites                              | Fantasy              | Terry           |                 | Pratchett      
  2 | The Return of the King                   | Fantasy              | J.              | R. R            | Tolkien        
  3 | A Storm of Swords 1: Steel & Snow        | Fantasy              | George          | R. R.           | Martin         
  5 | Sorcery                                  | Fantasy              | Terry           |                 | Pratchett      
  7 | A Clash of Kings                         | Fantasy              | George          | R. R.           | Martin         
  8 | The Light Fantastic                      | Fantasy              | Terry           |                 | Pratchett      
  9 | The Fellowship of the Ring               | Fantasy              | J.              | R. R            | Tolkien        
    |                                          |                      | Ursula          |                 | Le Guin        
```

Even though the data appears in the same order, because the `authors` table is now the **left** table, all of the authors are present, but the two books without an `author_id` are missing.

## `RIGHT JOIN`

![right-join](https://media.git.generalassemb.ly/user/15120/files/ee41ef00-0838-11e9-822a-340e1d32d3ad)

As its name suggests a `RIGHT JOIN` is the same as a `LEFT JOIN` except it favours the data from the **right** table:

```sql
-- books is the left table, and authors the right
SELECT books.id, title, genre, firstname, initials, lastname FROM books RIGHT JOIN authors ON books.author_id = authors.id;
```

```
 id |                  title                   |        genre         |    firstname    |    initials     |    lastname     
----+------------------------------------------+----------------------+-----------------+-----------------+-----------------
  1 | Equal Rites                              | Fantasy              | Terry           |                 | Pratchett      
  2 | The Return of the King                   | Fantasy              | J.              | R. R            | Tolkien        
  3 | A Storm of Swords 1: Steel & Snow        | Fantasy              | George          | R. R.           | Martin         
  5 | Sorcery                                  | Fantasy              | Terry           |                 | Pratchett      
  7 | A Clash of Kings                         | Fantasy              | George          | R. R.           | Martin         
  8 | The Light Fantastic                      | Fantasy              | Terry           |                 | Pratchett      
  9 | The Fellowship of the Ring               | Fantasy              | J.              | R. R            | Tolkien        
    |                                          |                      | Ursula          |                 | Le Guin        
```

Here `books` is the **left** table and `authors` the **right**. As you can see all the authors are present, but not all the books.

## `FULL JOIN`

![full-join](https://media.git.generalassemb.ly/user/15120/files/ee41ef00-0838-11e9-9d35-ea0e17d15e34)

A `FULL JOIN` or `FULL OUTER JOIN`, will ensure all the data from both the **left** and **right** tables are present, even where a join cannot be established:

```sql
SELECT books.id, title, genre, firstname, initials, lastname FROM books FULL JOIN authors ON books.author_id = authors.id;
```

```
 id |                  title                   |        genre         |    firstname    |    initials     |    lastname     
----+------------------------------------------+----------------------+-----------------+-----------------+-----------------
  1 | Equal Rites                              | Fantasy              | Terry           |                 | Pratchett      
  2 | The Return of the King                   | Fantasy              | J.              | R. R            | Tolkien        
  3 | A Storm of Swords 1: Steel & Snow        | Fantasy              | George          | R. R.           | Martin         
  4 | Tehanu                                   | Fantasy              |                 |                 |
  6 | The Tombs of Atuan                       | Fantasy              |                 |                 |
  7 | A Clash of Kings                         | Fantasy              | George          | R. R.           | Martin         
  8 | The Light Fantastic                      | Fantasy              | Terry           |                 | Pratchett      
  9 | The Fellowship of the Ring               | Fantasy              | J.              | R. R            | Tolkien        
  5 | Sorcery                                  | Fantasy              | Terry           |                 | Pratchett      
    |                                          |                      | Ursula          |                 | Le Guin        
```

All the data is now present.

## Further reading

* [SQL Joins Explained - SQL-Join.com](http://www.sql-join.com/sql-join-types/)
* [SQL | Join (Inner, Left, Right and Full Joins) - Geeks for Geeks](https://www.geeksforgeeks.org/sql-join-set-1-inner-left-right-and-full-joins/)
