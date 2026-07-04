# 💼 Employee Payroll Management System

> A full-stack web application to automate employee salary processing, leave management, and PDF payslip generation — built as a college project.

![Project Banner](https://img.shields.io/badge/Project-Payroll%20Management-1E40AF?style=for-the-badge)
![HTML](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## 🚀 Live Demo

> Open `index.html` directly in any browser — **no server required!**

### 🔐 Demo Login Accounts

| Role | Email | Password |
|------|-------|----------|
| 🛡️ Admin | admin@payrollpro.com | admin123 |
| 👔 HR Manager | hr@payrollpro.com | hr123 |
| 🧑‍💼 Employee | john@payrollpro.com | emp123 |

---

## 📋 Features

### 🔐 Authentication
- JWT-style login with role-based access control
- 3 roles: Admin, HR Manager, Employee
- Secure session management

### 👥 Employee Management
- Add, edit, view, activate/deactivate employees
- Department and designation assignment
- Search and filter employees

### 💰 Salary Structure
- Configure Basic, HRA, DA, Travel Allowance
- Set PF, ESI, Income Tax deductions
- Auto net salary calculation

### 🧾 Payroll Processing
- One-click monthly payroll run
- Gross salary, deductions, net pay calculation
- Payroll history tracking

### 📄 Payslip Generation
- Professional PDF payslip with company letterhead
- Full salary breakdown (earnings vs deductions)
- Print-ready payslip view

### 🏖️ Leave Management
- Apply for Casual / Sick / Earned leave
- HR approval / rejection workflow
- Leave balance tracker

### 📊 Analytics & Reports
- Department-wise payroll cost charts
- Top earners leaderboard
- Salary component breakdown

### 🏢 Department Management
- Add, edit, delete departments
- Manager assignment
- Per-department cost overview

---

## 🗂️ Project Structure

```
payroll-system/
│
├── index.html              ← Main application (single file)
├── README.md               ← Project documentation
├── LICENSE                 ← MIT License
│
├── docs/
│   ├── use-case-diagram.html     ← UML Use Case Diagram
│   ├── project-plan.html         ← Full project plan
│   └── project-ideas.html        ← Project ideas reference
│
└── presentation/
    └── PayrollManagement_Presentation.pptx  ← PPT slides
```

---

## 🛠️ Tech Stack (Frontend Prototype)

| Layer | Technology |
|-------|-----------|
| Structure | HTML5 |
| Styling | CSS3 (custom design system) |
| Logic | Vanilla JavaScript (ES6+) |
| Fonts | Google Fonts (Clash Display, Satoshi, JetBrains Mono) |
| Icons | Emoji-based icons |
| Data | In-memory JavaScript object (mock DB) |

### 🔮 For Production Upgrade

| Layer | Technology |
|-------|-----------|
| Frontend | React.js + Bootstrap 5 |
| Backend | Java 17 + Spring Boot 3 |
| Database | PostgreSQL |
| Auth | JWT + Spring Security |
| PDF | iText / JasperReports |
| Email | JavaMailSender + SMTP |
| Build | Maven |

---

## ⚡ Getting Started

### Run Locally (Prototype)
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/payroll-system.git

# Navigate to project folder
cd payroll-system

# Open in browser
open index.html
# OR just double-click index.html
```

### For Production (Spring Boot)
```bash
# Prerequisites
# - Java 17+
# - Maven 3.8+
# - PostgreSQL 14+

# Clone & build
git clone https://github.com/YOUR_USERNAME/payroll-system.git
cd payroll-system
mvn clean install
mvn spring-boot:run
```

---

## 📸 Screenshots

### Dashboard (Admin View)
- Statistics cards: Active employees, monthly payroll, pending leaves
- Department payroll bar chart
- Recent employees list
- Pending leave requests

### Payslip View
- Professional company letterhead
- Earnings breakdown (Basic, HRA, DA, TA)
- Deductions (PF, ESI, Tax)
- Net take-home salary highlighted

---

## 🗄️ Database Schema (PostgreSQL)

```sql
-- Users Table
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    emp_id INT REFERENCES employees(emp_id),
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(20) CHECK (role IN ('admin', 'hr', 'employee')),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Employees Table
CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    emp_code VARCHAR(10) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    dept_id INT REFERENCES departments(dept_id),
    designation VARCHAR(100),
    join_date DATE,
    status VARCHAR(10) DEFAULT 'active',
    bank_details VARCHAR(255)
);

-- Departments Table
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL,
    manager_id INT REFERENCES employees(emp_id)
);

