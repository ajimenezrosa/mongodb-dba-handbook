# 📚 Curso de Introducción a MongoDB

# Capítulo 18 - Operadores `$eq` y `$ne` en MongoDB: Comparaciones y Filtrado de Documentos

---

# Objetivos de aprendizaje

Al finalizar este capítulo serás capaz de:

* Comprender qué son los **Comparison Query Operators**.
* Utilizar correctamente los operadores `$eq` y `$ne`.
* Diferenciar entre consultas implícitas y explícitas.
* Consultar subdocumentos utilizando **Dot Notation**.
* Combinar operadores de consulta con operadores de actualización.
* Realizar actualizaciones masivas utilizando filtros.
* Aplicar buenas prácticas para escribir consultas más legibles y eficientes.

---

# Introducción

Uno de los aspectos más importantes al trabajar con MongoDB es la capacidad de localizar exactamente los documentos que necesitamos.

Para ello MongoDB dispone de una familia de operadores denominada:

```text
Comparison Query Operators
```

Estos operadores permiten comparar valores dentro de una consulta.

Entre los más utilizados se encuentran:

* `$eq` (Equal)
* `$ne` (Not Equal)
* `$gt` (Greater Than)
* `$gte` (Greater Than or Equal)
* `$lt` (Less Than)
* `$lte` (Less Than or Equal)
* `$in`
* `$nin`

En este capítulo estudiaremos los dos primeros.

---

# ¿Qué es un Comparison Query Operator?

Los **Comparison Query Operators** son operadores que permiten comparar el valor de un campo contra otro valor.

Su objetivo es decidir qué documentos serán devueltos o modificados.

Por ejemplo:

```text
¿El precio es igual a 100?

¿La cantidad es diferente de 20?

¿La edad es mayor que 18?
```

MongoDB responde estas preguntas mediante operadores de comparación.

---

# Preparando el entorno

Trabajaremos con la colección **inventory**.

```javascript
use("platziStore")
```

Creamos el conjunto de datos.

```javascript
db.inventory.drop()

db.inventory.insertMany([

{
    _id:1,
    item:{
        name:"Laptop",
        code:"100"
    },
    quantity:20
},

{
    _id:2,
    item:{
        name:"Mouse",
        code:"101"
    },
    quantity:15
},

{
    _id:3,
    item:{
        name:"Keyboard",
        code:"123"
    },
    quantity:20
},

{
    _id:4,
    item:{
        name:"Monitor",
        code:"123"
    },
    quantity:25
},

{
    _id:5,
    item:{
        name:"Tablet",
        code:"200"
    },
    quantity:30
}

])
```

Consultar los datos:

```javascript
db.inventory.find().pretty()
```

---

# Operador `$eq`

## ¿Qué significa?

`$eq` significa:

```text
Equal (Igual)
```

Devuelve únicamente los documentos cuyo valor sea exactamente igual al especificado.

---

# Uso implícito

MongoDB permite omitir `$eq` cuando únicamente queremos comparar igualdad.

Por ejemplo:

```javascript
db.inventory.find({

quantity:20

})
```

Internamente MongoDB interpreta esta consulta como:

```javascript
db.inventory.find({

quantity:{
$eq:20
}

})
```

Ambas consultas producen exactamente el mismo resultado.

---

# Uso explícito

También podemos escribir `$eq` de forma explícita.

```javascript
db.inventory.find({

quantity:{

$eq:20

}

})
```

Resultado:

```text
_id:1

_id:3
```

---

# ¿Cuándo utilizar la forma explícita?

La forma explícita mejora la legibilidad cuando combinamos varios operadores.

Ejemplo:

```javascript
db.inventory.find({

quantity:{

$eq:20

}

})
```

---

# Dot Notation

Los documentos pueden contener subdocumentos.

Ejemplo:

```javascript
{

item:{

name:"Laptop",

code:"100"

}

}
```

Para acceder a estos campos se utiliza la **Dot Notation**.

---

# Consultar por nombre

```javascript
db.inventory.find({

"item.name":"Laptop"

})
```

Resultado:

```text
Solo un documento.
```

---

# Consultar por código

```javascript
db.inventory.find({

"item.code":"123"

})
```

Resultado:

```text
Dos documentos.
```

---

# ¿Por qué usar comillas?

Cuando una clave contiene un punto (`.`), debe escribirse entre comillas.

Correcto:

```javascript
"item.name"
```

Incorrecto:

```javascript
item.name
```

Las comillas indican que se trata de una ruta dentro del documento.

---

# `$eq` con Dot Notation

También podemos utilizar la forma explícita.

```javascript
db.inventory.find({

"item.code":{

$eq:"123"

}

})
```

Obtendremos exactamente el mismo resultado.

---

# Operador `$ne`

## ¿Qué significa?

`$ne`

Significa:

```text
Not Equal
```

Devuelve todos los documentos cuyo valor sea diferente del especificado.

