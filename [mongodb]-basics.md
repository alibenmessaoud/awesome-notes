## Overview

Document oriented db; provides high performance, availability; 

- DB: Physical container of a collection; Based on OS FS.
- Collection: Group of MongoDB documents (RDBMS/ table); Acollection exists within a single database; Schema collection is option; 
- Document: A set of KV pairs; Dynamic schema means that document in the same collection do not need the same structure; 

## Executables

The following table presents the MySQL/Oracle executables and the corresponding MongoDB executables.

|                 | MySQL/Oracle  | MongoDB                                     |
| --------------- | ------------- | ------------------------------------------- |
| Database Server | mysqld/oracle | [*mongod*](https://gist.github.com/mongod/) |
| Database Client | mysql/sqlplus | [*mongo*](https://gist.github.com/mongo/)   |

## Terminology and Concepts

The following table presents the various SQL terminology and concepts and the corresponding MongoDB terminology and concepts.

| SQL Terms/Concepts                                           | MongoDB Terms/Concepts                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| database                                                     | [*database*](https://gist.github.com/glossary/#term-database) |
| table                                                        | [*collection*](https://gist.github.com/glossary/#term-collection) |
| row                                                          | [*document*](https://gist.github.com/glossary/#term-document) or [*BSON*](https://gist.github.com/glossary/#term-bson) document |
| column                                                       | [*field*](https://gist.github.com/glossary/#term-field)      |
| index                                                        | [*index*](https://gist.github.com/glossary/#term-index)      |
| table joins                                                  | embedded documents and linking                               |
| primary keySpecify any unique column or column combination as primary key. | [*primary key*](https://gist.github.com/glossary/#term-primary-key)In MongoDB, the primary key is automatically set to the[*_id*](https://gist.github.com/glossary/#term-id) field. |
| aggregation (e.g. group by)                                  | aggregation frameworkSee the [*SQL to Aggregation Framework Mapping Chart*](https://gist.github.com/sql-aggregation-comparison/). |

## Examples 

The following table presents the various SQL statements and the corresponding MongoDB statements. The examples in the table assume the following conditions:

- The SQL examples assume a table named `users`.

- The MongoDB examples assume a collection named `users` that contain documents of the following prototype:

  ```
  {
    _id: ObjectID("509a8fb2f3f4948bd2f983a0"),
    user_id: "abc123",
    age: 55,
    status: 'A'
  }
  ```

### Create and Alter 

The following table presents the various SQL statements related to table-level actions and the corresponding MongoDB statements.

| SQL Schema Statements                                        | MongoDB Schema Statements                                    | Reference                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `CREATE TABLE users (     id MEDIUMINT NOT NULL         AUTO_INCREMENT,     user_id Varchar(30),     age Number,     status char(1),     PRIMARY KEY (id) ) ` | Implicitly created on first [`insert`](https://gist.github.com/method/db.collection.insert/#db.collection.insert) operation. The primary key `_id` is automatically added if `_id` field is not specified.`db.users.insert( {     user_id: "abc123",     age: 55,     status: "A"  } ) `However, you can also explicitly create a collection:`db.createCollection("users") ` | See [`insert()`](https://gist.github.com/method/db.collection.insert/#db.collection.insert) and[`createCollection()`](https://gist.github.com/method/db.createCollection/#db.createCollection)for more information. |
| `ALTER TABLE users ADD join_date DATETIME `                  | Collections do not describe or enforce the structure of the constituent documents. See the [Schema Design](http://www.mongodb.org/display/DOCS/Schema+Design) wiki page for more information. | See [`update()`](https://gist.github.com/method/db.collection.update/#db.collection.update) and[`$set`](https://gist.github.com/operators/#_S_set) for more information on changing the structure of documents in a collection. |
| `ALTER TABLE users DROP COLUMN join_date `                   | Collections do not describe or enforce the structure of the constituent documents. See the [Schema Design](http://www.mongodb.org/display/DOCS/Schema+Design) wiki page for more information. | See [`update()`](https://gist.github.com/method/db.collection.update/#db.collection.update) and[`$set`](https://gist.github.com/operators/#_S_set) for more information on changing the structure of documents in a collection. |
| `CREATE INDEX idx_user_id_asc ON users(user_id) `            | `db.users.ensureIndex( { user_id: 1 } ) `                    | See [`ensureIndex()`](https://gist.github.com/method/db.collection.ensureIndex/#db.collection.ensureIndex)and [*indexes*](https://gist.github.com/core/indexes/) for more information. |
| `CREATE INDEX        idx_user_id_asc_age_desc ON users(user_id, age DESC) ` | `db.users.ensureIndex( { user_id: 1, age: -1 } ) `           | See [`ensureIndex()`](https://gist.github.com/method/db.collection.ensureIndex/#db.collection.ensureIndex)and [*indexes*](https://gist.github.com/core/indexes/) for more information. |
| `DROP TABLE users `                                          | `db.users.drop() `                                           | See [`drop()`](https://gist.github.com/method/db.collection.drop/#db.collection.drop) for more information. |

### Insert 

The following table presents the various SQL statements related to inserting records into tables and the corresponding MongoDB statements.

| SQL INSERT Statements                                        | MongoDB insert() Statements                                  | Reference                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `INSERT INTO users(user_id,                   age,                   status) VALUES ("bcd001",         45,         "A") ` | `db.users.insert( {        user_id: "bcd001",        age: 45,        status: "A" } ) ` | See [`insert()`](https://gist.github.com/method/db.collection.insert/#db.collection.insert) for more information. |

### Select 

The following table presents the various SQL statements related to reading records from tables and the corresponding MongoDB statements.

| SQL SELECT Statements                                        | MongoDB find() Statements                                    | Reference                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `SELECT * FROM users `                                       | `db.users.find() `                                           | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)for more information. |
| `SELECT id, user_id, status FROM users `                     | `db.users.find(     { },     { user_id: 1, status: 1 } ) `   | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)for more information. |
| `SELECT user_id, status FROM users `                         | `db.users.find(     { },     { user_id: 1, status: 1, _id: 0 } ) ` | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)for more information. |
| `SELECT * FROM users WHERE status = "A" `                    | `db.users.find(     { status: "A" } ) `                      | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)for more information. |
| `SELECT user_id, status FROM users WHERE status = "A" `      | `db.users.find(     { status: "A" },     { user_id: 1, status: 1, _id: 0 } ) ` | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)for more information. |
| `SELECT * FROM users WHERE status != "A" `                   | `db.users.find(     { status: { $ne: "A" } } ) `             | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$ne`](https://gist.github.com/operators/#_S_ne) for more information. |
| `SELECT * FROM users WHERE status = "A" AND age = 50 `       | `db.users.find(     { status: "A",       age: 50 } ) `       | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$and`](https://gist.github.com/operators/#_S_and)for more information. |
| `SELECT * FROM users WHERE status = "A" OR age = 50 `        | `db.users.find(     { $or: [ { status: "A" } ,              { age: 50 } ] } ) ` | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$or`](https://gist.github.com/operators/#_S_or) for more information. |
| `SELECT * FROM users WHERE age > 25 `                        | `db.users.find(     { age: { $gt: 25 } } ) `                 | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$gt`](https://gist.github.com/operators/#_S_gt) for more information. |
| `SELECT * FROM users WHERE age < 25 `                        | `db.users.find(    { age: { $lt: 25 } } ) `                  | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$lt`](https://gist.github.com/operators/#_S_lt) for more information. |
| `SELECT * FROM users WHERE age > 25 AND   age <= 50 `        | `db.users.find(    { age: { $gt: 25, $lte: 50 } } ) `        | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find),[`$gt`](https://gist.github.com/operators/#_S_gt), and [`$lte`](https://gist.github.com/operators/#_S_lte) for more information. |
| `SELECT * FROM users WHERE user_id like "%bc%" `             | `db.users.find(    { user_id: /bc/ } ) `                     | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$regex`](https://gist.github.com/operators/#_S_regex)for more information. |
| `SELECT * FROM users WHERE user_id like "bc%" `              | `db.users.find(    { user_id: /^bc/ } ) `                    | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`$regex`](https://gist.github.com/operators/#_S_regex)for more information. |
| `SELECT * FROM users WHERE status = "A" ORDER BY user_id ASC ` | `db.users.find( { status: "A" } ).sort( { user_id: 1 } ) `   | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`sort()`](https://gist.github.com/method/cursor.sort/#cursor.sort)for more information. |
| `SELECT * FROM users WHERE status = "A" ORDER BY user_id DESC ` | `db.users.find( { status: "A" } ).sort( { user_id: -1 } ) `  | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`sort()`](https://gist.github.com/method/cursor.sort/#cursor.sort)for more information. |
| `SELECT COUNT(*) FROM users `                                | `db.users.count() `*or*`db.users.find().count() `            | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`count()`](https://gist.github.com/method/cursor.count/#cursor.count) for more information. |
| `SELECT COUNT(user_id) FROM users `                          | `db.users.count( { user_id: { $exists: true } } ) `*or*`db.users.find( { user_id: { $exists: true } } ).count() ` | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find),[`count()`](https://gist.github.com/method/cursor.count/#cursor.count), and[`$exists`](https://gist.github.com/operators/#_S_exists) for more information. |
| `SELECT COUNT(*) FROM users WHERE age > 30 `                 | `db.users.count( { age: { $gt: 30 } } ) `*or*`db.users.find( { age: { $gt: 30 } } ).count() ` | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find),[`count()`](https://gist.github.com/method/cursor.count/#cursor.count), and [`$gt`](https://gist.github.com/operators/#_S_gt) for more information. |
| `SELECT DISTINCT(status) FROM users `                        | `db.users.distinct( "status" ) `                             | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`distinct()`](https://gist.github.com/method/db.collection.distinct/#db.collection.distinct)for more information. |
| `SELECT * FROM users LIMIT 1 `                               | `db.users.findOne() `*or*`db.users.find().limit(1) `         | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find),[`findOne()`](https://gist.github.com/method/db.collection.findOne/#db.collection.findOne), and [`limit()`](https://gist.github.com/method/cursor.limit/#cursor.limit) for more information. |
| `SELECT * FROM users LIMIT 5 SKIP 10 `                       | `db.users.find().limit(5).skip(10) `                         | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find),[`limit()`](https://gist.github.com/method/cursor.limit/#cursor.limit), and [`skip()`](https://gist.github.com/method/cursor.skip/#cursor.skip)for more information. |
| `EXPLAIN SELECT * FROM users WHERE status = "A" `            | `db.users.find( { status: "A" } ).explain() `                | See [`find()`](https://gist.github.com/method/db.collection.find/#db.collection.find)and [`explain()`](https://gist.github.com/method/cursor.explain/#cursor.explain)for more information. |

### Update Records

The following table presents the various SQL statements related to updating existing records in tables and the corresponding MongoDB statements.

| SQL Update Statements                                | MongoDB update() Statements                                  | Reference                                                    |
| ---------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `UPDATE users SET status = "C" WHERE age > 25 `      | `db.users.update(    { age: { $gt: 25 } },    { $set: { status: "C" } },    { multi: true } ) ` | See [`update()`](https://gist.github.com/method/db.collection.update/#db.collection.update), [`$gt`](https://gist.github.com/operators/#_S_gt), and [`$set`](https://gist.github.com/operators/#_S_set) for more information. |
| `UPDATE users SET age = age + 3 WHERE status = "A" ` | `db.users.update(    { status: "A" } ,    { $inc: { age: 3 } },    { multi: true } ) ` | See [`update()`](https://gist.github.com/method/db.collection.update/#db.collection.update), [`$inc`](https://gist.github.com/operators/#_S_inc), and [`$set`](https://gist.github.com/operators/#_S_set) for more information. |

### Delete Records 

The following table presents the various SQL statements related to deleting records from tables and the corresponding MongoDB statements.

| SQL Delete Statements                   | MongoDB remove() Statements           | Reference                                                    |
| --------------------------------------- | ------------------------------------- | ------------------------------------------------------------ |
| `DELETE FROM users WHERE status = "D" ` | `db.users.remove( { status: "D" } ) ` | See [`remove()`](https://gist.github.com/method/db.collection.remove/#db.collection.remove) for more information. |
| `DELETE FROM users `                    | `db.users.remove( ) `                 | See [`remove()`](https://gist.github.com/method/db.collection.remove/#db.collection.remove) for more information. |

### Useful commands

Start and stop services`sudo service mongod start` `sudo service mongod stop`

Connect to a database `mongo --host localhost:27017`

Show full list of databases: `show dbs;`
Create or change to another db: `use [dbname];`

Add a collection `db.createCollection("user")`

`show collections` and `show collections`

Basic count: `db.[collection].count({QUERY});`
Output find with nice formatting: `db.[collection].findOne({QUERY});` 
`db.[collection].find().pretty()`
Find distinct values for X: `db.[collection].distinct(X, {QUERY});`

Update one record: `db.[collection].update({QUERY}, {$set: {UPDATE_QUERY}});`
Update all records that fit the criteria: `db.[collection].update({QUERY}, {$set: {UPDATE_QUERY}}, false, true);`

Remove records from a collection: `db.[collection].remove({QUERY});`
Remove an entire collection: `db.[collection].drop();`
Fully delete a database:
`use [dbname];`
`db.dropDatabase();`

Show all current operations/queries/syncs: `db.currentOp();`

### Tools

https://mongoplayground.net/ to test pipelines online 
`docker run -p 27017:27017 -d -v mongodir:/data/bin mongo` to create a new DB based on a docker container and sharing data with a local directory called `mongodir`
`Robo3t` the professional GUI and IDE for MongoDB to access content

