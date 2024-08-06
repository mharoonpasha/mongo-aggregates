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

**Output**

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

## 3. List the top 2 most common favorite fruits among the users?

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

**Output**

```
_id: "banana"
count: 339

_id: "apple"
count: 338
```

## 4. Find the total number of males and females

### Fields - gender: "male"/"female"

```
[
    {
        $group: {
            _id: "$gender",
            count: {
                $sum: 1,
            },
        },
    },
]
```

**Output**

```
_id: "male"
count: 493

_id: "female"
count: 507
```

## 5. Which two countries have the highest registered users

### Fields - company: location : country: "USA"

```
[
    {
        $group: {
            _id: "$company.location.country",
            userCount: {
                $sum: 1,
            },
        },
    },
    {
        $sort: {
            userCount: -1,
        },
    },
    {
        $limit: 2,
    },
]
```

## 6. List all unique eye colors present in the collection.

### Fields - eyeColor: "green"

```
[
    {
        $group: {
            _id: "$eyeColor",
        },
    },
]
```
