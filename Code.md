
### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas Data Frames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
```

## District Summary

* Calculate the total number of schools

* Calculate the total number of students

* Calculate the total budget

* Calculate the average math score 

* Calculate the average reading score

* Calculate the overall passing rate (overall average score), i.e. (avg. math score + avg. reading score)/2

* Calculate the percentage of students with a passing math score (70 or greater)

* Calculate the percentage of students with a passing reading score (70 or greater)

* Create a dataframe to hold the above results

* Optional: give the displayed data cleaner formatting


```python
school_data_complete.head(5)
Math_over70=school_data_complete.loc[(school_data_complete["math_score"]>=70)]
Reading_over70=school_data_complete.loc[(school_data_complete["reading_score"]>=70)]
Passing_Rate.head()
Reading_over70.head()
                                                                                
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
      <th>student_name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school_name</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>0</td>
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
      <td>0</td>
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
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Bryan Miranda</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>94</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Sheena Carter</td>
      <td>F</td>
      <td>11th</td>
      <td>Huang High School</td>
      <td>82</td>
      <td>80</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
  </tbody>
</table>
</div>




```python
Total_School=school_data_complete["School ID"].nunique()
Total_Student=school_data_complete["Student ID"].nunique()
Total_Budget=school_data_complete["budget"].sum()
Average_math_score=school_data_complete["math_score"].mean()
Average_reading_score=school_data_complete["reading_score"].mean()
Overall_Passing_Rate=(Average_math_score+Average_reading_score)/2
Percentage_Math_Passing=Math_over70["Student ID"].nunique()/Total_Student
Percentage_Reading_Passing=Reading_over70["Student ID"].nunique()/Total_Student

Summary_Table_df=pd.DataFrame({"School Count":Total_School,"Student Count":Total_Student,"Total Budget":Total_Budget,"Average Math Score":Average_math_score,"Average Reading Score":Average_reading_score,"Overall Pass Score":Overall_Passing_Rate,"% of Math Passing":Percentage_Math_Passing,"% of Reading Passing":Percentage_Reading_Passing},index=[0])
Summary_Table_df

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
      <th>School Count</th>
      <th>Student Count</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Pass Score</th>
      <th>% of Math Passing</th>
      <th>% of Reading Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>82932329558</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>80.431606</td>
      <td>0.749809</td>
      <td>0.858055</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary

* Create an overview table that summarizes key metrics about each school, including:
  * School Name
  * School Type
  * Total Students
  * Total School Budget
  * Per Student Budget
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)
  
* Create a dataframe to hold the above results


```python
School_Table=school_data_complete.groupby(['school_name','type'])
School_Table.head()
School_Math_over70=School_Table["math_score"].apply(lambda x: (x>=70).sum())
School_Reading_over70=School_Table["reading_score"].apply(lambda x: (x>=70).sum())
School_Reading_over70
```




    school_name            type    
    Bailey High School     District    4077
    Cabrera High School    Charter     1803
    Figueroa High School   District    2381
    Ford High School       District    2172
    Griffin High School    Charter     1426
    Hernandez High School  District    3748
    Holden High School     Charter      411
    Huang High School      District    2372
    Johnson High School    District    3867
    Pena High School       Charter      923
    Rodriguez High School  District    3208
    Shelton High School    Charter     1688
    Thomas High School     Charter     1591
    Wilson High School     Charter     2204
    Wright High School     Charter     1739
    Name: reading_score, dtype: int64




```python
School_Total_Student=School_Table["student_name"].nunique()
School_Budget=School_Table["budget"].mean()
Per_Student_Budget=School_Budget/School_Total_Student
School_Average_math_score=School_Table["math_score"].mean()
School_Average_reading_score=School_Table["reading_score"].mean()
School_Percentage_of_Math_Pass=School_Math_over70/School_Total_Student
School_Percentage_of_Reading_Pass=School_Reading_over70/School_Total_Student
Overall_Passing_Rate=(School_Percentage_of_Math_Pass+School_Percentage_of_Reading_Pass)/2

School_Summary_Table=pd.DataFrame({"Total Student":School_Total_Student,
                                  "Total Budget":School_Budget,
                                  "Per Student Buedget":Per_Student_Budget,
                                  "Average Math Score":School_Average_math_score,
                                  "Average Reading Score":School_Average_reading_score,
                                 "Percentage of Math Passing":School_Percentage_of_Math_Pass,
                                   "Percentage of Reading Passing":School_Percentage_of_Reading_Pass,
                                  "Overall Passing Rate":Overall_Passing_Rate})
