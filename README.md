# MongoDB DBA HandBook

Repositorio personal de aprendizaje y práctica sobre **MongoDB**, creado como manual técnico para documentar consultas, configuraciones, ejercicios y ejemplos prácticos del curso.

Este repositorio forma parte de mi crecimiento profesional como **Administrador de Bases de Datos**, docente universitario y futuro especialista en **Data Engineering**.

---

## 👨‍💻 Autor

**José Alejandro Jiménez Rosa**
Database Administrator | SQL Server DBA | Docente Universitario
Universidad Católica Santo Domingo
Banco Popular Dominicano

---

## 🎯 Objetivo del repositorio

El objetivo de este repositorio es centralizar todo el contenido práctico relacionado con MongoDB, incluyendo:

* Configuración inicial del entorno.
* Uso de Docker Compose para levantar MongoDB localmente.
* Consultas CRUD.
* Operadores de comparación.
* Operadores para arrays.
* Expresiones avanzadas.
* Uso de regex.
* Upsert.
* Eliminación de documentos.
* Ordenamiento y límites.
* Ejercicios del curso.
* Buenas prácticas de documentación.

---

## 🧰 Tecnologías utilizadas

* MongoDB
* MongoDB Compass
* Docker
* Docker Compose
* JavaScript Mongo Shell
* GitHub
* EditorConfig

---

## 📁 Estructura sugerida del repositorio

```bash
mongodb-learning-lab/
│
├── README.md
├── .editorconfig
├── docker-compose.yml
│
├── 01-configuracion/
│   └── docker-compose.yml
│
├── 02-crud/
│   ├── insert-many.js
│   ├── find.js
│   └── delete-docs.js
│
├── 03-operadores-comparacion/
│   └── eq-ne.js
│
├── 04-arrays/
│   ├── array-operators.js
│   └── array-update-operators.js
│
├── 05-regex/
│   └── regex.js
│
├── 06-upsert/
│   └── upsert.js
│
├── 07-expresiones/
│   └── expressive-operator.js
│
├── 08-sort-limit/
│   └── sort-limit.js
│
└── docs/
    └── notas.md
```

---

# Configuración del proyecto

## Archivo `.editorconfig`

```ini
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
max_line_length = off

[CHANGELOG.md]
indent_size = false
```

---

## Docker Compose para MongoDB

```yaml
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

Para iniciar el contenedor:

```bash
docker compose up -d
```

Para detenerlo:

```bash
docker compose down
```

---

# Ejercicios MongoDB

## Base de datos utilizada

```javascript
use("platzi_store")
```

---

## Array Update Operators

```javascript
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "item ab", code: "123", description: "Single line description." }, qty: 15, tags: ["school", "book", "bag", "headphone", "appliance"] },
  { _id: 2, item: { name: "item cd", code: "123", description: "First line\nSecond line" }, qty: 20, tags: ["appliance", "school", "book"] },
  { _id: 3, item: { name: "item ij", code: "456", description: "Many spaces before     line" }, qty: 25, tags: ["school", "book"] },
  { _id: 4, item: { name: "item xy", code: "456", description: "Multiple\nline description" }, qty: 30, tags: ["electronics", "school"] },
  { _id: 5, item: { name: "item mn", code: "000" }, qty: 20, tags: ["appliance", "school"] }
])

db.inventory.find()
```

---

## Upsert

```javascript
use("platzi_store")

db.iot.drop()

db.iot.insertMany([
  { _id: 1, sensor: "A001", date: "2022-01-01", readings: [1, 2, 3, 4] },
  { _id: 2, sensor: "A001", date: "2022-01-02", readings: [1, 2, 3, 4] },
  { _id: 3, sensor: "A002", date: "2022-01-01", readings: [1, 2, 3, 4] },
  { _id: 4, sensor: "A002", date: "2022-01-02", readings: [1, 2, 3, 4] }
])

db.iot.find()
```

---

## Delete Documents

```javascript
use("platzi_store")

db.products.drop()

db.products.insertMany([
  { _id: 1, name: "Producto 1", price: 100 },
  { _id: 2, name: "Producto 2", price: 100 },
  { _id: 3, name: "Producto 3", price: 100 },
  { _id: 4, name: "Producto 4", price: 400 },
  { _id: 5, name: "Producto 5", price: 500 },
  { _id: 6, name: "Producto 6", price: 600 }
])

