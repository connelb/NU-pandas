

```python
import pandas as pd
import numpy as np
%config IPCompleter.greedy=True
```

### PyCity Schools Analysis

#### OBSERVED TREND 1
Per the standards applied, throughout the district (comprised of 15 schools and 32715 students), students perform better in reading than in math in terms of passing rate and average score.

#### OBSERVED TREND 2
Charter schools consistently appear in the top 5 schools based on the % Overall Passing Rate.  The district math scores are lower than the Charter schools results.

#### OBSERVED TREND 3
The spending rate does not indicate a better performance. It would appear school sizes of over 2000 students have lower math scores.



```python
# The path to our CSV files
students_csv_path = "students_complete.csv"
schools_csv_path = "schools_complete.csv"

# Read our data into pandas
students_df = pd.read_csv(students_csv_path)
students_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
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
    </tr>
  </tbody>
</table>
</div>




```python
# Read our data into pandas
schools_df = pd.read_csv(schools_csv_path)
schools_df = schools_df.rename(columns={"name":"school"})
schools_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
merged_df = pd.merge(schools_df,students_df, on="school", how="outer")
```

## District Summary


```python
#District Summary

school_series = merged_df["school"].count()
total_schools = merged_df["school"].unique().size
total_students = merged_df["name"].unique().size
average_math_score = merged_df["math_score"].mean()
average_reading_score = merged_df["reading_score"].mean()

number_with_math_score = merged_df["school"].count()
number_with_passing_math_score = merged_df.loc[merged_df["math_score"]>=60].count()["school"]
percent_passing_math = (number_with_passing_math_score/number_with_math_score)*100

number_with_reading_score = merged_df["school"].count()
number_with_passing_reading_score = merged_df.loc[merged_df["reading_score"]>=60].count()["school"]
percent_passing_reading = (number_with_passing_reading_score/number_with_reading_score)*100

#Overall Passing Rate (Average of the above two)- Not super clear
overall_passing_rate = (percent_passing_math + percent_passing_reading)/2

district_summary_df = pd.DataFrame({"Total Schools":[total_schools],
                                    "Total Students":[total_students],
                                   "Average Math Score":[average_math_score],
                                   "Average Reading Score":[average_reading_score],
                                   "%Passing Math":[percent_passing_math],
                                   "%Passing Reading":[percent_passing_reading],
                                   "Overall Passing Rate":[overall_passing_rate]
                                   })

district_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Rate</th>
      <th>Total Schools</th>
      <th>Total Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>92.445749</td>
      <td>100.0</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>96.222875</td>
      <td>15</td>
      <td>32715</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary


