# Mongodb Methods

## Database Methods:

1. use(databaseName): Switches to a specified database.

> use my_database_name

2. showDatabases(): Displays a list of databases on the MongoDB server.

> showDatabases()

3. db.dropDatabase(): Drops the current database.

> db.dropDatabase()

## Collection Methods:

1. db.createCollection(name, options): Creates a new collection.

2. db.getCollection(name): Retrieves a collection.

3. db.getCollectionNames(): Lists all collections in the current database.

4. db.collection.drop(): Drops a collection.
>  db.my_collection.drop()

## Index Methods:

1. db.collection.createIndex(keys, options): Creates an index on a collection.

> db.my_collection.createIndex({ "name": 1 })

2. db.collection.getIndexes(): Retrieves all indexes on a collection.

> db.my_collection.getIndexes()

3. db.collection.dropIndex(indexName): Drops an index from a collection.

> db.my_collection.dropIndex("index_name")

!---------------------------------------------------------------------------

# *** Document Methods: ***

## All CRUD Methods

### Inserting Documents:

1. db.collection.insert(document): Inserts a document into a collection.

> db.my_collection.insert({ "name": "John", "age": 30 })

2. insertOne(document): Inserts a single document into the collection.

> db.collection.insertOne({ "name": "John", "age": 30 })

3. insertMany(documents): Inserts multiple documents into the collection.

> db.collection.insertMany([
    { "name": "John", "age": 30 },
    { "name": "Jane", "age": 25 }
])


### Retrieving Documents:

1. db.collection.find(query, projection): Retrieves all documents from a collection based on a query.

> db.my_collection.find({ "field": "value" })

2. findOne(query, projection): Retrieves a single document that matches the query.

> db.collection.findOne({ "name": "John" })


### Updating Documents:


1. db.collection.update(query, update, options): Updates documents in a collection.

> db.my_collection.update({ "name": "John" }, { "$set": { "age": 31 } })

2. updateOne(filter, update, options): Updates a single document that matches the filter.

> db.collection.updateOne({ "name": "John" }, { "$set": { "age": 31 } })

3. updateMany(filter, update, options): Updates all documents that match the filter.

> db.collection.updateMany({ "status": "active" }, { "$set": { "status": "inactive" } })


### Deleting Documents:

1. db.collection.remove(query, options): Removes documents from a collection based on a query.

> db.my_collection.remove({ "name": "John" })

2. deleteOne(filter, options): Deletes a single document that matches the filter.

> db.collection.deleteOne({ "name": "John" })

3. deleteMany(filter, options): Deletes all documents that match the filter.

> db.collection.deleteMany({ "status": "inactive" })


### Counting Documents:

1. countDocuments(query, options): Counts documents that match the query.

> db.collection.countDocuments({ "status": "active" })







# MONGODB OPERATOR

## Comparison Operators:
1. $eq: Matches values that are equal to a specified value.

> { "age": { "$eq": 30 } }

2. $gt: Matches values that are greater than a specified value.

> { "age": { "$gt": 25 } }

3. $lt: Matches values that are less than a specified value.

> { "age": { "$lt": 40 } }

4. $in: Matches any of the values specified in an array.

> { "status": { "$in": ["active", "pending"] } }

## Logical Operators:

1. $and: Joins query clauses with a logical AND and returns all documents that match the conditions.

> { "$and": [ { "age": { "$gt": 25 } }, { "age": { "$lt": 40 } } ] }

2. $or: Joins query clauses with a logical OR and returns all documents that match at least one of the conditions.

> { "$or": [ { "status": "active" }, { "score": { "$gt": 80 } } ] }


3. $not: Inverts the effect of a query expression and returns documents that do not match the query expression.

> { "age": { "$not": { "$eq": 30 } } }


## Element Operators:

1. $exists: Matches documents that have the specified field.

> { "email": { "$exists": true } }

2. $type: Selects documents if a field is of the specified type.

> { "age": { "$type": "number" } }

## Array Operators:

1. $all: Matches arrays that contain all elements specified in the query.

> { "tags": { "$all": ["mongodb", "database"] } }

2. $elemMatch: Matches documents that contain an array field with at least one element that matches all the specified query criteria.

> { "comments": { "$elemMatch": { "author": "Alice", "score": { "$gt": 5 } } } }

