[Project provided by DataCamp](https://app.datacamp.com/learn/projects/analyzing_students_mental_health)

# Asking Questions

## Problem Statement

Does going to university in a different country affect your mental health? A Japanese international university surveyed its students in 2018 and published a study the following year that was approved by several ethical and regulatory boards. 

## Business Task

The original study found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression.

This case study is using the same data that the original analyst used to come to the same conclusion as them, as well as seeking other insights based on the data provided.

## Key Stakeholders

- International students (current and future)
- Faculty of the international university

## Key Questions

- Does going to a university in a different country affect your mental health?
- Do these students have a higher risk of mental health difficulties than the general population?
- Are social connectedness and acculturative stress key factors in predicting mental health difficulties?

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

While the survey provided us with a lot of data from the students, two of our questions are “Do these students have a higher risk of mental health difficulties than the general population?” and  “Are social connectedness and acculturative stress key factors in predicting mental health difficulties?” The only columns that we will be using are listed below. 

## Data Aggregation

It is easier to compare between test scores between international students and domestic students by finding the averages of the scores based on the students length of stay.

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

### Visualization
![stay](https://github.com/user-attachments/assets/3abc82e7-c34f-4423-ad29-214ecc9b43e8)

## Average Scores of International Students by Age

As we can see from the query results, the majority of the patients that had taken the survey were between the ages of 18 and 22, which makes sense considering those are the most common ages for university students. The visualization shows that the older the student is, the higher the social connectedness score. If we had a higher sample size for these older or “nontraditional” students, we would be able to analyze that data better as the average score between one to three students is less accurate than the average score of 20 or more students. 

### Query

```sql
SELECT
	age,
	COUNT(*) AS count_int,
	ROUND(AVG(todep), 2) AS average_phq,
	ROUND(AVG(tosc), 2) AS average_scs,
	ROUND(AVG(toas),2) AS average_as
FROM public.students
WHERE
	inter_dom = 'Inter'
GROUP BY 
	age
ORDER BY 
	age
```

### Query results

| age | count_int | average_phq | average_scs | average_as |
| --- | --- | --- | --- | --- |
| 17 | 3 | 4.67 | 37.33 | 70.67 |
| 18 | 28 | 8.75 | 34.11 | 80.61 |
| 19 | 41 | 8.44 | 37.9 | 74.1 |
| 20 | 34 | 7.35 | 38.21 | 73.26 |
| 21 | 39 | 9.23 | 37.74 | 75.23 |
| 22 | 14 | 8.36 | 38.14 | 70.43 |
| 23 | 12 | 9.67 | 32 | 81.25 |
| 24 | 6 | 4.67 | 42.33 | 74.33 |
| 25 | 9 | 6.11 | 37.33 | 80.78 |
| 27 | 1 | 10 | 35 | 42 |
| 28 | 3 | 3.33 | 39 | 71 |
| 29 | 4 | 3.75 | 43 | 63.5 |
| 30 | 3 | 9.33 | 41 | 97.33 |
| 31 | 4 | 5.75 | 43.5 | 80.25 |

### Visualization
![age](https://github.com/user-attachments/assets/bc69a8ce-7cd8-49b8-8481-e3c51e005dfa)

## International Graduate vs. International Undergraduate Students

Although undergraduate and graduate students seem to have the around the same average score for acculturative stress, undergraduate students seem to be scoring lower on social connectedness. This may be due to graduate students taking classes that are related to their degree, where they may have classes with many of the same people, whereas undergraduate students are taking many general education classes that are not directly related to their degree. Undergraduate students seem to be scoring almost twice as high as graduate students when it comes to depression, which could be related to social connectedness, but also could have many other factors that have lead to these average scores.

### Query

```sql
SELECT
	academic,
	COUNT(*) AS count_student,
	ROUND(AVG(todep), 2) AS average_phq,
	ROUND(AVG(tosc), 2) AS average_scs,
	ROUND(AVG(toas),2) AS average_as
FROM public.students
WHERE
	inter_dom = 'Inter'
GROUP BY 
	academic
```

### Query Results

| academic | count_student | average_phq | average_scs | average_as |
| --- | --- | --- | --- | --- |
| Under | 181 | 8.39 | 37.03 | 75.54 |
| Grad | 20 | 4.95 | 40.9 | 75.8 |

### Visualization
![average scores](https://github.com/user-attachments/assets/c9651e9a-50ee-46a5-9dbc-7bf4405204d6)

## Average Scores of All Students

One of our key questions is “Do these students have a higher risk of mental health difficulties than the general population?” By comparing the scores between international and domestic students, we can attempt to answer this question. While social connectedness seems to be having similar results despite being an international or domestic student, more international students seem to be experiencing more acculturative stress. However, based on the visualization this does not mean that international students have higher scores of depression, in fact it’s the opposite, as domestic students have higher scores of depression. This is where outside factors may be playing a bigger part than only social connectedness and acculturative stress.

### Query

```sql
SELECT
	inter_dom,
	COUNT(*) AS count_student,
	ROUND(AVG(todep), 2) AS average_phq,
	ROUND(AVG(tosc), 2) AS average_scs,
	ROUND(AVG(toas),2) AS average_as
FROM public.students
WHERE
	gender = 'Male'
	OR gender = 'Female'
GROUP BY 
	inter_dom
```

### Query results

| inter_dom | count_student | average_phq | average_scs | average_as |
| --- | --- | --- | --- | --- |
| Inter | 201 | 8.04 | 37.42 | 75.56 |
| Dom | 67 | 8.61 | 37.64 | 62.84 |

### Visualization
![average scores - all genders](https://github.com/user-attachments/assets/96a0d44b-dcf7-4a79-8586-248a9b1af8e7)

# Act

### Observations

1. As students are staying longer than five years at the university school, the visualization shows that as their social connectedness scores go down, their depression scores go up.
2. Our visualization shows that undergraduate students are experiencing less social connectedness than graduate students, and their depression scores are significantly higher. 
3. International students are experiencing more acculturative stress than domestic students, but domestic students are experiencing higher depression scores. Both domestic and international students experience around the same amount of social connectedness, so the high scores of depression among domestic students could be due to outside factors.

### Recommendation

There are several ways to improve social connectedness among students. 

- Creating communities for students based on their field of study. They can then meet other students that are studying the same subject as them, or even have classes with.
- A buddy program where international students pair up with a domestic student, and they can be a contact or resource for each other if they have any difficulties navigating a new country. Students can be paired up based on different things such as gender, age, field of study, interests, etc.
- Hosting more events for students to attend would also be beneficial. These could include study nights, movie nights, etc.

# Conclusion

## Final Conclusion

It’s safe to say that the original analysis was correct in predicting that social connectedness and acculturative stress are connected to students’ mental health, and my analysis supports that original hypothesis.

## Further Research and Improvements

I believe it would be beneficial for the survey to have a bigger sample size. Opening up the survey to other international schools in the country would allow analysts to get better insight on what may be affecting students’ mental health. The number of international students (201) greatly outweighed the number of domestic students (67), which could make the analysis less accurate. Same goes for undergraduate and graduate students; only 7% of students were graduate students, which leads us to the conclusion that there isn’t enough data to determine if these insights are accurate. Another survey taken from Japan Student Services Organization, there are approximately 279,274 international students total in Japan in the year 2023. Using a sample size calculator, we would need 384 or more measurements/surveys are needed to have a confidence level of 95% that the real value is within ±5% of the measured/surveyed value, which is 116 students less than what we had for this survey.
