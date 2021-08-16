### docker 安装mongodb

```shell
docker run --name mongo \
-p 27017:27017 \
-v /home/ubuntu/mongo/conf:/data/configdb \
-v /home/ubuntu/mongo/data:/data/db \
-v /home/ubuntu/mongo/backup:/data/backup \
-d mongo --auth
```



### 配置admin

```shell
docker exec -it mongo mongo admin

db.createUser({user:'admin',pwd:'admin',roles:[{role:'root',db:'admin'}],})

db.auth('admin','admin')
```



### 创建 database
```shell
use <DATABASE_NAME>
```



### 创建 user

```shell
use admin
db.auth('admin','admin')
use demo
db.createUser({user:'demo',pwd:'demo',roles:[{role:'dbOwner',db:'demo'}],})
```



### 创建 Collection

```shell
db.createCollection(name, options)
```



| Parameter | Type     | Description                                                  |
| :-------- | :------- | :----------------------------------------------------------- |
| `name`    | string   | The name of the collection to create. See [Naming Restrictions](https://docs.mongodb.com/manual/reference/limits/#std-label-restrictions-on-db-names). |
| `options` | document | Optional. Configuration options for creating a capped collection, for preallocating space in a new collection, or for creating a view. |



### Insert Document

```shell
db.collection.insertOne()
db.collection.insertMany() 
```



### Update Document

### 

```shell
db.collection.updateOne(filter, update, options)

db.collection.updateOne(
   <filter>,
   <update>,
   {
     upsert: <boolean>,
     writeConcern: <document>,
     collation: <document>,
     arrayFilters: [ <filterdocument1>, ... ],
     hint:  <document|string>
   }
)

db.collection.updateMany(filter, update, options)¶
db.collection.updateMany(
   <filter>,
   <update>,
   {
     upsert: <boolean>,
     writeConcern: <document>,
     collation: <document>,
     arrayFilters: [ <filterdocument1>, ... ],
     hint:  <document|string>        // Available starting in MongoDB 4.2.1
   }
)
```

```shell
db.students.insertMany([
   { _id: 1, test1: 95, test2: 92, test3: 90, modified: new Date("01/05/2020") },
   { _id: 2, test1: 98, test2: 100, test3: 102, modified: new Date("01/05/2020") },
   { _id: 3, test1: 95, test2: 110, modified: new Date("01/04/2020") }
])

db.students.updateOne( { _id: 3 }, [ { $set: { "test3": 98, modified: "$$NOW"} } ] )


db.students2.insertMany([
   { "_id" : 1, quiz1: 8, test2: 100, quiz2: 9, modified: new Date("01/05/2020") },
   { "_id" : 2, quiz2: 5, test1: 80, test2: 89, modified: new Date("01/05/2020") },
])

db.students2.updateMany( {},
  [
    { $replaceRoot: { newRoot:
       { $mergeObjects: [ { quiz1: 0, quiz2: 0, test1: 0, test2: 0 }, "$$ROOT" ] }
    } },
    { $set: { modified: "$$NOW"}  }
  ]
)

```



### Delete Document

```
db.collection.deleteOne()

db.collection.deleteOne(
   <filter>,
   {
      writeConcern: <document>,
      collation: <document>,
      hint: <document|string>        // Available starting in MongoDB 4.4
   }
)

db.collection.deleteMany()

db.collection.deleteMany(
   <filter>,
   {
      writeConcern: <document>,
      collation: <document>
   }
)
```



### Query Document



```shell
db.collection.find(query, projection)
```





### Query and Projection Operators



| Name                                                         | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`$eq`](https://docs.mongodb.com/manual/reference/operator/query/eq/#mongodb-query-op.-eq) | Matches values that are equal to a specified value.          |
| [`$gt`](https://docs.mongodb.com/manual/reference/operator/query/gt/#mongodb-query-op.-gt) | Matches values that are greater than a specified value.      |
| [`$gte`](https://docs.mongodb.com/manual/reference/operator/query/gte/#mongodb-query-op.-gte) | Matches values that are greater than or equal to a specified value. |
| [`$in`](https://docs.mongodb.com/manual/reference/operator/query/in/#mongodb-query-op.-in) | Matches any of the values specified in an array.             |
| [`$lt`](https://docs.mongodb.com/manual/reference/operator/query/lt/#mongodb-query-op.-lt) | Matches values that are less than a specified value.         |
| [`$lte`](https://docs.mongodb.com/manual/reference/operator/query/lte/#mongodb-query-op.-lte) | Matches values that are less than or equal to a specified value. |
| [`$ne`](https://docs.mongodb.com/manual/reference/operator/query/ne/#mongodb-query-op.-ne) | Matches all values that are not equal to a specified value.  |
| [`$nin`](https://docs.mongodb.com/manual/reference/operator/query/nin/#mongodb-query-op.-nin) | Matches none of the values specified in an array.            |

### Logical

| Name                                                         | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [`$and`](https://docs.mongodb.com/manual/reference/operator/query/and/#mongodb-query-op.-and) | Joins query clauses with a logical `AND` returns all documents that match the conditions of both clauses. |
| [`$not`](https://docs.mongodb.com/manual/reference/operator/query/not/#mongodb-query-op.-not) | Inverts the effect of a query expression and returns documents that do *not* match the query expression. |
| [`$nor`](https://docs.mongodb.com/manual/reference/operator/query/nor/#mongodb-query-op.-nor) | Joins query clauses with a logical `NOR` returns all documents that fail to match both clauses. |
| [`$or`](https://docs.mongodb.com/manual/reference/operator/query/or/#mongodb-query-op.-or) | Joins query clauses with a logical `OR` returns all documents that match the conditions of either clause. |