3. $size: Matches arrays with a specific number of elements.

> { "comments": { "$size": 3 } }


### Updating array fields

1. $push: Adds an item to an array.

> db.collection.updateOne({ "_id": ObjectId("...") }, { "$push": { "tags": "newTag" } })

2. $addToSet: Adds elements to an array only if they do not already exist in the set.

> db.collection.updateOne({ "_id": ObjectId("...") }, { "$addToSet": { "tags": "newTag" } })

3. $pop: Removes the first or last element of an array.

> db.collection.updateOne({ "_id": ObjectId("...") }, { "$pop": { "tags": 1 } })

4. $pull: Removes all occurrences of a value from an array.

> db.collection.updateOne({ "_id": ObjectId("...") }, { "$pull": { "tags": "oldTag" } })

5. $pullAll: Removes all matching values from an array.

> db.collection.updateOne({ "_id": ObjectId("...") }, { "$pullAll": { "tags": ["tag1", "tag2"] } })





# MONGODB AGGREGATION STAGE OPERATOR

1. $match: Filters documents to pass only those that match the specified condition(s).

```javascript
> db.collection.aggregate([
    { $match: { status: "A" } }
])
```

2. $group: Groups documents by a specified expression and applies aggregate functions to each group.

```javascript
> db.collection.aggregate([
    { $group: { _id: "$category", totalQuantity: { $sum: "$quantity" } } }
])
```

3. $project: Reshapes documents by including, excluding, or renaming fields.

```javascript
> db.collection.aggregate([
    { $project: { itemName: 1, itemPrice: 1, _id: 0 } }
])
```

4. $sort: Orders documents based on specified fields.

```javascript
> db.collection.aggregate([
    { $sort: { age: -1 } }
])

```

5. $limit: Limits the number of documents passed to the next stage.

```javascript
> db.collection.aggregate([
    { $limit: 5 }
])
```

6. $skip: Skips a specified number of documents.

```javascript
> db.collection.aggregate([
    { $skip: 10 }
])
```

7. $unwind: Deconstructs an array field from the input documents and outputs one document for each element.

```javascript
 db.collection.aggregate([
    { $unwind: "$tags" }
])

```

8. $lookup: Performs a left outer join to another collection in the same database to filter in documents from the "joined" collection for processing.

```javascript db.orders.aggregate([
    {
        $lookup:
        {
            from: "inventory",
            localField: "item",
            foreignField: "sku",
            as: "inventory_docs"
        }
    }
])
```

9. $sample: Randomly selects a specified number of documents from a collection.

```javascript
 db.collection.aggregate([
    { $sample: { size: 5 } }
])

```
10. $facet: Allows for multiple pipelines to be executed within a single aggregation stage.

```javascript 
db.collection.aggregate([
    {
        $facet: {
            categoryCounts: [ { $sortByCount: "$category" } ],
            totalQuantity: [ { $group: { _id: null, total: { $sum: "$quantity" } } } ]
        }
    }
])
```
11. $addFields: Adds new fields to documents

```javascript
db.collection.aggregate([
    { $addFields: { totalAmount: { $multiply: ["$price", "$quantity"] } } }
])
```

12. $replaceRoot: Replaces the input document with the specified document.

```javascript
db.collection.aggregate([
    { $replaceRoot: { newRoot: "$embeddedDocument" } }
])
```

13. $count: Returns a count of the number of documents at this stage of the aggregation pipeline.

```javascript
db.collection.aggregate([
    { $count: "totalDocuments" }
])
```

# MONGODB AGGREGATION EXPRESSION OPERATORS:

1. $add: Adds numbers together or concatenates strings.

```javascript
db.collection.aggregate([
    { $add: [ "$field1", "$field2" ] }
])

```

2. $subtract: Subtracts two numbers. 

```javascript
db.collection.aggregate([
    { $subtract: [ "$total", "$discount" ] }
])

```
3. $multiply: Multiplies numbers together.

```javascript
db.collection.aggregate([
    { $multiply: [ "$price", "$quantity" ] }
])

```
4. $divide: Divides one number by another.

```javascript
db.collection.aggregate([
    { $divide: [ "$totalRevenue", "$numCustomers" ] }
])

```
5. $mod: Calculates the remainder of dividing one number by another. 

