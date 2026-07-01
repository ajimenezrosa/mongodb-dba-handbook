# Projection en MongoDB

## ¿Qué es Projection?

La **Projection** permite seleccionar únicamente los campos que deseas que MongoDB devuelva en una consulta.

En lugar de retornar el documento completo, puedes indicar exactamente qué información necesitas.

**Sintaxis:**

```javascript
db.coleccion.find(
    { filtro },
    { proyeccion }
)
```

* Primer parámetro → Filtro (`Query`)
* Segundo parámetro → Projection

---

# Ejemplo

Supongamos esta colección:

```json
{
    "_id": ObjectId("..."),
    "nombre": "Alejandro",
    "edad": 35,
    "pais": "República Dominicana",
    "profesion": "DBA",
    "salario": 120000
}
```

---

# Mostrar solo un campo

```javascript
db.personas.find(
    {},
    {
        nombre: 1
    }
)
```

Resultado

```json
{
    "_id": ObjectId("..."),
    "nombre": "Alejandro"
}
```

Observa que **_id aparece automáticamente**.

---

# Mostrar varios campos

```javascript
db.personas.find(
    {},
    {
        nombre: 1,
        profesion: 1
    }
)
```

Resultado

```json
{
    "_id": ObjectId("..."),
    "nombre": "Alejandro",
    "profesion": "DBA"
}
```

---

# Ocultar el _id

```javascript
db.personas.find(
    {},
    {
        _id: 0,
        nombre: 1,
        profesion: 1
    }
)
```

Resultado

```json
{
    "nombre": "Alejandro",
    "profesion": "DBA"
}
```

---

# Excluir campos

También puedes trabajar al revés.

```javascript
db.personas.find(
    {},
    {
        salario: 0
    }
)
```

Resultado

```json
{
    "_id": ObjectId("..."),
    "nombre": "Alejandro",
    "edad": 35,
    "pais": "República Dominicana",
    "profesion": "DBA"
}
```

Solo desaparece el campo **salario**.

---

# No mezclar inclusión y exclusión

❌ Incorrecto

```javascript
db.personas.find(
    {},
    {
        nombre: 1,
        edad: 0
    }
)
```

MongoDB genera un error.

---

## La única excepción

Puedes ocultar `_id` mientras incluyes otros campos.

```javascript
{
    _id: 0,
    nombre: 1,
    edad: 1
}
```

Esto **sí es válido**.

---

# Projection con filtros

```javascript
db.personas.find(
    {
        edad: { $gte: 30 }
    },
    {
        nombre: 1,
        profesion: 1,
        _id: 0
    }
)
```

Resultado

```json
{
    "nombre": "Alejandro",
    "profesion": "DBA"
}
```

---

# En MongoDB Compass

En **Project** puedes escribir:

```json
{
    "nombre": 1,
    "edad": 1
}
```

o

```json
{
    "_id": 0,
    "nombre": 1,
    "edad": 1
}
```

---

# Ejemplos con el dataset de inventario

Supongamos un documento:

```json
{
    "_id": ObjectId("..."),
    "name": "Kit de Herramientas",
    "location": "Almacén A32",
    "price": 75.56,
    "category": "Herramientas",
    "stock": 20
}
```

### Mostrar nombre y precio

```javascript
db.inventory.find(
    {},
    {
        name: 1,
        price: 1
    }
)
```

---

### Mostrar todo menos el stock

```javascript
db.inventory.find(
    {},
    {
        stock: 0
    }
)
```

---

### Mostrar solo la ubicación

```javascript
db.inventory.find(
    {},
    {
        _id: 0,
        location: 1
    }
)
```

---

### Mostrar productos con precio mayor a 50

```javascript
db.inventory.find(
    {
        price: { $gt: 50 }
    },
    {
        _id: 0,
        name: 1,
        price: 1
    }
)
```

Resultado

```json
{
    "name": "Kit de Herramientas",
    "price": 75.56
}
```

---

# Resumen rápido

| Projection                 | Significado                                  |
| -------------------------- | -------------------------------------------- |
| `campo: 1`                 | Incluir el campo                             |
| `campo: 0`                 | Excluir el campo                             |
| `_id: 0`                   | Ocultar el `_id`                             |
| `find({}, { nombre:1 })`   | Todos los documentos, solo el campo `nombre` |
| `find(filtro, projection)` | Consulta con filtro y selección de campos    |
| No mezclar `1` y `0`       | Solo `_id` puede combinarse con inclusiones  |

## Puntos clave para el examen

* **Projection** controla **qué campos devuelve** una consulta.
* Se especifica como el **segundo parámetro** de `find()`.
* `1` significa **incluir** un campo.
* `0` significa **excluir** un campo.
* El campo `_id` se devuelve por defecto.
* Para ocultarlo debes indicar `_id: 0`.
* No puedes mezclar inclusiones (`1`) y exclusiones (`0`) en la misma proyección, excepto con `_id`.
* `find({}, { nombre: 1 })` devuelve **todos los documentos**, pero únicamente el campo `nombre` (más `_id`, salvo que lo excluyas).
