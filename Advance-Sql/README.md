# SQL Practice — Hard Category (42–52)

## 42. Group Patients Into Weight Groups
---
### Problem Statement
Group patients into weight groups (100–109 → 100 group, 110–119 → 110 group, etc.) and show patient count per group.

### Best Approach
Use FLOOR(weight/10)*10 to compute group ranges, then group and count.

### Alternative Options
Use CASE statements, but FLOOR is more scalable.

### Solution
```sql
SELECT FLOOR(weight / 10) * 10 AS weight_group,
COUNT(patient_id) AS patients_in_grp
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC;
```
---

## 43. Obesity Check Using BMI
---
### Problem Statement
Show patient_id, weight, height, and a boolean isObese column based on BMI ≥ 30.

### Best Approach
BMI formula: weight (kg) / (height in meters)^2.

### Alternative Options
ROUND() BMI and show numeric value.

### Solution
```sql
SELECT patient_id, weight, height,
(CASE WHEN weight / POWER(height/100.0, 2) >= 30 THEN 1 ELSE 0 END) AS isObese
FROM patients;
```
---

## 44. Patients Diagnosed With Epilepsy by Doctor Lisa
---
### Problem Statement
Show patient details + attending doctor’s specialty for Epilepsy cases treated by a doctor named Lisa.

### Best Approach
Join patients, admissions, and doctors; filter both diagnosis and doctor first_name.

### Alternative Options
Use aliases for cleaner readability.

### Solution
```sql
SELECT p.patient_id, p.first_name, p.last_name, d.specialty
FROM patients p
JOIN admissions a ON a.patient_id = p.patient_id
JOIN doctors d ON d.doctor_id = a.attending_doctor_id
WHERE a.diagnosis = 'Epilepsy'
AND d.first_name = 'Lisa';
```
---

## 45. Temporary Password Generation
---
### Problem Statement
For patients with admissions, generate temp_password = patient_id + length(last_name) + birth year.

### Best Approach
Use CONCAT + LENGTH + YEAR.

### Alternative Options
Can use CONCAT_WS for cleaner format.

### Solution
```sql
SELECT a.patient_id,
CONCAT(a.patient_id, LENGTH(p.last_name), YEAR(p.birth_date)) AS temp_password
FROM patients p
JOIN admissions a ON a.patient_id = p.patient_id
GROUP BY a.patient_id;
```
---

## 46. Insurance Groups and Total Admission Cost
---
### Problem Statement
Even patient_ids → insurance (cost = 10); odd → no insurance (cost = 50).  
Show total cost per group.

### Best Approach
CASE WHEN patient_id % 2 = 0 identifies insurance; apply cost using CASE.

### Alternative Options
Create a computed column first, then aggregate.

### Solution
```sql
SELECT 
  CASE WHEN patient_id % 2 = 0 THEN 'Yes' ELSE 'No' END AS has_insurance,
  SUM(CASE WHEN patient_id % 2 = 0 THEN 10 ELSE 50 END) AS cost
FROM admissions
GROUP BY has_insurance;
```
---

## 47. Provinces With More Male Than Female Patients
---
### Problem Statement
Show only province_names where male count > female count.

### Best Approach
Use conditional COUNT inside HAVING.

### Alternative Options
Use SUM(CASE WHEN gender='M' THEN 1 ELSE 0 END).

### Solution
```sql
SELECT pr.province_name
FROM province_names pr
JOIN patients p ON pr.province_id = p.province_id
GROUP BY pr.province_name
HAVING 
  COUNT(CASE WHEN p.gender='M' THEN 1 END) >
  COUNT(CASE WHEN p.gender='F' THEN 1 END);
```
---

## 48. Find Specific Patient Matching Multiple Criteria
---
### Problem Statement
Find patient matching:
- first_name contains 'r' after first 2 letters  
- gender = 'F'  
- birth month = Feb, May, Dec  
- weight 60–80  
- patient_id is odd  
- city = Kingston  

### Best Approach
Use multiple AND filters and LIKE for name pattern.

### Alternative Options
Use SUBSTRING instead of LIKE.

### Solution
```sql
SELECT *
FROM patients
WHERE SUBSTRING(first_name, 3, 1) = 'r'
AND gender = 'F'
AND MONTH(birth_date) IN (2,5,12)
AND weight BETWEEN 60 AND 80
AND patient_id % 2 = 1
AND city = 'Kingston';
```
---

## 49. Percent of Patients Who Are Male
---
### Problem Statement
Show percent of male patients rounded to two decimals.

### Best Approach
COUNT male / total * 100.

### Alternative Options
Use conditional aggregation directly.

### Solution
```sql
SELECT CONCAT(
  ROUND(
    (COUNT(CASE WHEN gender='M' THEN 1 END) * 100.0) / COUNT(*), 
  2), '%') AS percent_male
FROM patients;
```
---

## 50. Admissions Per Day + Change from Previous Day
---
### Problem Statement
Show total admissions per date and difference from the previous date.

### Best Approach
Use window function LAG.

### Alternative Options
Self-join previous date.

### Solution
```sql
SELECT admission_date,
COUNT(patient_id) AS admissions_day,
COUNT(*) - LAG(COUNT(*)) OVER (ORDER BY admission_date) AS admissions_change
FROM admissions
GROUP BY admission_date
ORDER BY admission_date;
```
---

## 51. Sort Province Names With ‘Ontario’ Always on Top
---
### Problem Statement
Sort provinces alphabetically but Ontario must always appear first.

### Best Approach
Create sort flag using CASE.

### Alternative Options
Use ORDER BY FIELD() in MySQL.

### Solution
```sql
SELECT province_name
FROM (
  SELECT province_name,
  CASE WHEN province_name = 'Ontario' THEN 0 ELSE 1 END AS sort_flag
  FROM province_names
) x
ORDER BY sort_flag ASC, province_name ASC;
```
---

## 52. Yearly Admissions Summary Per Doctor
---
### Problem Statement
Show doctor_id, doctor name, specialty, year, and total admissions for that year.

### Best Approach
Extract year, join doctors + admissions, then group.

### Alternative Options
Use EXTRACT(YEAR FROM admission_date).

### Solution
```sql
SELECT d.doctor_id,
CONCAT(d.first_name,' ',d.last_name) AS doctor_name,
d.specialty,
YEAR(a.admission_date) AS admission_year,
COUNT(*) AS total_admissions
FROM doctors d
JOIN admissions a ON a.attending_doctor_id = d.doctor_id
GROUP BY d.doctor_id, admission_year
ORDER BY d.doctor_id, admission_year;
```
---
