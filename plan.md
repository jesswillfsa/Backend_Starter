# Complete Backend Setup & Testing Plan

This guide walks you through creating a fully functioning backend step by step. Even if you’ve never done this before, just follow along! Before proceeding, make sure you've completed the setup steps in the README.

## Step 3: Create Your Project Files

1. Create the main file (`index.js`). You can do this in your terminal:

```bash
touch index.js
```

Or directly in your code editor (e.g., VSCode: Right-click in the file explorer → New File → `index.js`).

2. `.env` and `.gitignore` setup steps are covered in the README — make sure you’ve done those before continuing!

## Step 4: Set Up PostgreSQL

1. Create a database:

```sql
CREATE DATABASE your_database_name;
```

2. Connect to your database:

```bash
psql -d your_database_name
```

3. Create tables (Example):

> **Tip:** Think carefully about your database structure. How many tables do you need? What relationships exist between them? [Here’s a guide](https://www.postgresql.org/docs/current/ddl.html) on designing relational databases.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    password VARCHAR(255)
);
```

4. Add test data (optional):

```sql
INSERT INTO users (name, email, password) VALUES ('John Doe', 'john@example.com', 'hashed_password');
```

5. Dropping a table (if needed):

```sql
DROP TABLE users;
```

> **Tip:** Dropping a table deletes it and all its data, so use this carefully during development.

## Step 5: Build Your Backend (index.js)

Let’s break this down step by step so you understand what’s happening!

### Step 5.1: Set Up Your Server

```javascript
require('dotenv').config();
const express = require('express');
const cors = require('cors');

const app = express();
const port = 3000;

app.use(cors());
app.use(express.json());

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### Step 5.2: Connect to the Database

```javascript
const { Client } = require('pg');

const client = new Client({
  user: process.env.DB_USER,
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  password: process.env.DB_PASSWORD,
  port: process.env.DB_PORT
});

client.connect();
```

### Step 5.3: Add User Registration

```javascript
const bcrypt = require('bcryptjs');

app.post('/register', async (req, res) => {
  const { name, email, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const result = await client.query(
    'INSERT INTO users (name, email, password) VALUES ($1, $2, $3) RETURNING id, name, email',
    [name, email, hashedPassword]
  );
  res.json(result.rows[0]);
});
```

### Step 5.4: Add User Login & JWT Authentication

```javascript
const jwt = require('jsonwebtoken');

app.post('/login', async (req, res) => {
  const { email, password } = req.body;
  const result = await client.query('SELECT * FROM users WHERE email = $1', [email]);
  if (result.rows.length === 0) return res.status(401).json({ error: 'Invalid credentials' });

  const user = result.rows[0];
  const isMatch = await bcrypt.compare(password, user.password);
  if (!isMatch) return res.status(401).json({ error: 'Invalid credentials' });

  const token = jwt.sign({ id: user.id, email: user.email }, process.env.JWT_SECRET, { expiresIn: '1h' });
  res.json({ token });
});
```

### Step 5.5: Add a Protected Route

```javascript
app.get('/protected', (req, res) => {
  const token = req.headers['authorization'];
  if (!token) return res.status(401).json({ error: 'Access denied' });

  jwt.verify(token, process.env.JWT_SECRET, (err, decoded) => {
    if (err) return res.status(401).json({ error: 'Invalid token' });
    res.json({ message: 'Protected data', user: decoded });
  });
});
```

## Step 6: Test Your Backend

1. Start the server:

```bash
npm run dev
```

2. Use [Postman](https://www.postman.com/) to test your routes:

- **Register a user:** POST `/register`
- **Login a user:** POST `/login`
- **Access protected route:** GET `/protected` with `Authorization: Bearer <token>`

## Troubleshooting & Debugging

- **Connection errors:** Check database credentials and ensure PostgreSQL is running.
- **JWT errors:** Double-check the `JWT_SECRET` in your `.env` file.
- **Port conflicts:** Make sure nothing else is using port 3000.

With these steps, you’ll have a fully functioning backend! 🚀