```javascript
db.collection.aggregate([
    { $mod: [ "$quantity", 2 ] }
])

```
6. $concat: Concatenates strings.

```javascript
db.collection.aggregate([
    { $concat: [ "$firstName", " ", "$lastName" ] }
])

```
7. $trim: Removes whitespace or other characters from the beginning and end of a string.

```javascript
db.collection.aggregate([
    { $trim: { input: "$text", chars: " \t\n" } }
])

```
8. $toLower: Converts a string to lowercase. 

```javascript
db.collection.aggregate([
    { $toLower: "$field" }
])

```
9. $toUpper: Converts a string to uppercase.

```javascript
db.collection.aggregate([
    { $toUpper: "$field" }
])

```
10. $size: Returns the number of elements in an array.

```javascript
db.collection.aggregate([
    { $size: "$arrayField" }
])

```
11. $isArray: Checks if a value is an array.

```javascript
db.collection.aggregate([
    { $isArray: "$arrayField" }
])

```
12. $arrayElemAt: Returns the element at the specified array index.

```javascript
db.collection.aggregate([
    { $arrayElemAt: [ "$arrayField", 0 ] }
])

```
13. $indexOfArray: Returns the index of the first occurrence of a specified value in an array.

```javascript
db.collection.aggregate([
    { $indexOfArray: [ "$arrayField", "value" ] }
])

```
14. $slice: Returns a subset of an array.

```javascript
db.collection.aggregate([
    { $slice: [ "$arrayField", 2 ] }
])

```
15. $arrayToObject: Converts an array into a document.

```javascript
db.collection.aggregate([
    { $arrayToObject: { $map: { input: "$arrayField", as: "item", in: { k: "$$item.key", v: "$$item.value" } } } }
])

```
16. $let: Binds variables for use within the scope of a subexpression. 

```javascript
db.collection.aggregate([
    {
        $project: {
            fullName: {
                $let: {
                    vars: {
                        fullName: { $concat: ["$firstName", " ", "$lastName"] }
                    },
                    in: "$$fullName"
                }
            }
        }
    }
])

```
17. $literal: Returns a value as a literal without parsing.

```javascript
db.collection.aggregate([
    { $addFields: { literalValue: { $literal: "$$REMOVE" } } }
])

```

18. $objectToArray: Converts a document into an array of key-value pairs.

```javascript
db.collection.aggregate([
    {
        $project: {
            arrayField: { $objectToArray: "$documentField" }
        }
    }
])

```

19. $mergeObjects: Merges multiple documents into a single document.

```javascript
db.collection.aggregate([
    {
        $project: {
            mergedObject: { $mergeObjects: ["$firstDocument", "$secondDocument"] }
        }
    }
])

```

20. $dateToString: Converts a date to a string.

```javascript
db.collection.aggregate([
    {
        $project: {
            dateString: {
                $dateToString: {
                    format: "%Y-%m-%d",
                    date: "$dateField"
                }
            }
        }
    }
])

```

21. $dateFromString: Converts a string to a date.

```javascript
db.collection.aggregate([
    {
        $project: {
            dateObject: {
                $dateFromString: {
                    dateString: "$dateStringField"
                }
            }
        }
    }
])

```

22. $dateToParts: Returns the parts of a date as a document.

```javascript
db.collection.aggregate([
    {
        $project: {
            dateParts: { $dateToParts: { date: "$dateField" } }
        }
    }
])

```

23. $dateFromParts: Constructs a date from given date parts.

```javascript
db.collection.aggregate([
    {
        $project: {
            constructedDate: {
                $dateFromParts: {
                    year: 2023,
                    month: 1,
                    day: 15
                }
            }
        }
    }
])

```

24. $dateToString: Converts a date to a string.

```javascript
db.collection.aggregate([
    {
        $project: {
            dateString: {
                $dateToString: {
                    format: "%Y-%m-%d",
                    date: "$dateField"
                }
            }
        }
    }
])

```

25.  $concatArrays: expression to add an element to an existing array field.

```javascript
db.scores.aggregate([
   { $match: { _id: 1 } },
   { $addFields: { homework: { $concatArrays: [ "$homework", [ 7 ] ] } } }
])
```

