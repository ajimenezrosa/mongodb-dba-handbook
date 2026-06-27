# 📚 Curso de Introducción a MongoDB

# Capítulo 17 - Eliminación de Documentos en MongoDB: Métodos, Buenas Prácticas y Casos de Uso

---

# Objetivos de aprendizaje

Al finalizar este capítulo serás capaz de:

* Comprender cómo eliminar documentos en MongoDB.
* Utilizar correctamente `deleteOne()`.
* Eliminar múltiples documentos mediante `deleteMany()`.
* Eliminar una colección completa utilizando `drop()`.
* Conocer las diferencias entre cada método.
* Aplicar buenas prácticas para evitar pérdidas de información.
* Comprender el impacto de las operaciones de eliminación en ambientes de producción.

---

# Introducción

En cualquier sistema de bases de datos llega el momento en que ciertos datos dejan de ser necesarios.

Algunos ejemplos son:

* Productos descontinuados.
* Usuarios inactivos.
* Registros duplicados.
* Logs antiguos.
* Datos temporales.
* Información de pruebas.
* Inventario obsoleto.

MongoDB proporciona varios métodos para eliminar información dependiendo de la cantidad de documentos que deseamos borrar.

Los principales métodos son:

* `deleteOne()`
* `deleteMany()`
* `drop()`

Cada uno tiene un propósito específico y debe utilizarse con precaución.

---

# Preparando el entorno

Trabajaremos sobre una colección llamada **products**.

```javascript id="u9af5b"
use("platziStore")
```

Creamos algunos documentos de ejemplo.

```javascript id="5uh9fc"
db.products.drop()

db.products.insertMany([

{
    _id:1,
    name:"Laptop Dell",
    category:"Electronics",
    price:1200,
    stock:15
},

{
    _id:2,
    name:"Mouse Logitech",
    category:"Accessories",
    price:40,
    stock:80
},

{
    _id:3,
    name:"Mechanical Keyboard",
    category:"Accessories",
    price:100,
    stock:45
},

{
    _id:4,
    name:"Monitor Samsung",
    category:"Electronics",
    price:300,
    stock:20
},

{
    _id:5,
    name:"USB Cable",
    category:"Accessories",
    price:15,
    stock:150
}

])
```

Verificamos la información.

```javascript id="vjlwm4"
db.products.find().pretty()
```

---

# Eliminar un documento con `deleteOne()`

## ¿Qué hace?

Elimina únicamente el **primer documento** que coincida con el filtro especificado.

Si varios documentos cumplen la condición, solamente se elimina uno.

---

## Sintaxis

```javascript id="k6j6m0"
db.coleccion.deleteOne(
    { filtro }
)
```

---

## Ejemplo 1

Eliminar el producto con `_id = 1`.

```javascript id="3e9f90"
db.products.deleteOne(
{
    _id:1
}
)
```

MongoDB responde:

```javascript id="2h2lks"
{
    acknowledged:true,
    deletedCount:1
}
```

---

## Verificación

```javascript id="0z5bfr"
db.products.find()
```

El documento ya no existe.

---

# Eliminar utilizando ObjectId

Cuando MongoDB genera automáticamente el identificador, debe utilizarse el tipo `ObjectId`.

Ejemplo:

```javascript id="dvjlwm"
db.products.deleteOne({

_id:ObjectId("64d5a123456789abcdef1234")

})
```

---

# Eliminar múltiples documentos con `deleteMany()`

## ¿Qué hace?

Elimina todos los documentos que cumplan la condición especificada.

---

## Sintaxis

```javascript id="1vh61w"
db.coleccion.deleteMany(
    { filtro }
)
```

---

## Ejemplo

Eliminar todos los productos cuyo precio sea menor a 100.

```javascript id="hy4mgh"
db.products.deleteMany(

{

price:{

$lt:100

}

}

)
```

MongoDB responde:

```javascript id="mfrx1u"
{

acknowledged:true,

deletedCount:2

}
```

---

## Otro ejemplo

Eliminar todos los productos de la categoría:

```text id="55icuu"
Accessories
```

```javascript id="ysv2wq"
db.products.deleteMany(

{

category:"Accessories"

}

)
```

---

# Eliminar todos los documentos de una colección

Si queremos eliminar todos los documentos, existen dos posibilidades.

## Opción 1

```javascript id="0a7hwd"
db.products.deleteMany({})
```

Resultado:

Todos los documentos desaparecen.

La colección continúa existiendo.

---

## Opción 2

Utilizar:

```javascript id="lg3w5w"
drop()
```

---

# Método `drop()`

## ¿Qué hace?

Elimina completamente la colección.

No solamente elimina los documentos.

También elimina:

* Índices.
* Validaciones.
* Estadísticas.
* Configuración de la colección.

---

## Sintaxis

```javascript id="yotb2h"
db.products.drop()
```

MongoDB responde:

