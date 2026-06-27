# 📚 Curso de Introducción a MongoDB

# Capítulo 14 - Actualización Masiva de Documentos con `updateMany()`

---

# Objetivos de aprendizaje

Al finalizar este capítulo serás capaz de:

* Comprender cuándo utilizar `updateMany()`.
* Actualizar múltiples documentos de forma eficiente.
* Modificar campos existentes.
* Agregar nuevos atributos.
* Renombrar campos.
* Eliminar atributos innecesarios.
* Aplicar buenas prácticas durante las actualizaciones masivas.

---

# Introducción

En una base de datos es muy común necesitar modificar cientos o incluso millones de documentos.

Por ejemplo:

* Incrementar salarios.
* Actualizar inventarios.
* Cambiar estados de pedidos.
* Agregar nuevos campos.
* Corregir errores en los datos.
* Migrar información a un nuevo modelo de datos.

Realizar estas modificaciones documento por documento sería extremadamente lento.

MongoDB resuelve este problema mediante el método:

```javascript
updateMany()
```

Este método permite modificar todos los documentos que cumplan una condición determinada utilizando una sola operación.

---

# Sintaxis General

```javascript
db.coleccion.updateMany(
    { filtro },
    { operadorDeActualizacion }
)
```

Donde:

* **Filtro:** determina qué documentos serán modificados.
* **Operador de actualización:** especifica los cambios que se aplicarán.

---

# Ejemplo básico

Supongamos la siguiente colección:

```javascript
{
    city:"Cleveland",
    pop:12500
}

{
    city:"Cleveland",
    pop:22000
}

{
    city:"Miami",
    pop:35000
}
```

Queremos aumentar la población de todas las ciudades llamadas **Cleveland**.

```javascript
db.zips.updateMany(
    { city:"Cleveland" },
    {
        $inc:{
            pop:1000
        }
    }
)
```

Resultado:

```javascript
{
    city:"Cleveland",
    pop:13500
}

{
    city:"Cleveland",
    pop:23000
}
```

Los documentos de otras ciudades no serán modificados.

---

# ¿Qué devuelve updateMany()?

Después de ejecutar la operación MongoDB devuelve un resultado similar al siguiente:

```javascript
{
    acknowledged:true,
    matchedCount:34,
    modifiedCount:34
}
```

## Significado

| Campo         | Descripción                                       |
| ------------- | ------------------------------------------------- |
| acknowledged  | Indica que la operación fue aceptada por MongoDB. |
| matchedCount  | Número de documentos que cumplen el filtro.       |
| modifiedCount | Número de documentos realmente modificados.       |

Si un documento ya contenía el valor indicado, puede aparecer en `matchedCount` pero no en `modifiedCount`.

---

# Operador `$inc`

Incrementa un valor numérico.

## Sintaxis

```javascript
$inc
```

Ejemplo:

```javascript
db.zips.updateMany(
    {
        city:"Cleveland"
    },
    {
        $inc:{
            pop:1000
        }
    }
)
```

Antes:

```javascript
pop:25000
```

Después:

```javascript
pop:26000
```

También puede disminuir valores.

```javascript
$inc:{
    stock:-5
}
```

---

# Operador `$set`

Permite:

* Modificar un valor existente.
* Crear un campo nuevo si no existe.

## Ejemplo

```javascript
db.zips.updateMany(
    {
        city:"Cleveland"
    },
    {
        $set:{
            myAttribute:"Hola"
        }
    }
)
```

Resultado:

```javascript
{
    city:"Cleveland",
    pop:25000,
    myAttribute:"Hola"
}
```

Si el atributo ya existía:

```javascript
myAttribute:"Adiós"
```

Después:

```javascript
myAttribute:"Hola"
```

---

# Operador `$rename`

Sirve para cambiar el nombre de un atributo.

No modifica el contenido.

## Ejemplo

Antes:

```javascript
{
    myAttribute:"Hola"
}
```

Consulta:

```javascript
db.zips.updateMany(
    {
        city:"Cleveland"
    },
    {
        $rename:{
            "myAttribute":"myData"
        }
    }
)
```

