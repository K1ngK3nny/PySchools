

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
merged_df = pd.merge(students_df, schools_df, on='School Name')
#combined_df = merged_df.drop(["School ID"], axis=1)
combined_df.head()


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
      <th>Student ID</th>
      <th>Student Name</th>
      <th>gender</th>
      <th>grade</th>
      <th>School Name</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
  </tbody>
</table>
</div>




```python
avg_math_score = combined_df["math_score"].mean()
total_students = combined_df["Student ID"].max() + 1
total_budget = combined_df["budget"].sum()
total_schools = len(combined_df["School Name"].unique())
students_pm = combined_df.loc[(combined_df["math_score"] >70)]
students_pr = combined_df.loc[(combined_df["reading_score"] > 70)]
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
      <th>15</th>
      <td>78.985371</td>
      <td>77.681899</td>
      <td>72.392137</td>
      <td>82.971662</td>
      <td>82932329558</td>
      <td>39170</td>
    </tr>
  </tbody>
</table>
</div>




```python
total_students = combined_df['Student ID'].max() + 1
school_type = combined_df.set_index(["School Name"])['type']
#school_type = combined_df.loc[:, ['type']]
#total_students = combined_df['Student ID'].max()
total_students_perSchool = combined_df["School Name"].value_counts()
total_budget = combined_df.groupby(['School Name']).sum()['budget']
per_student = (total_budget.sum())/total_students
avg_ms = combined_df["math_score"].mean()
avg_rs = combined_df["reading_score"].mean()
students_pm = combined_df.loc[(student_df["math_score"] >70)]
students_pr = combined_df.loc[(student_df["reading_score"] >70)]
num_pm = students_pm["math_score"].count()
num_pr = students_pr["reading_score"].count()
pct_pm = (num_pm/total_students) * 100
pct_pr = (num_pr/total_students) * 100
ovr_pr = (pct_pm + pct_pr)/2 * 100




```


```python
School_Summary = pd.DataFrame({"School Type": school_type,
                              "Total Students": total_students,
                              "Total Budget": total_budget,
                              "Per Student Budget": per_student,
                              "Average Math Score": avg_ms,
                              "Average Reading Score": avg_rs,
                              "Percent Passing Math": pct_pm,
                              "Percent Passing Reading": pct_pr,
                              "Overall Passing Rate": ovr_pr})

