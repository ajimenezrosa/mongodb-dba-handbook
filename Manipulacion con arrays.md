# 📚 Curso de Introducción a MongoDB

# Capítulo 15 - Manipulación de Arrays con `$push`, `$pull` y `$in`

---

# Objetivos de aprendizaje

Al finalizar este capítulo serás capaz de:

* Comprender cómo MongoDB almacena arrays dentro de los documentos.
* Agregar elementos a un array utilizando `$push`.
* Eliminar elementos específicos mediante `$pull`.
* Remover múltiples valores utilizando `$pull` junto con `$in`.
* Diferenciar cuándo utilizar `$set`, `$unset`, `$push` y `$pull`.
* Aplicar buenas prácticas al modificar arrays.

---

# Introducción

Una de las características más poderosas de MongoDB es su capacidad para almacenar **arrays** directamente dentro de un documento.

Por ejemplo, un producto puede contener una lista de etiquetas (tags), categorías, imágenes, comentarios o cualquier otro conjunto de datos relacionados.

```javascript
{
    _id: 1,
    item: {
        name: "Laptop",
        code: "LP100"
    },
    quantity: 20,
    tags: [
        "electronics",
        "computer",
        "office"
    ]
}
```

Modificar estos arrays manualmente sería poco eficiente.

MongoDB proporciona operadores especializados para manipular listas sin necesidad de reemplazar el documento completo.

Los principales operadores son:

* `$push`
* `$pull`
* `$in`

---

# Preparando el entorno

Para practicar utilizaremos la base de datos **platziStore** y la colección **inventory**.

```javascript
use("platziStore")
```

Antes de comenzar es recomendable eliminar cualquier dato previo.

```javascript
db.inventory.drop()
```

Luego insertamos algunos documentos.

```javascript
db.inventory.insertMany([

{
    _id:1,
    item:{
        name:"Laptop",
        code:"LP100",
        description:"Gaming Laptop"
    },
    quantity:15,
    tags:[
        "electronics",
        "computer",
        "office"
    ]
},

{
    _id:2,
    item:{
        name:"Headphones",
        code:"HP200",
        description:"Wireless"
    },
    quantity:50,
    tags:[
        "electronics",
        "audio",
        "music"
    ]
},

{
    _id:3,
    item:{
        name:"Notebook",
        code:"NB300",
        description:"School Notebook"
    },
    quantity:100,
    tags:[
        "school",
        "book",
        "office"
    ]
},

{
    _id:4,
    item:{
        name:"Tablet",
        code:"TB400",
        description:"Android Tablet"
    },
    quantity:30,
    tags:[
        "electronics",
        "school",
        "book"
    ]
},

{
    _id:5,
    item:{
        name:"Microwave",
        code:"MW500",
        description:"Kitchen Appliance"
    },
    quantity:12,
    tags:[
        "appliance",
        "kitchen",
        "home"
    ]
}

])
```

Verificamos los datos.

```javascript
db.inventory.find().pretty()
```

---

# ¿Qué es un Array en MongoDB?

Un array es una colección ordenada de valores almacenados dentro de un documento.

Ejemplo:

```javascript
tags:[
    "electronics",
    "school",
    "book"
]
```

Cada elemento puede ser:

* String
* Número
* Objeto
* Booleano
* Otro Array

---

# Operador `$push`

## ¿Qué hace?

Agrega un nuevo elemento al final de un array.

No reemplaza el array.

Simplemente añade un nuevo valor.

---

## Sintaxis

```javascript
db.coleccion.updateOne(
    { filtro },
    {
        $push:{
            campo:valor
        }
    }
)
```

---

## Ejemplo

Queremos agregar la etiqueta:

```text
headphone
```

al documento con `_id = 4`.

```javascript
db.inventory.updateOne(

{
    _id:4
},

{
    $push:{
        tags:"headphone"
    }
}

)
```

---

## Resultado

Antes

```javascript
tags:[
    "electronics",
    "school",
    "book"
]
```

Después

```javascript
tags:[
    "electronics",
    "school",
    "book",
    "headphone"
]
```

MongoDB devuelve:

```javascript
{
    acknowledged:true,
    matchedCount:1,
    modifiedCount:1
}
```

---

# Operador `$pull`

## ¿Qué hace?

Elimina todos los elementos del array que coincidan con un valor determinado.

---

## Sintaxis

```javascript
$pull
```

---

## Ejemplo

Eliminar la palabra:

```text
book
```

de todos los documentos.

```javascript
db.inventory.updateMany(

{},

{

$pull:{
    tags:"book"
}

}

)
```

---

## Resultado

Antes

```javascript
tags:[
    "school",
    "book",
    "office"
]
```

Después

```javascript
tags:[
    "school",
    "office"
]
```

---

MongoDB responde:

```javascript
{
    acknowledged:true,
    matchedCount:5,
    modifiedCount:3
}
```

---

## ¿Por qué solo modificó 3 documentos?