-- Salary Structure Table
CREATE TABLE salary_structure (
    salary_id SERIAL PRIMARY KEY,
    emp_id INT REFERENCES employees(emp_id),
    basic DECIMAL(10,2),
    hra DECIMAL(10,2),
    da DECIMAL(10,2),
    ta DECIMAL(10,2),
    pf_deduction DECIMAL(10,2),
    esi_deduction DECIMAL(10,2),
    tax_deduction DECIMAL(10,2),
    effective_from DATE DEFAULT NOW()
);

-- Payroll Table
CREATE TABLE payroll (
    payroll_id SERIAL PRIMARY KEY,
    emp_id INT REFERENCES employees(emp_id),
    month VARCHAR(20) NOT NULL,
    year INT NOT NULL,
    gross_salary DECIMAL(10,2),
    total_deductions DECIMAL(10,2),
    net_salary DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'pending',
    processed_date DATE
);

-- Leaves Table
CREATE TABLE leaves (
    leave_id SERIAL PRIMARY KEY,
    emp_id INT REFERENCES employees(emp_id),
    leave_type VARCHAR(20),
    from_date DATE,
    to_date DATE,
    days INT,
    reason TEXT,
    status VARCHAR(20) DEFAULT 'pending',
    applied_on TIMESTAMP DEFAULT NOW()
);
```

---

## 📡 REST API Endpoints (Spring Boot)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | User login, returns JWT |
| GET | `/api/employees` | List all employees |
| POST | `/api/employees` | Add new employee |
| PUT | `/api/employees/{id}` | Update employee |
| DELETE | `/api/employees/{id}` | Deactivate employee |
| GET | `/api/salary/{empId}` | Get salary structure |
| PUT | `/api/salary/{empId}` | Update salary |
| POST | `/api/payroll/run` | Run monthly payroll |
| GET | `/api/payslip/{empId}/{month}` | Download PDF payslip |
| POST | `/api/leaves/apply` | Apply for leave |
| PUT | `/api/leaves/{id}/approve` | Approve leave |
| PUT | `/api/leaves/{id}/reject` | Reject leave |
| GET | `/api/reports/summary` | Payroll summary report |
| GET | `/api/departments` | List departments |
| POST | `/api/departments` | Add department |

---

## 👥 User Roles & Permissions

| Feature | Admin | HR Manager | Employee |
|---------|-------|-----------|---------|
| View Dashboard | ✅ | ✅ | ✅ |
| Manage Employees | ✅ | ✅ | ❌ |
| Manage Departments | ✅ | ❌ | ❌ |
| Process Payroll | ✅ | ✅ | ❌ |
| View All Payslips | ✅ | ✅ | ❌ |
| View Own Payslip | ✅ | ✅ | ✅ |
| Approve Leaves | ✅ | ✅ | ❌ |
| Apply for Leave | ❌ | ❌ | ✅ |
| System Settings | ✅ | ❌ | ❌ |
| View Analytics | ✅ | ✅ | ❌ |

---

## 🏫 College Info

- **College:** V.S.B. Engineering College, Karur
- **Department:** Computer Science & Engineering
- **Regulation:** 2023
- **Project Type:** Full-Stack Web Application
- **Academic Year:** 2024–2025

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## ⭐ Show Your Support

Give a ⭐ if this project helped you!

---

*Made with ❤️ for college project submission*
