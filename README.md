# Students' Mental Health
This project was provided by DataCamp.

## Problem Statement
Does going to university in a different country affect your mental health? A Japanese international university surveyed its students in 2018 and published a study the following year that was approved by several ethical and regulatory boards.

## Business Task
The original study found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression.
This case study is using the same data that the original analyst used to come to the same conclusion as them, as well as seeking other insights based on the data provided.

## Key Stakeholders
- International students (current and future)
- Faculty of the international university

## Key Questions
- Does going to a univeristy in a different country affect your mental health?
- Do these students have a higher risk of mental health difficulties than the general population?
- Are social connectedness and acculturative stress key factors in predicting mental health difficulties

## Assumptions and Limitations
The project an data from this case study was provided by DataCamp. DataCamp is a website that teaches companies and individuals the skills they need to work with data, and make it more accessible to learn. The data is limited to what was in the survey that the students had taken.

# Data Collection
## Data Location
There is no information on DataCamp’s website about where the original data was collected. However the data is located on DataCamp’s website with their public databases for analysts to practice their skills.

## Data Credibility and Bias
The data was provided by DataCamp, which since the platform is used specifically for education purposes, we can trust that it is reliable. For the sake of the case study, the data is taken from a survey of almost 300 students, which was collected by the original analyst. Any biases would come from the way that the data was collected, such as ways that certain questions are asked that may allude to certain outcomes.

## Potential Problems
Problems that may be found in the data could be related to several things. Another is that this survey only collected insights from students at a single university; sending the survey to multiple international universities would provide a better analysis since there would be more data to look at. With only surveying one university, you may not be able to identify if there are any trends on students’ mental health based on the university that they go to. There also is not information on whether all international students took this survey, or if the amount of students was a sample size. There may also be other factors that could influence scores such as things going on in the students’ personal lives.

# Data Cleaning

## Relevance to the Business Question

While the survey provided us with a lot of data from the students, two of our questions are “Do these students have a higher risk of mental health difficulties than the general population?” and  “Are social connectedness and acculturative stress key factors in predicting mental health difficulties?” The only columns that we will be using are listed to the right. 

| Field Name | Description |
| --- | --- |
| `inter_dom` | Types of students (international or domestic) |
| `japanese_cate` | Japanese language proficiency |
| `english_cate` | English language proficiency |
| `academic` | Current academic level (undergraduate or graduate) |
| `age` | Current age of student |
| `stay` | Current length of stay in years |
| `todep` | Total score of depression (PHQ-9 test) |
| `tosc` | Total score of social connectedness (SCS test) |
| `toas` | Total score of acculturative stress (ASISS test) |

## Data Aggregation
It is easier to compare betwen test scores between international students and domestic students by finding the averages of the scores based on the students length of stay.

# Data Analysis and Visualizations

## Data Organization and Formatting

Remembering our key questions: 

- Does going to a university in a different country affect your mental health?
- Are social connectedness and acculturative stress key factors in predicting mental health difficulties?
- Do these students have a higher risk of mental health difficulties than the general population?

By finding the average scores of these tests that track these factors, we can compare the scores of social connectedness and acculturative stress to scores of depression. The average of the scores also brings a decimal as a result, so by using the ROUND() function, we can round the averages to the first two decimal points.

Other factors were explored such as age and academic status (undergraduate vs. graduate).

## Average Scores of International Students by Stay

The amount of students that have stayed five years or longer that have taken the survey are significantly less than students that have stayed less than five years. This could be due to most university undergraduate programs lasting around four years. Later in the analysis we can see that out of all of the international students that took the survey, only about 10% of the students that took the survey were graduate students, however that does not mean that all of the students that have stayed longer than five years are graduate students. There may have been students that completed an undergraduate program at a different university, and decided to come to the international university for their graduate program, which would put them in the 1 year category.

### Query

```sql
SELECT
	stay,
	COUNT(*) AS count_int,
	ROUND(AVG(todep), 2) AS average_phq,
	ROUND(AVG(tosc), 2) AS average_scs,
	ROUND(AVG(toas),2) AS average_as
FROM public.students
WHERE
	inter_dom = 'Inter'
GROUP BY 
	stay 
ORDER BY 
	stay DESC
```

### Query results

| stay | count_int | average_phq | average_scs | average_as |
| --- | --- | --- | --- | --- |
| 10 | 1 | 13 | 32 | 50 |
| 8 | 1 | 10 | 44 | 65 |
| 7 | 1 | 4 | 48 | 45 |
| 6 | 3 | 6 | 38 | 58.67 |
| 5 | 1 | 0 | 34 | 91 |
| 4 | 14 | 8.57 | 33.93 | 87.71 |
| 3 | 46 | 9.09 | 37.13 | 78 |
| 2 | 39 | 8.28 | 37.08 | 77.67 |
| 1 | 95 | 7.48 | 38.11 | 72.8 |