. 

```javascript

```

. 

```javascript

```


. 

```javascript

```

. 

```javascript

```

. 

```javascript

```

. 

```javascript

```


# MONGODB AGGREGIATIONS PIPELINE

## steps
1. we write aggregiations inside the array.
2. we write mongodb aggregiation pipeline in stages.
3. each stages is an source of input to anathor stages.

## here we practice with some questions

>  How many user are active? 
```javascript
[
  {
    $match: {
      isActive: true,
    },
  },
  {
    $count: "activeUser",
  },
]

//output:-
{
  "activeUser": 516
}
```

>  What is the average age of all users?
```javascript
//on the basic of gender grouping
[
  {
    $group: {
      _id: "$gender",
      averageAge: {
        $avg: "$age"
      }
    }
  }
]
//output:-
{
  "_id": "male",
  "averageAge": 29.851926977687626
}
{
  "_id": "female",
  "averageAge": 29.81854043392505
}
```
```javascript
//on the basics of all user grouping
[
  {
    $group: {
      _id: null,
      averageAge: {
        $avg: "$age"
      }
    }
  }
]
//output:-
{
  "_id": null,
  "averageAge": 29.835
}
```

> List the top 3 most  common favourite fruits among the users.
```javascript
[
  {
    $group: {
      _id: "$favoriteFruit",
      count: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      count: -1
    }
  },
  {
    $limit: 3
  }
]

//output
{
  "_id": "banana",
  "count": 339
}
{
  "_id": "apple",
  "count": 338
}
{
  "_id": "strawberry",
  "count": 323
}
```

> Find the total no of males or females.

```javascript
[
  {
    $group: {
      _id: "$gender",
      count: {
        $sum: 1
      }

    }
  }
]
//output
{
  "_id": "male",
  "count": 493
}
{
  "_id": "female",
  "count": 507
}
```

> Which 2 country has the highest number of registered users?

```javascript
[
  {
    $group: {
      _id: "$company.location.country",
      count: {
        $sum: 1
      }
    }
  },
  {
      $sort: {
        count: -1
      }
    },
  {
    $limit: 2
  }
]
//Output
{
  "_id": "Germany",
  "count": 261
}
{
  "_id": "USA",
  "count": 255
}
```

> LIst all unique eye color present in the collection.

```javascript
[
  {
    $group: {
      _id: "$eyeColor",
      count: {
        $sum: 1
      }
    },
  },
]

//Output
{
  "_id": "blue",
  "count": 333
}
{
  "_id": "brown",
  "count": 337
}
{
  "_id": "green",
  "count": 330
}
```

> what is the average no of tags per user
```javascript
//unwinding the element in the array..
[
  {
    $unwind: "$tags",    
  },
  {
    $group: {
      _id: "$_id",
      numberOfTag: {
        $sum: 1
      } 
    }
  },
  {
    $group: {
      _id: null,
      averageNumberOfTag: {
        $avg: "$numberOfTag"
      }
    }
  }
]

//output
{
  "averageNumberOfTag": 3.556,
  "_id": null
}
```


```javascript
//Anathor methods on unwinding the element of array..
[
  {
    $addFields: {
      numberOfTags: {
        $size: {$ifNull: ["$tags", []]}
      }
    }
  },
  {
    $group: {
      _id: null,
      avgNoOfTags: {
        $avg: "$numberOfTags"
      }
     
    }
  }
]
//Output
{
  "_id": null,
  "avgNoOfTags": 3.556
}
```


> "Filtering of Array"   
> How many users have "enum" as  one of the tags?


```javascript
//here we use match for filtering of document which have array  with these element are present inside it.   
[
  {
    $match: {
      tags: "enim"
    }
  },
  {
    $count: "userWithEnimTags"
  }
]

//Output
{
  "userWithEnimTags": 62
}
```

> What are the names & age  of users who are inactive  & have 'velit' as a tag?

```javascript
//1. we use match op to filter on basis of req
//2. using project to show limited value of document.
[
  {
    $match: {
      isActive: false, tags: "velit"
    }
  },
  {
    $project: {
      name: 1,
      age: 1, 
    }
  }
  
]

//output
{
  "_id": {
    "$oid": "65c290bbae35aff3689eb259"
  },
  "name": "Aurelia Gonzales",
  "age": 20
}
```


