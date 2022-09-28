---
tags: [tools, mongodb, dbms]
---

# MongoDB

*Last update: 28 Sep 2022*

### General info

Comparison between a SQL DBMS and MongoDB

| SQL | MongoDB |
| --- | --------|
| schema | db |
| table | collection |
| record | document |

In MongoDB, every document should contain an *_id* field. 
Its type is String if manually assigned or ObjectId if assigned by MongoDB.

An URI in srv format:

    mongodb+srv://USERNAME:PASSWORD@HOST/DATABASE?options


### Command line tools

* **mongod** : run the server in interactive mode
* **mongo**  : shell client (CLI)
* **mongodb-compass**: graphical client

### Commands

Select the DB that it is created if not existing

    use <db>

Show all collections in a db

    show collections    

Insert one or more documents in a collection

    db.<coll>.insertOne ( <document> )
    db.<coll>.insertMany ( [ <document1>, <document2>..] )

Insert one or more documents in a collection skipping any error

    db.<coll>.insertMany ( [ <document1>, <document2>..], { "ordered":false} )
	
Query by fields

    db.<coll>.find ( {fields} )

Query by fields with projection (1=show the field, 0=hide the field)

    db.<coll>.find ( {fields}, {projection})

Query with formatted results

    db.<coll>.find ( ... ).pretty()

Count the documents returned by the query

    db.<coll>.find ( ... ).count()

Update one document

    db.<coll>.updateOne(<selector>, {operators})

Update many document

    db.<coll>.updateMany(<selector>, {operators})

Upsert (insert or update)

    db.<coll>.upsert

Updates operators:

* scalar: $set, $unset, $min, $max, $inc..
* array: $addToSet, $pop, $popAll, $push, $pull
  
