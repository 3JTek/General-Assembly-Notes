# NoSQL Databases

The most common database is the grandfather of them all SQL, which stands for _Structured Query Language_. It describes the language used to add, edit and delete data from a database. SQL databases consist of a set of tables \(similar to a spreadsheet\).

NoSQL on the other hand describes a new breed of databases which are JavaScript based and are closer to a collection of arrays containing objects of data. Ultimately a database is a data store which can hold data in a specific, searchable way.

Almost every website has a database attached to it which contains the site's content. This allows the content to be _dynamic_, i.e. it can be changed. Commonly this is done with a _Content Management System_ or CMS: a user interface which allows an end user to modify the database without having to have prior coding knowledge.

Each noSQL database is made up of _collections_ of data. A collection is basically an array which contains the data. Each item in the collection can be considered an object. These items are referred to as _documents_ or _records_.

## MongoDB

We shall be using MongoDB as our noSQL database of choice on this course. We can interact with it directly from the terminal using the `mongo` command.

A common error when trying to connect to mongo is the following:

```
MongoDB shell version v3.6.2
connecting to: mongodb://127.0.0.1:27017
2018-02-13T11:27:58.883+0000 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll), reason: Connection refused
2018-02-13T11:27:58.887+0000 E QUERY    [thread1] Error: couldn't connect to server 127.0.0.1:27017, connection attempt failed :
connect@src/mongo/shell/mongo.js:251:13
@(connect):1:6
exception: connect failed
```

It is saying that it could not connect to the database. To fix this we must ensure the database instance is running and listening for connections. To do that we should open a new tab and run `mongod`. Your terminal screen will fill with logs, the last line should read:

```
2018-02-13T11:30:21.214+0000 I NETWORK  [initandlisten] waiting for connections on port 27017
```

Now you know that your database is up and running. Back on the original tab, run the `mongo` command, again you will see a bunch of logs, at the bottom you should see the following:

```
2018-02-13T11:30:21.192+0000 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
>
```

You are now ready to interact with your database.

## Creating a database

To create a new database we just need to tell mongo to use a non-existent database:

```sh
use myDB
```

The database doesn't actually exists at this moment in time, mongo will make it as soon as we actually try to add data.

## CRUD

CRUD stands for **Create, Read, Update, Delete**, and describes the main actions that can be performed on a database. Let's look at the syntax of each action:

### Create

```
db.trainers.insert({ name: 'Campus', brand: 'Adidas', price: 79.99, color: 'navy' });
db.trainers.insert([{
    name: 'Air Force 1',
    brand: 'Nike',
    price: 129.99,
    color: 'white'
  }, {
    name: 'All Star',
    brand: 'Converse',
    price: 89.99,
    color: 'navy'
  }])
```

Here we are inserting records into the `trainers` collection. Again the `trainers` collection did not exist until we put data into it.

Note that we can insert a single record, or an array of records.

### Read

We can get data from our database using `find`:

```
db.trainers.find({ color: 'navy' })
```

This will return all the navy trainers.

```
db.trainers.findOne({ color: 'navy' })
```

This will find the first navy trainer.

### Update

In order to update a record, we first need to find it, then pass our updates:

```
db.trainers.update({ name: 'Campus' }, { $set: { name: 'Samba' } })
```

Here we found a single record with a `name` property of `Campus` and `$set` it to `Samba`.

It's important that if we do not use `$set` we will replace the record:

```
db.trainers.update({ name: 'Campus' }, { name: 'Samba' })
```

The record is now literally `{ name: 'Samba' }`, and no longer has `price`, `brand` or `color` properties.

#### Updating multiple records

```
db.trainers.update({ color: 'navy' }, { $set: { color: 'red' } }, { multi: true })
```

By default update will only update a single record. If we want to update more that one at a time, we need to pass a settings object as the third argument with a `multi` property of `true`.

### Delete

`delete` is a reserved word in JavaScript, so in order to delete a record, we use `remove`:

```
db.trainers.remove({ color: 'navy' })
```

**This will remove multiple records, so be careful!**

It's worth noting that **there is no undo** with a database. When it's gone, it's gone!

## ObjectId

When we insert a record into a noSQL database it is given a unique ID. This is vital to help manage the database and so that we can _associate_ records to each other.

It's important to note two things about the `ObjectId`:

1. It is not a string, but an object with its own methods. This means we cannot compare it directly to a string of the same value
2. It is stored in an `_id` property, not `id`, which can trip you up at first

## Associating data

There are two ways of associating \(or linking\) data in a noSQL database: _referencing_ or _embedding_.

### Referencing

We want to keep the amount of data to a minimum in any database, so rather than repeating data, we can reference it. Let's look at a simple example, students in a cohort at GA.

We will have two collections, one for students, and one for the cohort:

```
db.cohorts.insert([{ name: 'WDI32', campus: 'London' }, { name: 'WDI33', campus: 'London' }])
db.students.insert([{ name: 'David Grey' }, { name: 'Caroline Quentin' }, { name: 'Jessica Ennis' }, { name: 'Jools Holland' }])
```

David Grey and Caroline Quentin will be attending WD32, whereas Jessica Ennis and Jools Holland will be attending WDI33. We can make that link with a reference to the relevant `ObjectId`. Let's give each student a `cohort` property with the relevant `ObjectId`:

```
const wdi32 = db.cohorts.findOne({ name: 'WDI32' });
const wdi33 = db.cohorts.findOne({ name: 'WDI33' });

db.students.update({ name: 'David Grey' }, { $set: { cohort: wdi32._id } })
db.students.update({ name: 'Caroline Quentin' }, { $set: { cohort: wdi32._id } })
db.students.update({ name: 'Jessica Ennis' }, { $set: { cohort: wdi33._id } })
db.students.update({ name: 'Jools Holland' }, { $set: { cohort: wdi33._id } })
```

In this way we have created a _reference_ to another record in the database. The `cohort` property does not actually contain the cohort information, but instead reference of where to find the record. This means that we don't have to duplicate the cohort information across the database, which saves space.

If the cohort information is modified then that change will be reflected for all associated students.

### Embedding

With embedding we can store data inside a record. A good example would be notes on a student. Let's say we want to add some notes to Jessica Ennis. Rather than keeping these notes in a separate collection and referencing them, we can _embed_ them directly on her record:

```
db.students.update({ name: 'Jessica Ennis' }, { $set: { notes: ['Enrolled Feb 2018'] } })
db.students.update({ name: 'Jessica Ennis' }, { $push: { notes: 'Completed pre-work' } })
```

This is a good way to store information specific to a record since the data is on the record, so does not require a database lookup to find it. Also, if we were to delete Jessica Ennis' record, we would also delete all the associated notes.

### Overview

| **Embedded** | **Referenced** |
| --- | --- |
| Must be specific to a single record | Can be linked to multiple records |
| Requires no lookup | Requires an extra lookup |
| Simpler | More complex |

## Further reading

* [SQL vs NoSQL: The Differences](https://www.sitepoint.com/sql-vs-nosql-differences/)
* [MongoDB Cheatsheet](https://blog.codecentric.de/files/2012/12/MongoDB-CheatSheet-v1_0.pdf)
* [The MongoDB Tutorial](https://www.hacksparrow.com/the-mongodb-tutorial.html)



