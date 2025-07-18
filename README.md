# 🏥 Healthcare SQL Project

This project demonstrates data analysis using **MySQL** on a simulated healthcare database. It explores patients, appointments, billing, doctors, and prescriptions to extract meaningful insights through SQL queries.

---

## 📁 Database Overview

The project uses a relational database with the following tables:

| Table Name      | Description                                    |
| --------------- | ---------------------------------------------- |
| `patients`      | Personal and demographic details of patients   |
| `appointments`  | Appointment records between patients & doctors |
| `billing`       | Billing information for appointments           |
| `doctors`       | Doctor details and specializations             |
| `prescriptions` | Medications prescribed during appointments     |

---

## 🧱 Database Setup

```sql
CREATE DATABASE healthcares;
USE healthcares;

-- Import data
-- Run: SOURCE healthcare.sql;
```

---

## 🔍 SQL Queries & Use Cases

### 🔹 Appointments & Prescriptions

```sql
-- Appointments for a specific patient
SELECT * FROM Appointments WHERE patient_id = 1;

-- Prescriptions for a specific appointment
SELECT * FROM Prescriptions WHERE appointment_id = 1;
```

### 🔹 Billing Insights

```sql
-- Billing info for appointment
SELECT * FROM Billing WHERE appointment_id = 2;

-- Total billed & paid
SELECT 
  (SELECT SUM(amount) FROM billing) AS total_billed,
  (SELECT SUM(amount) FROM billing WHERE status = 'paid') AS total_paid;
```

### 🔹 Appointments with Billing Status

```sql
SELECT 
  a.appointment_id, p.first_name AS patient_first_name, p.last_name AS patient_last_name,
  d.first_name AS doctor_first_name, d.last_name AS doctor_last_name,
  b.amount, b.payment_date, b.status
FROM appointments a
JOIN patients p ON a.patient_id = p.patient_id
JOIN doctors d ON a.doctor_id = d.doctor_id
JOIN billing b ON a.appointment_id = b.appointment_id;
```

### 🔹 Doctor & Patient Analytics

```sql
-- Appointments per doctor
SELECT d.doctor_id, d.first_name, d.last_name, COUNT(a.appointment_id) AS number_of_appointments
FROM doctors d
LEFT JOIN appointments a ON d.doctor_id = a.doctor_id
GROUP BY d.doctor_id;

-- Appointments per specialty
SELECT d.specialty, COUNT(a.appointment_id) AS number_of_appointments
FROM appointments a
JOIN doctors d ON a.doctor_id = d.doctor_id
GROUP BY d.specialty;
```

### 🔹 Appointment Trends

```sql
-- Monthly trend
SELECT DATE_FORMAT(appointment_date, '%Y-%m') AS month, COUNT(*) AS number_of_appointments
FROM appointments
GROUP BY month
ORDER BY month;

-- Yearly trend
SELECT YEAR(appointment_date) AS year, COUNT(*) AS number_of_appointments
FROM appointments
GROUP BY year;
```

### 🔹 Prescription Analysis

```sql
-- Most frequently prescribed medications
SELECT medication, COUNT(*) AS frequency,
       SUM(CAST(SUBSTRING_INDEX(dosage, ' ', 1) AS UNSIGNED)) AS total_dosage
FROM prescriptions
GROUP BY medication
ORDER BY frequency DESC;
```

### 🔹 Patient Engagement & Billing Patterns

```sql
-- Patients with appointments in last 30 days
SELECT DISTINCT p.*
FROM patients p
JOIN appointments a ON p.patient_id = a.patient_id
WHERE a.appointment_date >= CURDATE() - INTERVAL 30 DAY;

-- Patients with pending bills
SELECT p.patient_id, p.first_name, p.last_name, SUM(b.amount) AS total_unpaid
FROM patients p
JOIN appointments a ON p.patient_id = a.patient_id
JOIN billing b ON a.appointment_id = b.appointment_id
WHERE b.status = 'pending'
GROUP BY p.patient_id;
```

---

## 📊 Advanced Reports

### 🔸 Correlation Between Appointments and Billing

```sql
SELECT p.patient_id, p.first_name, p.last_name, SUM(b.amount) AS total_billed
FROM patients p
JOIN appointments a ON p.patient_id = a.patient_id
JOIN billing b ON a.appointment_id = b.appointment_id
GROUP BY p.patient_id
ORDER BY total_billed DESC
LIMIT 10;
```

### 🔸 Unpaid Appointments

```sql
-- Appointments without billing
SELECT a.appointment_id, a.patient_id, a.doctor_id, a.appointment_date
FROM appointments a
LEFT JOIN billing b ON a.appointment_id = b.appointment_id
WHERE b.billing_id IS NULL;
```

### 🔸 Doctor Revenue Report

```sql
SELECT d.first_name, d.last_name, d.specialty, SUM(b.amount) AS total_billed 
FROM doctors d
JOIN appointments a ON d.doctor_id = a.doctor_id
JOIN billing b ON a.appointment_id = b.appointment_id
GROUP BY d.first_name, d.last_name, d.specialty
ORDER BY total_billed DESC;
```

---

## 📈 Example Use Cases

* ✅ Track patient appointment history
* 💰 Analyze revenue & pending billing
* 🧑‍⚕️ Measure doctor workload & specialties
* 💊 Identify common medications
* 📅 Visualize appointment trends over time
* 🔍 Spot missed appointments or unbilled cases

---

## 🧰 Tools Used

* **MySQL** – Querying and managing relational data
* **SQL Joins & Aggregates** – For analytics
* **Functions** – `DATE_FORMAT()`, `CURDATE()`, `GROUP BY`, `LEFT JOIN`, `SUBSTRING_INDEX()`

---

## 🚀 Getting Started

1. Install MySQL or use any SQL IDE (e.g., MySQL Workbench, DBeaver).
2. Clone the repo or download `healthcare.sql`.
3. Run the script to create tables and insert data.
4. Execute the SQL queries for analysis.

---

📬 Contact

Made by Chankshi Shrawankar 🔗 www.linkedin.com/in/chankshishrawankar | ✉️ chankshishrawankar@gmail.com

---

Would you like a **Power BI / Excel dashboard** or **ER diagram** added to this project? I can help generate those too.
