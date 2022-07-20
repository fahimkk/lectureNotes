# Installation

Install monogo db from the officail website
Install mongosh. Which provide a shell to work with.

mongoDB basic commands

```powershell-interactive
sudo systemctl start/status/stop/restart mongod
```

To start a `mongosh` session

```powershell-interactive
mongosh
```

## mongoDB shell commands

### Basic commands

To show all the databases

```powershell-interactive
show dbs
```

To use a db or create a new db. `use` command create a new db if the db name not exists.

```powershell-interactive
use db_name
```

To view collections in db. Collections are same as table in sql db

```powershell-interactive
show collections
```

To access a currend database

```powershell-interactive
db
```

To Drop a db

```powershell-interactive
db.dropDatabase()
```

To add a collection to db

```powershell-interactive
db.collection_name.insertOne(js_obj)
```

here collection_name is the name of the collection, and if it not exists this will create one.

`document` - every single element stored inside a collection is called document.

`insertOne` takes a single js object as argument and add to the collection.

`insertMany` takes a list of objs as argument and insert that into the collection.

```powershell-interactive
db.collection_name.insertMany{[js_obj_1, js_obj_2]}
```

### Query Command

Read informations from db

`find()` command return every single thing inside this collection

```powershell-interactive
db.collection_name.find()
```

`db.collection_name.find().limit(num)` will return first num documents from that collection.

`db.collection_name.find().sort({key: 1}).limit(num)` to get the sorted documents
`sort` takes an js obj as argument with key value, if you want to sort document wrt name use name instead of key, and `1` or normal sort and `-1` for `reverse` sort.
We can also pass multiple key value in the sort argument, `eg: {age: 1, name:-1}`. First this will sort wrt age and if two have same age then sort wrt name.

`db.collection_name.find().skip(1).limit(2)` this will first skip the 1st element and return next 2 elements.

To find every elements with name x `db.collection_name.find({name: x})`
This query also written as (for more complex query) `db.collection_name.find({name: {$eq: x}})` will return all the elements where name is x. some sample commands are `ne` for not equal, `gt` for greater than, `in` for check element in a list, etc.

There are two methods to do `and` query.

`db.collection_name.find({age: {$gte: 20, $lte: 40}, name: x})`

`db.collection_name.find({$and: [{age: 20}, {name: x}]})`

To do `or` query, `db.collection_name.find({$or: [{age: {$lte: 20}}, {name: x}]})`

To specify what you want to return pass a 2nd argument in find method, value `1` means return this and value `0` means not return this. Always returns id, to exclude id and only return name `db.collection_name.find({name: x}, {name:1, _id:0})`

To check for key exists (returns if the key's value is null) - to return all the elements where age param is exists `db.collection_name.find({age: {$exists: true}})`

`findOne(condition)` - returns only one element matched the condition
`countDocuments(condition)` - returns obj count matched with the condition

`distinct` is will return a list of all the distinct values wrt key

```powershell-interactive
db.collection_name.disctict("name")
# this will return all the distinct names
```

### Update document

to upadate a document with age 26 to 27, `db.collection_name.updateOne({age: 26}, { $set: {age: 27}})`.

instead of `$set` use `$inc` for an increment.
To unset a property, ie remove a property use `$unset`.

To push an element to an array, `{ $push: { arrayName: "element" }}`. For removing an element form an array instead of `$push` use `$pull`.

To rename a column (name to firstName), `db.collection_name.updateOne({age: 26}, { $rename: {name: "firstName"} })

To update many documents

```powershell-interactive
db.collection_name.updateMany({address: {$exists: true}, { $unset: {address: ""} } })
```

### Deleting document

`deleteOne` for single document deleting.

`deleteMany` for deleting multiple documents.
