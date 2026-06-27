# 📚 Curso de Introducción a MongoDB

# Capítulo 16 - Upsert en MongoDB para Sistemas IoT y Actualizaciones Inteligentes

---

# Objetivos de aprendizaje

Al finalizar este capítulo serás capaz de:

* Comprender qué es un **Upsert**.
* Diferenciar entre **Insert**, **Update** y **Upsert**.
* Utilizar la opción `upsert:true`.
* Implementar modelos de datos para sensores IoT.
* Agregar información de manera incremental utilizando arrays.
* Corregir errores utilizando `$pop`.
* Optimizar aplicaciones evitando consultas innecesarias.
* Aplicar Upsert en proyectos reales.

---

# Introducción

En aplicaciones modernas es muy común recibir información continuamente desde múltiples dispositivos.

Algunos ejemplos son:

* Sensores IoT
* Dispositivos GPS
* Servidores
* Aplicaciones móviles
* Equipos médicos
* Sistemas de monitoreo
* Logs de aplicaciones
* Redes de telecomunicaciones

Todos estos dispositivos generan datos constantemente.

Una pregunta muy común es:

> **¿Debo insertar un nuevo documento o actualizar uno existente?**

Resolver esta decisión manualmente implica realizar varias consultas a la base de datos.

MongoDB ofrece una solución mucho más elegante mediante **Upsert**.

---

# ¿Qué es Upsert?

La palabra **Upsert** proviene de la combinación de dos operaciones:

```text
UPDATE + INSERT
```

Es decir:

* Si el documento existe → se actualiza.
* Si el documento no existe → se crea automáticamente.

Todo ocurre en **una sola operación**.

---

# ¿Por qué utilizar Upsert?

Sin Upsert normalmente el flujo sería:

```text
Buscar documento

↓

¿Existe?

↓

Sí ------------→ UPDATE

↓

No ------------→ INSERT
```

Esto implica:

* Dos consultas a la base de datos.
* Más código.
* Mayor latencia.
* Mayor consumo de recursos.

Con Upsert el flujo cambia a:

```text
UPDATE

↓

Si existe → Actualiza

↓

Si no existe → Inserta automáticamente
```

Una sola operación.

---

# Caso de estudio: Sensores IoT

Supongamos un sistema de monitoreo ambiental.

Cada sensor envía una lectura cada pocos minutos.

En lugar de crear miles de documentos por día, el modelo será:

> **Un sensor + una fecha = un documento**

Ejemplo:

```javascript
{
    sensor:"A0001",
    date:"2022-01-01",

    readings:[
        20,
        22,
        24,
        25,
        26
    ]
}
```

Cada nueva lectura simplemente se agrega al array.

---

# ¿Por qué MongoDB es ideal para IoT?

MongoDB ofrece varias ventajas para este tipo de aplicaciones:

* Escrituras muy rápidas.
* Documentos flexibles.
* Arrays dinámicos.
* Escalabilidad horizontal.
* Alta disponibilidad.
* Excelente rendimiento para millones de lecturas.

Por estas razones MongoDB es ampliamente utilizado en proyectos de Internet de las Cosas (IoT).

---

# Preparando el entorno

Trabajaremos con la base de datos:

```javascript
use("platziStore")
```

Creamos la colección.

```javascript
db.iot.drop()
```

Insertamos un documento inicial.

```javascript
db.iot.insertOne({

sensor:"A0001",

date:"2022-01-01",

readings:[

20,
22,
25

]

})
```

Consultamos la información.

```javascript
db.iot.find().pretty()
```

Resultado:

```javascript
{
    sensor:"A0001",

    date:"2022-01-01",

    readings:[
        20,
        22,
        25
    ]
}
```

---

# Solución tradicional

Sin utilizar Upsert normalmente escribiríamos:

```text
find()

↓

count()

↓

if()

↓

insertOne()

↓

updateOne()
```

En pseudocódigo:

```javascript
if(documentoExiste){

updateOne()

}else{

insertOne()

}
```

Aunque funciona, esta solución requiere varias operaciones.

---

# Agregando nuevas lecturas

Cuando el documento ya existe simplemente agregamos una lectura.

```javascript
db.iot.updateOne(

{
sensor:"A0001",
date:"2022-01-01"
},

{

$push:{
readings:30
}

}

)
```

Resultado:

```javascript
readings:[

20,
22,
25,
30

]
```

---

# Error común al utilizar `$push`

Muchos desarrolladores cometen este error.

Incorrecto:

```javascript
$push:{
readings:[
30
]
}
```

Esto produce:

```javascript
readings:[

20,
22,
25,

[
30
]

]
```

Observa que ahora existe un **sub-array**, lo cual rompe el modelo.

---

# Corrigiendo el error con `$pop`

MongoDB dispone del operador:

```javascript
$pop
```

Permite eliminar elementos de un array según su posición.

---

## Eliminar el último elemento

```javascript
db.iot.updateOne(

{
sensor:"A0001"
},

{

$pop:{
readings:1
}

}

)
```

Resultado:

```javascript
readings:[

20,
22,
25

]
```

---

## Eliminar el primer elemento

```javascript
db.iot.updateOne(

{
sensor:"A0001"
},

{

$pop:{
readings:-1
}

}

)
```