> How mant user have a phone no with '+1 (940)'?

```javascript
//1. we use match to filter doc on the basics of regex exp
//2. count the value

[
  {
    $match: {
      "company.phone":  /^\+1 \(940\)/
    }
  },
  {
    $count: 'userWithSpecialPhoneNumber'
  }
]

//output
{
  "userWithSpecialPhoneNumber": 5
}
```

> Who has regestered the most recently? & also add first 4 user
with limited value

```javascript
// using sort pipeline for sorting the doc on basis of   registered early,
[
  {
    $sort: {
      registered: -1
    }
  },
  {
    $limit: 4
  },
  {
    $project: {
      name: 1,
      registered: 1,
      favoriteFruit: 1
    }
  }
]
```

> Categories user by their favourite fruit.

```javascript
//1. we group the user on the basis of favouriteFruit
//2. After that create users fields inside the doc
//3. by using push we are creating array with name as a element. who love their favourite fruit.
[
  {
    $group: {
      _id: "$favoriteFruit",
      users: {
        $push: "$name"
      }
     
    }
  }
]

//output
{
  "users": [
    "Aurelia Gonzales",
    "Alison Farmer",
    ...........

    ...........
    "Chrystal Clay",
    "Britney Bell",
    "Iva Gonzales"
  ],
  "_id": "banana"
}
```

> How many user have 'ad' as the second tag in their list of tags
```javascript
[
  {
    $match: {
      "tags.1": "ad",
    }
  },
  {
    $count: 'secondTagAd'
  }
]

//output
{
  "secondTagAd": 12
}
```

> Find user who have both 'enim', & 'id' as their tags.

```javascript
// match to filter doc
//but using all operator we check all req are fulfill

[
  {
    $match: {
      tags: {
        $all: ["enim", "id"]
      }
    }
  }, 
]

//op:-
{
  "company": {
    "email": "aureliagonzales@yurture.com",
    "phone": "+1 (940) 501-3963",
    "location": {
      "country": "USA",
      "address": "694 Hewes Street"
    },
    "title": "YURTURE"
  },
  "tags": [
    "enim",
    "id",
    "velit",
    "ad",
    "consequat"
  ],
  "_id": {
    "$oid": "65c290bbae35aff3689eb259"
  },
  "index": 0,
  "name": "Aurelia Gonzales",
  "isActive": false,
  "registered": {
    "$date": "2015-02-11T04:22:39.000Z"
  },
  "eyeColor": "green",
  "age": 20,
  "gender": "female",
  "favoriteFruit": "banana"
}
```

> List all the company located in the USA with their corresponding user count

```javascript
[
  {
    $match: {
      "company.location.country": "USA",
    },
  },
  {
    $group: {
      _id: "$company.title",
      userCount: {
        $sum: 1,
      },
    },
  },
  {
    $sort: {
      userCount: -1
    }
  },
  {
    $limit: 5
  }
]
//op:-
{
  "userCount": 2,
  "_id": "YURTURE"
}....
```

> bring the value from the other models

```javascript
//1. we use lookup to get data from anathor models into present model
//2. but the problem here is that the return value is come in array form inside which details are in object form
//3. using first to select the 1 value from array & we convert them from  array to object with all value as a new name.
//4.  
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details",
    },
  },
  {
    $addFields: {
      author_details: {
        $first: "$author_details",
      },
    },
  },
  {
    $project: {
      author_details: 1,
    },
  },
]

//op:-
{
  "title": "The Great Gatsby",
  "author_id": 100,
  "genre": "Classic",
  "author_details": {
    "birth_year": 1896,
    "_id": 100,
    "name": "F. Scott Fitzgerald"
  },
  "_id": 1
}
```

```javascript
//anathor method to convert from array to obj
//using arrayElementAt
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details",
    },
  },
  {
    $addFields: {
      author_details: {
        $arrayElemAt: ["$author_details", 0],
      },
    },
  },
]

//op:-
{
  "_id": 1,
  "title": "The Great Gatsby",
  "author_id": 100,
  "genre": "Classic",
  "author_details": {
    "birth_year": 1896,
    "_id": 100,
    "name": "F. Scott Fitzgerald"
  }
}
```