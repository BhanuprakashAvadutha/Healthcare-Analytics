# Healthcare Analytics — SQL Project

SQL project modeling a real-world Hospital Management System. Demonstrates schema design, relational integrity, window functions, and complex analytical queries.

## Schema Design

```
patients ──< appointments >── doctors
    │                              │
    └──< prescriptions        departments
```

**Tables:** patients, doctors, departments, appointments, prescriptions, medications, sales_transactions

## Key Queries Demonstrated

### Window Functions — Running totals and rankings
```sql
SELECT 
    doctor_name,
    department,
    COUNT(patient_id) AS patient_count,
    RANK() OVER (PARTITION BY department ORDER BY COUNT(patient_id) DESC) AS dept_rank,
    SUM(COUNT(patient_id)) OVER (PARTITION BY department) AS dept_total
FROM appointments a
JOIN doctors d ON a.doctor_id = d.doctor_id
GROUP BY doctor_name, department;
```

### Patient Retention — Cohort analysis
```sql
WITH cohorts AS (
    SELECT 
        patient_id,
        DATE_TRUNC('month', joining_date) AS cohort_month,
        DATEDIFF(month, joining_date, leaving_date) AS tenure_months
    FROM patients
    WHERE leaving_date IS NOT NULL
)
SELECT 
    cohort_month,
    AVG(tenure_months) AS avg_tenure,
    COUNT(*) AS cohort_size
FROM cohorts
GROUP BY cohort_month
ORDER BY cohort_month;
```

### Department Revenue Analysis
```sql
SELECT 
    d.department,
    COUNT(DISTINCT a.patient_id) AS unique_patients,
    SUM(p.Sales_transactions) AS total_revenue,
    ROUND(AVG(p.Sales_transactions), 2) AS avg_revenue_per_patient
FROM departments d
JOIN doctors doc ON d.department = doc.department
JOIN appointments a ON doc.doctor_id = a.doctor_id
JOIN patients p ON a.patient_id = p.patient_id
GROUP BY d.department
ORDER BY total_revenue DESC;
```

## Stack
- **SQL Server (SSMS)** — Schema creation, query development
- **Database:** 40 patients, 15 doctors, 5 departments, 200+ appointments

## What I learned
Designing normalized schemas for healthcare data requires careful thought about referential integrity — patient records must never be deleted if appointments reference them. Used `ON DELETE RESTRICT` throughout and tested edge cases with orphaned prescription records.

## Run locally
```sql
-- 1. Create schema (healthcare project sql.ssmssln)
-- 2. Run data inserts
-- 3. Execute analytical queries in Healthcare_Analytics.md
```
