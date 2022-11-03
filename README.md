# School_District_Analysis


## Project Overview
In this project, I have been tasked with assisting the chief data scientist of a city school district with data analysis on student funding and students' standardized test scores. In addition to reviewing every student's math and reading scores, and the type of schools they attend, we will be aggregating the data to demonstrate trends in school performance based on school budget.

## Results

### Deliverable 1: Collect the Data

We imported this csv [filename](/Resources/new_full_student_data.csv) that contained all the student data for our analysis.
```
# Create the path and import the data
full_student_data = os.path.join('Resources/new_full_student_data.csv')
student_df = pd.read_csv(full_student_data)
```

```
# Verify that the data was properly imported
student_df.head()
```

 ![This is an image](/Resources/screenshot1.png)

### Deliverable 2: Prepare the Data

We prepared the data by running the following functions to clean the data and remove null values and duplicative rows:
 	
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

After changing the column to an integer the data looks better:
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

### Deliverable 4: Drill Down into the Data
We performed deeper analysis by breaking down the data on math and reading scores among the different grades.

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

```

# Use loc with conditionals to select all reading scores from 10th graders at Dixon High School.
reading_scores=student_df.loc[((student_df['school_name'] == "Dixon High School") 
                    & (student_df['grade'] == 10))][['school_name', 'reading_score']]
reading_scores
```
![This is an image](/Resources/screenshot7.png)

```
student_df.loc[(student_df['grade'] == 11) | (student_df['grade'] == 12), ['reading_score']].mean()
reading_score    74.900381
dtype: float64
```

### Deliverable 5: Make Comparisons Between District and Charter Schools
Our final deliverable was to compare district vs charter schools for budget, size, and scores.
  
```
# Use groupby and mean to find the average reading and math scores for each school type.
Avg_reading_grp_by_schooltype = student_df.groupby(['school_type']).reading_score.mean()
Avg_reading_grp_by_schooltype

school_type
Charter    72.450603
Public     72.281219
Name: reading_score, dtype: float64
```

```Avg_math_grp_by_schooltype = student_df.groupby(['school_type']).math_score.mean()
Avg_math_grp_by_schooltype

school_type
Charter    66.761883
Public     62.951576
Name: math_score, dtype: float64
```
```
Avg_Scores_by_school_type_df = pd.DataFrame({'Average Math Score': Avg_math_grp_by_schooltype,
                                                'Average Reading Score': Avg_reading_grp_by_schooltype,
                                               })
                     
Avg_Scores_by_school_type_df.tail()     
```

![This is an image](/Resources/screenshot8.png)

```
# Use the `groupby`, `count`, and `sort_values` functions to find the
# total number of students at each school and sort from most students to least students.
Total_student_by_school = student_df.groupby('school_name').count()
Total_student_by_school.iloc[0:, 0:1].sort_values(by='student_id', ascending=False)
```

![This is an image](/Resources/screenshot9.png)


## Summary

After the analysis the results conclude that there was very little difference in average reading scores among charter and public schools and only a slight difference in average math scores. It is difficult to access whether school budget had an impact without further analysis.

- The code file can be found here:  [filename](/Student_Data_Challenge_Starter_Code.ipynb)