Resultado:

```javascript
readings:[

22,
25

]
```

---

# ¿Qué es Upsert?

Upsert es una opción disponible en los métodos de actualización.

Su sintaxis es:

```javascript
{
upsert:true
}
```

---

# Sintaxis General

```javascript
db.coleccion.updateOne(

{ filtro },

{ actualizacion },

{

upsert:true

}

)
```

---

# Primer ejemplo

Supongamos que el sensor todavía no ha enviado información para el día:

```text
2022-01-04
```

Ejecutamos:

```javascript
db.iot.updateOne(

{

sensor:"A0001",

date:"2022-01-04"

},

{

$push:{
readings:23
}

},

{

upsert:true

}

)
```

---

# ¿Qué ocurre?

MongoDB busca el documento.

No lo encuentra.

Entonces crea automáticamente:

```javascript
{

sensor:"A0001",

date:"2022-01-04",

readings:[

23

]

}
```

Sin utilizar:

```javascript
insertOne()
```

---

# Segunda lectura del mismo día

Más tarde llega otra lectura.

Ejecutamos exactamente la misma consulta.

```javascript
db.iot.updateOne(

{

sensor:"A0001",

date:"2022-01-04"

},

{

$push:{
readings:25
}

},

{

upsert:true

}

)
```

Ahora sí encuentra el documento.

Resultado:

```javascript
{

sensor:"A0001",

date:"2022-01-04",

readings:[

23,
25

]

}
```

No crea un nuevo documento.

Simplemente agrega la lectura.

---

# Flujo de funcionamiento

```text
Nueva lectura

↓

updateOne()

↓

¿Existe documento?

↓

Sí ------------→ UPDATE

↓

No ------------→ INSERT

↓

Fin
```

Todo automáticamente.

---

# Ventajas de Upsert

* Una sola consulta.
* Menor latencia.
* Código más limpio.
* Mayor rendimiento.
* Menor tráfico de red.
* Reduce condiciones de carrera.
* Ideal para aplicaciones en tiempo real.

---

# Comparación

| Operación                      | Si existe                       | Si no existe            |
| ------------------------------ | ------------------------------- | ----------------------- |
| `insertOne()`                  | Error por duplicado (si aplica) | Inserta                 |
| `updateOne()`                  | Actualiza                       | No hace nada            |
| `updateOne(...,{upsert:true})` | Actualiza                       | Inserta automáticamente |

---

# Casos de uso reales

## Internet de las Cosas (IoT)

Guardar lecturas de sensores.

---

## Sistemas de Logs

Actualizar registros diarios.

---

## Monitoreo de Servidores

Guardar métricas por servidor y fecha.

---

## Comercio Electrónico

Actualizar inventario automáticamente.

---

## Perfiles de Usuario

Crear preferencias la primera vez que un usuario inicia sesión.

---

## Analítica

Actualizar estadísticas diarias.

---

## Sistemas Bancarios

Actualizar saldos consolidados.

---

## CRM

Crear historial de clientes automáticamente.

---

# Buenas prácticas

## Utiliza filtros únicos

Siempre identifica correctamente el documento.

Ejemplo:

```javascript
{

sensor:"A0001",

date:"2022-01-04"

}
```

---

## Evita documentos duplicados

El filtro debe representar una clave lógica del negocio.

En este ejemplo:

```text
Sensor + Fecha
```

representan un único documento.

---

## Utiliza índices

Para mejorar el rendimiento.

```javascript
db.iot.createIndex({

sensor:1,

date:1

})
```

---

## Utiliza `$push`

Cuando trabajes con arrays.

Evita reemplazar el array completo utilizando `$set`.

---

# Ejercicios

## Ejercicio 1

Crear un documento para el sensor:

```text
B0005
```

con la fecha:

```text
2022-05-10
```

utilizando únicamente **Upsert**.

---

## Ejercicio 2

Agregar tres nuevas lecturas al mismo documento.

---

## Ejercicio 3

Eliminar la última lectura utilizando `$pop`.

---

## Ejercicio 4

Eliminar la primera lectura utilizando `$pop`.

---

## Ejercicio 5

Crear un índice compuesto sobre:

* sensor
* date

y verificar que las búsquedas sean más eficientes.

---

# Resumen

En este capítulo aprendiste uno de los conceptos más importantes de MongoDB: **Upsert**, una técnica que combina las operaciones de inserción y actualización en una sola instrucción.

También estudiaste cómo modelar datos para sistemas IoT utilizando un documento por sensor y por día, donde todas las lecturas se almacenan en un único array. Además, conociste el uso de operadores como **`$push`** para agregar lecturas y **`$pop`** para corregir errores eliminando el primer o último elemento del array.

El uso de **`upsert:true`** simplifica el desarrollo de aplicaciones, reduce el número de consultas a la base de datos y mejora el rendimiento general del sistema. Por ello, es una de las herramientas más utilizadas en soluciones de IoT, monitoreo, sincronización de datos, sistemas de logs, analítica y muchas otras aplicaciones que requieren operaciones eficientes y escalables.

---

## Próximo capítulo

**Operadores Avanzados para Actualización de Arrays: `$each`, `$position`, `$slice` y `$sort`**
