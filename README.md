# 🎓 College ERP — Student Welfare & Academic Management System

![C++](https://img.shields.io/badge/Language-C%2B%2B-blue.svg)
![OOP](https://img.shields.io/badge/Paradigm-OOP-orange.svg)
![STL](https://img.shields.io/badge/STL-Vectors-green.svg)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen.svg)

A terminal-based **College ERP (Student Welfare) System** in C++ that automates the academic calculations every student actually needs day-to-day: attendance percentage against the 75% mandatory threshold, subject-wise grading, semester GPA, and overall CGPA — all wrapped around a simple ID-based student database with full CRUD support.

---

## 🧠 Why This Project Matters

Most student-facing college tools are really just **calculators with a database attached** — but getting that combination right (correct formulas, sane data structures, and a UI that doesn't crash on edge cases) is harder than it looks. This project doesn't just check attendance against 75%; it **derives** how many additional classes a student needs to attend to cross that threshold, mathematically, instead of hardcoding a lookup table. The same rigor carries through to a full 10-point GPA/CGPA engine that mirrors the grading scale used by real Indian universities.

---

## ✨ Core Features

### 🔐 Access Control
- ID-based passcode login — only registered student IDs can enter the system, with immediate rejection of unrecognized credentials.

### 🗂️ Student Database Management
| Capability | Description |
|---|---|
| Show Student Data | Lists every registered student's name and ID |
| Add a New Student | Registers a new student record into the live database |
| Remove a Student | Deletes a student by ID, but **only after an explicit Y/N confirmation step** showing the matched record first |

### 📊 Academic Calculation Engine
| Capability | Description |
|---|---|
| Calculate Attendance | Computes attendance % from classes conducted vs. attended, and tells the student **exactly how many more classes** they need to attend to reach 75% |
| Calculate Grade | Converts subject marks into a letter grade (O, A+, A, B+, B, C, or Fail) using standard university grade bands |
| Calculate Semester GPA | Accepts marks + credit points for every subject in a semester and computes the credit-weighted GPA |
| Calculate CGPA | Aggregates GPA across multiple semesters into a cumulative CGPA |

---

## 🏗️ Architecture & Class Design

The system uses a clean **composition relationship** — the `college` class manages a collection of `student_data` objects rather than duplicating student fields itself:

```
┌──────────────────┐
│   student_data     │   (record class)
├──────────────────┤
│ name               │
│ id                  │
│ display()           │
└──────────────────┘
          ▲
          │  "has many" (composition, via vector)
          │
┌──────────────────┐
│      college       │   (manager / orchestrator class)
├──────────────────┤
│ vector<student_data>│
├──────────────────┤
│ add_data()           │
│ remove_data()         │
│ display_all_data()    │
│ login_data_id()        │
│ calculate_attendance() │
│ grade_system()          │
│ grade_points()           │
│ gpa()                     │
│ cgpa()                     │
└──────────────────┘
```

- **`student_data`** — A minimal, focused record class storing only what's needed to identify a student: `name` and `id`.
- **`college`** — The single orchestrator class. It *owns* a `vector<student_data>` (composition, not inheritance) and exposes every academic and administrative operation as a clean member function, keeping `main()` limited to menu navigation and input collection.

This keeps student identity data (`student_data`) decoupled from the business logic that operates on it (`college`) — so the calculation engine could be pointed at a completely different storage backend later without touching the math.

---

## ⚙️ Technical Highlights

- **Derived (not hardcoded) attendance math** — instead of a lookup table, the "classes needed" calculation is algebraically derived from the inequality `(attended + x) / (conducted + x) ≥ 0.75`, solved for `x`, and implemented directly as `3×conducted − 4×attended`. This shows real mathematical reasoning behind the feature, not a guessed constant.
- **Confirmation-gated deletion** — `remove_data()` never deletes blind; it shows the matched student record and requires explicit confirmation before calling `vector::erase()`.
- **Full 10-point grading pipeline** — `grade_points()` maps raw marks to the standard Indian university 10-point scale (10 down to 0), which directly feeds into `gpa()`'s credit-weighted average calculation: `Σ(credit × grade point) / Σ(credit)`.
- **Multi-semester CGPA rollup** — `cgpa()` takes a variable number of semester GPAs and reduces them to a single cumulative figure, mirroring exactly how a real transcript's CGPA is computed.
- **ANSI-Highlighted Feedback** — Bold terminal formatting (`\033[1m`) is used specifically to draw attention to the actionable insight (how many classes are needed), rather than decorating the whole screen — a small but deliberate UX choice.
- **STL `vector`-backed Dynamic Records** — The student database grows or shrinks at runtime with no fixed capacity limit, using `vector::push_back()` and `vector::erase()`.

---

## 💻 Sample Interaction

```
WELCOME TO THE COLLEGE - STUDENT WELFARE SYSTEM
PLEASE ENTER YOUR FOUR DIGIT PASSCODE: 1060
ENTERED PIN IS CORRECT!!
WELCOME Atharv Verma
--------------

SELECT THE FUNCTION FROM THE DROP DOWN MENU:
1. CALCULATE ATTENDANCE
2. CALCULATE GRADE
3. CALCULATE SEMESTER GPA
4. CALCULATE CGPA
5. SHOW STUDENT DATA
6. ADD A NEW STUDENT
7. REMOVE A STUDENT
PLEASE ENTER YOUR FUNCTION: 1
PLEASE ENTER THE TOTAL CLASS CONDUCTED: 60
PLEASE ENTER THE NUMBER OF CLASS ATTENDED: 40
THE ATTENDANCE IS: 66.6667%
NOTE: YOU DO NOT SATISFY THE CRITERIA OF 75%, ATTEND 20 CLASSES TO REACH 75%
```

---

## 🚀 Getting Started

### Prerequisites
- A C++ compiler supporting C++11 or later (e.g., `g++`)

### Compile & Run
```bash
g++ college_erp_system.cpp -o college_erp
./college_erp
```
> 💡 Sample login passcode (matches a pre-loaded student ID): `1060`

---

## 🔭 Known Limitations & Future Enhancements

This was built as a focused exercise in academic-calculation logic and basic data management. Honest gaps worth closing in a future iteration:
- **Fixed-size arrays in `gpa()`/`cgpa()`** — subject/semester counts are capped at 50 via plain C-style arrays; switching to `vector` would remove that ceiling and add bounds safety.
- **Strict-exit input handling** — several functions call `exit(0)` immediately on any unexpected menu input rather than re-prompting, which is safe but harsh for a real user-facing tool.
- **Menu display loop mismatch** — the function menu loop currently prints only the first 7 items, so the "8. EXIT" option works when selected but isn't shown on screen — a one-line fix worth making.
- **PIN doubles as both ID and password** — login currently just matches the entered passcode to a student ID, with no separate secret; a real system would pair ID with an actual password or OTP.
- **No persistence layer** — like the other systems in this series, all data lives in memory and resets on restart; the natural next step is file or database-backed storage.

---

## 👤 Author

**Atharv Verma**
B.Tech, Computer Science and Engineering — SRM Institute of Science and Technology
📧 verma.atharv@gmail.com

---

*Part of a series of OOP-driven systems projects, this one focused on translating real academic formulas (attendance thresholds, grade scales, GPA/CGPA weighting) into clean, working code.*
