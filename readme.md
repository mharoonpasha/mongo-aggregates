# MongoDB Aggregates

## 1. How many users are active in this db,

### Fields - isActive : true/false

```
[
    {
        $match: {
            isActive: true,
        },
    },
    {
        $count: "activeUsers"
    },
]
```

** Output **

```
activeUsers: 516
```

## 2. What is the average age of all users?

### Fields - age: 20, gender: male/female

```
[
    {
        $group: {
            _id: null,
            // _id: "$gender",
            averageAge: {
                $avg: "$age",
            },
        },
    },
]
```

## 2. List the top 2 most common favorite fruits among the users?

### Fields - favoriteFruit: "banana"

```
[
    {
        $group: {
            _id: "$favoriteFruit",
            count: {
                $sum: 1,
            },
        },
    },
    {
        $sort: {
            count: -1,
        },
    },
    {
        $limit: 2,
    },
]
```

** Output **

```
_id: "banana"
count: 339

_id: "apple"
count: 338
```