School_Summary_Table

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
      <th></th>
      <th>Total Student</th>
      <th>Total Budget</th>
      <th>Per Student Buedget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage of Math Passing</th>
      <th>Percentage of Reading Passing</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th>type</th>
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
      <th>District</th>
      <td>4799</td>
      <td>3124928</td>
      <td>651.162325</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>0.691394</td>
      <td>0.849552</td>
      <td>0.770473</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <th>Charter</th>
      <td>1834</td>
      <td>1081356</td>
      <td>589.616140</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>0.953653</td>
      <td>0.983097</td>
      <td>0.968375</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <th>District</th>
      <td>2886</td>
      <td>1884411</td>
      <td>652.949064</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>0.674290</td>
      <td>0.825017</td>
      <td>0.749653</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <th>District</th>
      <td>2676</td>
      <td>1763916</td>
      <td>659.161435</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>0.699178</td>
      <td>0.811659</td>
      <td>0.755419</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <th>Charter</th>
      <td>1449</td>
      <td>917500</td>
      <td>633.195307</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>0.946170</td>
      <td>0.984127</td>
      <td>0.965148</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <th>District</th>
      <td>4501</td>
      <td>3022020</td>
      <td>671.410798</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>0.687403</td>
      <td>0.832704</td>
      <td>0.760053</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <th>Charter</th>
      <td>424</td>
      <td>248087</td>
      <td>585.110849</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>0.931604</td>
      <td>0.969340</td>
      <td>0.950472</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <th>District</th>
      <td>2858</td>
      <td>1910635</td>
      <td>668.521693</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>0.670399</td>
      <td>0.829951</td>
      <td>0.750175</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <th>District</th>
      <td>4618</td>
      <td>3094650</td>
      <td>670.127761</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>0.681031</td>
      <td>0.837375</td>
      <td>0.759203</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <th>Charter</th>
      <td>955</td>
      <td>585858</td>
      <td>613.463874</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>0.952880</td>
      <td>0.966492</td>
      <td>0.959686</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <th>District</th>
      <td>3902</td>
      <td>2547363</td>
      <td>652.835213</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>0.680164</td>
      <td>0.822142</td>
      <td>0.751153</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <th>Charter</th>
      <td>1738</td>
      <td>1056600</td>
      <td>607.940161</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>0.951093</td>
      <td>0.971231</td>
      <td>0.961162</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <th>Charter</th>
      <td>1619</td>
      <td>1043130</td>
      <td>644.305127</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>0.941939</td>
      <td>0.982705</td>
      <td>0.962322</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <th>Charter</th>
      <td>2248</td>
      <td>1319574</td>
      <td>586.999110</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>0.953292</td>
      <td>0.980427</td>
      <td>0.966859</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <th>Charter</th>
      <td>1783</td>
      <td>1049400</td>
      <td>588.558609</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>0.942232</td>
      <td>0.975322</td>
      <td>0.958777</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```

## Top Performing Schools (By Passing Rate)

* Sort and display the top five schools in overall passing rate


```python
Top_Performing_Schools = School_Summary_Table.sort_values(["Overall Passing Rate"], ascending=False)
Top_Performing_Schools.head(5)
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
      <th></th>
      <th>Total Student</th>
      <th>Total Budget</th>
      <th>Per Student Buedget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage of Math Passing</th>
      <th>Percentage of Reading Passing</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th>type</th>
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
      <th>Charter</th>
      <td>1834</td>
      <td>1081356</td>
      <td>589.616140</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>0.953653</td>
      <td>0.983097</td>
      <td>0.968375</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <th>Charter</th>
      <td>2248</td>
      <td>1319574</td>
      <td>586.999110</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>0.953292</td>
      <td>0.980427</td>
      <td>0.966859</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <th>Charter</th>
      <td>1449</td>
      <td>917500</td>
      <td>633.195307</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>0.946170</td>
      <td>0.984127</td>
      <td>0.965148</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <th>Charter</th>
      <td>1619</td>
      <td>1043130</td>
      <td>644.305127</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>0.941939</td>
      <td>0.982705</td>
      <td>0.962322</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <th>Charter</th>
      <td>1738</td>
      <td>1056600</td>
      <td>607.940161</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>0.951093</td>
      <td>0.971231</td>
      <td>0.961162</td>
    </tr>
  </tbody>
