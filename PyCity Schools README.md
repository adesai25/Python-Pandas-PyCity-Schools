
## Option 2: Academy of Py

Well done! Having spent years analyzing financial records for big banks, you've finally scratched your idealistic itch and joined the education sector. In your latest role, you've become the Chief Data Scientist for your city's school district. In this capacity, you'll be helping the  school board and mayor make strategic decisions regarding future school budgets and priorities.

As a first task, you've been asked to analyze the district-wide standardized test results. You'll be given access to every student's math and reading scores, as well as various information on the schools they attend. Your responsibility is to aggregate the data to and showcase obvious trends in school performance. 

Your final report should include each of the following:

**District Summary**

* Create a high level snapshot (in table form) of the district's key metrics, including:
  * Total Schools
  * Total Students
  * Total Budget
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)

**School Summary**

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

**Top Performing Schools (By Passing Rate)**

* Create a table that highlights the top 5 performing schools based on Overall Passing Rate. Include:
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

**Top Performing Schools (By Passing Rate)**

* Create a table that highlights the bottom 5 performing schools based on Overall Passing Rate. Include all of the same metrics as above.

**Math Scores by Grade**

* Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

**Reading Scores by Grade**

* Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

**Scores by School Spending**

* Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)

**Scores by School Size**

* Repeat the above breakdown, but this time group schools based on a reasonable approximation of school size (Small, Medium, Large).

**Scores by School Type**

* Repeat the above breakdown, but this time group schools based on school type (Charter vs. District).

As final considerations:

* Your script must work for both data-sets given.
* You must use the Pandas Library and the Jupyter Notebook.
* You must submit a link to your Jupyter Notebook with the viewable Data Frames. 
* You must include an exported markdown version of your Notebook called  `README.md` in your GitHub repository.  
* You must include a written description of three observable trends based on the data. 
* See [Example Solution](PyCitySchools/PyCitySchools_Example.pdf) for a reference on the expected format. 


