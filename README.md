# Hospital_data_histrory
# select database project_medical_data_history
use project_medical_data_history;

# 1 Show first name, last name, and gender of patients whos gender is M
select first_name,last_name,gender from patients where gender='M';

# 2 Show first name and last name of patients who does not have allergies.
select first_name,last_name from patients where allergies is null;

# 3 Show first name of patients that start with the letter'C';
select first_name from patients where first_name LIKE 'C%';

# 4 Show first name and last name of patients that weight within the range of 100 to 120
select first_name ,last_name from patients where weight >= 100 and weight<=120;
select first_name ,last_name from patients WHERE weight BETWEEN 100 AND 120;

# 5 Update the patients table for the allergies column. If the patient&#39;s allergies is null then
# replace it with &#39;NKA&#39;
update patients set allergies = 'NKA' where allergies is null;

# 6 Show first name and last name concatenated into one column to show their full name.
select concat(first_name," ",last_name) as full_name from patients;

# 7 Show first name, last name, and the full provinceprovince_names name of each patient.
select patients.first_name,patients.last_name,province_names.province_name from patients join province_names on 
patients.province_id=province_names.province_id;

# 8 Show how many patients have a birth_date with 2010 as the birth year.
select count(*) from patients where birth_date  >='2010-01=01' and birth_date <='2010-12-31';
select count(*) from patients where year(birth_date)=2010;

# 9 Show the first_name, last_name, and height of the patient with the greatest height.
select first_name,last_name,height from patients where height = (select MAX(height) from patients);

# 10 Show all columns for patients who have one of the following patient_ids:
select * from patients where patient_id in (1,45,534,879,1000);

# 11 Show the total number of admissions
select count(admission_date) from admissions;

# 12 Show all the columns from admissions where the patient was admitted and discharged on the same day.
select * from admissions where admission_date = discharge_date;

# 13 Show the total number of admissions for patient_id 579.
select count(patient_id) from admissions where patient_id = '579';

# 14 Based on the cities that our patients live in, show unique cities that are in province_id 'NS' 
select distinct city from patients where province_id = 'NS';

# 15 Write a query to find the first_name, last name and birth date of patients 
# who have height more than 160 and weight more than 70
select first_name,last_name,birth_date,weight,height from patients where weight >70 and height > 160;

# 16 Show unique birth years from patients and order them by ascending
select distinct year(birth_date) AS birth_year from patients order by birth_year asc;

# 17 Show unique first names from the patients table which only occurs once in the list.
SELECT first_name FROM patients GROUP BY first_name HAVING COUNT(*) = 1;

# 18 Show patient_id and first_name from patients where their first_name start and ends with
# 's' and is at least 6 characters long.
select patient_id,first_name from patients where first_name like 's%' and first_name like '%s'
and length(first_name)=6;

# 19 how patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.   Primary diagnosis is stored in the admissions table.
select patients.patient_id,patients.first_name,patients.last_name from patients 
join admissions on patients.patient_id = admissions.patient_id 
where admissions.diagnosis = "Dementia";

# 20 Display every patient&#39;s first_name. Order the list by the length of each name and then by alphbetically.
select first_name from patients order by length(first_name) asc,first_name asc;

# 21 Show the total amount of male patients and the total amount of female patients in the
# patients table. Display the two results in the same row.
select (select count(*) from patients  where gender = 'M') as male_count,
(select count(gender) from patients where gender = 'F') as female_count from patients
 limit 1;
 
 # 22 same as 21 
 
 # 23. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the
 # same diagnosis
 SELECT patient_id, diagnosis FROM admissions GROUP BY patient_id, diagnosis
HAVING COUNT(*) > 1;

# 24 Show the city and the total number of patients in the city.
# Order from most to least patients and then by city name ascending
 select city, count(*) as total_patient from patients group by city 
 order by total_patient,city asc;
 
# 25 Show first name, last name and role of every person that is either patient or doctor.
# The roles are either &quot;Patient&quot; or &quot;Doctor&quot;
select first_name,last_name,("doctor") as role from doctors UNION
select first_name,last_name,("patient") as role from patients;

# 26 Show all allergies ordered by popularity. Remove NULL values from query.
select allergies,count(*) from patients  where allergies is not null group by allergies order by count(*) desc;

# 27 Show all patient&#39;s first_name, last_name, and birth_date who were born in the 1970s
# decade. Sort the list starting from the earliest birth_date.
select patient_id,first_name,last_name,birth_date from patients where 
birth_date >= '1970-01-01' and birth_date <= '1979-12-31' order by birth_date asc;

# 28 We want to display each patient&#39;s full name in a single column. Their last_name in all
# upper letters must appear first, then first_name in all lower case letters. Separate the
# #EX: SMITH,jane
SELECT patient_id,CONCAT(upper(last_name),",",lower(first_name)) as full_name from patients ORDER BY first_name DESC;

# 29 Show the province_id(s), sum of height; 
# where the total sum of its patient's height is greater than or equal to 7,000.
select province_id,sum(height) from patients group by province_id
having sum(height) >= 7000;

# 30 Show the difference between the largest weight and smallest weight for patients with the
# last name &#39;&#39;Maroni&#39;;
select abs((select max(weight) from patients where last_name = "Maroni") - 
 (select min(weight) from patients where last_name = "Maroni")) as difference_wight;
select abs(max(weight) - min(weight)) from patients where last_name="Maroni";

# 31 Show all of the days of the month (1-31) and how many admission_dates occurred on that day. 
# Sort by the day with most admissions to least admissions.
select day(admission_date),count(patient_id) from admissions
group by day(admission_date) order by count(patient_id) desc;

# 32 Show all of the patients grouped into weight groups. Show the total amount of patients in each weight group. 
# Order the list by the weight group decending
SELECT CONCAT(FLOOR(weight / 10) * 10, '-', FLOOR(weight / 10) * 10 + 9) AS weight_group,
COUNT(*) AS total_patients_in_group
FROM patients GROUP BY weight_group ORDER BY weight_group DESC;

# 33 Show patient_id, weight, height, isObese from the patients table. 
# Display isObese as a boolean 0 or 1. Obese is defined as weight(kg)/(height(m).
# Weight is in units kg. Height is in units cm.
SELECT patient_id,weight,height,
CASE WHEN (weight / (height / 100 * height / 100)) >= 30 THEN 1 ELSE 0 END AS isObese
FROM patients;
select patient_id, weight,height,round(weight/height) as isobese from patients;
 
# 34 Show patient_id, first_name, last_name, and attending doctor's specialty. Show only the #patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'. Check #patients, admissions, and doctors tables for required information.
select patients.patient_id,patients.first_name,patients.last_name,doctors.specialty from patients join admissions on patients.patient_id=admissions.patient_id
join doctors on admissions.attending_doctor_id=doctors.doctor_id
where admissions.diagnosis="Epilepsy" and doctors.first_name="Lisa";

# 35 All patients who have gone through admissions, can see their medical documents on our site. 
# Those patients are given a temporary password after their first admission. 
# Show the patient_id and temp_password.
select patient_id,concat(patient_id,length(last_name),year(birth_date)) as temp_password 
from patients WHERE patient_id IN (SELECT DISTINCT patient_id FROM admissions);




