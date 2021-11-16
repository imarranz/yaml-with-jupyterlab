# Using YAML with Jupyter

![]('/figures/yaml-with-jupyterlab-logo.png')

I have recently started working with YAML to set up my [Data Science Projects](https://github.com/imarranz/data-science-workflow-management) and I think it is a really useful tool. Here are examples of how to use YAML with our jupyter notebooks and links to other sources.

## Sources

[reading and writing YAML](https://stackabuse.com/reading-and-writing-yaml-to-a-file-in-python/)  
[writing YAML files](https://towardsdatascience.com/writing-yaml-files-with-python-a6a7fc6ed6c3)  
[YAML tutorial](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started)  
[Using YAML in python for structured data](https://kitchingroup.cheme.cmu.edu/blog/2014/02/03/Using-YAML-in-python-for-structured-data/)  


```python
import pandas as pd
import yaml
```

## Config

The content of YAML file is the following code.

```
name_file: 'https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv'

rename_day:
 Thur: Thursday
 Fri: Friday
 Sat: Saturday
 Sun: Sunday
    
a_list:
 - Monday
 - Tuesday
 - Wednesday
 - Thursday
 - Friday
 - Saturday
 - Sunday
 
a_dictionary:
  Mon: Monday
  Tue: Tuesday
  Wed: Wednesday
  Thu: Thursday
  Fri: Friday
  Sat: Saturday
  Sun: Sunday
  
customers:
  customer_a:
    time: Dinner
    day: 
      - Thursday
      - Friday
  customer_b:
    time: Lunch
    day: 
      - Thursday
      - Friday  
  customer_c:
    time: Dinner
    day: 
      - Saturday
      - Sunday
  customer_d:
    time: Lunch
    day: 
      - Saturday
      - Sunday        
 
set_table_styles:
  hover:
    selector: 'tr:hover'
    props: [['background-color', 'black'], ['color', 'white']]
  nth-of-type(odd):
    selector: 'tr:nth-of-type(odd)'
    props: [['background', '#bbb']]
  nth-of-type(even):
    selector: 'tr:nth-of-type(even)'
    props: [['background', 'white']]
  th:
    selector: 'th'
    props: [['background', '#606060'], ['color', 'white'] , ['font-family', 'verdana'], ['font-size', '1.1em']]
  td:
    selector: 'td'
    props: [['font-family', 'verdana'], ['font-size', '0.9em']]
```    


```python
with open('./yaml/config.yaml', 'r') as ymlfile:
    cfg = yaml.full_load(ymlfile)
```

A numeric variable


```python
cfg['a_value']
```




    23.45



A text variable


```python
cfg['name_file']
```




    'https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv'



A list


```python
cfg['a_list']
```




    ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']



A dictionary


```python
cfg['a_dictionary']
```




    {'Mon': 'Monday',
     'Tue': 'Tuesday',
     'Wed': 'Wednesday',
     'Thu': 'Thursday',
     'Fri': 'Friday',
     'Sat': 'Saturday',
     'Sun': 'Sunday'}



A complex structure


```python
cfg['customers']
```




    {'customer_a': {'time': 'Dinner', 'day': ['Thursday', 'Friday']},
     'customer_b': {'time': 'Lunch', 'day': ['Thursday', 'Friday']},
     'customer_c': {'time': 'Dinner', 'day': ['Saturday', 'Sunday']},
     'customer_d': {'time': 'Lunch', 'day': ['Saturday', 'Sunday']}}



## Data import

Data import with configurations.


```python
data = \
pd\
    .read_csv(cfg['name_file'])\
    .replace({'day': cfg['rename_day']})
```


```python
data
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
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sunday</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sunday</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sunday</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sunday</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sunday</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>239</th>
      <td>29.03</td>
      <td>5.92</td>
      <td>Male</td>
      <td>No</td>
      <td>Saturday</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>240</th>
      <td>27.18</td>
      <td>2.00</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Saturday</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>241</th>
      <td>22.67</td>
      <td>2.00</td>
      <td>Male</td>
      <td>Yes</td>
      <td>Saturday</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>242</th>
      <td>17.82</td>
      <td>1.75</td>
      <td>Male</td>
      <td>No</td>
      <td>Saturday</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>243</th>
      <td>18.78</td>
      <td>3.00</td>
      <td>Female</td>
      <td>No</td>
      <td>Thursday</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>244 rows × 7 columns</p>
</div>



## Data Show

Table with styles from yaml file.


```python
data\
    .head(5)\
    .style\
        .set_table_styles(
            [cfg['set_table_styles']['nth-of-type(odd)'],
             cfg['set_table_styles']['nth-of-type(even)'],
             cfg['set_table_styles']['th'],
             cfg['set_table_styles']['td'],
             cfg['set_table_styles']['hover']])\
        .hide_index()
```




<style type="text/css">
#T_be4e1_ tr:nth-of-type(odd) {
  background: #bbb;
}
#T_be4e1_ tr:nth-of-type(even) {
  background: white;
}
#T_be4e1_ th {
  background: #606060;
  color: white;
  font-family: verdana;
  font-size: 1.1em;
}
#T_be4e1_ td {
  font-family: verdana;
  font-size: 0.9em;
}
#T_be4e1_ tr:hover {
  background-color: black;
  color: white;
}
</style>
<table id="T_be4e1_">
  <thead>
    <tr>
      <th class="col_heading level0 col0" >total_bill</th>
      <th class="col_heading level0 col1" >tip</th>
      <th class="col_heading level0 col2" >sex</th>
      <th class="col_heading level0 col3" >smoker</th>
      <th class="col_heading level0 col4" >day</th>
      <th class="col_heading level0 col5" >time</th>
      <th class="col_heading level0 col6" >size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td id="T_be4e1_row0_col0" class="data row0 col0" >16.990000</td>
      <td id="T_be4e1_row0_col1" class="data row0 col1" >1.010000</td>
      <td id="T_be4e1_row0_col2" class="data row0 col2" >Female</td>
      <td id="T_be4e1_row0_col3" class="data row0 col3" >No</td>
      <td id="T_be4e1_row0_col4" class="data row0 col4" >Sunday</td>
      <td id="T_be4e1_row0_col5" class="data row0 col5" >Dinner</td>
      <td id="T_be4e1_row0_col6" class="data row0 col6" >2</td>
    </tr>
    <tr>
      <td id="T_be4e1_row1_col0" class="data row1 col0" >10.340000</td>
      <td id="T_be4e1_row1_col1" class="data row1 col1" >1.660000</td>
      <td id="T_be4e1_row1_col2" class="data row1 col2" >Male</td>
      <td id="T_be4e1_row1_col3" class="data row1 col3" >No</td>
      <td id="T_be4e1_row1_col4" class="data row1 col4" >Sunday</td>
      <td id="T_be4e1_row1_col5" class="data row1 col5" >Dinner</td>
      <td id="T_be4e1_row1_col6" class="data row1 col6" >3</td>
    </tr>
    <tr>
      <td id="T_be4e1_row2_col0" class="data row2 col0" >21.010000</td>
      <td id="T_be4e1_row2_col1" class="data row2 col1" >3.500000</td>
      <td id="T_be4e1_row2_col2" class="data row2 col2" >Male</td>
      <td id="T_be4e1_row2_col3" class="data row2 col3" >No</td>
      <td id="T_be4e1_row2_col4" class="data row2 col4" >Sunday</td>
      <td id="T_be4e1_row2_col5" class="data row2 col5" >Dinner</td>
      <td id="T_be4e1_row2_col6" class="data row2 col6" >3</td>
    </tr>
    <tr>
      <td id="T_be4e1_row3_col0" class="data row3 col0" >23.680000</td>
      <td id="T_be4e1_row3_col1" class="data row3 col1" >3.310000</td>
      <td id="T_be4e1_row3_col2" class="data row3 col2" >Male</td>
      <td id="T_be4e1_row3_col3" class="data row3 col3" >No</td>
      <td id="T_be4e1_row3_col4" class="data row3 col4" >Sunday</td>
      <td id="T_be4e1_row3_col5" class="data row3 col5" >Dinner</td>
      <td id="T_be4e1_row3_col6" class="data row3 col6" >2</td>
    </tr>
    <tr>
      <td id="T_be4e1_row4_col0" class="data row4 col0" >24.590000</td>
      <td id="T_be4e1_row4_col1" class="data row4 col1" >3.610000</td>
      <td id="T_be4e1_row4_col2" class="data row4 col2" >Female</td>
      <td id="T_be4e1_row4_col3" class="data row4 col3" >No</td>
      <td id="T_be4e1_row4_col4" class="data row4 col4" >Sunday</td>
      <td id="T_be4e1_row4_col5" class="data row4 col5" >Dinner</td>
      <td id="T_be4e1_row4_col6" class="data row4 col6" >4</td>
    </tr>
  </tbody>
</table>





```python
cfg_tables = \
[cfg['set_table_styles']['nth-of-type(odd)'],
 cfg['set_table_styles']['nth-of-type(even)'],
 cfg['set_table_styles']['th'],
 cfg['set_table_styles']['td'],
 cfg['set_table_styles']['hover']]
```


```python
data.head(5).style.set_table_styles(cfg_tables).hide_index()
```




<style type="text/css">
#T_5c92e_ tr:nth-of-type(odd) {
  background: #bbb;
}
#T_5c92e_ tr:nth-of-type(even) {
  background: white;
}
#T_5c92e_ th {
  background: #606060;
  color: white;
  font-family: verdana;
  font-size: 1.1em;
}
#T_5c92e_ td {
  font-family: verdana;
  font-size: 0.9em;
}
#T_5c92e_ tr:hover {
  background-color: black;
  color: white;
}
</style>
<table id="T_5c92e_">
  <thead>
    <tr>
      <th class="col_heading level0 col0" >total_bill</th>
      <th class="col_heading level0 col1" >tip</th>
      <th class="col_heading level0 col2" >sex</th>
      <th class="col_heading level0 col3" >smoker</th>
      <th class="col_heading level0 col4" >day</th>
      <th class="col_heading level0 col5" >time</th>
      <th class="col_heading level0 col6" >size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td id="T_5c92e_row0_col0" class="data row0 col0" >16.990000</td>
      <td id="T_5c92e_row0_col1" class="data row0 col1" >1.010000</td>
      <td id="T_5c92e_row0_col2" class="data row0 col2" >Female</td>
      <td id="T_5c92e_row0_col3" class="data row0 col3" >No</td>
      <td id="T_5c92e_row0_col4" class="data row0 col4" >Sunday</td>
      <td id="T_5c92e_row0_col5" class="data row0 col5" >Dinner</td>
      <td id="T_5c92e_row0_col6" class="data row0 col6" >2</td>
    </tr>
    <tr>
      <td id="T_5c92e_row1_col0" class="data row1 col0" >10.340000</td>
      <td id="T_5c92e_row1_col1" class="data row1 col1" >1.660000</td>
      <td id="T_5c92e_row1_col2" class="data row1 col2" >Male</td>
      <td id="T_5c92e_row1_col3" class="data row1 col3" >No</td>
      <td id="T_5c92e_row1_col4" class="data row1 col4" >Sunday</td>
      <td id="T_5c92e_row1_col5" class="data row1 col5" >Dinner</td>
      <td id="T_5c92e_row1_col6" class="data row1 col6" >3</td>
    </tr>
    <tr>
      <td id="T_5c92e_row2_col0" class="data row2 col0" >21.010000</td>
      <td id="T_5c92e_row2_col1" class="data row2 col1" >3.500000</td>
      <td id="T_5c92e_row2_col2" class="data row2 col2" >Male</td>
      <td id="T_5c92e_row2_col3" class="data row2 col3" >No</td>
      <td id="T_5c92e_row2_col4" class="data row2 col4" >Sunday</td>
      <td id="T_5c92e_row2_col5" class="data row2 col5" >Dinner</td>
      <td id="T_5c92e_row2_col6" class="data row2 col6" >3</td>
    </tr>
    <tr>
      <td id="T_5c92e_row3_col0" class="data row3 col0" >23.680000</td>
      <td id="T_5c92e_row3_col1" class="data row3 col1" >3.310000</td>
      <td id="T_5c92e_row3_col2" class="data row3 col2" >Male</td>
      <td id="T_5c92e_row3_col3" class="data row3 col3" >No</td>
      <td id="T_5c92e_row3_col4" class="data row3 col4" >Sunday</td>
      <td id="T_5c92e_row3_col5" class="data row3 col5" >Dinner</td>
      <td id="T_5c92e_row3_col6" class="data row3 col6" >2</td>
    </tr>
    <tr>
      <td id="T_5c92e_row4_col0" class="data row4 col0" >24.590000</td>
      <td id="T_5c92e_row4_col1" class="data row4 col1" >3.610000</td>
      <td id="T_5c92e_row4_col2" class="data row4 col2" >Female</td>
      <td id="T_5c92e_row4_col3" class="data row4 col3" >No</td>
      <td id="T_5c92e_row4_col4" class="data row4 col4" >Sunday</td>
      <td id="T_5c92e_row4_col5" class="data row4 col5" >Dinner</td>
      <td id="T_5c92e_row4_col6" class="data row4 col6" >4</td>
    </tr>
  </tbody>
</table>



