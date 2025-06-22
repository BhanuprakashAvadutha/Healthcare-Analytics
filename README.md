
# 🏥 Healthcare Analytics SQL Project

A comprehensive SQL project that models a real-world Healthcare Management System. This project demonstrates database design, relational integrity, complex SQL queries, and healthcare-related insights.

---

## 📊 Project Overview

This project simulates a hospital system where doctors, patients, appointments, and treatments are managed. It includes schema creation, sample data, and complex queries using joins, window functions, and aggregate operations.

---

## 🧱 Database Schema

### 👨‍⚕️ Patients Table
```sql
CREATE TABLE patients (
    patiaent_id INT PRIMARY KEY,
    Patients VARCHAR(250),
    Doctors VARCHAR(250),
    Departments VARCHAR(250),
    Medications VARCHAR(250),
    Prescriptions VARCHAR(250),
    Appointments VARCHAR(250),
    Sales_transactions INT,
    joining_date DATE,
    leaving_date DATE,
    gender VARCHAR(50)
);
```

### 🩺 Doctors Table
```sql
CREATE TABLE doctors (
    doctor_name VARCHAR(100),
    patiaent_id INT,
    doctor_id INT PRIMARY KEY,
    department VARCHAR(100),
    doctor_age INT,
    gender VARCHAR(50),
    FOREIGN KEY (patiaent_id) REFERENCES patients(patiaent_id)
);
```

---

## ✍️ Sample Data Inserts

### ➕ Insert Patients (sample from 40 rows)
```sql
INSERT INTO patients (
    patiaent_id, Patients, Doctors, Departments, Medications, Prescriptions,
    Appointments, Sales_transactions, joining_date, leaving_date, gender
)
VALUES
(1, 'Ananya Sharma', 'Dr. Rupa', 'Cardiology', 'Atenolol', 'Rx001', 'App001', 3, '2024-01-10', '2024-01-20', 'Female'),
(2, 'Raj Verma', 'Dr. Bhanu', 'Orthopedics', 'Ibuprofen', 'Rx002', 'App002', 2, '2024-02-05', '2024-02-15', 'Male');
-- (38 more rows in original code)
```

### ➕ Insert Doctors (sample from 40 rows)
```sql
INSERT INTO doctors (doctor_name, patiaent_id, doctor_id, department, doctor_age, gender)
VALUES
('Dr. Rupa', 1, 101, 'Cardiology', 45, 'Female'),
('Dr. Bhanu', 2, 102, 'Orthopedics', 50, 'Male');
-- (38 more rows in original code)
```

---

## 🛠️ Schema Updates (ALTER TABLEs)
```sql
ALTER TABLE patients
ALTER COLUMN gender VARCHAR(50);

ALTER TABLE patients
ALTER COLUMN Departments VARCHAR(250);

ALTER TABLE doctors
ALTER COLUMN doctor_name VARCHAR(100);

ALTER TABLE doctors
ALTER COLUMN gender VARCHAR(50);
```

---

## 📥 Sample Queries

### 🔎 Window Function - Rank Within Department
```sql
SELECT d.doctor_name, d.department,
       ROW_NUMBER() OVER (PARTITION BY d.department ORDER BY d.doctor_age) AS rank_within_dept
FROM patients p
JOIN doctors d ON p.patiaent_id = d.patiaent_id;
```

### 📈 Count Female Patients by Department (HAVING)
```sql
SELECT d.department, COUNT(*) AS female_patients
FROM patients p
JOIN doctors d ON p.patiaent_id = d.patiaent_id
WHERE p.gender = 'Female'
GROUP BY d.department
HAVING COUNT(*) >= 3;
```

### 🥈 Second Highest Doctor Age
```sql
SELECT DISTINCT doctor_name, doctor_age
FROM doctors
WHERE doctor_age = (
    SELECT MAX(doctor_age)
    FROM doctors
    WHERE doctor_age < (
        SELECT MAX(doctor_age) FROM doctors
    )
);
```

### 🧠 Doctors Older Than Average
```sql
SELECT *
FROM doctors
WHERE doctor_age > (
    SELECT AVG(doctor_age) FROM doctors
);
```

### 🔁 Count of Repeat Doctor Appearances
```sql
SELECT doctor_name, COUNT(*) AS doctor_appearance
FROM doctors
GROUP BY doctor_name
HAVING COUNT(*) > 1;
```

---

## 💼 Business Value

- Identify top-performing departments
- Evaluate gender-based patient distribution
- Recognize senior vs junior doctors
- Reveal treatment trends by week
- Optimize staffing based on doctor loads

---

## 🚀 How to Run

1. Clone this repository
2. Open SQL Server Management Studio
3. Run the schema files
4. Execute the data insert scripts
5. Query results will reflect real-world insights

---

## 🧠 Skills Demonstrated

- SQL Data Modeling
- Relational Joins and Integrity
- Aggregate Functions and Filtering
- Window Functions and Ranking
- Query Optimization
- Git Version Control

---

## 👨‍💻 Author

**Bhanu Prakash Avadutha**  
*Aspiring Data Analyst | SQL Enthusiast*

---
