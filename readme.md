# MongoDB Aggregate

## 1. How many users are active in this db,

### Fields - isActive : true/false

```
[
    {
        $match: {
            isActive: true,
        }
    },
    {
        $count: "activeUsers"
    }
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
            averageAge: {
                $avg: "$age",
            }
        }
    }
]
```