```python
#Load dependencies
import pandas as pd

#show what's in the file you're in
!ls
```

    Py City Schools.ipynb     PyCitySchools_Example.pdf [34mraw_data[m[m



```python
#Load in first(school) file
schools= "raw_data/schools_complete.csv"

#Load in second(students) file
students="raw_data/students_complete.csv"
```


```python
#read in school file
school_pd = pd.read_csv(schools)
school_pd.head()


#Change the 'name' column of schools_df to 'school'.
# This will make merging and accessing the column more intuitive later,
# and keeps the dataset consistent throughout the entire analysis.
school_pd = school_pd.rename(columns={"name":"school"})


school_pd.head()
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
#Read in students file
students_pd = pd.read_csv(students)
students_pd.head()
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



# District Summary


```python

#District Summary

#Total Number of Schools###########################
total_schools = school_pd['school'].count()

print('Total Number of School:',total_schools)


#Total Number of Students###########################
total_students = students_pd['Student ID'].count()

print('Total Number of Students:',total_students)


#Total Budget#######################################
total_budget = school_pd['budget'].sum()

print('Total Budget:',total_budget)


#Average Math Score##################################
ave_math = students_pd['math_score'].mean()

print('Average Math Score:',ave_math)


#Average Reading Score###############################
ave_read = students_pd['reading_score'].mean()

print('Average Reading Score:',ave_read )
```

    Total Number of School: 15
    Total Number of Students: 39170
    Total Budget: 24649428
    Average Math Score: 78.98537145774827
    Average Reading Score: 81.87784018381414



```python
#Passing Math########################################
pass_math = students_pd.loc[students_pd['math_score'] >= 70, ['math_score']]

pass_math.head()



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
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>79</td>
    </tr>
    <tr>
      <th>4</th>
      <td>84</td>
    </tr>
    <tr>
      <th>5</th>
      <td>94</td>
    </tr>
    <tr>
      <th>6</th>
      <td>80</td>
    </tr>
    <tr>
      <th>8</th>
      <td>87</td>
    </tr>
  </tbody>
</table>
</div>




```python
pass_math_count= pass_math['math_score'].count()
pass_math_count

print(pass_math_count)

percent_pass_math = pass_math_count/total_students
percent_pass_math
```

    29370





    0.749808526933878




```python
#Passing Read########################################
pass_read = students_pd.loc[students_pd['reading_score'] >= 70, ['reading_score']]

pass_read.head(5)
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
      <th>reading_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>94</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90</td>
    </tr>
    <tr>
      <th>4</th>
      <td>97</td>
    </tr>
    <tr>
      <th>5</th>
      <td>94</td>
    </tr>
    <tr>
      <th>6</th>
      <td>82</td>
    </tr>
  </tbody>
</table>
</div>




```python
pass_read_count= pass_read['reading_score'].count()
pass_read_count

print(pass_read_count)

percent_pass_read = pass_read_count/total_students
percent_pass_read
```

    33610





    0.8580546336482001




```python
#Overall Passing Rate (Average of the above two)

overall_passing = (percent_pass_read + percent_pass_math)/2

overall_passing
```




    0.8039315802910391



# District Summary Answer


```python
# Create a DataFrame of frames using a dictionary of lists
district_summary = pd.DataFrame({
    "Total Schools": [total_schools],
    "Total Students": [total_students],
    "Total Budget":[total_budget],
    "Average Math Score": [ave_math],
    "Average Reading Score": [ave_read], 
    "% Passing Math":[percent_pass_math],
    "% Passing Reading":[percent_pass_read],
    "Overall Passing Rate":[overall_passing],
})


district_summary = district_summary[["Total Schools",
                                     "Total Students",
                                     "Total Budget",
                                     "Average Math Score",
                                     "Average Reading Score", 
                                     "% Passing Math",
                                     "% Passing Reading",
                                     "Overall Passing Rate"]]
district_summary


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
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24649428</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>0.749809</td>
      <td>0.858055</td>
      <td>0.803932</td>
    </tr>
  </tbody>
</table>
</div>



# School Summary


```python
#School Summary

# School Name
school_name = school_pd['school']
school_name

# School Type
school_type = school_pd['type']
school_type

# Total Students
total_students_school = school_pd['size']

print("Total Students per School")
print(total_students_school)

#Total Students per School

total_students_per_school = students_pd['school'].value_counts()
#print(total_students_per_school)
# Total School Budget

#total school budget is total_budget calculated previously see above


# Per Student Budget

per_student_budget = school_pd['budget']/total_students_school
print("Per Student Budget")
print(per_student_budget)


#print(total_school_budget)


```

    Total Students per School
    0     2917
    1     2949
    2     1761
    3     4635
    4     1468
    5     2283
    6     1858
    7     4976
    8      427
    9      962
    10    1800
    11    3999
    12    4761
    13    2739
    14    1635
    Name: size, dtype: int64
    Per Student Budget
    0     655.0
    1     639.0
    2     600.0
    3     652.0
    4     625.0
    5     578.0
    6     582.0
    7     628.0
    8     581.0
    9     609.0
    10    583.0
    11    637.0
    12    650.0
    13    644.0
    14    638.0
    dtype: float64



```python
# Average Math Score
grouped_school_name = students_pd.groupby(['school'])



#print grouped_school_name
grouped_school_name.count().head(10)


#Average math score 
aver_math_by_school = grouped_school_name['math_score'].mean()

#print(aver_math_by_school)

# Average Reading Score
aver_read_by_school = grouped_school_name['reading_score'].mean()

#print(aver_read_by_school)


# build dataframe with calculated average score. reset the index so school can be used for merging later
df_of_averages = pd.DataFrame({"Average Math Score": aver_math_by_school,
                              "Average Reading Score": aver_read_by_school,}).reset_index()
df_of_averages


# % Passing Math
students_math_passing = students_pd.loc[students_pd['math_score'] > 70]
#print(students_math_passing)

all_students_math_passing= pd.DataFrame({"school": students_math_passing["school"],
                                       "Count_students_passing_math": 0})

students_pass_math_school = all_students_math_passing.groupby(['school']).count().reset_index()

#print(students_pass_math_school)


# % Passing Reading
students_read_passing = students_pd.loc[students_pd['reading_score'] > 70]
#print(students_read_passing)

all_students_read_passing= pd.DataFrame({"school": students_read_passing["school"],
                                       "Count_students_passing_read": 0})

students_pass_read_school = all_students_read_passing.groupby(['school']).count().reset_index()

#print(students_pass_read_school)

#group students by school to calculate average math and reading scores per school
school_group = students_pd.groupby(['school'])

# create 'super table' of schools_df, students passing reading, students passing math and average math scores.
# we'll pull columns from this table to build the summary.
# normally, merge works on two dataframes, but we'll use a little dot-notation magic to do it all in one statement
#new_school_df = pd.merge(school_pd, df_of_averages, on = 'school') \
#.merge(students_pass_math_school, on = 'school') \
#.merge(students_pass_read_school, on = 'school') 

#new_school_df = pd.merge(school_pd, df_of_averages,students_pass_math_school,students_pass_read_school,on='school')

#new_school_df

new_school_df = pd.merge(school_pd, df_of_averages, on = 'school') \
.merge(students_pass_math_school, on = 'school') \
.merge(students_pass_read_school, on = 'school') 


#print(grouped_school_name.count().head(10))
new_school_df

#calculate percent of students passing math
percent_passing_math_school = (new_school_df['Count_students_passing_math'] / new_school_df['size']) * 100

#calculate percentage of students passing read
percent_students_passing_reading_school = (new_school_df['Count_students_passing_read'] / \
                                      new_school_df['size']) * 100

#calculate overall passing rate
overall_passing_school = ((percent_passing_math_school + percent_students_passing_reading_school) / 2)



# create district_summary dataframe for display
schools_summary_df = pd.DataFrame({'School Name': new_school_df['school'],
                               'School Type': new_school_df['type'],
                               'Total Students': new_school_df['size'],
                               'Total School Budget': new_school_df['budget'],
                               'Per Student Budget': (new_school_df['budget'] / new_school_df['size']),
                               'Average Math Score': new_school_df['Average Math Score'],
                               'Average Reading Score': new_school_df['Average Reading Score'],
                               '% Passing Math': percent_passing_math_school,
                               '% Passing Reading':percent_students_passing_reading_school,
                               'Overall Passing Rate': overall_passing_school})

school_summary_df = schools_summary_df[['School Name', 'School Type', 'Total Students', 'Total School Budget',
                                 'Per Student Budget', 'Average Math Score', 'Average Reading Score',
                                 '% Passing Math', '% Passing Reading', 'Overall Passing Rate']] \
                 .set_index('School Name').rename_axis(None)
```


```python
#print(grouped_school_name.count().head(10))
#new_school_df

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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>89.892107</td>
      <td>92.617831</td>
      <td>91.254969</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>89.713896</td>
      <td>93.392371</td>
      <td>91.553134</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>64.630225</td>
      <td>79.300643</td>
      <td>71.965434</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>65.753925</td>
      <td>77.510040</td>
      <td>71.631982</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>90.214067</td>
      <td>92.905199</td>
      <td>91.559633</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools


```python
top_performing_schools_df = school_summary_df.nlargest(5, 'Overall Passing Rate')
top_performing_schools_df
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>90.932983</td>
      <td>93.254490</td>
      <td>92.093736</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>91.683992</td>
      <td>92.203742</td>
      <td>91.943867</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>90.277778</td>
      <td>93.444444</td>
      <td>91.861111</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>89.558665</td>
      <td>93.864370</td>
      <td>91.711518</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>90.632319</td>
      <td>92.740047</td>
      <td>91.686183</td>
    </tr>
  </tbody>
</table>
</div>



# Bottom Performing Schools


```python

bottom_performing_schools_df = school_summary_df.nsmallest(5, 'Overall Passing Rate')
bottom_performing_schools_df
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>64.066017</td>
      <td>77.744436</td>
      <td>70.905226</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>63.318478</td>
      <td>78.813850</td>
      <td>71.066164</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>63.852132</td>
      <td>78.281874</td>
      <td>71.067003</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>63.750424</td>
      <td>78.433367</td>
      <td>71.091896</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>64.746494</td>
      <td>78.187702</td>
      <td>71.467098</td>
    </tr>
  </tbody>
</table>
</div>



# Math Score by School Grade

Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.


```python

#use groupby to create a dataset grouped by high school and grade, then average the columns and reset the indexes
#so we can reuse the school and grade columns later
avg_scores_by_high_school_df = students_pd.groupby(['school', 'grade']).mean().reset_index()



#use loc to isolate individual grades
ninth_grade_df = avg_scores_by_high_school_df.loc[avg_scores_by_high_school_df['grade'] == '9th']
tenth_grade_df = avg_scores_by_high_school_df.loc[avg_scores_by_high_school_df['grade'] == '10th']
eleventh_grade_df = avg_scores_by_high_school_df.loc[avg_scores_by_high_school_df['grade'] == '11th']
twelfth_grade_df = avg_scores_by_high_school_df.loc[avg_scores_by_high_school_df['grade'] == '12th']

#reduce dataframes to school, math score, reading score
ninth_grade_red_df = pd.DataFrame({"school": ninth_grade_df["school"],
                                   "reading_score_9th": ninth_grade_df['reading_score'],
                                   "math_score_9th": ninth_grade_df['math_score']})
tenth_grade_red_df = pd.DataFrame({"school": tenth_grade_df["school"],
                                   "reading_score_10th": tenth_grade_df['reading_score'],
                                   "math_score_10th": tenth_grade_df['math_score']})
eleventh_grade_red_df = pd.DataFrame({"school": eleventh_grade_df["school"],
                                   "reading_score_11th": eleventh_grade_df['reading_score'],
                                   "math_score_11th": eleventh_grade_df['math_score']})
twelfth_grade_red_df = pd.DataFrame({"school": twelfth_grade_df["school"],
                                   "reading_score_12th": twelfth_grade_df['reading_score'],
                                   "math_score_12th": twelfth_grade_df['math_score']})

all_scores_by_grade_hs_df = pd.merge(ninth_grade_red_df, tenth_grade_red_df, on = 'school')\
.merge(eleventh_grade_red_df, on = 'school')\
.merge(twelfth_grade_red_df, on = 'school') 
            

math_scores_by_grade_df = pd.DataFrame({'school': all_scores_by_grade_hs_df['school'],
                                        '9th': all_scores_by_grade_hs_df['math_score_9th'],
                                        '10th': all_scores_by_grade_hs_df['math_score_10th'],
                                        '11th': all_scores_by_grade_hs_df['math_score_11th'],
                                        '12th': all_scores_by_grade_hs_df['math_score_12th']})

math_scores_by_grade_df = math_scores_by_grade_df[['school', '9th', '10th', '11th', '12th']]\
                 .set_index('school').rename_axis(None)
math_scores_by_grade_df
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



# Reading Score by School Grade
Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.


```python
#now we need to create another dataframe with reading scores
avg_read_scores_by_high_school_df = pd.DataFrame({'school': all_scores_by_grade_hs_df['school'],
                                        '9th': all_scores_by_grade_hs_df['reading_score_9th'],
                                        '10th': all_scores_by_grade_hs_df['reading_score_10th'],
                                        '11th': all_scores_by_grade_hs_df['reading_score_11th'],
                                        '12th': all_scores_by_grade_hs_df['reading_score_12th']})

#Reassemble columns in display order, set index to school and delete index label
avg_read_scores_by_high_school_df = avg_read_scores_by_high_school_df[['school', '9th', '10th', '11th', '12th']]\
                 .set_index('school').rename_axis(None)
    
#display dataframe
avg_read_scores_by_high_school_df
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



# Scores broken down by School Spending
Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. 

Include in the table each of the following:

Average Math Score

Average Reading Score

% Passing Math

% Passing Reading

Overall Passing Rate (Average of the above two)


```python
#create a copy to save the old data frame

school_spending = school_summary_df.copy()

#create bins to hold values
bins = [0, 550, 600, 650, 700]

#name the bins
view_groups = ['>$550', '\$550-\$600', '\$600-\$650', '\$650-\$700']

view_groups

#Convert student budget into a float
school_spending['Per Student Budget']=school_spending['Per Student Budget']\
.replace('[\$]','',regex=True).astype('float')


#append a new column that shows the spend ranges
school_spending['Spending Ranges Per Student'] = pd.cut(school_spending['Per Student Budget'],bins, labels=view_groups)


school_spending

#build the dataset and include the column we created above
school_spending = school_spending[['Average Math Score',
                                            'Average Reading Score',
                                            '% Passing Math',
                                            '% Passing Reading',
                                            'Overall Passing Rate',
                                            'Spending Ranges Per Student']]

#group by Spending Range
average_scores_by_spending_df = school_spending.groupby('Spending Ranges Per Student').mean()

average_scores_by_spending_df
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
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges Per Student</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&gt;$550</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>\$550-\$600</th>
      <td>83.436210</td>
      <td>83.892196</td>
      <td>90.258770</td>
      <td>93.184236</td>
      <td>91.721503</td>
    </tr>
    <tr>
      <th>\$600-\$650</th>
      <td>79.423466</td>
      <td>82.044963</td>
      <td>74.208085</td>
      <td>83.721459</td>
      <td>78.964772</td>
    </tr>
    <tr>
      <th>\$650-\$700</th>
      <td>76.959583</td>
      <td>81.058567</td>
      <td>64.032486</td>
      <td>78.500776</td>
      <td>71.266631</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by the Size of a School

Repeat the above breakdown, but this time group schools based on a reasonable approximation of school size (Small, Medium, Large).



```python

#copy the data frame to save the old one
school_summary_bysize = school_summary_df.copy()

#create bins
bins = [0, 1000, 3000, 5000]

#name the bins
view_groups = ['Small (<1000) Students', 'Medium (1000-3000) Students', 'Large (3000-5000) Students']

#convert school size to numeric values
school_summary_bysize['Total Students'] = pd.to_numeric(school_summary_size['Total Students'])

#Cut the school size into the bins we define above 
school_summary_bysize['School Size'] = pd.cut(school_summary_size['Total Students'],
                                                                  bins, labels=view_groups)

#selecte columns for dataframe
school_summary_bysize = school_summary_bysize[['Average Math Score', 'Average Reading Score',
                                          '% Passing Math', '% Passing Reading', 'Overall Passing Rate',
                                          'School Size']]

#group by size bins and and take the aveage of scores
scores_by_school_bysize_df = school_summary_bysize.groupby('School Size').mean()


scores_by_school_bysize_df
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
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
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
      <th>Small (&lt;1000) Students</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>91.158155</td>
      <td>92.471895</td>
      <td>91.815025</td>
    </tr>
    <tr>
      <th>Medium (1000-3000) Students</th>
      <td>81.176821</td>
      <td>82.933187</td>
      <td>81.490258</td>
      <td>88.248440</td>
      <td>84.869349</td>
    </tr>
    <tr>
      <th>Large (3000-5000) Students</th>
      <td>77.063340</td>
      <td>80.919864</td>
      <td>64.323717</td>
      <td>78.378664</td>
      <td>71.351190</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Type

Repeat the above breakdown, but this time group schools based on school type (Charter vs. District).


```python
#create a copy to save the old version
school_summary_type = school_summary_df.copy()

#select columns
school_summary_type = school_summary_type[['School Type','Average Math Score', 'Average Reading Score',
                                          '% Passing Math', '% Passing Reading', 'Overall Passing Rate']]
                                          
#group by school type and average columns
scores_by_school_type_df = school_summary_type.groupby('School Type').mean()

#display dataframe
scores_by_school_type_df
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
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
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
      <td>83.473852</td>
      <td>83.896421</td>
      <td>90.363226</td>
      <td>93.052812</td>
      <td>91.708019</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>64.302528</td>
      <td>78.324559</td>
      <td>71.313543</td>
    </tr>
  </tbody>
</table>
</div>