</table>
</div>



## Bottom Performing Schools (By Passing Rate)

* Sort and display the five worst-performing schools


```python
Bottom_Performing_Schools = School_Summary_Table.sort_values(["Overall Passing Rate"], ascending=True)
Bottom_Performing_Schools.head(5)
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
      <th></th>
      <th>Total Student</th>
      <th>Total Budget</th>
      <th>Per Student Buedget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage of Math Passing</th>
      <th>Percentage of Reading Passing</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school_name</th>
      <th>type</th>
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
      <th>District</th>
      <td>2886</td>
      <td>1884411</td>
      <td>652.949064</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>0.674290</td>
      <td>0.825017</td>
      <td>0.749653</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <th>District</th>
      <td>2858</td>
      <td>1910635</td>
      <td>668.521693</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>0.670399</td>
      <td>0.829951</td>
      <td>0.750175</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <th>District</th>
      <td>3902</td>
      <td>2547363</td>
      <td>652.835213</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>0.680164</td>
      <td>0.822142</td>
      <td>0.751153</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <th>District</th>
      <td>2676</td>
      <td>1763916</td>
      <td>659.161435</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>0.699178</td>
      <td>0.811659</td>
      <td>0.755419</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <th>District</th>
      <td>4618</td>
      <td>3094650</td>
      <td>670.127761</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>0.681031</td>
      <td>0.837375</td>
      <td>0.759203</td>
    </tr>
  </tbody>
</table>
</div>



## Math Scores by Grade

* Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

  * Create a pandas series for each grade. Hint: use a conditional statement.
  
  * Group each series by school
  
  * Combine the series into a dataframe
  
  * Optional: give the displayed data cleaner formatting


```python
Math_Score_By_Grade=pd.pivot_table(school_data_complete,["math_score"],["school_name"],["grade"])
Math_Score_By_Grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">math_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school_name</th>
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



## Reading Score by Grade 

* Perform the same operations as above for reading scores


```python
Reading_Score_By_Grade=pd.pivot_table(school_data_complete,["reading_score"],["school_name"],["grade"])
Reading_Score_By_Grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">reading_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school_name</th>
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

* Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)


```python
# Sample bins. Feel free to create your own bins.
spending_bins = [0, 585, 615, 645, 675]
group_names = ["<$585", "$585-615", "$615-645", "$645-675"]
```


```python
School_Summary_Table["Spending Range"]=pd.cut(School_Summary_Table["Per Student Buedget"],spending_bins,labels=group_names)
Grouped_by_SpendingRange=School_Summary_Table.groupby(["Spending Range"])
SpendingRange_Average_math_score=Grouped_by_SpendingRange["Average Math Score"].mean()
SpendingRange_Average_reading_score=Grouped_by_SpendingRange["Average Reading Score"].mean()
SpendingRange_Percentage_of_Math_Pass=Grouped_by_SpendingRange["Percentage of Math Passing"].mean()
SpendingRange_Percentage_of_Reading_Pass=Grouped_by_SpendingRange["Percentage of Reading Passing"].mean()
SpendingRange_Overall_Passing_Rate=(SpendingRange_Percentage_of_Math_Pass+SpendingRange_Percentage_of_Reading_Pass)/2
SpendingRange_Summary_Table=pd.DataFrame({                              
                                  "Average Math Score":SpendingRange_Average_math_score,
                                  "Average Reading Score":SpendingRange_Average_reading_score,
                                 "Percentage of Math Passing":SpendingRange_Percentage_of_Math_Pass,
                                   "Percentage of Reading Passing":SpendingRange_Percentage_of_Reading_Pass,
                                  "Overall Passing Rate":SpendingRange_Overall_Passing_Rate})
SpendingRange_Summary_Table

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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage of Math Passing</th>
      <th>Percentage of Reading Passing</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$585</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>83.503495</td>
      <td>83.917613</td>
      <td>0.947459</td>
      <td>0.974318</td>
      <td>0.960889</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>83.384924</td>
      <td>83.832844</td>
      <td>0.944055</td>
      <td>0.983416</td>
      <td>0.963735</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>0.683408</td>
      <td>0.829772</td>
      <td>0.756590</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Size

