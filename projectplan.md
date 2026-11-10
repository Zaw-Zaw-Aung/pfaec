# 📘 Personal Finance & Expense Tracker — Complete Project Specification

## 1. Project Overview
* **Project Name:** Personal Finance & Expense Tracker
* **Goal:**
  * Systematically record daily incomes and expenses.
  * Set budget limits to control spending habits.
  * View financial data clearly through interactive charts.
  * Foster positive financial habits.
* **Target Users:** Individual users and family budget managers.
* **Core Modules:**
  1. Authentication (Sign up / Sign in)
  2. Transactions Management (Income / Expense records)
  3. Categories Management
  4. Budgeting & Goals
  5. Analytics & Dashboard

---

## 2. Functional Requirements

### Part (1) — User Authentication
* **Register / Login:** Create an account and log in using Email & Password.
* **Security:**
  * Password hashing using `bcrypt`.
  * Session management using JSON Web Tokens (`JWT`).
  * Separation of Access Token and Refresh Token.
  * Email Verification and Password Reset flow.
  * Rate Limiting (to prevent brute-force attacks).
  * CORS, Helmet, and HTTPS security middleware.
  * 2FA (Optional) — highly recommended for a financial app.

### Part (2) — Transactions Management
* **Add Transaction:**
  * Select between Income / Expense.
  * Input Amount, Date, Category, and Note/Description.
* **View / Filter Transactions:**
  * View all transactions in a table format.
  * Filtering & Sorting by month, date range, or category.
  * Pagination (10/20 items per page) or Infinite Scroll.
* **Edit / Delete:**
  * Edit or delete incorrectly entered records.
  * Soft Delete + Undo feature.
* **Recurring Transactions:**
  * Automated logging for recurring items like monthly salary, rent, and subscriptions.

### Part (3) — Categories Management
* **Default Categories:** Salary, Food/Dining, Rent, Utilities, etc.
* **Custom Categories:** Allow users to create personal custom categories.
* **Customization:** Assign icons and colors to categories.
* **Type Classification:** Categorize as either income or expense.

### Part (4) — Budgets & Goals
* **Budgets:**
  * Set budget limits per category or overall monthly spending.
  * Alert/Warning notifications when spending reaches 80% / 90% / 100% of the budget.
  * In-app + Email / Push Notifications.
* **Saving Goals:**
  * Target-based savings (e.g., "Save 500,000 for a car").
  * Progress tracking bar.
  * Deadline setting.

### Part (5) — Dashboard & Analytics
* **Summary Cards:** Total income, total expenses, and current balance for the current month.
* **Charts:**
  * Pie Chart / Donut Chart — Spending breakdown by category.
  * Bar Chart / Line Chart — Monthly/daily income vs. expense comparison.
  * Spending Trends — Comparison with the previous month.
  * Top Spending Categories ranking.
* **Reports:**
  * Monthly/Yearly financial reports.
  * Savings rate calculation.
  * Data Export — CSV, Excel, PDF.

---

## 3. Non-Functional Requirements

| Category | Requirement |
| :--- | :--- |
| **Performance** | Real-time updates on Dashboard/Charts immediately upon adding a new transaction. |
| **Responsive Design** | Mobile-friendly layout across phones, tablets, and laptops (using Tailwind CSS). |
| **Data Reliability** | Secure and error-free data storage using MySQL or MongoDB. |
| **Scalability** | Capable of handling a growing number of concurrent users. |
| **Security** | Encryption at rest, audit logs, and GDPR-style data export/deletion. |
| **Accessibility** | Screen reader support, Dark Mode, and Loading Skeletons. |

---

## 4. Suggested Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Frontend** | React or Next.js + Tailwind CSS + Recharts |
| **State Management** | Redux Toolkit / Zustand / React Query |
| **Form Validation** | Zod + React Hook Form |
| **Backend** | Node.js + Express or Spring Boot |
| **Database** | MySQL (Relational DB — best suited for financial data) |
| **Authentication** | JWT + bcrypt + Refresh Token |
| **File Storage** | AWS S3 / Cloudinary (for exported files) |
| **Notification** | Firebase Cloud Messaging / Nodemailer |
| **Testing** | Jest, Supertest, React Testing Library |
| **CI/CD** | GitHub Actions |
| **Deployment** | Vercel (Frontend) + Railway / Render (Backend) |
| **Monitoring** | Sentry, LogRocket |

---

## 5. Database Schema & Relationships

### Core Tables
* **Users Table:** `id`, `email`, `password_hash`, `is_verified`, `created_at`, `updated_at`
* **Categories Table:** `id`, `user_id` (NULL for defaults), `name`, `type` (income/expense), `icon`, `color`
* **Transactions Table:** `id`, `user_id`, `category_id`, `amount`, `type`, `date`, `description`, `is_deleted`, `created_at`
* **Budgets Table:** `id`, `user_id`, `category_id`, `monthly_limit`, `month_year`, `alert_threshold`

