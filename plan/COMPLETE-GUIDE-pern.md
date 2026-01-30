# COMPLETE GUIDE: Student Manager (Option B - PERN Stack)

> **This document contains BOTH the Project Plan (Strategy) AND the Offline Manual (Code).**
> Print this file or save it to a USB drive. It is your "Bible" for the project.

---

# PART 1: THE STRATEGY (The "What" and "Who")

## 1. Overview
**Stack**: PERN (Postgres, Express, React, Node).
**Goal**: Learn **Full Stack Development** by building the API yourself.
**Architecture**:
```mermaid
graph LR
    Browser[React App] --JSON Request--> Server[Node.js Express API]
    Server --SQL Query--> DB[(PostgreSQL)]
    DB --Row Data--> Server
    Server --JSON Response--> Browser
```

## 2. Team Role Distribution (6 Members)
*Split: 3 Backend, 3 Frontend.*

### Team Backend (The Server)
1.  **Backend Architect**:
    *   **Job**: Setup `server.js`, CORS, Headers.
    *   **Goal**: Make the server reply "Hello" to the internet.
2.  **Database Engineer**:
    *   **Job**: Write SQL scripts to create tables.
    *   **Goal**: Ensure data is structured correctly.
3.  **Controller Dev**:
    *   **Job**: Write the logic functions (`loginUser`, `markAttendance`).
    *   **Goal**: Connect the Routes to the Database.

### Team Frontend (The Client)
4.  **React Lead**:
    *   **Job**: Setup API Service (`axios`), handle Errors.
    *   **Goal**: Ensure React can talk to Node.js.
5.  **Auth UI Dev**:
    *   **Job**: Login Forms, Loading States.
    *   **Goal**: Handle "Wrong Password" UI nicely.
6.  **Dashboard UI Dev**:
    *   **Job**: Display lists of students.
    *   **Goal**: Make the JSON data look beautiful.

## 3. Hosting Strategy (The Triple Deploy)
1.  **Database**: Supabase/Neon (Free Tier).
2.  **Backend**: Render/Railway (Free Tier).
3.  **Frontend**: Vercel (Free Tier).

---

# PART 2: THE OFFLINE MANUAL (The "How-To")

## 1. Project Structure
```text
/my-project
  /server         <-- Backend (Node.js)
    index.js
    db.js
  /client         <-- Frontend (React)
    (Standard Vite structure)
```

## 2. Backend Manual (The Server Code)

### A) Commands
```bash
cd server
npm init -y
npm install express cors pg dotenv
```

### B) `server/index.js` (Main File)
```javascript
const express = require('express')
const cors = require('cors')
const { pool } = require('./db') 

const app = express()
app.use(cors()) 
app.use(express.json()) 

// Route: Get All Students
app.get('/api/students', async (req, res) => {
  try {
    const result = await pool.query('SELECT * FROM students')
    res.json(result.rows)
  } catch (err) {
    res.status(500).json({ error: err.message })
  }
})

app.listen(5000, () => console.log("Server port 5000"))
```

### C) `server/db.js` (DB Connection)
```javascript
const { Pool } = require('pg')
require('dotenv').config()

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: { rejectUnauthorized: false } 
})

module.exports = { pool }
```

### D) The `.env` File
```text
DATABASE_URL=postgres://user:pass@host:5432/db
```

## 3. Database Manual (SQL Scripts)

*Run these in your Supabase SQL Editor.*

```sql
-- 1. Create Table
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE,
    class_name VARCHAR(50)
);

-- 2. Insert Mock Data
INSERT INTO students (name, class_name) VALUES ('Test Student', '10-A');
```

## 4. Frontend Manual (React Integration)

### How to Fetch Data
```javascript
import { useEffect, useState } from 'react'

function App() {
  const [students, setStudents] = useState([])

  useEffect(() => {
    // Note: Port 5000 is BACKEND
    fetch('http://localhost:5000/api/students')
      .then(res => res.json())
      .then(data => setStudents(data))
  }, [])

  return (
    <div>
      {students.map(s => <div key={s.id}>{s.name}</div>)}
    </div>
  )
}
```

## 5. Debugging Guide (PERN Edition)

### "CORS Error"
*   **Fix**: Ensure `app.use(cors())` is the *first* line after `const app = express()`.

### "Connection Refused"
*   **Fix**: Check if Node server is actually running (`node index.js`).

### "Process is not defined"
*   **Fix**: You forgot `require('dotenv').config()` in Node, or you are trying to use `process.env` in React (use `import.meta.env` in Vite).

## 6. Deployment (Detailed)
1.  **Backend**: Push `/server` to GitHub. Go to Render.com -> New Web Service. Connect Repo. Add `DATABASE_URL` env var.
2.  **Frontend**: Push `/client` to GitHub. Go to Vercel.com. Connect Repo. Add `VITE_API_URL` env var (set it to your Render URL).
