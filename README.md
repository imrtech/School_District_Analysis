# School_District_Analysis


## Project Overview
In this project, I have been tasked with assisting the chief data scientist of a city school district with data analysis on student funding and students' standardized test scores. In addition to reviewing every student's math and reading scores, and the type of schools they attend, we will be aggregating the data to demonstrate trends in school performance based on school budget.

## Results

We performed the following tasks:

### Deliverable 1: Collect the Data

- Here is the link to the student_data.csv file: [filename](/Resources/new_full_student_data.csv)

 ![This is an image](/Resources/screenshot1.png)

### Deliverable 2: Prepare the Data
We ran the following functions to clean the data and remove null values and duplicative rows:
 	'isna()
	'dropna() 
	'duplicated()

```
#Check for null values
student_df.isna().sum()
student_id          0
student_name        0
grade               0
school_name         0
reading_score    1968
math_score        982
school_type         0
school_budget       0
dtype: int64
```

```
# Drop rows with null values and verify removal
student_df = student_df.dropna()
student_df.isna().sum()
student_id       0
student_name     0
grade            0
school_name      0
reading_score    0
math_score       0
school_type      0
school_budget    0
dtype: int64
```
```
# Check for duplicated rows
student_df.duplicated().sum()
1836
```
```
# Drop duplicated rows and verify removal
student_df = student_df.drop_duplicates()
student_df.duplicated().sum()
0
```

We confirmed the data types:

```# Check data types
student_df.dtypes
student_id         int64
student_name      object
grade             object
school_name       object
reading_score    float64
math_score       float64
school_type       object
school_budget      int64
dtype: object
```

Upon review, we noticed that the grade column was an object and needed to be changed to a numeric column.

```# Examine the grade column to understand why it is not an int
student_df['grade']
0         9th
1         9th
2         9th
3         9th
5         9th
         ... 
19508    10th
19509    12th
19511    11th
19512    11th
19513    12th
Name: grade, Length: 14831, dtype: object
```
```
# Remove the non-numeric characters and verify the contents of the column
student_df['grade'] = student_df['grade'].str.replace('th', '')
student_df['grade']
0         9
1         9
2         9
3         9
5         9
         ..
19508    10
19509    12
19511    11
19512    11
19513    12
Name: grade, Length: 14831, dtype: object
```

Aftering changing the column to an integer the data looks better:
```
# Change the grade column to the int type and verify column types
student_df['grade'] = student_df['grade'].astype(int)
student_df.dtypes
student_id         int64
student_name      object
grade              int32
school_name       object
reading_score    float64
math_score       float64
school_type       object
school_budget      int64
dtype: object
student_df.tail()
```
 ![This is an image](/Resources/screenshot2.png)


### Deliverable 3: Summarize the Data

We used summary statistics to look closely at the math and reading scores.

```
# Display summary statistics for the DataFrame
student_df.describe()
```
![This is an image](/Resources/screenshot3.png)

```
# Display the mean math score using the mean function
student_df['math_score'].mean()
64.6757332614119
```
```
# Store the minimum reading score as min_reading_score
min_reading_score = student_df['reading_score'].min()
min_reading_score
10.5
```
```
### Deliverable 4: Drill Down into the Data
We performed deeper analysis by breaking down the data on math and reading scores among the different grades.
```

```
# Use loc to display the grade column
student_df.loc[:,'grade']
0         9
1         9
2         9
3         9
5         9
         ..
19508    10
19509    12
19511    11
19512    11
19513    12
Name: grade, Length: 14831, dtype: int32
```
```
# Use `iloc` to display the first 3 rows and columns 3, 4, and 5.
student_df.iloc[0:3, 3:6]
```

![This is an image](/Resources/screenshot4.png)

```
# Select the rows for grade nine and display their summary statistics using `loc` and `describe`.
student_df.loc[student_df['grade'] == 9].describe()
```

![This is an image](/Resources/screenshot5.png)

```
# Store the row with the minimum overall reading score as `min_reading_row`
# using `loc` and the `min_reading_score` found in Deliverable 3.
min_reading_score = student_df['reading_score'].min()
min_reading_row = student_df.loc[student_df['reading_score'] == min_reading_score]
min_reading_row
```

![This is an image](/Resources/screenshot6.png)


## Summary

The minimum reading score of 10.5 came from a 10 grader from a charter school with a budget of 870,334. We ran more analysis on all 10th graders from the same school.
   


In summary, 



- The code file can be found here:  [filename](/Student_Data_Challenge_Starter_Code.ipynb)

