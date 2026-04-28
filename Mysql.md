Du installierst zuerst das Paket:
```bash
npm install mysql2
```
Dann kannst du es in Node.js einbinden. Für dein MVC-Projekt würde ich direkt die Promise-Variante nehmen, weil sie mit async/await sauberer ist.

# 1. Datenbankverbindung anlegen

Lege zum Beispiel diese Datei an:
```bash
config/database.js
// config/database.js

const mysql = require('mysql2/promise');

const pool = mysql.createPool({
    host: 'localhost',
    user: 'root',
    password: '',
    database: 'testdatenbank',

    waitForConnections: true,
    connectionLimit: 10,
    queueLimit: 0
});

module.exports = pool;
```
Wichtig:
```
mysql2/promise = Version für async/await
createPool     = Verbindungspool statt einzelner Verbindung
```
Ein Pool ist für Webserver besser, weil nicht für jede Anfrage komplett neu verbunden werden muss.

# 2. Model mit MySQL verwenden

Zum Beispiel:
```js
models/userModel.js
// models/userModel.js

const db = require('../config/database.js');

async function getAllUsers() {
    const [rows] = await db.query('SELECT * FROM users');

    return rows;
}

async function getUserById(id) {
    const [rows] = await db.query(
        'SELECT * FROM users WHERE id = ?',
        [id]
    );

    return rows[0];
}

module.exports = {
    getAllUsers,
    getUserById
};
```
Das hier ist wichtig:
```sql
'SELECT * FROM users WHERE id = ?',
[id]
```
Das ? ist ein Platzhalter. mysql2 setzt den Wert sicher ein. Das schützt dich vor SQL-Injection.

# 3. Controller

controllers/userController.js
```js
// controllers/userController.js

const userModel = require('../models/userModel.js');

async function showUsers(req, res) {
    try {
        const users = await userModel.getAllUsers();

        res.json(users);
    } catch (error) {
        console.error(error);

        res.status(500).json({
            message: 'Fehler beim Laden der Benutzer'
        });
    }
}

async function showUserById(req, res) {
    try {
        const id = req.params.id;
        const user = await userModel.getUserById(id);

        if (!user) {
            return res.status(404).json({
                message: 'Benutzer nicht gefunden'
            });
        }

        res.json(user);
    } catch (error) {
        console.error(error);

        res.status(500).json({
            message: 'Fehler beim Laden des Benutzers'
        });
    }
}

module.exports = {
    showUsers,
    showUserById
};
```
# 4. Route
routes/userRoutes.js
```js
// routes/userRoutes.js

const express = require('express');
const userController = require('../controllers/userController.js');

const router = express.Router();

router.get('/users', userController.showUsers);
router.get('/users/:id', userController.showUserById);

module.exports = router;
```
# 5. In index.js einbinden
```js
// index.js

const express = require('express');

const mainRouter = require('./routes/mainRoutes.js');
const userRouter = require('./routes/userRoutes.js');

const app = express();

app.use(express.json());

app.use('/api', mainRouter);
app.use('/api', userRouter);

app.listen(3000, () => {
    console.log('Backend läuft auf http://localhost:3000');
});
```
Dann hättest du:
```
http://localhost:3000/api/users
http://localhost:3000/api/users/1
```
Minimales Test-Beispiel

Falls du erstmal nur testen willst, ohne MVC:

```js
// test-db.js

const mysql = require('mysql2/promise');

async function main() {
    const connection = await mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: '',
        database: 'testdatenbank'
    });

    const [rows] = await connection.query('SELECT 1 + 1 AS result');

    console.log(rows);

    await connection.end();
}

main();
```
Starten mit:
```
node test-db.js
```
Erwartete Ausgabe:
```
[ { result: 2 } ]
```
Mit .env sauberer machen

Installieren:
```
npm install dotenv
```
Datei:
```bash
.env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=testdatenbank
DB_CONNECTION_LIMIT=10
```
Dann config/database.js:
```js
// config/database.js

require('dotenv').config();

const mysql = require('mysql2/promise');

const pool = mysql.createPool({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME,

    waitForConnections: true,
    connectionLimit: Number(process.env.DB_CONNECTION_LIMIT) || 10,
    queueLimit: 0
});

module.exports = pool;
```
Und .env unbedingt in .gitignore eintragen:
```bash
.env
```
Merksatz
´´´
mysql2
= MySQL-Treiber für Node.js

mysql2/promise
= MySQL-Treiber mit async/await

createPool()
= Verbindungen wiederverwenden

db.query(sql, values)
= SQL ausführen

? Platzhalter
= Werte sicher einsetzen
´´´
Für dein MVC-Denken:

Route nimmt URL entgegen.
Controller entscheidet, was passieren soll.
Model fragt MySQL ab.
Controller gibt JSON oder HTML zurück.
