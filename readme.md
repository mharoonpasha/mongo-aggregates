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

## 5. What is the average number of tags per user

### Fields - tags: [string]

**Option - 1**

```
[
    {
        $unwind: "$tags"
    },
    {
        $group: {
            _id: "$_id",
            numberOfTags: {$sum: 1},
        },
    },
    {
        $group: {
            _id: null,
            averageNumberOfTags: {$avg: "$numberOfTags"}
        }
    }
]
```

**Option - 2**

```
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
            averageNumberOfTags: {$avg: "$numberOfTags"}
        }
    }
]
```

## 6. How many users have "enim" as one of their tags?

### Fields - tags: [string]

```
[
    {
        $match: {
            tags: "enim"
        }
    },
    {
        $count: "userWithEnimTag"
    }
]
```

## 7. What are the names and age of users who are inactive and have "velit" as a tag?

### Fields - tags: [string], age: 20, isActive: true/false, name: string

```
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
        },
    },
]
```

## 8. How many users have a phone number starting with "+1 (940)"?

### Fields - company: phone: "+1 (940) xxx

```
[
    {
        $match: {
            "company.phone": /^\+1 \(940\)/
        }
    },
    {
        $count: "UsersWithSpecialPhoneNumber"
    }
]
```

## 9. Who has registered most recently?

### Fields - registered: Date

```
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
            favoriteFruit: 1,
        }
    }
]
```

## 10. Categorize users by their favorite fruit

### Fields - favoriteFruit: string

```
[
    {
        $group: {
            _id: "$favoriteFruit",
            users: {$push: "$name"} // push the specified fields to array "users"
        }
    }
]
```

## 11. How may users have "ad" as their second tag in their list of tags?

### Fields - tag: [string]

```
[
    {
        $match: {
            "tags.1": "ad"
        }
    },
    {
        $count: "secondTagAdUsers"
    }
]
```

## 12. Find Users who have both "enim" and "id" as their tags

### Fields - tag: [string]

```
[
    {
        $match: {
            tags: {$all: ["enim", "id"]}
        }
    }
]
```

## 13. List all companies located in the UAS with their corresponding user count.

### Fields - companies: location: country: "USA"

```
[
    {
        $match: {
            "company.location.country": "USA"
        }
    },
    {
        $group: {
            // _id: "$company.title",
            _id: null,
            userCount: {$sum: 1}
        }
    }
]
```

## 14. Lookups

### Collections

**authors**

```
_id: number
name: String
birth_year: number
```

**books**

```
_id: number
title: String
author_id: number
genre: string
```

**lookup on books**

```
[
    {
        $lookup: {
            from: "authors",
            localField: "author_id",
            foreignField: "_id",
            as: "author_details
        }
    },
    {
        $addFields: {
            author_details: {
                //$first: "$author_details"
                $arrayElemAt: ["$author_details", 0]
            }
        }
    }
]
```
