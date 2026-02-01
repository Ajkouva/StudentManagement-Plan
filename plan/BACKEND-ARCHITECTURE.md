# MASTER BACKEND ARCHITECTURE

> **Status**: FINALIZED for PERN Stack Implementation.
> **Reference**: Matches the "Dark Mode Flowchart" provided by the team.

---

## 1. Database Schema (The Foundation)

### Table 1: `users` (Authentication)
*The Gatekeeper. Stores valid logins.*
| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | UUID | Primary Key (e.g., `550e8400-e29b...`) |
| `email` | VARCHAR | Unique Login ID |
| `password_hash` | VARCHAR | **Bcrypt Encrypted** (never plain text) |
| `role` | VARCHAR | 'STUDENT' or 'TEACHER' |

### Table 2: `students` (Student Profile)
*Linked to `users`. Stores the ID Card details.*
| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | SERIAL | Primary Key (Internal ID: 1, 2, 3) |
| `user_id` | UUID | Foreign Key -> `users.id` |
| `name` | VARCHAR | Full Name (e.g., "Rahul") |
| `class_name` | VARCHAR | e.g., "Class 10-A" |
| `roll_no` | INT | Roll Number (e.g., 21) |
| `student_id_code`| VARCHAR | **Display ID** (e.g., "STD001") |

### Table 3: `teachers` (Teacher Profile)
*Linked to `users`. Stores Teacher details.*
| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | SERIAL | Primary Key |
| `user_id` | UUID | Foreign Key -> `users.id` |
| `name` | VARCHAR | Teacher Name |
| `subject` | VARCHAR | Main Subject (e.g., "Maths") |

### Table 4: `attendance` (The ledger)
*Stores daily status for students.*
| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | SERIAL | Primary Key |
| `student_id` | INT | Foreign Key -> `students.id` |
| `date` | DATE | YYYY-MM-DD format |
| `status` | VARCHAR | 'PRESENT', 'ABSENT', 'HOLIDAY' |

---

## 2. API Endpoints (The Wiring)

### 🔐 AUTHENTICATION
**1. Login API**
*   **Endpoint**: `POST /api/auth/login`
*   **Input**: `{ "email": "teacher@school.com", "password": "pass" }`
*   **Logic**:
    1.  Verify Password.
    2.  Check Role.
    3.  If Default Password is used, prompt change (Optional).
*   **Output**:
    ```json
    {
      "token": "eyJh...",
      "role": "TEACHER",
      "name": "Mr. Sharma",
      "id": 1
    }
    ```

---

### 👨‍🏫 TEACHER FLOW

**2. Teacher Dashboard (Stats)**
*   **Endpoint**: `GET /api/teacher/dashboard`
*   **Response** (Matches Flowchart):
    ```json
    {
      "total_students": 45,
      "present_today": 42,
      "absent_today": 3,
      "academic_performance": "Coming Soon" // Placeholder
    }
    ```

**3. Add New Student (The "Invite")**
*   **Endpoint**: `POST /api/students/create`
*   **Input**:
    ```json
    {
      "name": "New Student",
      "email": "student@gmail.com",
      "password": "SchoolPassword123", // Manually set by Teacher
      "class": "10-A",
      "roll_no": 22
    }
    ```
*   **Logic**: Creates `user` (Login) **AND** `student` (Profile) records.

**4. Attendance Register (The List)**
*   **Endpoint**: `GET /api/teacher/attendance-sheet?date=2024-02-01`
*   **Response**:
    ```json
    [
      {
        "student_id": 1,
        "name": "Rahul",
        "email": "rahul@gmail.com",
        "student_id_code": "STD001",
        "status": "PRESENT" // or null if not marked yet
      },
      {
        "student_id": 2,
        "name": "Anjali",
        "email": "anjali@gmail.com",
        "student_id_code": "STD002",
        "status": "ABSENT"
      }
    ]
    ```

**5. Save Attendance (Bulk)**
*   **Endpoint**: `POST /api/attendance/bulk`
*   **Input**:
    ```json
    {
      "date": "2024-02-01",
      "records": [
        { "student_id": 1, "status": "PRESENT" },
        { "student_id": 2, "status": "ABSENT" }
      ]
    }
    ```

---

### 🎓 STUDENT FLOW

**6. Student Dashboard (ID Card & Stats)**
*   **Endpoint**: `GET /api/student/dashboard`
*   **Response**:
    ```json
    {
      "profile": {
        "name": "Rahul",
        "id_code": "STD001",
        "class": "10-A"
      },
      "attendance_summary": {
        "percentage": 89,
        "total_present": 45,
        "total_days": 50
      },
      "performance": "Coming Soon"
    }
    ```

**7. Detailed Attendance (Calendar View)**
*   **Endpoint**: `GET /api/student/calendar?month=02`
*   **Response** (For the Color-Coded Calendar):
    ```json
    [
      { "date": "2024-02-01", "status": "PRESENT", "color": "green" },
      { "date": "2024-02-02", "status": "ABSENT", "color": "red" },
      { "date": "2024-02-03", "status": "HOLIDAY", "color": "grey" }
    ]
    ```

---

## 3. Special Logic Notes

### A. The "Genesis" Start
Since there is no "Sign Up" button (only Login), you must run a **SQL Script** to create the very first Teacher Account. After that, this Teacher adds everyone else.

### B. Color Logic
*   **Green**: Status = 'PRESENT'
*   **Red**: Status = 'ABSENT'
*   **Grey**: Status = 'HOLIDAY' or 'LEAVE'
*   **White/Null**: Logic for "Future Dates" (Not marked yet).

### C. Future Scope
*   **Academic Performance**: This API is currently a placeholder (`"Coming Soon"`). Do not build the database tables for Grades yet (as per instruction).
