# Case Study on EdTech Startup Data Using SQL

I did a Case Study on one of the datasets taken from Kaggle in the EdTech domain.
Here the SQL queries used in the case study are JOINS, GROUP BY, ORDER BY, WINDOW functions, and AGGREGATION FUNCTIONS like (AVG, COUNT, etc.)

![EdTech Startup logo](https://user-images.githubusercontent.com/72240938/212818020-c2350aea-3f53-49a9-ab21-2d0489ad6001.png)


### [Link To Dataset](https://www.kaggle.com/datasets/nxtwaveda/data-analyst)

The datasets I worked with in this case study are leads_basic_details, leads_interaction_details, and leads_demo_watched_details.

1. Write a code to join tables leads_basic_details and leads_demo_watched_details with mentioned columns.

Ans:
Code:

SELECT lbd.lead_id, lbd.age, lbd.current_education, lbd.gender,lbd.current_city, lbd.lead_gen_source, ldwd.language, ldwd.watched_percentage
from leads_basic_details lbd
right join leads_demo_watched_details ldwd on lbd.lead_id = ldwd.lead_id;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212816583-3f26eb36-7ee1-4dff-b3d9-cdcff1892bbc.png)

2. Write a code to join tables leads_basic_details and leads_interaction_details with mentioned columns.

Ans:
Code:

SELECT lbd.lead_id, lbd.age, lbd.current_education, lbd.gender,lbd.current_city, lbd.lead_gen_source,lid.lead_stage, lid.call_status
from leads_basic_details lbd
right join leads_interaction_details lid on lbd.lead_id=lid.lead_id;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212816742-b465e374-d8ba-4273-802d-05957402dbe6.png)

3. Make a column for age defining age named AgeGroup and update them in the groups i.e. Below 20 and 20-30.

Ans:
Code:

ALTER table leads_demowatchedbasic_details
ADD COLUMN AgeGroup varchar(10);
UPDATE leads_demowatchedbasic_details
SET AgeGroup = "Under 20"
where age<20;
UPDATE leads_demowatchedbasic_details
SET AgeGroup = "20-30"
where age>=20 and age<=30;

4. Find average demo video watched percentage for each Gender and Language respectively.

Ans:
Code:

SELECT ldd.gender, ldd.language,
avg(watched_percentage) over(partition by gender, language) as AvgPercentage
from leads_demowatchedbasic_details ldd;

## Output:
For Gender : Female
English: 52.7813
Hindi: 41.8462
Telugu: 47.8286
For Gender : Male
English: 69.6875
Hindi: 60.7143
Telugu: 60.037

5.  Find City which has got most number of leads overall.

Ans:
Code:

SELECT current_city, COUNT(*) AS TotalLeads
from `leads_demowatchedbasic_details` ldd
group by current_city
order by TotalLeads desc;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817015-c21172dd-93fc-48df-a92e-190a50723206.png)

6. Find average watched percentage by language.

Ans:
Code:

SELECT language, avg(watched_percentage) as AvgWatchedPercentage
from `leads_demowatchedbasic_details` ldd
group by language
order by AvgWatchedPercentage desc;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817092-6870eef8-3223-420d-9f2c-9ea22eb311ae.png)

7. Find source from which highest leads have been generated.
Ans:
Code:

SELECT lead_gen_source, count(*) as TotalCountOfLeadGen
from `leads_demowatchedbasic_details` ldd
group by lead_gen_source
order by TotalCountOfLeadGen desc;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817277-c8766d44-a4be-4520-a915-9baf200fc7c8.png)


8. Find the number of successful and unsuccessful calls.
Ans:
Code:

SELECT call_status, count(*) as TotalLeads from `leads_interaction_details`
group by call_status;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817422-9424a8a9-c75f-48ae-b947-db4d73ee718e.png)


9. Delete age in the table interactionbasic_details which are outliers i.e. Age more than 100.
Ans:
Code:

DELETE from `leads_interactionbasic_details`
where age>100;

10. Find Age Group by call status of successful calls.
Ans:
Code:

SELECT lid.AgeGroup, count(*) as TotalSuccessfulCalls
from leads_interactionbasic_details lid
where call_status="successful"
group by AgeGroup
order by TotalSuccessfulCalls desc;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817532-c7b07b0b-60d5-4cda-98b1-3db86d266848.png)

11. Find Age Group by call status of total unsuccessful calls.
Ans:
Code:

SELECT lid.AgeGroup, count(*) as TotalUnsuccessfulCalls
from leads_interactionbasic_details lid
where call_status="unsuccessful"
group by AgeGroup
order by TotalUnsuccessfulCalls desc;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817588-19a71778-ad83-4202-a7c3-71305a7166e6.png)


12. Find avg duration of demo session watched for different age groups, current_education and language.
Ans:
Code:

SELECT ldd.AgeGroup, ldd.current_education, ldd.language,
avg(watched_percentage) over(partition by AgeGroup, current_education, language) as AvgPercentage
from leads_demowatchedbasic_details ldd
order by AvgPercentage desc;

## Output:

For Age Group:- 20-30 & English Language:
Intermediate Completed: 76%
Looking For Job: 65.45%
Degree: 65.18%
Intermediate: 57.64%
B.Tech : 56%
10th Completed: 40%

For Age Group:- 20-30 & Hindi Language:
Intermediate Completed: 87%
Intermediate: 59.3%
B.Tech: 54.25%
Looking For Job: 44.2%
Degree: 26.66%

For Age Group:- 20-30 & Telugu Language:
Intermediate Completed: 69%
Degree: 64.5%
Looking For Job: 56.13%
B.Tech: 54.44%
10th Completed: 31%
For Age Group:- Under 20 & Hindi Language:
Intermediate: 59.3%
B.Tech: 54.25%

For Age Group:- Under 20 & English Language:
Intermediate: 57.64%
B.Tech: 56%

13. Find Age Group by Average demo session watched percentage
Ans:
Code:

SELECT AgeGroup, avg(watched_percentage) as AvgWatchedPercentage
from leads_demowatchedbasic_details
group by AgeGroup
order by AvgWatchedPercentage desc;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817827-ce2307c1-e5a3-4fb4-b27d-7f5ca0cef43a.png)

14. Find lead stages by No. of successful calls
Ans:
Code:
SELECT lead_stage, COUNT(*) as TotalSuccessfulLeads
from leads_interactionbasic_details
where call_status="successful"
group by lead_stage;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817894-140c674c-84ec-4724-9258-bc30d715a4a6.png)

15. Find lead stages by no. of unsuccessful calls.
Ans:
Code:

SELECT lead_stage, COUNT(*) as TotalUnsuccessfulLeads
from leads_interactionbasic_details
where call_status="unsuccessful"
group by lead_stage;

## Output:
![image](https://user-images.githubusercontent.com/72240938/212817964-9da795ef-6a5c-4fac-a7f3-e211e8cedb16.png)


## Key Insights:

* With respect to Gender “Female”, English is mostly preferred when the demo session is watched.•With respect to Gender “Male”, English is mostly preferred when the demo session is watched.
* Hyderabad city has the most number of leads.
* English has the most average watched percentage overall.
* Social Media, SEO & Email-Marketing has the highest total count of lead generation.
* 85% is the total % of successful calls and 15 % is the total % of unsuccessful calls.
* Lead in lead_stage has the highest number of calls.
* Conversion in lead_stage has the least no. of calls.
* % Successful calls are the highest in the “conversion” lead stage.
* % Unsuccessful calls are the highest in the “lead” lead stage.










