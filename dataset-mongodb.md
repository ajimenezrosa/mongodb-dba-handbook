- [EditConfig](#editorconfig)
- [Docker Compose](#docker-compose)

# EditConfig

```
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
# editorconfig-tools is unable to ignore longs strings or urls
max_line_length = off

[CHANGELOG.md]
indent_size = false
``` 

### Docker Compose

```yml
version: '3.9'

services:
  mongodb:
    image: mongo:5.0
    ports:
      - 27017:27017
    volumes:
      - ./mongo_data:/data/db
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=root
```

### Array Update Operators

```js
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "item ab", code: "123", description : "Single line description."    }, qty: 15, tags: [ "school", "book", "bag", "headphone", "appliance" ], },
  { _id: 2, item: { name: "item cd", code: "123", description : "First line\nSecond line"     }, qty: 20, tags: [ "appliance", "school", "book" ] },
  { _id: 3, item: { name: "item ij", code: "456", description : "Many spaces before     line" }, qty: 25, tags: [ "school", "book" ] },
  { _id: 4, item: { name: "item xy", code: "456", description : "Multiple\nline description"  }, qty: 30, tags: [ "electronics", "school" ] },
  { _id: 5, item: { name: "item mn", code: "000" }, qty: 20, tags: [ "appliance", "school" ] },
])

db.inventory.find()
```

### Upsert

```js
use("platzi_store")

db.iot.drop()

db.iot.insertMany([
  { _id: 1, sensor: "A001", date: "2022-01-01", readings: [1,2,3,4] },
  { _id: 2, sensor: "A001", date: "2022-01-02", readings: [1,2,3,4] },
  { _id: 3, sensor: "A002", date: "2022-01-01", readings: [1,2,3,4] },
  { _id: 4, sensor: "A002", date: "2022-01-02", readings: [1,2,3,4] },
])

db.iot.find()
```

## Delete doccs

```js
use("platzi_store")

db.products.drop()

db.products.insertMany([
  { _id: 1, name: "Producto 1", price: 100 },
  { _id: 2, name: "Producto 2", price: 100 },
  { _id: 3, name: "Producto 3", price: 100 },
  { _id: 4, name: "Producto 4", price: 400 },
  { _id: 5, name: "Producto 5", price: 500 },
  { _id: 6, name: "Producto 6", price: 600 },
])

db.products.find()

```

## Usando $eq y $ne

```js
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "ab", code: "123" }, qty: 15, tags: ["A", "B", "C"] },
  { _id: 2, item: { name: "cd", code: "123" }, qty: 20, tags: ["B"] },
  { _id: 3, item: { name: "ij", code: "456" }, qty: 25, tags: ["A", "B"] },
  { _id: 4, item: { name: "xy", code: "456" }, qty: 30, tags: ["B", "A"] },
  { _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: [["A", "B"], "C"] },
])

db.inventory.find()
```

## Regex

```js
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "ab", code: "123", description : "Single line description."    }, qty: 15, tags: ["A", "B", "C"] },
  { _id: 2, item: { name: "cd", code: "123", description : "First line\nSecond line"     }, qty: 20, tags: ["B"] },
  { _id: 3, item: { name: "ij", code: "456", description : "Many spaces before     line" }, qty: 25, tags: ["A", "B"] },
  { _id: 4, item: { name: "xy", code: "456", description : "Multiple\nline description"  }, qty: 30, tags: ["B", "A"] },
  { _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: [["A", "B"], "C"] },
])

db.inventory.find()
```

## Arrays Opertators

```js
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "ab", code: "123", description : "Single line description."    }, qty: 15, tags: [ "school", "book", "bag", "headphone", "appliance" ] },
  { _id: 2, item: { name: "cd", code: "123", description : "First line\nSecond line"     }, qty: 20, tags: [ "appliance", "school", "book" ] },
  { _id: 3, item: { name: "ij", code: "456", description : "Many spaces before     line" }, qty: 25, tags: [ "school", "book" ] },
  { _id: 4, item: { name: "xy", code: "456", description : "Multiple\nline description"  }, qty: 30, tags: [ "electronics", "school" ] },
  { _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: [ "appliance", "school" ] },
])

db.inventory.find()
```

```js
use("platzi_store");

db.survey.drop();

db.survey.insertMany([
  {
    _id: 1,
    results: [
      { product: "abc", score: 10 },
      { product: "xyz", score: 5 },
    ],
  },
  {
    _id: 2,
    results: [
      { product: "abc", score: 8 },
      { product: "xyz", score: 7 },
    ],
  },
  {
    _id: 3,
    results: [
      { product: "abc", score: 7 },
      { product: "xyz", score: 8 },
    ],
  },
  {
    _id: 4,
    results: [
      { product: "abc", score: 7 },
      { product: "def", score: 8 },
    ],
  },
]);

db.survey.find();
```

## Expresive operator

```js
use("platzi_store")

db.monthlyBudget.drop()

db.monthlyBudget.insertMany([
  { "_id" : 1, "category" : "food",    "budget": 400, "spent": 450 },
  { "_id" : 2, "category" : "drinks",  "budget": 100, "spent": 150 },
  { "_id" : 3, "category" : "clothes", "budget": 100, "spent": 50  },
  { "_id" : 4, "category" : "misc",    "budget": 500, "spent": 300 },
  { "_id" : 5, "category" : "travel",  "budget": 200, "spent": 650 }
])

db.monthlyBudget.find()
```

## Sort & Limit

```js
use("platzi_store")

db.categories.drop()

db.categories.insertMany([
  { _id: 1, name: "category 1" },
  { _id: 2, name: "category 2" },
  { _id: 3, name: "category 3" },
  { _id: 4, name: "category 4" },
  { _id: 5, name: "category 5" },
  { _id: 6, name: "category 6" },
  { _id: 7, name: "category 7" },
  { _id: 8, name: "category 8" },
  { _id: 9, name: "category 9" },
  { _id: 10, name: "category 10" },
])

db.categories.find()
```