---

## Sintaxis

```javascript
db.inventory.find({

quantity:{

$ne:20

}

})
```

Resultado:

```text
_id:2

_id:4

_id:5
```

Los documentos con cantidad 20 quedan excluidos.

---

# Diferencia entre `$eq` y `$ne`

| Operador | Devuelve                                                    |
| -------- | ----------------------------------------------------------- |
| `$eq`    | Solo los documentos iguales al valor indicado.              |
| `$ne`    | Todos los documentos excepto los iguales al valor indicado. |

---

# Uso de `$ne` en actualizaciones

Los operadores de consulta también funcionan con:

* `updateOne()`
* `updateMany()`
* `findOneAndUpdate()`

Ejemplo:

Incrementar en 10 la cantidad de todos los productos cuya cantidad sea distinta de 20.

```javascript
db.inventory.updateMany(

{

quantity:{

$ne:20

}

},

{

$inc:{

quantity:10

}

}

)
```

Resultado:

Antes:

```text
15

25

30
```

Después:

```text
25

35

40
```

Los productos con cantidad igual a 20 permanecen sin cambios.

---

# Query Operators vs Update Operators

Es muy importante distinguir ambos conceptos.

## Query Operators

Seleccionan los documentos.

Ejemplo:

```javascript
$eq

$ne

$gt

$lt
```

Su función es responder:

```text
¿Qué documentos serán afectados?
```

---

## Update Operators

Modifican la información.

Ejemplo:

```javascript
$set

$inc

$push

$unset

$rename
```

Su función es responder:

```text
¿Qué cambios se aplicarán?
```

---

# Ejemplo combinado

```javascript
db.inventory.updateMany(

{

quantity:{

$ne:20

}

},

{

$inc:{

quantity:5

}

}

)
```

Aquí:

```text
$ne
```

selecciona los documentos.

Mientras que:

```text
$inc
```

realiza la modificación.

---

# Comparación de operadores

| Tipo             | Operadores                                                |
| ---------------- | --------------------------------------------------------- |
| Query Operators  | `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte`, `$in`, `$nin` |
| Update Operators | `$set`, `$inc`, `$push`, `$pull`, `$rename`, `$unset`     |

---

# Casos de uso reales

## Inventarios

Buscar productos con una cantidad específica.

---

## Comercio Electrónico

Excluir productos agotados.

---

## Recursos Humanos

Encontrar empleados distintos a un departamento.

---

## Sistemas Bancarios

Actualizar cuentas que no estén bloqueadas.

---

## Sistemas IoT

Buscar sensores cuya temperatura sea diferente de un valor esperado.

---

## Aplicaciones Médicas

Filtrar pacientes con resultados distintos a un diagnóstico.

---

# Buenas prácticas

## Utiliza la forma implícita cuando sea sencilla

```javascript
quantity:20
```

es perfectamente válida.

---

## Utiliza la forma explícita cuando combines operadores

Facilita la lectura y el mantenimiento del código.

---

## Aprovecha Dot Notation

Permite consultar campos internos sin necesidad de desnormalizar la información.

---

## Indexa los campos consultados frecuentemente

Por ejemplo:

```javascript
db.inventory.createIndex({

quantity:1

})
```

o

```javascript
db.inventory.createIndex({

"item.code":1

})
```

Esto mejora considerablemente el rendimiento de las búsquedas.

---

# Ejercicios

## Ejercicio 1

Buscar todos los productos cuya cantidad sea igual a **25** utilizando la forma implícita.

---

## Ejercicio 2

Realizar la misma consulta utilizando `$eq`.

---

## Ejercicio 3

Buscar todos los documentos cuyo código del producto sea **123**.

---

## Ejercicio 4

Buscar todos los productos cuya cantidad sea distinta de **20**.

---

## Ejercicio 5

Incrementar en **5** unidades la cantidad de todos los productos cuya cantidad sea diferente de **20**.

---

## Ejercicio 6

Crear un índice sobre el campo:

```javascript
quantity
```

y otro sobre:

```javascript
"item.code"
```

---

# Resumen

En este capítulo aprendiste a utilizar los operadores de comparación **`$eq`** y **`$ne`**, fundamentales para construir consultas precisas en MongoDB.

Descubriste que `$eq` puede utilizarse de forma implícita o explícita para buscar documentos con un valor exacto, mientras que `$ne` permite excluir aquellos que coinciden con un valor determinado. También aprendiste a consultar subdocumentos utilizando **Dot Notation**, una técnica esencial para acceder a campos anidados dentro de documentos.

Finalmente, comprendiste la diferencia entre **Query Operators**, que seleccionan los documentos, y **Update Operators**, que modifican la información, permitiéndote construir consultas más claras, eficientes y fáciles de mantener.

---

## Próximo capítulo

**Capítulo 19 - Operadores de Comparación en MongoDB: `$gt`, `$gte`, `$lt` y `$lte`**