### Additional Tables (Recommended)
* **Recurring Transactions:** `id`, `user_id`, `category_id`, `amount`, `frequency`, `next_date`, `is_active`
* **Notifications:** `id`, `user_id`, `type`, `message`, `is_read`, `created_at`
* **Goals:** `id`, `user_id`, `name`, `target_amount`, `current_amount`, `deadline`, `created_at`
* **Currencies:** `code`, `name`, `symbol`, `exchange_rate`, `updated_at`
* **Audit Logs:** `id`, `user_id`, `action`, `entity`, `entity_id`, `created_at`
* **User Settings:** `user_id`, `timezone`, `currency`, `language`, `theme`, `notification_preferences`

---

## 6. Currency & Number Formatting
* Display amounts with commas (e.g., `150,000 MMK`).
* Properly format decimal values.
* Implement a reusable currency formatting helper function.
* Multi-Currency Support (e.g., MMK, USD, THB).
* Store exchange rate history.

---

## 7. Date & Timezone Handling
* Store local dates accurately (Myanmar Standard Time or user's local timezone).
* Calculate date ranges correctly on the backend to avoid discrepancies during monthly filtering.
* Store in UTC and display in the user's local timezone.

---

## 8. Pagination / Infinite Scrolling
* Prevent UI sluggishness as transaction records grow:
  * Pagination (10/20 items per page) or
  * Infinite Scroll.
* Cursor-based pagination is preferred for optimal performance.

---

## 9. Error Handling & Validation
* **Frontend Validation:** 
  * Form validation to prevent letters or negative amounts in numeric currency fields.
  * Utilize `Zod` + `React Hook Form`.
* **Backend Validation:**
  * Server-side data verification.
  * Input sanitization to prevent SQL Injection and XSS attacks.
  * Global error handler returning meaningful error messages.

---

## 10. Advanced Features
* **Shared / Family Accounts:** Budget sharing with Role-based access control (Owner, Editor, Viewer).
* **API Documentation:** Swagger / OpenAPI.
* **Undo Delete:** Soft delete + restore capability.
* **Dark Mode.**
* **Loading Skeletons & Empty States.**
* **Data Export / Import:** CSV, Excel, PDF, and bank statement import.
* **Data Privacy:** GDPR-style right to be forgotten (user data export/deletion).
* **Audit Logs:** History tracking for transaction changes.
* **Bank Integration (Future):** Open Banking APIs.

---

## 11. Deployment & DevOps

| Category | Recommendation |
| :--- | :--- |
| **Hosting** | Vercel (Frontend) + Railway / Render / AWS (Backend) |
| **Environment** | `.env` + Secrets management |
| **Database Backup** | Daily automated backups |
| **Monitoring** | Sentry, LogRocket |
| **CI/CD** | GitHub Actions — automated testing & deployment |

---

## 12. Development Roadmap

* 🟢 **Phase 1 — MVP (Minimum Viable Product)**
  * User Authentication (Register/Login/JWT)
  * Transactions CRUD
  * Categories (Default + Custom)
  * Basic Dashboard (Summary Cards + Pie Chart)
  * Database schema setup
  * Basic validation

* 🟡 **Phase 2 — Core Features**
  * Budgets & Alerts
  * Advanced Charts (Bar/Line)
  * Filtering & Sorting
  * Pagination
  * Dark Mode
  * CSV Export

* 🔵 **Phase 3 — Advanced Features**
  * Recurring Transactions
  * Goals Module
  * Multi-Currency Support
  * Email/Push Notifications
  * PDF/Excel Reports
  * Refresh Token + Email Verification

* 🔴 **Phase 4 — Production Ready**
  * Shared/Family Accounts
  * Two-Factor Authentication (2FA)
  * Audit Logs
  * Testing (Unit + Integration)
  * CI/CD + Monitoring
  * GDPR Compliance

---

## 13. Project Assessment Summary

| Status | Stage |
| :--- | :--- |
| **For MVP** | ✅ Complete |
| **For Production-ready App** | ⚠️ ~70% Complete (Needs the advanced features listed above) |
| **For Portfolio / Capstone** | ✅ Excellent (Adding Testing & Deployment sections makes it stand out even more) |

---

## 14. Conclusion & Recommendations
This document serves as a comprehensive specification for the Personal Finance & Expense Tracker project.

**Recommendations:**
1. Build the project step-by-step starting from **Phase 1 (MVP)**.
2. Control the scope — avoid trying to build every feature all at once.
3. Integrate testing early in the development cycle.
4. Prioritize security, as this is a financial application.
5. Iterate based on real user feedback.