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