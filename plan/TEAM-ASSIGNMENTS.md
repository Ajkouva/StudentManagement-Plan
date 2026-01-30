# OFFICIAL TEAM ASSIGNMENTS: Student Management System (PERN Stack)

**Project Goal**: Build a Full Stack App using React, Node.js, and PostgreSQL.
**Timeline**: 2-4 Weeks.

---

## 🏗️ THE FRONTEND TEAM (2 Members)
*Responsible for everything the user sees.*

### 👤 Member 1: Frontend Architect (Lead)
> *Focus: React Logic, API Connection, Routing.*
- [ ] **Initialize Project**: Run `npm create vite@latest client`.
- [ ] **Router Setup**: Install `react-router-dom` and create `App.jsx` routes (`/`, `/dashboard`).
- [ ] **API Service**: Create `src/api/axios.js` to handle all calls to the backend.
- [ ] **State Management**: Create `AuthContext` to store the logged-in user's info.
- [ ] **Integration**: Connect the Login Form variables to the API `POST /login` endpoint.

### 👤 Member 2: UI/UX Engineer
> *Focus: Design, CSS, Components.*
- [ ] **Design System**: Install Tailwind CSS and Shadcn/UI (optional).
- [ ] **Layout**: Build `Sidebar.jsx` and `Navbar.jsx` (Responsive).
- [ ] **Pages**: Build the visual HTML/CSS for:
    -   Login Page (Centered Card).
    -   Student Dashboard (Grid of Attendance Cards).
    -   Teacher Dashboard (Table of Students).
- [ ] **Polish**: Add "Loading Spinners" and "Toast Notifications" (Success/Error messages).

---

## ⚙️ THE BACKEND TEAM (2 Members)
*Responsible for the API and Database.*

### 👤 Member 3: Backend Architect (Lead)
> *Focus: Server Setup, Database Connection, Deployment.*
- [ ] **Initialize Server**: Run `npm init` and install Express, CORS, PG.
- [ ] **Server Core**: Create `server.js` and make "Hello World" work.
- [ ] **Database Connection**: Create `config/db.js` using `pg.Pool` to connect to Supabase/Neon.
- [ ] **Deployment**: Deploy the Backend to **Render.com** and ensure it's online.
- [ ] **Environment**: Manage the `.env` file (Database URL, Port).

### 👤 Member 4: API Developer
> *Focus: Routes, Controllers, SQL Queries.*
- [ ] **SQL Schema**: Write the `CREATE TABLE students` and `attendance` scripts.
- [ ] **Routes**: Create endpoints like:
    -   `GET /api/students` (Fetch all).
    -   `POST /api/attendance` (Mark present).
    -   `POST /api/login` (Verify password).
- [ ] **Logic**: Write the SQL queries inside the routes (e.g., `SELECT * FROM students WHERE email = $1`).

---

## 🛡️ THE SPECIALISTS (2 Members)

### 👤 Member 5: The Cyber Specialist (Security & Network)
> *Focus:Protecting the App, Testing Vulnerabilities.*
- [ ] **Security Middleware**: Install `helmet` and `express-rate-limit` on the Backend.
- [ ] **Input Validation**: Ensure the API rejects bad data (e.g., SQL Injection attempts).
- [ ] **Penetration Testing**:
    -   Try to "Hack" the login (Bypass password).
    -   Try to "Spam" the attendance button (Rate limiting check).
    -   Use **Postman** to try sending data without being logged in.
- [ ] **Network Analysis**: Use Browser Network Tab to inspect Headers and ensure no sensitive info leaks.

### 👤 Member 6: The Product Manager (Presenter)
> *Focus: Documentation, Testing, Presentation.*
- [ ] **Requirements**: Write the "User Stories" (e.g., "As a teacher, I want to see a red mark for absent students").
- [ ] **Manual Testing** (QA):
    -   Click every button. Try to break the app.
    -   Report bugs to the Devs (e.g., "Login button doesn't work on Mobile").
- [ ] **Documentation**: Write the `README.md` (How to run the project).
- [ ] **The Pitch**: Create the PowerPoint/Canva presentation.
    -   Slide 1: Problem Statement.
    -   Slide 2: Tech Stack (Explain why we used PERN).
    -   Slide 3: Live Demo Script (Who clicks what during the demo).

---

## 🚀 WEEKLY PLAN

### Week 1: Foundation
-   **Frontend**: UI Mockups (Static HTML/CSS).
-   **Backend**: "Hello World" API running.
-   **Security**: Setup Repo protection (Branch rules).

### Week 2: Connection
-   **Backend**: Working Login API.
-   **Frontend**: Connect Login Form to API.

### Week 3: Features
-   **All**: Attendance Marking feature complete.
-   **PM**: Draft Presentation.

### Week 4: Polish & Launch
-   **Security**: Final Penetration Test.
-   **PM**: RECORD the demo video (Backup plan).
-   **Devs**: Deploy to Vercel/Render.
