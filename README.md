###STEP 1: SQL-data-cleaning
```SQL
SELECT * FROM club_member_info cmi 
LIMIT 10;
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

### STEP 2: We created a new table 
```SQL
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```

### STEP 3: Copied data from the original data so they will not affect the root one
```SQL
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info ;
```

### STEP 4: found that Data has unnecessary whitespaces, we use TRIM to clean leading and trailing them.
```SQL
UPDATE club_member_info_cleaned 
SET full_name = TRIM(LOWER(full_name));
```

### STEP 5: we can't call MEDIAN function directly in SQLite. However, this syntax could be applied by doing subqueries
```SQL
UPDATE club_member_info_cleaned 
SET age = (Select Median(age) FROM club_member_info_cleaned) WHERE age >100 or age ISNULL;
```

### STEP 6: since there are some missing value for martial_status. We count the number of each available catagories
```SQL
SELECT COUNT (*)
FROM club_member_info_cleaned
WHERE martial_status = 'married';
```
#####The query above is for 'married' status. Same syntax for 'single' and 'divorced'
#####We found that the dominated group is 'married' with 881 counts.
##### Let's update the missing value martial_status by 'married'
```SQL
UPDATE club_member_info_cleaned 
SET martial_status = 'married' WHERE martial_status ='';
```
### STEP 7: update mis-typo martial status 'divored' by 'divorced'
```SQL
UPDATE club_member_info_cleaned 
SET martial_status = 'divorced' WHERE martial_status ='divored';
```