Aunque los cinco documentos fueron evaluados (`matchedCount = 5`), únicamente tres contenían el valor **book**.

Los otros dos no necesitaban cambios.

---

# Operador `$in`

## ¿Qué hace?

Busca uno o varios valores dentro de una lista.

Generalmente se combina con:

* `$pull`
* `$match`
* `$find`

---

## Ejemplo

Eliminar varias etiquetas simultáneamente.

```javascript
db.inventory.updateMany(

{},

{

$pull:{

tags:{

$in:[
"appliance",
"school"
]

}

}

}

)
```

---

## Resultado

Antes

```javascript
tags:[
    "electronics",
    "school",
    "book"
]
```

Después

```javascript
tags:[
    "electronics",
    "book"
]
```

Si el array contiene:

```javascript
[
"appliance",
"kitchen",
"school"
]
```

Después

```javascript
[
"kitchen"
]
```

---

# Combinando `$pull` con `$in`

Esta es una de las combinaciones más utilizadas en MongoDB.

Permite eliminar varios elementos en una sola operación.

```javascript
$pull + $in
```

En lugar de ejecutar:

```javascript
$pull:{
    tags:"school"
}
```

y luego:

```javascript
$pull:{
    tags:"appliance"
}
```

podemos hacer:

```javascript
$pull:{
    tags:{
        $in:[
            "school",
            "appliance"
        ]
    }
}
```

Esto reduce el número de operaciones y mejora el rendimiento.

---

# Comparación de operadores

| Operador | Función                                                |
| -------- | ------------------------------------------------------ |
| `$push`  | Agrega un elemento al final del array.                 |
| `$pull`  | Elimina elementos que coincidan con una condición.     |
| `$in`    | Busca múltiples valores dentro de un array o consulta. |
| `$set`   | Modifica un campo completo.                            |
| `$unset` | Elimina un campo del documento.                        |

---

# Ejemplos prácticos

## Agregar un nuevo tag

```javascript
db.inventory.updateOne(
{
    _id:2
},
{
    $push:{
        tags:"bluetooth"
    }
}
)
```

---

## Eliminar una categoría

```javascript
db.inventory.updateOne(
{
    _id:2
},
{
    $pull:{
        tags:"audio"
    }
}
)
```

---

## Eliminar varias categorías

```javascript
db.inventory.updateMany(
{},
{
$pull:{
tags:{
$in:[
"electronics",
"office"
]
}
}
}
)
```

---

# Buenas prácticas

## Verifica primero los documentos

Antes de actualizar:

```javascript
db.inventory.find()
```

---

## Evita duplicados

`$push` puede insertar valores repetidos.

Si deseas evitar duplicados utiliza:

```javascript
$addToSet
```

Ejemplo:

```javascript
db.inventory.updateOne(
{
    _id:1
},
{
$addToSet:{
tags:"electronics"
}
}
)
```

Si el valor ya existe, MongoDB no lo agregará nuevamente.

---

## No reemplaces el array completo

Evita:

```javascript
$set:{
tags:[
"nuevo"
]
}
```

A menos que realmente quieras reemplazar toda la lista.

---

# Casos de uso reales

## Comercio Electrónico

Agregar nuevas categorías a un producto.

```javascript
$push
```

---

## Redes Sociales

Eliminar hashtags.

```javascript
$pull
```

---

## Sistemas de Inventario

Eliminar múltiples etiquetas obsoletas.

```javascript
$pull + $in
```

---

## Gestión de Usuarios

Agregar nuevos roles.

```javascript
$push
```

---

# Ejercicios

## Ejercicio 1

Agrega la etiqueta:

```text
gaming
```

al producto con `_id = 1`.

---

## Ejercicio 2

Elimina la etiqueta:

```text
music
```

del documento correspondiente.

---

## Ejercicio 3

Elimina las etiquetas:

```text
electronics
office
school
```

utilizando una sola consulta.

---

## Ejercicio 4

Utiliza `$addToSet` para agregar la etiqueta:

```text
office
```

sin permitir duplicados.

---

## Ejercicio 5

Consulta todos los documentos cuyo array `tags` contenga el valor:

```text
electronics
```

---

# Resumen

En este capítulo aprendiste cómo manipular arrays de manera eficiente utilizando los operadores especializados de MongoDB.

Los operadores estudiados fueron:

* **`$push`** → Agrega un elemento al final de un array.
* **`$pull`** → Elimina elementos que coincidan con un valor o condición.
* **`$in`** → Permite trabajar con múltiples valores en una sola operación.
* **`$addToSet`** → Agrega un elemento únicamente si aún no existe.

Dominar estos operadores es esencial para trabajar con documentos complejos y modelos de datos flexibles, una de las principales ventajas de MongoDB frente a las bases de datos relacionales.

---

## Próximo capítulo

**Actualización Avanzada de Arrays con Operadores Posicionales (`$`, `$[]` y `$[<identifier>]`)**
