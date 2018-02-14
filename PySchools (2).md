

```python
# dependencies
import csv
import pandas as pd
import numpy as np
import os

```


```python
school_file = "schools_complete.csv"
student_file = "students_complete.csv"
school_df = pd.read_csv(school_file)
student_df = pd.read_csv(student_file)









```


```python
students_df = student_df.rename(columns = {'name':'Student Name', 'school':'School Name'})
schools_df = school_df.rename(columns = {'name':'School Name'})


```


```python
avg_math_score = student_df["math_score"].mean()
total_students = student_df["Student ID"].max()
total_budget = schools_df["budget"].sum()
students_pm = student_df.loc[(student_df["math_score"] >70)]
students_pr = student_df.loc[(student_df["reading_score"] > 70)]
num_pr = students_pr["reading_score"].count()
num_pm = students_pm["math_score"].count()
pct_passing_math = (num_pm/total_students) * 100
pct_passing_reading = (num_pr/total_students) * 100
passing_rate = (pct_passing_math + pct_passing_reading)/2

```


```python
District_Summary = pd.DataFrame({"Average Math Score": [avg_math_score],  
                                 "Total Budget": [total_budget],
                                 "Total Students": [total_students],
                                 "Total Schools": [total_schools],
                                "Percent Passing Math": [pct_passing_math],
                                "Percent Passing Reading": [pct_passing_reading],
                                "Overall Passing Rate": [passing_rate]})

District_Summary.set_index("Total Schools")
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Overall Passing Rate</th>
      <th>Percent Passing Math</th>
      <th>Percent Passing Reading</th>
      <th>Total Budget</th>
      <th>Total Students</th>
    </tr>
    <tr>
      <th>Total Schools</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>78.985371</td>
      <td>77.683883</td>
      <td>72.393985</td>
      <td>82.97378</td>
      <td>24649428</td>
      <td>39169</td>
    </tr>
  </tbody>
</table>
</div>




```python
merged_df = pd.merge(schools_df, students_df, on='School Name')
merged_df.drop(["School ID"], axis=1).head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>Student Name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
school_name = merged_df["School Name"]
school_type = merged_df["type"]
total_students = merged_df["Student ID"].max()
total_budget = merged_df['budget'].sum()
per_student = total_budget/total_students
avg_ms = merged_df["math_score"].mean()
avg_rs = merged_df["reading_score"].mean()
students_pm = merged_df.loc[(merged_df["math_score"] >70)]
students_pr = merged_df.loc[(merged_df["reading_score"] >70)]
num_pm = students_pm["math_score"].count()
num_pr = students_pr["reading_score"].count()
pct_pm = (num_pm/total_students) * 100
pct_pr = (num_pr/total_students) * 100
ovr_pr = (pct_pm + pct_pr)/2
```


```python
School_Summary = pd.DataFrame({"School Name": [school_name],
                               "School Type": [school_type],
                              "Total Students": [total_students],
                              "Total Budget": [total_budget],
                              "Per Student Budget": [per_student],
                              "Average Math Score": [avg_ms],
                              "Average Reading Score": [avg_rs],
                              "Percent Passing Math": [pct_pm],
                              "Percent Passing Reading": [pct_pr],
                              "Overall Passing Rate": [ovr_pr]})
School_Summary.groupby(by="School Name")
School_Summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
      <th>Percent Passing Math</th>
      <th>Percent Passing Reading</th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Budget</th>
      <th>Total Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>77.683883</td>
      <td>2.117295e+06</td>
      <td>72.393985</td>
      <td>82.97378</td>
      <td>0         Huang High School
1         Huang Hi...</td>
      <td>0        District
1        District
2        D...</td>
      <td>82932329558</td>
      <td>39169</td>
    </tr>
  </tbody>
</table>
</div>


