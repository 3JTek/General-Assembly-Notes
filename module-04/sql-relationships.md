# SQL Relationships

Now we've seen how to make a simple database table, however the real power of a _relational database_, as its name suggests is how we can build relationships _between_ tables of data.

With noSQL databases, we either _embed_ or _reference_ data, but with a SQL database we make _joins_ between tables.

Let's take a look at a typical example of an embedded and referenced noSQL database, and look at how we could model this with SQL:

Let's looks at a cheeses, comments and categories:

```js

const categorySchema = new mongoose.Schema({
  name: { type: String, minlength: 2, required: true }
});

const cheeseSchema = new mongoose.Schema({
  name: { type: String, minlength: 2, required: true },
  origin: { type: String, minlength: 2, required: true },
  tastingNotes: { type: String, maxlength: 360, required: true },
  image: { type: String, pattern: /^https?:\/\/.+/ },
  comments: [{
    content: { type: String, required: true } // embedded comment schema
  }]
  category: [{ type: mongoose.Schema.ObjectId, ref: 'Category', required: true }], // referenced categories
});
```

Here we have the following relationships:

* A cheese can have many comments
* A comment can only belong to one cheese
* A cheese can belong to many categories
* A category can have many cheeses

## ERD

With SQL we can express this with the following diagram:

