

# 🍃 Curso de MongoDB | Caputulo 19

![MongoDB](https://img.shields.io/badge/MongoDB-Database-47A248?style=for-the-badge\&logo=mongodb\&logoColor=white)
![NoSQL](https://img.shields.io/badge/NoSQL-Document%20Database-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-En%20Progreso-success?style=for-the-badge)

---

# 📚 Descripción

Este repositorio contiene toda la documentación, ejemplos, ejercicios y prácticas desarrolladas durante el curso de **MongoDB**.

El objetivo es aprender desde los conceptos fundamentales hasta consultas avanzadas utilizando MongoDB, Mongo Shell y MongoDB Compass.

---

# 🎯 Objetivos

Al finalizar este curso serás capaz de:

* Comprender qué es una base de datos NoSQL.
* Instalar MongoDB y MongoDB Compass.
* Crear bases de datos y colecciones.
* Insertar documentos.
* Realizar consultas básicas y avanzadas.
* Utilizar operadores de comparación.
* Trabajar con operadores lógicos.
* Manipular arreglos y subdocumentos.
* Actualizar documentos.
* Eliminar documentos.
* Crear índices.
* Optimizar consultas.
* Diseñar modelos de datos en MongoDB.

---

# 🛠 Tecnologías

* MongoDB Community Edition
* MongoDB Compass
* Mongo Shell (mongosh)
* Visual Studio Code

---

# 📂 Estructura del Repositorio

```text
MongoDB-Course/
│
├── README.md
├── datasets/
│   ├── inventory.json
│   ├── restaurants.json
│   └── sample_data.json
│
├── scripts/
│   ├── insertMany.js
│   ├── queries.js
│   ├── updates.js
│   └── deletes.js
│
├── images/
│
├── exercises/
│
└── notes/
```

---

# 📖 Contenido del Curso

## Módulo 1

* Introducción a MongoDB
* ¿Qué es NoSQL?
* Instalación
* MongoDB Compass
* Mongo Shell

---

## Módulo 2

### Bases de datos

```javascript
show dbs

use platzi_store

db
```

---

## Módulo 3

### Crear Colecciones

```javascript
db.createCollection("inventory")
```

---

## Módulo 4

### Insertar Documentos

```javascript
db.inventory.insertOne({
    item:{
        name:"Laptop",
        code:"100"
    },
    quantity:20,
    price:800
})
```

---

### Insertar múltiples documentos

```javascript
db.inventory.insertMany([
{
    item:{
        name:"Laptop",
        code:"100"
    },
    quantity:20,
    price:800
},
{
    item:{
        name:"Mouse",
        code:"101"
    },
    quantity:50,
    price:20
}
])
```

---

# 🔎 Consultas Básicas

Mostrar todos los documentos

```javascript
db.inventory.find()
```

---

Buscar por igualdad

```javascript
db.inventory.find({
    quantity:20
})
```

---

Buscar por código

```javascript
db.inventory.find({
    "item.code":"100"
})
```

---

# 📊 Operadores de Comparación

## Igual

```javascript
$eq
```

Ejemplo

```javascript
db.inventory.find({
    quantity:{
        $eq:20
    }
})
```

---

## Diferente

```javascript
$ne
```

```javascript
db.inventory.find({
    quantity:{
        $ne:20
    }
})
```

---

## Mayor que

```javascript
$gt
```

```javascript
db.inventory.find({
    quantity:{
        $gt:20
    }
})
```

---

## Mayor o igual

```javascript
$gte
```

```javascript
db.inventory.find({
    quantity:{
        $gte:20
    }
})
```

---

## Menor que

```javascript
$lt
```

```javascript
db.inventory.find({
    quantity:{
        $lt:20
    }
})
```

---

## Menor o igual

```javascript
$lte
```

```javascript
db.inventory.find({
    quantity:{
        $lte:20
    }
})
```

---

# 📈 Consultas por Rangos

Entre 20 y 40

```javascript
db.inventory.find({
    quantity:{
        $gte:20,
        $lte:40
    }
})
```

---

Mayor que 15 y menor que 30

```javascript
db.inventory.find({
    quantity:{
        $gt:15,
        $lt:30
    }
})
```

---

# 📌 Consultas con Subdocumentos

```javascript
db.inventory.find({
    "item.code":"100"
})
```

---

Subdocumento + rango

```javascript
db.inventory.find({
    "item.code":"100",
    quantity:{
        $gte:20,
        $lte:50
    }
})
```

---

# 💡 Operadores aprendidos

| Operador | Significado   |
| -------- | ------------- |
| $eq      | Igual         |
| $ne      | Diferente     |
| $gt      | Mayor que     |
| $gte     | Mayor o igual |
| $lt      | Menor que     |
| $lte     | Menor o igual |

---

# 🧪 Ejercicios

* Buscar productos con cantidad mayor a 20.
* Buscar productos entre 20 y 40 unidades.
* Buscar productos con precio mayor a 500.
* Buscar productos cuyo código sea "100".
* Buscar productos diferentes del código "101".

---

# 📚 Recursos

* Documentación oficial de MongoDB
* MongoDB Compass
* Mongo Shell
* MongoDB Atlas

---

# 🚀 Próximos temas

* Operadores Lógicos
* Arrays
* Arrays de Objetos
* $and
* $or
* $in
* $nin
* $exists
* $regex
* Aggregation Framework
* Índices
* Pipeline
* Lookup
* Relaciones entre documentos

---

# 👨‍💻 Autor

**José Alejandro Jiménez Rosa**

* Database Administrator (DBA)
* SQL Server | PostgreSQL | MongoDB
* Profesor Universitario
* Desarrollador Full Stack

---

# ⭐ Licencia

Este repositorio fue creado con fines educativos como material de apoyo para el aprendizaje de MongoDB.

Este formato está optimizado para GitHub e incluye una estructura escalable para que puedas ir agregando cada clase del curso, ejemplos, scripts y ejercicios conforme avances.
