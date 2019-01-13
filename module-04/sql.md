# SQL

SQL stands for _Structured Query Language_. It is a language that is used query and modify a relational database. But what is a relational database?

## Relational databases

If noSQL is similar to a collection of arrays of objects, a relation database is more similar to a set of Excel spreadsheets.

A spreadsheet is a table, the columns describe the data that the table can hold, and each row is a single set of those data attributes.

Let's look at an example. Imagine we want to store information about cars in a database. With noSQL we would store a collection of car records, maybe something like this:

```js
[{
  _id: ObjectID('5bf6cdc9c01645611b25d2ae'),
  make: 'Ford',
  model: 'Mustang GT',
  doors: 2,
  seats: 2,
  license: 'GD12 6YT',
  color: 'red'
}, {
  _id: ObjectID('5bf6cdc9c01645611b25d2bf'),
  make: 'Fiat',
  model: 'Uno',
  doors: 5,
  seats: 5,
  license: 'VT12 8YG',
  color: 'white'
}]
```

If we wanted to store the same information in a relational database it might look something like this:

| id | make | model | doors | seats | license | color |
|----|------|-------|-------|-------|---------|-------|
| 1 | Ford | Mustang GT | 2 | 2 | GD12 6YT | red |
| 2 | Fiat | Uno | 5 | 5 | VT12 8YG | white |

## Using SQL

So now we have a little context, let's look at how we can use SQL to create and manage a simple database.

Although SQL is a standardised language it does come in different _flavours_. On this course we will be using a relational database management system called PostgreSQL. There are many others out there, all with their nuances, but broadly speaking they will all accept the same SQL commands.

## Creating tables

To create the table above, we would need to write the following SQL command:

```sql
CREATE TABLE cars (
  id SERIAL PRIMARY KEY,
  make CHAR(40) NOT NULL,
  model VARCHAR NOT NULL,
  doors INT(1),
  seats INT(1),
  license CHAR(8) NOT NULL,
  color CHAR(10)
);
```

> **Note:** The semi-colon is required, without it the command will not run!

Here we are creating a table called _cars_. It should have 7 columns, each with a name and a specific type of data it can handle:

| data type | description |
|-----------|-------------|
| `SERIAL` | Not strictly a data type as such, `SERIAL` indicates an auto-incrementing integer starting at 1 |
| `CHAR(max)` | A string of characters up to a maximum of `max` |
| `VARCHAR` | A string of unspecified length. `CHAR` is more performant but `VARCHAR` is more flexible |
| `INT(length)` | An integer up to `length` numbers |

Other attributes can be added to the fields:

| attribute | description |
|-----------|-------------|
| `PRIMARY KEY` | This specifies this column as being a unique identifier for the row. Each entry in this column must be unique. |
| `NOT NULL` | This specifies that each row must have a value in this column. It stipulates that this column is not optional |

## Modifying tables

If we want to moidfy a table we can do it like so:

```sql
-- change the make column to be VARCHAR
ALTER TABLE cars ALTER COLUMN make TYPE VARCHAR;

-- add a max_speed column
ALTER TABLE cars ADD COLUMN max_speed INT(3);

-- remove the doors column
ALTER TABLE cars DROP COLUMN doors;
```

## Deleting tables

To delete a table, we can use the following command:

```sql
DROP TABLE cars;
```

>**Note**: There is no undo! Make sure you have a backup of your data before running destructive commands!

## Creating data _INSERT_

Now that we have created our cars table, we can add some data to it:

```sql
INSERT INTO cars (make, model, doors, seats, license, color) VALUES
('Ford', 'Mustang', 2, 2, 'GD12 6YT', 'red'),
('Fiat', 'Uno', 5, 5, 'VT12 8YG', 'white');
```

## Reading data _SELECT_

We can now select the data from our table in very specific ways:

```sql
-- select all the data from the cars table
SELECT * FROM cars;

-- select just the make and model
SELECT make, model FROM cars;

-- select all cars with less than 4 seats
SELECT * FROM cars WHERE seats < 4;

-- select all cars whose license plate contains the letter Y
-- the % is a wildcard, similar to * in the terminal
SELECT make, model, license FROM cars WHERE license LIKE '%Y%'
```

## Updating the data _UPDATE_

**BE CAREFUL HERE**, there is no undo! Make sure you have a backup of your data before making destructive changes to the dataset. It's important to always add a `WHERE` clause otherwise all the data will be modified!

```sql
-- set ALL cars to be Renault Meganes
UPDATE cars SET make = 'Renault', model = 'Megane';

-- set the car with id of 1 to be a Ferrari Countach
UPDATE cars SET make = 'Ferrari', model = 'Countach' WHERE id = 1;
```

## Deleting data _DELETE_

Again **BE CAREFUL HERE**, there is no undo! Make sure you have a backup of your data before making destructive changes to the dataset! It's important to always add a `WHERE` clause otherwise all the data will be modified!

```sql
-- delete all Fiats
DELETE FROM cars WHERE make = 'Fiat'

-- delete all the cars
DELETE FROM cars
```

## Further reading

* [SQL - Wikipedia](https://en.wikipedia.org/wiki/SQL)
* [PostgreSQL Cheat Sheet - PostgreSQL Tutorial](http://www.postgresqltutorial.com/wp-content/uploads/2018/03/PostgreSQL-Cheat-Sheet.pdf)