```python
#create a group by school
grouped_school = merged_df.groupby(['school'])

school_type = grouped_school['type'].first()
total_students_per_school = grouped_school['name'].count()
#merged_df['total_students_per_school']= grouped_school['name'].count()
total_school_budget = grouped_school['budget'].sum()
school_budget_per_Student = total_school_budget/grouped_school['name'].count()
average_math_score = grouped_school['math_score'].mean()
average_reading_score = grouped_school['reading_score'].mean()

#create new series for passing math
number_with_math_score = grouped_school['math_score'].count()
merged_df['passing_math_score'] = pd.DataFrame(merged_df.loc[merged_df["math_score"]>= 60,'math_score'])
percent_passing_math_per_school = (grouped_school['passing_math_score'].count()/number_with_math_score)*100
# merged_df['percent_passing_math_per_school']= (grouped_school['passing_math_score'].count()/number_with_math_score)*100

#create new series for passing reading
number_with_reading_score = grouped_school['reading_score'].count()
merged_df['passing_reading_score'] = pd.DataFrame(merged_df.loc[merged_df["reading_score"]>= 60,'reading_score'])
percent_passing_reading_per_school = (grouped_school['passing_reading_score'].count()/number_with_math_score)*100
# merged_df['percent_passing_reading_per_school'] = (grouped_school['passing_reading_score'].count()/number_with_math_score)*100

percent_overall_passing_rate = (percent_passing_math_per_school + percent_passing_reading_per_school)/2
# merged_df['percent_overall_passing_rate'] =(percent_passing_math_per_school + percent_passing_reading_per_school)/2

school_summary_df = pd.DataFrame({
    "School Type":school_type,
    "Total Students":total_students_per_school,
    "Total School budget": total_school_budget,
    "Per Student Budget":school_budget_per_Student,
    "Average Math Score":average_math_score,
    "Average Reading Score":average_reading_score,
    "% Passing Math":percent_passing_math_per_school,
    "% Passing Reading":percent_passing_reading_per_school,
    "% Overall Passing Rate":percent_overall_passing_rate
})


school_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Per Student Budget</th>
      <th>School Type</th>
      <th>Total School budget</th>
      <th>Total Students</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>Bailey High School</th>
      <td>94.764871</td>
      <td>89.529743</td>
      <td>100.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>3124928.0</td>
      <td>District</td>
      <td>15549641728</td>
      <td>4976</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1081356.0</td>
      <td>Charter</td>
      <td>2009159448</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>94.218379</td>
      <td>88.436758</td>
      <td>100.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>1884411.0</td>
      <td>District</td>
      <td>5557128039</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>94.651333</td>
      <td>89.302665</td>
      <td>100.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>1763916.0</td>
      <td>District</td>
      <td>4831365924</td>
      <td>2739</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>917500.0</td>
      <td>Charter</td>
      <td>1346890000</td>
      <td>1468</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>94.541532</td>
      <td>89.083064</td>
      <td>100.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>3022020.0</td>
      <td>District</td>
      <td>14007062700</td>
      <td>4635</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>248087.0</td>
      <td>Charter</td>
      <td>105933149</td>
      <td>427</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>94.429208</td>
      <td>88.858416</td>
      <td>100.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>1910635.0</td>
      <td>District</td>
      <td>5573322295</td>
      <td>2917</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>94.591472</td>
      <td>89.182945</td>
      <td>100.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>3094650.0</td>
      <td>District</td>
      <td>14733628650</td>
      <td>4761</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>585858.0</td>
      <td>Charter</td>
      <td>563595396</td>
      <td>962</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>94.273568</td>
      <td>88.547137</td>
      <td>100.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>2547363.0</td>
      <td>District</td>
      <td>10186904637</td>
      <td>3999</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>1056600.0</td>
      <td>Charter</td>
      <td>1860672600</td>
      <td>1761</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>1043130.0</td>
      <td>Charter</td>
      <td>1705517550</td>
      <td>1635</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>1319574.0</td>
      <td>Charter</td>
      <td>3012587442</td>
      <td>2283</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>1049400.0</td>
      <td>Charter</td>
      <td>1888920000</td>
      <td>1800</td>
    </tr>
  </tbody>
</table>
</div>



## Top Performing Schools (By Passing Rate) 


```python
#Top Performing Schools (By Passing Rate) 

school_summary_df.sort_values(by='% Overall Passing Rate', ascending=False).head(5)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Per Student Budget</th>
      <th>School Type</th>
      <th>Total School budget</th>
      <th>Total Students</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>Cabrera High School</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1081356.0</td>
      <td>Charter</td>
      <td>2009159448</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>917500.0</td>
      <td>Charter</td>
      <td>1346890000</td>
      <td>1468</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>248087.0</td>
      <td>Charter</td>
      <td>105933149</td>
      <td>427</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>585858.0</td>
      <td>Charter</td>
      <td>563595396</td>
      <td>962</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>1056600.0</td>
      <td>Charter</td>
      <td>1860672600</td>
      <td>1761</td>
    </tr>
  </tbody>
</table>
</div>



## Bottom Performing Schools (By Passing Rate)


```python
#Bottom Performing Schools (By Passing Rate)
school_summary_df.sort_values(by='% Overall Passing Rate', ascending=True).head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Per Student Budget</th>
      <th>School Type</th>
      <th>Total School budget</th>
      <th>Total Students</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
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
      <th>Figueroa High School</th>
      <td>94.218379</td>
      <td>88.436758</td>
      <td>100.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>1884411.0</td>
      <td>District</td>
      <td>5557128039</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>94.273568</td>
      <td>88.547137</td>
      <td>100.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>2547363.0</td>
      <td>District</td>
      <td>10186904637</td>
      <td>3999</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>94.429208</td>
      <td>88.858416</td>
      <td>100.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>1910635.0</td>
      <td>District</td>
      <td>5573322295</td>
      <td>2917</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>94.541532</td>
      <td>89.083064</td>
      <td>100.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>3022020.0</td>
      <td>District</td>
      <td>14007062700</td>
      <td>4635</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>94.591472</td>
      <td>89.182945</td>
      <td>100.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>3094650.0</td>
      <td>District</td>
      <td>14733628650</td>
      <td>4761</td>
    </tr>
  </tbody>
</table>
</div>



## Math Scores by Grade


```python
#Math Scores by Grade