![erd](https://media.git.generalassemb.ly/user/15120/files/9357bb80-0796-11e9-82a0-737dbcf72168)

This is known as an _Entity Relationship Diagram_ or ERD.

### Table

Each block represents a table, with the columns it contains:

![table](https://media.git.generalassemb.ly/user/15120/files/e3cb1b00-078e-11e9-9852-9db8d2b10122)

### Relationship

The lines between the tables represent a relationship between the tables:

![relationship](https://media.git.generalassemb.ly/user/15120/files/19243880-0790-11e9-8dca-54979eb1ecfb)

### Type of relationship

The ends of the line indicate the type of relationship. These are sometimes called _crows feet_ because the many-to-many line looks a bit like a crows foot.

![crows feet](https://media.git.generalassemb.ly/user/15120/files/e8440380-078f-11e9-9183-1af4e402cda5)

## SQL Implementation

Now that we understand our relationships, we can begin to build out our database. Here's the SQL commands to build the tables:

```sql
-- cheeses table
CREATE TABLE cheeses (
  id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL,
  origin VARCHAR NOT NULL,
  tasting_notes TEXT,
  image VARCHAR
);

-- comments table
CREATE TABLE comments (
  id SERIAL PRIMARY KEY,
  content TEXT NOT NULL,
  cheese_id INT NOT NULL
);

-- category table
CREATE TABLE categories (
  id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL
);
```

Before we move forward, let's add some data to our tables:

```sql
-- add category data
INSERT INTO categories (name) VALUES
  ('soft'),
  ('hard'),
  ('blue');

-- add cheese data
INSERT INTO cheeses (name, origin) VALUES
  ('Brie', 'France'),
  ('Mozzarella', 'Italy'),
  ('Edam', 'Netherlands'),
  ('Cheddar', 'English'),
  ('Gorgonzola', 'Italy');

-- add comment data
INSERT INTO comments (content, cheese_id) VALUES
  ('Not for me, mouldy nonsense!', 1),
  ('Great on pizza or in a salad', 2),
  ('Elegant and nutty', 3),
  ('Perfect for cheese on toast especially with a bit of Worcester sauce, yum', 4),
  ('An absolute classic!', 5),
  ('Bland', 2),
  ('Also bland', 3);
```

## One-to-many relationship

Remember the relationship between cheeses and comments:

* A cheese can have **many** comments
* A comment belongs to **one** cheese

You'll notice that where there is a one-to-many relationship we need to reference one table in the other. This is actually very similar to a reference in a SQL database.

Because the comment can only belong to one cheese, we add the ID of the cheese that it belongs to in the cheese table.

## Making joins

Now we can get all the comments for a specific cheese:

```sql
SELECT * FROM cheeses
JOIN comments ON cheeses.id = comments.cheese_id
WHERE cheeses.name = 'Mozzarella';
```

And this is the output:

```
 id |    name    | origin | tasting_notes | image | category_id | id |           content            | cheese_id
----+------------+--------+---------------+-------+-------------+----+------------------------------+-----------
  2 | Mozzarella | Italy  |               |       |           1 |  3 | Great on pizza or in a salad |         2
  2 | Mozzarella | Italy  |               |       |           1 |  6 | Bland                        |         2
```

As you can see the two tables have literally been joined together, matching the cheese's `id` to the comment's `cheese_id`. The cheese data is repeated, but we get both comments.

We can be a little less verbose, by requesting specific columns:

```sql
SELECT cheeses.id, name, origin, content AS comment FROM cheeses
JOIN comments ON cheeses.id = comments.cheese_id
WHERE cheeses.name = 'Mozzarella';
```

```
 id |    name    | origin |           comment
----+------------+--------+------------------------------
  2 | Mozzarella | Italy  | Great on pizza or in a salad
  2 | Mozzarella | Italy  | Bland
```

Notice that with the `id` field we needed to specify the `cheeses.id` so as not to be confused wit the comment's `id`. We've also renamed the `content` field to `comment` for clarity.

## Many-to-many relationships

Remember the relationship between cheeses and categories:

* A cheese can belong to **many** categories
* A category can have **many** cheeses

Many-to-many relationships are a little bit more complex to create in SQL, because they require an intermediary table, called a _join table_ to be created.

We cannot store a cheese ID in the category, because that category could then only have one cheese, and we can't store a category ID on the cheese because then the cheese could only belong to one category.

Instead we need to create a _join table_ which stores references to both tables. Let's make that now:

```sql
-- create a categories_cheeses join table
CREATE TABLE categories_cheeses (
  category_id INT NOT NULL,
  cheese_id INT NOT NULL
);
```

Let's remind ourselves of the data currently in the database:

#### Cheeses
```
 id |    name
----+------------
  1 | Brie
  2 | Mozzarella
  3 | Edam
  4 | Cheddar
  5 | Gorgonzola
```

#### Categories
```
 id | name
----+------
  1 | soft
  2 | hard
  3 | blue
```

Ok, let's put our cheeses into their categories:

```sql
INSERT INTO categories_cheeses (category_id, cheese_id) VALUES
  (1, 1), -- soft, Bire
  (1, 2), -- soft, Mozzarella
  (2, 3), -- hard, Edam
  (2, 4), -- hard, Cheddar
  (1, 5), -- soft, Gorgonzola
  (3, 5); -- blue, Gorgonzola
```

Ok, let's check the data has been added correctly:

```sql
-- get all soft cheeses
SELECT cheeses.id, cheeses.name, origin, categories.name AS category FROM cheeses
JOIN categories_cheeses ON cheeses.id = categories_cheeses.cheese_id
JOIN categories ON categories.id = categories_cheeses.category_id
WHERE categories.name = 'soft';
```

```
 id |    name    | origin | category
----+------------+--------+----------
  1 | Brie       | France | soft
  2 | Mozzarella | Italy  | soft
```

```sql
-- get all the categories of Gorgonzola
SELECT cheeses.id, cheeses.name, origin, categories.name AS category FROM cheeses
JOIN categories_cheeses ON cheeses.id = categories_cheeses.cheese_id
JOIN categories ON categories.id = categories_cheeses.category_id
WHERE cheeses.name = 'Gorgonzola';
```

```
 id |    name    | origin | category
----+------------+--------+----------
  5 | Gorgonzola | Italy  | hard
  5 | Gorgonzola | Italy  | blue
```

> **Note:** Although a look up table is required when we build our database, it is not added to the ERD. The table is implied.

## Further reading

* [The 3 Types of Relationships in Database Design - database.guide](https://database.guide/the-3-types-of-relationships-in-database-design/)
* [ERD "Crows Foot" Relationship Symbols Cheat Sheet - Vivek M Chawla](https://www.vivekmchawla.com/erd-crows-foot-relationship-symbols-cheat-sheet/)
* [Draw.IO - Great for making ERD](https://draw.io)