* Perform the same operations as above, based on school size.


```python
# Sample bins. Feel free to create your own bins.
size_bins = [0, 1000, 2000, 5000]
group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
```


```python
school_data_complete["School Size"]=pd.cut(school_data_complete["size"],size_bins,labels=group_names)
Grouped_by_size=school_data_complete.groupby(["School Size"])
Grouped_by_size.head()
Size_Math_over70=Grouped_by_size["math_score"].apply(lambda x: (x>=70).sum())
Size_Reading_over70=Grouped_by_size["reading_score"].apply(lambda x: (x>=70).sum())
Size_Average_math_score=Grouped_by_size["math_score"].mean()
Size_Average_Reading_score=Grouped_by_size["reading_score"].mean()
Size_Percentage_of_Math_Pass=Size_Math_over70/Grouped_by_size["student_name"].nunique()
Size_Percentage_of_Reading_Pass=Size_Reading_over70/Grouped_by_size["student_name"].nunique()
Size_Overall_Passing_Rate=(Size_Percentage_of_Math_Pass+Size_Percentage_of_Reading_Pass)/2

Size_Summary_Table=pd.DataFrame({
                                  "Average Math Score":Size_Average_math_score,
                                  "Average Reading Score":Size_Average_Reading_score,
                                 "Percentage of Math Passing":Size_Percentage_of_Math_Pass,
                                   "Percentage of Reading Passing":Size_Percentage_of_Reading_Pass,                                   "Percentage of Reading Passing":Size_Percentage_of_Reading_Pass,
                                  "Overall Passing Rate":Size_Overall_Passing_Rate})
Size_Summary_Table



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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage of Math Passing</th>
      <th>Percentage of Reading Passing</th>
      <th>Overall Passing Rate</th>
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
      <th>Small (&lt;1000)</th>
      <td>83.828654</td>
      <td>83.974082</td>
      <td>0.950473</td>
      <td>0.971595</td>
      <td>0.961034</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>83.372682</td>
      <td>83.867989</td>
      <td>0.987132</td>
      <td>1.020416</td>
      <td>1.003774</td>
    </tr>
    <tr>
      <th>Large (2000-5000)</th>
      <td>77.477597</td>
      <td>81.198674</td>
      <td>0.792793</td>
      <td>0.948376</td>
      <td>0.870585</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Type

* Perform the same operations as above, based on school type.


```python
Grouped_by_type=school_data_complete.groupby(["type"])
Grouped_by_type.head()
Type_Math_over70=Grouped_by_type["math_score"].apply(lambda x: (x>=70).sum())
Type_Reading_over70=Grouped_by_type["reading_score"].apply(lambda x: (x>=70).sum())
Type_Average_math_score=Grouped_by_type["math_score"].mean()
Type_Average_Reading_score=Grouped_by_type["reading_score"].mean()
Type_Percentage_of_Math_Pass=Type_Math_over70/Grouped_by_type["student_name"].nunique()
Type_Percentage_of_Reading_Pass=Type_Reading_over70/Grouped_by_type["student_name"].nunique()
Type_Overall_Passing_Rate=(Type_Percentage_of_Math_Pass+Type_Percentage_of_Reading_Pass)/2

Type_Summary_Table=pd.DataFrame({
                                  "Average Math Score":Type_Average_math_score,
                                  "Average Reading Score":Type_Average_Reading_score,
                                 "Percentage of Math Passing":Type_Percentage_of_Math_Pass,
                                   "Percentage of Reading Passing":Type_Percentage_of_Reading_Pass,                                   "Percentage of Reading Passing":Size_Percentage_of_Reading_Pass,
                                  "Overall Passing Rate":Type_Overall_Passing_Rate})
Type_Summary_Table

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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Percentage of Math Passing</th>
      <th>Percentage of Reading Passing</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.406183</td>
      <td>83.902821</td>
      <td>1.007584</td>
      <td>NaN</td>
      <td>1.023413</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.987026</td>
      <td>80.962485</td>
      <td>0.761695</td>
      <td>NaN</td>
      <td>0.844066</td>
    </tr>
    <tr>
      <th>Large (2000-5000)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.948376</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.020416</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Small (&lt;1000)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.971595</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