nineth_grade_grouped = merged_df[merged_df['grade']=='9th'].groupby(['school'])
tenth_grade_grouped = merged_df[merged_df['grade']=='10th'].groupby(['school'])
eleventh_grade_grouped = merged_df[merged_df['grade']=='11th'].groupby(['school'])
twelfth_grade_grouped = merged_df[merged_df['grade']=='12th'].groupby(['school'])

nineth_grade_math_score = nineth_grade_grouped['math_score'].mean()
tenth_grade_math_score = tenth_grade_grouped['math_score'].mean()
eleventh_grade_math_score = eleventh_grade_grouped['math_score'].mean()
twelfth_grade_math_score = twelfth_grade_grouped['math_score'].mean()

school_math_grades_df = pd.DataFrame({
    "9th":nineth_grade_math_score,
    "10th":tenth_grade_math_score,
    "11th":eleventh_grade_math_score,
    "12th":twelfth_grade_math_score,
})

school_math_grades_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>



## Reading Scores by Grade


```python
#Reading Scores by Grade

nineth_grade_reading_score = nineth_grade_grouped['reading_score'].mean()
tenth_grade_reading_score = tenth_grade_grouped['reading_score'].mean()
eleventh_grade_reading_score = eleventh_grade_grouped['reading_score'].mean()
twelfth_grade_reading_score = twelfth_grade_grouped['reading_score'].mean()

school_reading_grades_df = pd.DataFrame({
    "9th":nineth_grade_reading_score,
    "10th":tenth_grade_reading_score,
    "11th":eleventh_grade_reading_score,
    "12th":twelfth_grade_reading_score,
})

school_reading_grades_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
      <td>81.303155</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
      <td>83.676136</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
      <td>81.198598</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
      <td>80.632653</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
      <td>83.369193</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
      <td>80.866860</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
      <td>83.677165</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
      <td>81.290284</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
      <td>81.260714</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
      <td>83.807273</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
      <td>80.993127</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
      <td>84.122642</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
      <td>83.728850</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
      <td>83.939778</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Spending


```python
#Scores by School Spending -4 reasonable bins to group school spending
bins = [0,500000,1000000,2000000,4000000]

# Create the names for the four bins
group_names = ['very Low', 'low', 'medium', 'high']

merged_df['Spending Ranges'] = pd.cut(merged_df["budget"], bins, labels=group_names)

grouped_spending_ranges = merged_df.groupby(['Spending Ranges'])

grouped_spending_ranges_math = grouped_spending_ranges['math_score'].mean()
grouped_spending_ranges_reading = grouped_spending_ranges['reading_score'].mean()

#create new series for passing math
number_with_math_score_spending_grp = grouped_spending_ranges['math_score'].count()
percent_passing_math_per_school_spending_grp = (grouped_spending_ranges['passing_math_score'].count()/number_with_math_score_spending_grp)*100

# create new series for passing reading
number_with_reading_score_spending_grp = grouped_spending_ranges['reading_score'].count()
percent_passing_reading_per_school_spending_grp = (grouped_spending_ranges['passing_reading_score'].count()/number_with_math_score_spending_grp)*100

percent_overall_passing_rate_spending_grp = (percent_passing_math_per_school_spending_grp + percent_passing_reading_per_school_spending_grp)/2


performance_by_spending_ranges_df = pd.DataFrame({
    "Average Math Score":grouped_spending_ranges_math,
    "Average Reading Score":grouped_spending_ranges_reading,
    "% Passing Math":percent_passing_math_per_school_spending_grp,
    "% Passing Reading":percent_passing_reading_per_school_spending_grp,
    "% Overall Passing Rate":percent_overall_passing_rate_spending_grp,
})

