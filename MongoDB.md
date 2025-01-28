# MongoDB Cheat Sheet

## Basics

### Connecting to MongoDB
```bash
mongo
```

### Show Databases
```javascript
show dbs
```

### Switch Database
```javascript
use <database_name>
```

### Show Collections
```javascript
show collections
```

## CRUD Operations

### Create
```javascript
db.<collection>.insertOne({ <document> })
db.<collection>.insertMany([{ <document1> }, { <document2> }])
```

### Read
```javascript
db.<collection>.find({ <query> })
db.<collection>.findOne({ <query> })
```

### Update
```javascript
db.<collection>.updateOne({ <query> }, { $set: { <update_fields> } })
db.<collection>.updateMany({ <query> }, { $set: { <update_fields> } })
db.<collection>.replaceOne({ <query> }, { <new_document> })
```

### Delete
```javascript
db.<collection>.deleteOne({ <query> })
db.<collection>.deleteMany({ <query> })
```

## Indexes

### Create Index
```javascript
db.<collection>.createIndex({ <field>: <1 or -1> })
```

### List Indexes
```javascript
db.<collection>.getIndexes()
```

### Drop Index
```javascript
db.<collection>.dropIndex({ <field>: <1 or -1> })
```

## Aggregation

### Basic Aggregation
```javascript
db.<collection>.aggregate([
    { $match: { <query> } },
    { $group: { _id: "$<field>", total: { $sum: 1 } } }
])
```

## Backup and Restore

### Backup
```bash
mongodump --db <database_name> --out <backup_directory>
```

### Restore
```bash
mongorestore --db <database_name> <backup_directory>/<database_name>
```

## User Management

### Create User
```javascript
db.createUser({
    user: "<username>",
    pwd: "<password>",
    roles: [{ role: "<role>", db: "<database_name>" }]
})
```

### List Users
```javascript
db.getUsers()
```

### Drop User
```javascript
db.dropUser("<username>")
```

## Miscellaneous

### Server Status
```javascript
db.serverStatus()
```

### Database Stats
```javascript
db.stats()
```

### Collection Stats
```javascript
db.<collection>.stats()
```

[MongoDb Docs](https://www.mongodb.com/docs/)