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