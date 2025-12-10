# Beginner SQL Practice

## 1. Patients with Gender 'M'
---
### Problem Statement
Show first name, last name, and gender of patients whose gender is 'M'.

### Best Approach
Filter rows where gender = 'M'.

### Alternative Options
Use LOWER(gender) if data is inconsistent.

### Solution
```sql
SELECT first_name, last_name, gender
FROM patients
WHERE gender = 'M';
```

---

## 2. Patients Without Allergies
---
### Problem Statement
Show first and last names of patients who have NULL allergies.

### Best Approach
Use IS NULL.

### Alternative Options
None required.

### Solution
```sql
SELECT first_name, last_name
FROM patients
WHERE allergies IS NULL;
```

---

## 3. First Names Starting With 'C'
---
### Problem Statement
Show first names of patients that start with the letter 'C'.

### Best Approach
Use LIKE for pattern matching.

### Alternative Options
Use ILIKE for PostgreSQL case-insensitive match.

### Solution
```sql
SELECT first_name
FROM patients
WHERE first_name LIKE 'C%';
```

---

## 4. Weight Between 100 and 120
---
### Problem Statement
Show patients weighing between 100 and 120 (inclusive).

### Best Approach
Use range-based conditions.

### Alternative Options
Use BETWEEN 100 AND 120.

### Solution
```sql
SELECT first_name, last_name
FROM patients
WHERE weight >= 100
AND weight <= 120;
```

---

## 5. Replace NULL Allergies with 'NKA'
---
### Problem Statement
Replace NULL allergy values with 'NKA'.

### Best Approach
Update rows where allergies IS NULL.

### Alternative Options
None required.

### Solution
```sql
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

---

## 6. Concatenate First and Last Name
---
### Problem Statement
Show first and last name as full_name.

### Best Approach
Use CONCAT.

### Alternative Options
Use || in PostgreSQL.

### Solution
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM patients;
```

---

## 7. Show Full Province Name
---
### Problem Statement
Show first name, last name, and full province name via JOIN.

### Best Approach
Join patients with province_names.

### Alternative Options
None required.

### Solution
```sql
SELECT first_name, last_name, province_name
FROM patients
JOIN province_names
ON province_names.province_id = patients.province_id;
```

---

## 8. Count Patients Born in 2010
---
### Problem Statement
Count how many patients were born in 2010.

### Best Approach
Use LIKE '2010%' for date matching.

### Alternative Options
Use YEAR(birth_date) = 2010 (MySQL).

### Solution
```sql
SELECT COUNT(birth_date) AS total_patients
FROM patients
WHERE birth_date LIKE '2010%';
```

---

## 9. Tallest Patient
---
### Problem Statement
Show first name, last name, height of tallest patient.

### Best Approach
ORDER BY height DESC LIMIT 1.

### Alternative Options
Use MAX(height) with subquery.

### Solution
```sql
SELECT first_name, last_name, height
FROM patients
ORDER BY height DESC
LIMIT 1;
```

---

## 10. Patients With Specific IDs
---
### Problem Statement
Show all columns for patients with IDs: 1,45,534,879,1000.

### Best Approach
Use IN().

### Alternative Options
None.

### Solution
```sql
SELECT *
FROM patients
WHERE patient_id IN (1,45,534,879,1000);
```

---

## 11. Total Admissions
---
### Problem Statement
Show total number of admissions.

### Best Approach
Use COUNT(*).

### Alternative Options
COUNT(patient_id).

### Solution
```sql
SELECT COUNT(*) AS pts
FROM admissions;
```

---

## 12. Same-Day Admission & Discharge
---
### Problem Statement
Show patients admitted and discharged on the same day.

### Best Approach
Compare both dates.

### Alternative Options
None.

### Solution
```sql
SELECT *
FROM admissions
WHERE admission_date = discharge_date;
```

---

## 13. Count Admissions for Patient 579
---
### Problem Statement
Show total admissions for patient 579.

### Best Approach
Filter by patient_id then count.

### Alternative Options
None.

### Solution
```sql
SELECT patient_id, COUNT(*) AS admissions
FROM admissions
WHERE patient_id = 579;
```

---

## 14. Unique Cities in Province 'NS'
---
### Problem Statement
Show unique cities where patients belong to province 'NS'.

### Best Approach
Use DISTINCT with a filter.

### Alternative Options
Join unnecessary if province stored in patients table.

### Solution
```sql
SELECT DISTINCT city
FROM patients
WHERE province_id = 'NS';
```

---

## 15. Height & Weight Condition
---
### Problem Statement
Show patients with height > 160 and weight > 70.

### Best Approach
Use AND for multi-condition filtering.

### Alternative Options
None.

### Solution
```sql
SELECT first_name, last_name, birth_date
FROM patients
WHERE height > 160
AND weight > 70;
```

---

## 16. Hamilton Patients With Allergies
---
### Problem Statement
Show Hamilton patients whose allergies are NOT NULL.

### Best Approach
Filter by city + IS NOT NULL.

### Alternative Options
None.

### Solution
```sql
SELECT first_name, last_name, allergies
FROM patients
WHERE city = 'Hamilton'
AND allergies IS NOT NULL;
```

---