db.products.find()
```

---

## Operadores `$eq` y `$ne`

```javascript
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "ab", code: "123" }, qty: 15, tags: ["A", "B", "C"] },
  { _id: 2, item: { name: "cd", code: "123" }, qty: 20, tags: ["B"] },
  { _id: 3, item: { name: "ij", code: "456" }, qty: 25, tags: ["A", "B"] },
  { _id: 4, item: { name: "xy", code: "456" }, qty: 30, tags: ["B", "A"] },
  { _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: [["A", "B"], "C"] }
])

db.inventory.find()
```

---

## Regex

```javascript
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "ab", code: "123", description: "Single line description." }, qty: 15, tags: ["A", "B", "C"] },
  { _id: 2, item: { name: "cd", code: "123", description: "First line\nSecond line" }, qty: 20, tags: ["B"] },
  { _id: 3, item: { name: "ij", code: "456", description: "Many spaces before     line" }, qty: 25, tags: ["A", "B"] },
  { _id: 4, item: { name: "xy", code: "456", description: "Multiple\nline description" }, qty: 30, tags: ["B", "A"] },
  { _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: [["A", "B"], "C"] }
])

db.inventory.find()
```

---

## Arrays Operators

```javascript
use("platzi_store")

db.inventory.drop()

db.inventory.insertMany([
  { _id: 1, item: { name: "ab", code: "123", description: "Single line description." }, qty: 15, tags: ["school", "book", "bag", "headphone", "appliance"] },
  { _id: 2, item: { name: "cd", code: "123", description: "First line\nSecond line" }, qty: 20, tags: ["appliance", "school", "book"] },
  { _id: 3, item: { name: "ij", code: "456", description: "Many spaces before     line" }, qty: 25, tags: ["school", "book"] },
  { _id: 4, item: { name: "xy", code: "456", description: "Multiple\nline description" }, qty: 30, tags: ["electronics", "school"] },
  { _id: 5, item: { name: "mn", code: "000" }, qty: 20, tags: ["appliance", "school"] }
])

db.inventory.find()
```

---

## Arrays con documentos embebidos

```javascript
use("platzi_store")

db.survey.drop()

db.survey.insertMany([
  {
    _id: 1,
    results: [
      { product: "abc", score: 10 },
      { product: "xyz", score: 5 }
    ]
  },
  {
    _id: 2,
    results: [
      { product: "abc", score: 8 },
      { product: "xyz", score: 7 }
    ]
  },
  {
    _id: 3,
    results: [
      { product: "abc", score: 7 },
      { product: "xyz", score: 8 }
    ]
  },
  {
    _id: 4,
    results: [
      { product: "abc", score: 7 },
      { product: "def", score: 8 }
    ]
  }
])

db.survey.find()
```

---

## Expressive Operator

```javascript
use("platzi_store")

db.monthlyBudget.drop()

db.monthlyBudget.insertMany([
  { _id: 1, category: "food", budget: 400, spent: 450 },
  { _id: 2, category: "drinks", budget: 100, spent: 150 },
  { _id: 3, category: "clothes", budget: 100, spent: 50 },
  { _id: 4, category: "misc", budget: 500, spent: 300 },
  { _id: 5, category: "travel", budget: 200, spent: 650 }
])

db.monthlyBudget.find()
```

---

## Sort & Limit

```javascript
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
  { _id: 10, name: "category 10" }
])

db.categories.find()
```

---

# Buenas prácticas

* No subir contraseñas reales al repositorio.
* No subir cadenas de conexión de producción.
* No subir backups con datos sensibles.
* Usar datos ficticios para prácticas.
* Documentar cada consulta con su objetivo.
* Mantener los scripts separados por tema.
* Usar nombres claros para carpetas y archivos.
* Agregar comentarios cuando una consulta sea compleja.

---

# Estado del repositorio

🚧 Repositorio en construcción
📚 Contenido basado en prácticas de MongoDB
🧪 Enfoque práctico para aprendizaje, documentación y consulta futura

---

# Licencia

Este repositorio puede ser utilizado con fines educativos y de referencia personal.

---

> “La práctica constante convierte el conocimiento técnico en experiencia profesional.”