School_Summary = School_Summary[['School Name', 'School Type', 'Total Students', 'Total Budget', 'Per Student Budget', 'Average Math Score', 'Average Reading Score', 'Percent Passing Math', 'Percent Passing Reading', 'Overall Passing Rate']] 
School_Summary["Total School Budget"] = School_Summary["Total Students"].map("${:,.2f}".format)
School_Summary["Per Student Budget"] = School_Summary["Per Student Budget"].map("${:,.2f}".format)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-175-81b832ca8757> in <module>()
          7                               "Percent Passing Math": pct_pm,
          8                               "Percent Passing Reading": pct_pr,
    ----> 9                               "Overall Passing Rate": ovr_pr})
         10 
         11 School_Summary = School_Summary[['School Name', 'School Type', 'Total Students', 'Total Budget', 'Per Student Budget', 'Average Math Score', 'Average Reading Score', 'Percent Passing Math', 'Percent Passing Reading', 'Overall Passing Rate']]
    

    ~\Anaconda3\lib\site-packages\pandas\core\frame.py in __init__(self, data, index, columns, dtype, copy)
        273                                  dtype=dtype, copy=copy)
        274         elif isinstance(data, dict):
    --> 275             mgr = self._init_dict(data, index, columns, dtype=dtype)
        276         elif isinstance(data, ma.MaskedArray):
        277             import numpy.ma.mrecords as mrecords
    

    ~\Anaconda3\lib\site-packages\pandas\core\frame.py in _init_dict(self, data, index, columns, dtype)
        409             arrays = [data[k] for k in keys]
        410 
    --> 411         return _arrays_to_mgr(arrays, data_names, index, columns, dtype=dtype)
        412 
        413     def _init_ndarray(self, values, index, columns, dtype=None, copy=False):
    

    ~\Anaconda3\lib\site-packages\pandas\core\frame.py in _arrays_to_mgr(arrays, arr_names, index, columns, dtype)
       5499 
       5500     # don't force copy because getting jammed in an ndarray anyway
    -> 5501     arrays = _homogenize(arrays, index, dtype)
       5502 
       5503     # from BlockManager perspective
    

    ~\Anaconda3\lib\site-packages\pandas\core\frame.py in _homogenize(data, index, dtype)
       5798                 # Forces alignment. No need to copy data since we
       5799                 # are putting it into an ndarray later
    -> 5800                 v = v.reindex(index, copy=False)
       5801         else:
       5802             if isinstance(v, dict):
    

    ~\Anaconda3\lib\site-packages\pandas\core\series.py in reindex(self, index, **kwargs)
       2424     @Appender(generic._shared_docs['reindex'] % _shared_doc_kwargs)
       2425     def reindex(self, index=None, **kwargs):
    -> 2426         return super(Series, self).reindex(index=index, **kwargs)
       2427 
       2428     @Appender(generic._shared_docs['fillna'] % _shared_doc_kwargs)
    

    ~\Anaconda3\lib\site-packages\pandas\core\generic.py in reindex(self, *args, **kwargs)
       2513         # perform the reindex on the axes
       2514         return self._reindex_axes(axes, level, limit, tolerance, method,
    -> 2515                                   fill_value, copy).__finalize__(self)
       2516 
       2517     def _reindex_axes(self, axes, level, limit, tolerance, method, fill_value,
    

    ~\Anaconda3\lib\site-packages\pandas\core\generic.py in _reindex_axes(self, axes, level, limit, tolerance, method, fill_value, copy)
       2531             obj = obj._reindex_with_indexers({axis: [new_index, indexer]},
       2532                                              fill_value=fill_value,
    -> 2533                                              copy=copy, allow_dups=False)
       2534 
       2535         return obj
    

    ~\Anaconda3\lib\site-packages\pandas\core\generic.py in _reindex_with_indexers(self, reindexers, fill_value, copy, allow_dups)
       2625                                                 fill_value=fill_value,
       2626                                                 allow_dups=allow_dups,
    -> 2627                                                 copy=copy)
       2628 
       2629         if copy and new_data is self._data:
    

    ~\Anaconda3\lib\site-packages\pandas\core\internals.py in reindex_indexer(self, new_axis, indexer, axis, fill_value, allow_dups, copy)
       3884         # some axes don't allow reindexing with dups
       3885         if not allow_dups:
    -> 3886             self.axes[axis]._can_reindex(indexer)
       3887 
       3888         if axis >= self.ndim:
    

    ~\Anaconda3\lib\site-packages\pandas\core\indexes\base.py in _can_reindex(self, indexer)
       2834         # trying to reindex on an axis with duplicates
       2835         if not self.is_unique and len(indexer):
    -> 2836             raise ValueError("cannot reindex from a duplicate axis")
       2837 
       2838     def reindex(self, target, method=None, level=None, limit=None,
    

    ValueError: cannot reindex from a duplicate axis



```python
School_Summary.head()
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percent Passing Math</th>
      <th>Percent Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>(t, y, p, e)</td>
      <td>1</td>
      <td>3124928</td>
      <td>3124928.0</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>2835600.0</td>
      <td>3250000.0</td>
      <td>3042800.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>(t, y, p, e)</td>
      <td>1</td>
      <td>1081356</td>
      <td>1081356.0</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>2835600.0</td>
      <td>3250000.0</td>
      <td>3042800.0</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>(t, y, p, e)</td>
      <td>1</td>
      <td>1884411</td>
      <td>1884411.0</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>2835600.0</td>
      <td>3250000.0</td>
      <td>3042800.0</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>(t, y, p, e)</td>
      <td>1</td>
      <td>1763916</td>
      <td>1763916.0</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>2835600.0</td>
      <td>3250000.0</td>
      <td>3042800.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>(t, y, p, e)</td>
      <td>1</td>
      <td>917500</td>
      <td>917500.0</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>2835600.0</td>
      <td>3250000.0</td>
      <td>3042800.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
students_df = student_df.rename(columns = {'school': 'School Name'})
```


```python
merged_df = pd.merge(students_df, schools_df, on='School Name')
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
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>School Name</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
  </tbody>
</table>
</div>




```python
#school_name = merged_df.set_index(["School Name"])['type']
Avg_MathScore_by_School = merged_df.groupby('School Name').mean()['math_score']
#avg_ms_by_grade = merged_df.groupby('School Name').head()
#merged_df["Average Math Score"].math_score.mean().head()
Avg_MathScore_by_Grade = merged_df.groupby('grade').mean()['math_score']

Avg_Math_Score_df = pd.DataFrame({"School Name": [school_name],
                                 "Grade": [Avg_MathScore_by_Grade],
                                 "Average Math Score": [Avg_MathScore_by_School]})
Avg_Math_Score_df = Avg_Math_Score_df[["School Name", "Grade", "Average Math Score"]]
Avg_Math_Score_df.head()

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
      <th>Grade</th>
      <th>Average Math Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>School Name
Huang High School     District
Hua...</td>
      <td>grade
10th    78.941483
11th    79.083548
12th...</td>
      <td>School Name
Bailey High School       77.048432...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#reduced_df = avg_ms_by_grade.loc[:,['grade','School Name']]
#reduced_df.drop_duplicates('School Name')
```


```python
ninth = combined_df[(combined_df['grade']=='9th')]
tenth = combined_df[(combined_df['grade']=='10th')]
eleventh = combined_df[(combined_df['grade']=='11th')]
twelfth = combined_df[(combined_df['grade']=='12th')]

ninth_avg = ninth.groupby(['School Name']).mean()['reading_score']
tenth_avg = tenth.groupby(['School Name']).mean()['reading_score']
eleventh_avg = eleventh.groupby(['School Name']).mean()['reading_score']
twelfth_avg = twelfth.groupby(['School Name']).mean()['reading_score']

avg_scores = pd.DataFrame({'9th': ninth_avg,
                          '10th': tenth_avg,
                          '11th': eleventh_avg,
                          '12th': twelfth_avg})
avg_scores = avg_scores({"9th", "10th", "11th", "12th"})
scores_by_grade.index.name = None
avg_scores.head()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-181-4c9a3ec9f332> in <module>()
         13                           '11th': eleventh_avg,
         14                           '12th': twelfth_avg})
    ---> 15 avg_scores = avg_scores({"9th", "10th", "11th", "12th"})
         16 scores_by_grade.index.name = None
         17 avg_scores.head()
    

    TypeError: 'DataFrame' object is not callable

