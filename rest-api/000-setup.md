# Setup

## Express App

1. `npm init -y`
    - Untuk generate package.json
2. `npm install --save express sequelize pg bcryptjs jsonwebtoken cors`
    - `express`: Web framework
    - `sequelize`: ORM
    - `pg`: Client untuk sambungin PostgreSQL ke JavaScript
    - `bcryptjs`: Untuk hash password
    - `jsonwebtoken`: Untuk generate/verify access token (keperluan auth)
    - `cors`: Untuk membuka akses server kita agar bisa diakses oleh client

Typical `app.js`:

```javascript
const express = require('express')
const cors = require('cors')
const app = express()

// Route definitions
const routes = require('./routes/')

// kalau env var PORT undefined, defaults to 3000
const PORT = process.env.PORT || 3000

// membuka akses server kita agar bisa diakses oleh client
app.use(cors())

// memungkinkan express app kita menerima req.body dalam bentuk x-www-form-urlencoded
app.use(express.urlencoded({ extended: true }))

// memungkinkan express app kita menerima req.body dalam bentuk json
app.use(express.json())

// pasang routes ke app
app.use(routes)

app.listen(PORT, function () {
    console.log('Express app listening on port', PORT)
})
```

## DB Setup

1. `npx sequelize-cli init`
2. Ubah `config.json` hasil init, sesuai dengan credentials yang kita pakai di PostgreSQL
3. `npx sequelize-cli db:create`
    - Untuk membuat database

Useful commands:

-   `npx sequelize-cli model:generate --name ModelName --attributes column:datatype,column:datatype`
    -   Untuk membuat model + migration
-   `npx sequelize-cli db:migrate`
    -   Untuk menjalankan semua migration (create table, add column, etc.)
-   `npx sequelize-cli db:migrate:undo`
    -   Untuk undo migration terakhir
-   `npx sequelize-cli db:migrate:undo:all`
    -   Untuk undo semua migration
-   `npx sequelize-cli seed:generate --name seed_name`
    -   Untuk membuat seed file
-   `npx sequelize-cli db:seed:all`
    -   Untuk menjalankan semua seed