Resultado:

```javascript
{
    myData:"Hola"
}
```

---

# Operador `$unset`

Elimina completamente un campo.

## Ejemplo

```javascript
db.zips.updateMany(
    {
        city:"Cleveland"
    },
    {
        $unset:{
            myData:""
        }
    }
)
```

Resultado:

Antes:

```javascript
{
    city:"Cleveland",
    pop:25000,
    myData:"Hola"
}
```

Después:

```javascript
{
    city:"Cleveland",
    pop:25000
}
```

El campo desaparece completamente.

---

# Comparación de Operadores

| Operador  | Función                                   |
| --------- | ----------------------------------------- |
| `$inc`    | Incrementa o disminuye valores numéricos. |
| `$set`    | Modifica o crea un campo.                 |
| `$rename` | Cambia el nombre de un campo.             |
| `$unset`  | Elimina un campo.                         |

---

# Ejemplo completo

Supongamos esta colección:

```javascript
{
    name:"John",
    city:"Cleveland",
    salary:3500
}

{
    name:"Maria",
    city:"Cleveland",
    salary:4200
}
```

Aplicamos varias modificaciones.

```javascript
db.employees.updateMany(

{
    city:"Cleveland"
},

{

$inc:{
    salary:500
},

$set:{
    active:true
},

$rename:{
    salary:"monthlySalary"
}

}

)
```

Resultado:

```javascript
{
    name:"John",
    city:"Cleveland",
    monthlySalary:4000,
    active:true
}

{
    name:"Maria",
    city:"Cleveland",
    monthlySalary:4700,
    active:true
}
```

---

# Buenas prácticas

## Siempre filtra los documentos

Evita ejecutar:

```javascript
db.users.updateMany(
    {},
    {
        $set:{
            active:true
        }
    }
)
```

A menos que realmente quieras modificar todos los documentos.

---

## Verifica primero con `find()`

Antes de ejecutar una actualización:

```javascript
db.users.find({
    city:"Cleveland"
})
```

Después ejecuta:

```javascript
updateMany()
```

Esto evita errores.

---

## Haz respaldos antes de cambios masivos

Especialmente en ambientes de producción.

---

## Usa transacciones cuando sea necesario

Si varias colecciones deben actualizarse juntas, utiliza transacciones para mantener la consistencia.

---

# Ventajas de `updateMany()`

* Actualización masiva.
* Menor tiempo de ejecución.
* Reduce llamadas al servidor.
* Mantiene consistencia en los datos.
* Código más limpio.
* Ideal para migraciones de datos.

---

# Casos de uso reales

## Recursos Humanos

Incrementar salario a todos los empleados.

```javascript
$inc
```

---

## Comercio Electrónico

Agregar un nuevo atributo.

```javascript
$set
```

---

## Migración de Base de Datos

Renombrar columnas.

```javascript
$rename
```

---

## Limpieza de Datos

Eliminar atributos obsoletos.

```javascript
$unset
```

---

# Ejercicios

## Ejercicio 1

Incrementa en 10 unidades el stock de todos los productos de la categoría **Electronics**.

---

## Ejercicio 2

Agrega el campo:

```javascript
available:true
```

a todos los productos.

---

## Ejercicio 3

Renombra el campo:

```javascript
price
```

por

```javascript
unitPrice
```

---

## Ejercicio 4

Elimina el campo:

```javascript
description
```

de todos los documentos.

---

# Resumen

Durante este capítulo aprendiste a utilizar el método `updateMany()` para realizar actualizaciones masivas en MongoDB.

También conociste los operadores más utilizados para modificar documentos:

* `$inc` → Incrementar valores.
* `$set` → Crear o modificar atributos.
* `$rename` → Cambiar el nombre de un campo.
* `$unset` → Eliminar atributos.

Estas operaciones son fundamentales para administrar grandes volúmenes de información de forma eficiente y segura, y forman parte de las tareas cotidianas de cualquier desarrollador o administrador de bases de datos MongoDB.

---

## Próximo capítulo

**Eliminación Masiva de Documentos con `deleteMany()`**