performance_by_spending_ranges_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
    </tr>
    <tr>
      <th>Spending Ranges</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>very Low</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
    </tr>
    <tr>
      <th>low</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.544856</td>
      <td>83.906996</td>
    </tr>
    <tr>
      <th>medium</th>
      <td>97.327500</td>
      <td>94.654999</td>
      <td>100.0</td>
      <td>80.213577</td>
      <td>82.529094</td>
    </tr>
    <tr>
      <th>high</th>
      <td>94.556638</td>
      <td>89.113276</td>
      <td>100.0</td>
      <td>77.070764</td>
      <td>80.928365</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Size


```python
#Scores by School Size- school size (Small, Medium, Large).

bins = [0,1000,2000,5000]
group_names = ['Small', 'Medium', 'Large']

grouped_school = merged_df.groupby(['school'])
total_students_per_school = grouped_school['name'].count()

merged_df['School Size'] = pd.cut(merged_df['size'], bins, labels=group_names)

grouped_school_size = merged_df.groupby(['School Size'])

grouped_school_size_math = grouped_school_size['math_score'].mean()
grouped_school_size_reading = grouped_school_size['reading_score'].mean()

#create new series for passing math
number_with_math_score_school_size_grp = grouped_school_size['math_score'].count()
percent_passing_math_per_school_size_grp = (grouped_school_size['passing_math_score'].count()/number_with_math_score_school_size_grp)*100

# create new series for passing reading
number_with_reading_score_school_size_grp = grouped_school_size['reading_score'].count()
percent_passing_reading_per_school_size_grp = (grouped_school_size['passing_reading_score'].count()/number_with_reading_score_school_size_grp)*100

percent_overall_passing_rate_school_size_grp = (percent_passing_math_per_school_size_grp + percent_passing_reading_per_school_size_grp)/2

school_size_df = pd.DataFrame({
    "Average Math Score":grouped_school_size_math,
    "Average Reading Score":grouped_school_size_reading,
    "% Passing Math":percent_passing_math_per_school_size_grp,
    "% Passing Reading":percent_passing_reading_per_school_size_grp,
    "% Overall Passing Rate":percent_overall_passing_rate_school_size_grp,
})

school_size_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.828654</td>
      <td>83.974082</td>
    </tr>
    <tr>
      <th>Medium</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.372682</td>
      <td>83.867989</td>
    </tr>
    <tr>
      <th>Large</th>
      <td>94.943436</td>
      <td>89.886872</td>
      <td>100.0</td>
      <td>77.477597</td>
      <td>81.198674</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Type- Charter vs. District


```python
#Scores by School Type- Charter vs. District)
grouped_type = merged_df.groupby("type")

grouped_type_math = grouped_type['math_score'].mean()
grouped_type_reading = grouped_type['reading_score'].mean()

#create new series for passing math
number_with_math_score_grouped_type = grouped_type['math_score'].count()
percent_passing_math_grouped_type = (grouped_type['passing_math_score'].count()/number_with_math_score_grouped_type)*100

# create new series for passing reading
number_with_reading_score_grouped_type = grouped_type['reading_score'].count()
percent_passing_reading_grouped_type = (grouped_type['passing_reading_score'].count()/number_with_reading_score_grouped_type)*100

percent_overall_passing_rate_grouped_type = (percent_passing_math_grouped_type + percent_passing_reading_grouped_type)/2

school_type_df = pd.DataFrame({
    "Average Math Score":grouped_type_math,
    "Average Reading Score":grouped_type_reading,
    "% Passing Math":percent_passing_math_grouped_type,
    "% Passing Reading":percent_passing_reading_grouped_type,
    "% Overall Passing Rate":percent_overall_passing_rate_grouped_type,
})

school_type_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Overall Passing Rate</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>83.406183</td>
      <td>83.902821</td>
    </tr>
    <tr>
      <th>District</th>
      <td>94.515495</td>
      <td>89.030991</td>
      <td>100.0</td>
      <td>76.987026</td>
      <td>80.962485</td>
    </tr>
  </tbody>
</table>
</div>