```javascript id="ifskpi"
true
```

La colección deja de existir.

---

# Diferencia entre `deleteMany({})` y `drop()`

| Método           | Elimina documentos | Elimina colección | Conserva índices |
| ---------------- | ------------------ | ----------------- | ---------------- |
| `deleteMany({})` | ✅                  | ❌                 | ✅                |
| `drop()`         | ✅                  | ✅                 | ❌                |

Esta diferencia es muy importante en ambientes de producción.

---

# Resultado de las operaciones

## deleteOne()

```javascript id="tlu8pm"
{

acknowledged:true,

deletedCount:1

}
```

---

## deleteMany()

```javascript id="yl7vwg"
{

acknowledged:true,

deletedCount:8

}
```

---

## drop()

```javascript id="0us1pj"
true
```

---

# Buenas prácticas

## Siempre verifica antes de eliminar

Ejecuta primero un `find()`.

Ejemplo:

```javascript id="94w9yc"
db.products.find({

category:"Accessories"

})
```

Después ejecuta:

```javascript id="z0hnzl"
deleteMany()
```

---

## Utiliza filtros específicos

Evita consultas ambiguas.

Incorrecto:

```javascript id="e8f4f4"
db.products.deleteMany({})
```

A menos que realmente quieras eliminar toda la colección.

---

## Realiza respaldos

Antes de eliminar información importante ejecuta:

```text id="vjlwmj"
mongodump
```

o realiza un respaldo mediante MongoDB Atlas.

---

## Utiliza ambientes de prueba

Nunca pruebes consultas destructivas directamente sobre producción.

---

## Revisa el valor de `deletedCount`

Siempre valida cuántos documentos fueron eliminados.

---

# Casos de uso reales

## Comercio Electrónico

Eliminar productos descontinuados.

```javascript id="afhy3g"
deleteMany()
```

---

## Recursos Humanos

Eliminar empleados de prueba.

---

## Sistemas IoT

Eliminar lecturas antiguas.

---

## Logs

Eliminar registros con más de un año.

---

## Sistemas Bancarios

Eliminar sesiones expiradas.

---

## Aplicaciones Web

Eliminar tokens vencidos.

---

# Comparación de métodos

| Método         | Cantidad de documentos | Uso recomendado                                  |
| -------------- | ---------------------- | ------------------------------------------------ |
| `deleteOne()`  | Uno                    | Eliminar un registro específico                  |
| `deleteMany()` | Muchos                 | Eliminaciones masivas con filtro                 |
| `drop()`       | Toda la colección      | Reiniciar o eliminar completamente una colección |

---

# Advertencias importantes

## `drop()` no puede deshacerse

Una vez ejecutado:

```javascript id="jqztxf"
db.products.drop()
```

Toda la colección desaparece.

No existe un comando de **Undo**.

---

## `deleteMany({})` también puede ser peligroso

Esta consulta:

```javascript id="91xvlt"
db.products.deleteMany({})
```

Elimina todos los documentos.

Aunque la colección permanece, la información se pierde.

---

## Siempre revisa el filtro

Antes de ejecutar:

```javascript id="nrd26d"
deleteMany()
```

ejecuta:

```javascript id="hq3iyh"
find()
```

con exactamente el mismo filtro.

---

# Ejercicios

## Ejercicio 1

Eliminar el producto cuyo `_id` sea igual a **3**.

---

## Ejercicio 2

Eliminar todos los productos cuyo precio sea menor a **50**.

---

## Ejercicio 3

Eliminar todos los productos cuya categoría sea **Electronics**.

---

## Ejercicio 4

Eliminar todos los documentos de la colección utilizando:

```javascript id="kfl44v"
deleteMany({})
```

---

## Ejercicio 5

Eliminar completamente la colección utilizando:

```javascript id="g03ry6"
drop()
```

---

## Ejercicio 6

Crear nuevamente la colección e insertar cinco productos de prueba.

---

# Resumen

En este capítulo aprendiste a eliminar documentos en MongoDB utilizando los tres métodos principales:

* **`deleteOne()`** para eliminar un único documento que cumpla una condición.
* **`deleteMany()`** para eliminar múltiples documentos utilizando un filtro.
* **`drop()`** para eliminar completamente una colección junto con sus documentos, índices y configuración.

También conociste las diferencias entre estos métodos, los riesgos asociados a las operaciones destructivas y las buenas prácticas recomendadas, como verificar previamente los documentos con `find()`, realizar respaldos antes de eliminar información importante y probar las consultas en entornos de desarrollo.

Dominar estas operaciones es fundamental para el mantenimiento de bases de datos MongoDB y constituye una habilidad esencial para administradores de bases de datos y desarrolladores que trabajan con grandes volúmenes de información.

---

## Próximo capítulo

**Capítulo 18 - Consultas Avanzadas en MongoDB: Proyecciones, Operadores Lógicos y Expresiones de Comparación**
