Claro. Aquí tienes un **README.md** con estilo profesional para GitHub, listo para copiar y pegar en tu repositorio.

# 📚 MongoDB Sample Training Database

![MongoDB](https://img.shields.io/badge/MongoDB-Database-green?logo=mongodb)
![JavaScript](https://img.shields.io/badge/Language-JavaScript-yellow)
![License](https://img.shields.io/badge/License-MIT-blue)

## 📖 Descripción

Este proyecto contiene un conjunto de datos diseñado para practicar MongoDB desde un nivel básico hasta avanzado.

La base de datos **sample_training** incluye varias colecciones relacionadas que permiten aprender los conceptos fundamentales de MongoDB:

* CRUD (Create, Read, Update, Delete)
* Aggregation Framework
* Relaciones utilizando `$lookup`
* Índices
* Text Search
* Geospatial Queries
* Arrays y documentos embebidos
* Operadores de actualización
* Optimización de consultas

---

# Base de datos

```javascript
use("sample_training")
db.dropDatabase()
```

---

# Colecciones incluidas

| Colección | Descripción                              |
| --------- | ---------------------------------------- |
| students  | Información de estudiantes               |
| products  | Catálogo de productos                    |
| orders    | Órdenes de compra                        |
| employees | Empleados de una empresa                 |
| books     | Libros para practicar búsquedas de texto |
| cities    | Ubicaciones geográficas (GeoJSON)        |

---

# Estructura del Proyecto

```
sample_training/
│
├── students.js
├── products.js
├── orders.js
├── employees.js
├── books.js
├── cities.js
├── indexes.js
└── README.md
```

---

# Colección Students

```javascript
db.students.insertMany([

{
    _id:1,
    first_name:"John",
    last_name:"Smith",
    age:22,
    gender:"Male",
    email:"john@email.com",
    city:"New York",
    country:"USA",
    courses:["MongoDB","Python","Java"],
    grades:[
        {course:"MongoDB",score:95},
        {course:"Python",score:88},
        {course:"Java",score:90}
    ],
    active:true
},

{
    _id:2,
    first_name:"Maria",
    last_name:"Lopez",
    age:25,
    gender:"Female",
    email:"maria@email.com",
    city:"Miami",
    country:"USA",
    courses:["NodeJS","MongoDB"],
    grades:[
        {course:"NodeJS",score:98},
        {course:"MongoDB",score:97}
    ],
    active:true
},

{
    _id:3,
    first_name:"Pedro",
    last_name:"Garcia",
    age:31,
    gender:"Male",
    email:"pedro@email.com",
    city:"Santo Domingo",
    country:"Dominican Republic",
    courses:["SQL","MongoDB"],
    grades:[
        {course:"SQL",score:81},
        {course:"MongoDB",score:78}
    ],
    active:false
},

{
    _id:4,
    first_name:"Ana",
    last_name:"Martinez",
    age:28,
    gender:"Female",
    email:"ana@email.com",
    city:"Madrid",
    country:"Spain",
    courses:["Python","Docker","MongoDB"],
    grades:[
        {course:"Python",score:99},
        {course:"Docker",score:90},
        {course:"MongoDB",score:92}
    ],
    active:true
}

])
```

---

# Colección Products

```javascript
db.products.insertMany([

{
_id:1,
name:"Laptop Dell",
category:"Electronics",
price:1250,
stock:20,
brand:"Dell",
rating:4.8,
tags:["Laptop","Computer","Windows"]
},

{
_id:2,
name:"iPhone 15",
category:"Phones",
price:1100,
stock:45,
brand:"Apple",
rating:4.9,
tags:["Phone","Apple"]
},

{
_id:3,
name:"Samsung TV",
category:"Electronics",
price:890,
stock:15,
brand:"Samsung",
rating:4.6,
tags:["TV","4K"]
},

{
_id:4,
name:"Gaming Mouse",
category:"Accessories",
price:65,
stock:120,
brand:"Logitech",
rating:4.5,
tags:["Gaming","Mouse"]
},

{
_id:5,
name:"Mechanical Keyboard",
category:"Accessories",
price:120,
stock:70,
brand:"Keychron",
rating:4.9,
tags:["Keyboard"]
}

])
```

---

# Colección Orders

```javascript
db.orders.insertMany([

{
_id:1,
student_id:1,
order_date:new Date(),
products:[
    {product_id:1,qty:1},
    {product_id:4,qty:2}
],
status:"Completed",
total:1380
},

{
_id:2,
student_id:2,
order_date:new Date(),
products:[
    {product_id:2,qty:1}
],
status:"Pending",
total:1100
},

{
_id:3,
student_id:3,
order_date:new Date(),
products:[
    {product_id:3,qty:1},
    {product_id:5,qty:1}
],
status:"Completed",
total:1010
}

])
```

---

# Colección Employees

```javascript
db.employees.insertMany([

{
_id:1,
name:"Robert Brown",
department:"IT",
salary:5500,
skills:["MongoDB","Linux","Docker"],
hireDate:new Date("2020-01-12")
},

{
_id:2,
name:"Laura Green",
department:"HR",
salary:3800,
skills:["Recruitment"],
hireDate:new Date("2019-10-01")
},

{
_id:3,
name:"Kevin White",
department:"IT",
salary:7200,
skills:["AWS","MongoDB","Python"],
hireDate:new Date("2018-05-20")
}

])
```

---

# Colección Books

```javascript
db.books.insertMany([

{
title:"MongoDB in Action",
pages:420,
price:50,
authors:["Kyle Banker"],
publisher:"Manning",
year:2023
},

{
title:"Learning Python",
pages:1600,
price:70,
authors:["Mark Lutz"],
publisher:"O'Reilly",
year:2022
},

{
title:"Docker Deep Dive",
pages:380,
price:45,
authors:["Nigel Poulton"],
publisher:"LeanPub",
year:2024
}

])
```

---

# Colección Cities

```javascript
db.cities.insertMany([

{
name:"New York",
location:{
type:"Point",
coordinates:[-74.0059,40.7128]
}
},

{
name:"Miami",
location:{
type:"Point",
coordinates:[-80.1918,25.7617]
}
},

{
name:"Santo Domingo",
location:{
type:"Point",
coordinates:[-69.9312,18.4861]
}
}

])
```

---

# Índices

```javascript
db.students.createIndex({email:1},{unique:true})

db.products.createIndex({price:1})

db.products.createIndex({category:1})

db.orders.createIndex({student_id:1})

db.books.createIndex({title:"text",publisher:"text"})

db.cities.createIndex({location:"2dsphere"})
```

---

# Ejercicios para practicar

## Consultas básicas

```javascript
db.students.find()
```

```javascript
db.students.find({age:{$gte:25}})
```

```javascript
db.products.find().sort({price:-1})
```

```javascript
db.products.find().limit(3)
```

---

## Actualizaciones

```javascript
db.students.updateOne(
{_id:1},
{$set:{age:23}}
)
```

```javascript
db.students.updateOne(
{_id:2},
{$push:{courses:"Docker"}}
)
```

---

## Eliminación

```javascript
db.students.deleteOne({_id:4})
```

---

## Aggregation

```javascript
db.products.aggregate([
{
$group:{
_id:"$category",
averagePrice:{$avg:"$price"},
count:{$sum:1}
}
}
])
```

---

## JOIN con `$lookup`

```javascript
db.orders.aggregate([
{
$lookup:{
from:"students",
localField:"student_id",
foreignField:"_id",
as:"student"
}
}
])
```

---

## `$unwind`

```javascript
db.orders.aggregate([
{
$unwind:"$products"
}
])
```

---

## Text Search

```javascript
db.books.find({
$text:{
$search:"MongoDB"
}
})
```

---

## Geospatial Query

```javascript
db.cities.find({
location:{
$near:{
$geometry:{
type:"Point",
coordinates:[-69.9,18.5]
},
$maxDistance:500000
}
}
})
```

---

# Objetivos de aprendizaje

Después de completar este proyecto podrás practicar:

* ✅ CRUD completo
* ✅ Filtros avanzados
* ✅ Arrays
* ✅ Documentos embebidos
* ✅ Aggregation Framework
* ✅ `$lookup`
* ✅ `$group`
* ✅ `$match`
* ✅ `$project`
* ✅ `$sort`
* ✅ `$limit`
* ✅ `$skip`
* ✅ Índices
* ✅ Text Search
* ✅ Geospatial Queries
* ✅ Optimización de consultas
* ✅ Modelado de datos en MongoDB

---

# Próximo nivel

Para un entorno más realista se recomienda ampliar el conjunto de datos con:

* 1,000 estudiantes
* 5,000 clientes
* 10,000 productos
* 100,000 órdenes
* 50 categorías
* 500 empleados
* 20 sucursales
* 1 millón de documentos para pruebas de rendimiento

Este volumen de información permite practicar optimización, creación de índices, análisis de rendimiento y consultas avanzadas, simulando un entorno de producción.

---

## Licencia

MIT License

---

**Desarrollado con ❤️ para aprender MongoDB desde cero hasta nivel avanzado.**
