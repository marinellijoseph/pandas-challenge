

```python
import pandas as pd
import numpy as np
```


```python
# Reference the file where the json is located
json_path = "purchase_data2.json"

# Import the data into a Pandas DataFrame
hero_data = pd.read_json(json_path)
hero_data.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count


```python
players = hero_data["SN"].nunique()
Player_Count = pd.DataFrame({"Total Players": [players]})
Player_Count
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
# unique items
unique_items = hero_data["Item ID"].nunique()
# average price
average_price = hero_data["Price"].mean()
# number of purchases
total_purchases = hero_data["SN"].nunique()
# total revenue
total_revenue = hero_data["Price"].sum()

# new table
Purchasing_Analysis = pd.DataFrame({"Number of Unique Items": [unique_items],
                                   "Average Price": [average_price],
                                   "Number of Purchases": [total_purchases],
                                   "Total Revenue": [total_revenue]
})
Purchasing_Analysis = Purchasing_Analysis[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]
Purchasing_Analysis = Purchasing_Analysis.round(2)
Purchasing_Analysis
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>64</td>
      <td>2.92</td>
      <td>74</td>
      <td>228.1</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
# count of total and genders
total_gender = hero_data["Gender"].count()
male = hero_data["Gender"].value_counts()["Male"]
female = hero_data["Gender"].value_counts()["Female"]
other = (total_gender - male - female)
# calculate percentages
male_percent = male/total_gender * 100
female_percent = female/total_gender* 100
other_percent = other/total_gender* 100

# new table
Gender_Demographics = pd.DataFrame({"Gender": ["Male", "Female", "Other"],
                                   "Percentage of Players": [male_percent,female_percent, other_percent],
                                   "Total Count": [male,female, other]
})
Gender_Demographics = Gender_Demographics.round(2)
Gender_Demographics = Gender_Demographics[["Gender", "Percentage of Players","Total Count"]]
Gender_Demographics

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
      <th>Gender</th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>82.05</td>
      <td>64</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>16.67</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other</td>
      <td>1.28</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
# extract gender dfs
male_data = hero_data.loc[hero_data["Gender"] == "Male"]
female_data = hero_data.loc[hero_data["Gender"] == "Female"]
other_data = hero_data.loc[hero_data["Gender"] == "Other / Non-Disclosed"]

# average price per gender 
average_male = male_data["Price"].mean()
average_female = female_data["Price"].mean()
average_other = other_data["Price"].mean()

# total purchase value
total_male = male_data["Price"].sum()
total_female = female_data["Price"].sum()
total_other = other_data["Price"].sum()


# normalized totals???


Purchasing_Analysis_Gender = pd.DataFrame({"Gender": ["Male", "Female", "Other"], 
                                        "Purchase Count": [male, female, other],
                                        "Average Purchase Price":[average_male, average_female, average_other],
                                        "Total Purchase Value":[total_male, total_female, total_other]})
#                                        "Normalized Totals":[]
#                                            })
Purchasing_Analysis_Gender = Purchasing_Analysis_Gender.round(2)
Purchasing_Analysis_Gender = Purchasing_Analysis_Gender[["Gender", "Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
Purchasing_Analysis_Gender
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
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>64</td>
      <td>2.88</td>
      <td>184.60</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>13</td>
      <td>3.18</td>
      <td>41.38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other</td>
      <td>1</td>
      <td>2.12</td>
      <td>2.12</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
# Figure out the minimum and maximum ages
print(hero_data["Age"].max())
print(hero_data["Age"].min())
```

    40
    7



```python
# Create bins in which to place values based on age 
bins = [0,9,14,19,24,29,34,39,45]

# Create labels for these bins
group_labels = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

# Slice the data and place it into bins
pd.cut(hero_data["Age"],bins,labels = group_labels)

# Place the data series into a new column inside of the DataFrame
hero_data["Age Group"] = pd.cut(hero_data["Age"],bins,labels=group_labels)

# extract age group dfs
groupone_data = hero_data.loc[hero_data["Age Group"] == "<10"]
grouptwo_data = hero_data.loc[hero_data["Age Group"] == "10-14"]
groupthree_data = hero_data.loc[hero_data["Age Group"] == "15-19"]
groupfour_data = hero_data.loc[hero_data["Age Group"] == "20-24"]
groupfive_data = hero_data.loc[hero_data["Age Group"] == "25-29"]
groupsix_data = hero_data.loc[hero_data["Age Group"]== "30-34"]
groupseven_data = hero_data.loc[hero_data["Age Group"]== "35-39"]
groupeight_data = hero_data.loc[hero_data["Age Group"]== "40+"]

# total purchase value per age group
total_groupone = groupone_data["Price"].sum()
total_grouptwo = grouptwo_data["Price"].sum()
total_groupthree = groupthree_data["Price"].sum()
total_groupfour = groupfour_data["Price"].sum() / groupfour_data["Price"].count()
total_groupfive = groupfive_data["Price"].sum()
total_groupsix = groupsix_data["Price"].sum()
total_groupseven = groupseven_data["Price"].sum()
total_groupeight = groupeight_data["Price"].sum()

# average purchase price per age group
average_groupone = total_groupone / groupone_data["Price"].count()
average_grouptwo = total_grouptwo / grouptwo_data["Price"].count()
average_groupthree = total_groupthree / groupthree_data["Price"].count()
average_groupfour = total_groupfour / groupfour_data["Price"].count()
average_groupfive = total_groupfive / groupfive_data["Price"].count()
average_groupsix = total_groupsix / groupsix_data["Price"].count()
average_groupseven = total_groupseven / groupseven_data["Price"].count()
average_groupeight = total_groupeight / groupeight_data["Price"].count()

# purchase count
count_groupone = groupone_data["Price"].count()
count_grouptwo = grouptwo_data["Price"].count()
count_groupthree = groupthree_data["Price"].count()
count_groupfour = groupfour_data["Price"].count()
count_groupfive = groupfive_data["Price"].count()
count_groupsix = groupsix_data["Price"].count()
count_groupseven = groupseven_data["Price"].count()
count_groupeight = groupeight_data["Price"].count()
```


```python
Age_Demographics = pd.DataFrame({"Age Group": ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"], 
                                "Purchase Count":[count_groupone, count_grouptwo, count_groupthree, count_groupfour, count_groupfive, count_groupsix, count_groupseven, count_groupeight], 
                                "Average Purchase Value":[average_groupone, average_grouptwo, average_groupthree, average_groupfour, average_groupfive, average_groupsix, average_groupseven, average_groupeight], 
                                "Total Purchase Value":[total_groupone, total_grouptwo, total_groupthree, total_groupfour, total_groupfive, total_groupsix, total_groupseven, total_groupeight]})

Age_Demographics = Age_Demographics.round(2)
Age_Demographics
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
      <th>Age Group</th>
      <th>Average Purchase Value</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;10</td>
      <td>2.76</td>
      <td>5</td>
      <td>13.82</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10-14</td>
      <td>2.99</td>
      <td>3</td>
      <td>8.96</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-19</td>
      <td>2.76</td>
      <td>11</td>
      <td>30.41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20-24</td>
      <td>0.08</td>
      <td>36</td>
      <td>3.02</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-29</td>
      <td>2.90</td>
      <td>9</td>
      <td>26.11</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30-34</td>
      <td>1.98</td>
      <td>7</td>
      <td>13.89</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35-39</td>
      <td>3.56</td>
      <td>6</td>
      <td>21.37</td>
    </tr>
    <tr>
      <th>7</th>
      <td>40+</td>
      <td>4.65</td>
      <td>1</td>
      <td>4.65</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
# Using GroupBy in order to separate the data into fields according to "SN" values
grouped_herodata = hero_data.groupby(['SN'])
# The object returned is a "GroupBy" object and cannot be viewed normally...
print(grouped_herodata)
grouped_herodata.head(10)
top_spenders = grouped_herodata['Price'].sum().nlargest(5)

#top_spenders variables
first_spender = top_spenders.index[0]
second_spender = top_spenders.index[1]
third_spender = top_spenders.index[2]
fourth_spender = top_spenders.index[3]
fifth_spender = top_spenders.index[4]

# extract first spender dataframes and calculations
first_spender_data = hero_data.loc[hero_data["SN"] == first_spender]
first_count = first_spender_data["Price"].count()
first_total = first_spender_data["Price"].sum()
first_average = first_total / first_count

# extract second spender dataframes and calculations
second_spender_data = hero_data.loc[hero_data["SN"] == second_spender]
second_count = second_spender_data["Price"].count()
second_total = second_spender_data["Price"].sum()
second_average = second_total / second_count

# extract third spender dataframes and calculations
third_spender_data = hero_data.loc[hero_data["SN"] == third_spender]
third_count = third_spender_data["Price"].count()
third_total = third_spender_data["Price"].sum()
third_average = third_total / third_count

# extract fourth spender dataframes and calculations
fourth_spender_data = hero_data.loc[hero_data["SN"] == fourth_spender]
fourth_count = fourth_spender_data["Price"].count()
fourth_total = fourth_spender_data["Price"].sum()
fourth_average = fourth_total / fourth_count

# extract fifth spender dataframes and calculations
fifth_spender_data = hero_data.loc[hero_data["SN"] == fifth_spender]
fifth_count = fifth_spender_data["Price"].count()
fifth_total = fifth_spender_data["Price"].sum()
fifth_average = fifth_total / fifth_count

# new table
Top_Spenders_Analysis = pd.DataFrame({"SN":[first_spender, second_spender, third_spender, fourth_spender,fifth_spender], 
                                "Purchase Count":[first_count, second_count, third_count, fourth_count, fifth_count], 
                                "Average Purchase Value":[first_average, second_average, third_average, fourth_average, fifth_average], 
                                "Total Purchase Value":[first_total, second_total, third_total, fourth_total, fifth_total]})

Top_Spenders_Analysis = Top_Spenders_Analysis.round(2)
Top_Spenders_Analysis = Top_Spenders_Analysis[["SN", "Purchase Count", "Average Purchase Value", "Total Purchase Value"]]
Top_Spenders_Analysis
```

    <pandas.core.groupby.DataFrameGroupBy object at 0x10dec6748>





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
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Average Purchase Value</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sundaky74</td>
      <td>2</td>
      <td>3.70</td>
      <td>7.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aidaira26</td>
      <td>2</td>
      <td>2.56</td>
      <td>5.13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Eusty71</td>
      <td>1</td>
      <td>4.81</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Chanirra64</td>
      <td>1</td>
      <td>4.78</td>
      <td>4.78</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alarap40</td>
      <td>1</td>
      <td>4.71</td>
      <td>4.71</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
# Using GroupBy in order to separate the data into fields according to "Item Name" values
grouped_itemname = hero_data.groupby(['Item ID'])
# The object returned is a "GroupBy" object and cannot be viewed normally...
print(grouped_itemname)
grouped_itemname.head(10)
top_populars = grouped_itemname["Item ID"].count().nlargest(5)

# top_populars variables
first_popular = top_populars.index[0]
second_popular = top_populars.index[1]
third_popular = top_populars.index[2]
fourth_popular = top_populars.index[3]
fifth_popular = top_populars.index[4]

# extract first popular dataframes and calculations
first_popular_data = hero_data.loc[hero_data["Item ID"] == first_popular]
first_popularprice = first_popular_data["Price"].mean()
first_popularcount = first_popular_data["Price"].count()
first_populartotal = first_popular_data["Price"].sum()
first_popularname = first_popular_data["Item Name"].max()

# extract second popular dataframes and calculations
second_popular_data = hero_data.loc[hero_data["Item ID"] == second_popular]
second_popularprice = second_popular_data["Price"].mean()
second_popularcount = second_popular_data["Price"].count()
second_populartotal = second_popular_data["Price"].sum()
second_popularname = second_popular_data["Item Name"].max()

# extract third popular dataframes and calculations
third_popular_data = hero_data.loc[hero_data["Item ID"] == third_popular]
third_popularprice = third_popular_data["Price"].mean()
third_popularcount = third_popular_data["Price"].count()
third_populartotal = third_popular_data["Price"].sum()
third_popularname = third_popular_data["Item Name"].max()

# extract fourth popular dataframes and calculations
fourth_popular_data = hero_data.loc[hero_data["Item ID"] == fourth_popular]
fourth_popularprice = fourth_popular_data["Price"].mean()
fourth_popularcount = fourth_popular_data["Price"].count()
fourth_populartotal = fourth_popular_data["Price"].sum()
fourth_popularname = fourth_popular_data["Item Name"].max()

# extract fifth popular dataframes and calculations
fifth_popular_data = hero_data.loc[hero_data["Item ID"] == fifth_popular]
fifth_popularprice = fifth_popular_data["Price"].mean()
fifth_popularcount = fifth_popular_data["Price"].count()
fifth_populartotal = fifth_popular_data["Price"].sum()
fifth_popularname = fifth_popular_data["Item Name"].max()

# new table
Top_populars_Analysis = pd.DataFrame({"Item ID":[first_popular,second_popular, third_popular, fourth_popular, fifth_popular ],
    "Item Name":[first_popularname, second_popularname, third_popularname, fourth_popularname, fifth_popularname],
    "Purchase Count":[first_popularcount, second_popularcount, third_popularcount, fourth_popularcount, fifth_popularcount], 
    "Price":[first_popularprice, second_popularprice, third_popularprice, fourth_popularprice, fifth_popularprice], 
    "Total Purchase Value":[first_populartotal, second_populartotal, third_populartotal, fourth_populartotal, fifth_populartotal]})

Top_populars_Analysis = Top_populars_Analysis.round(2)
Top_populars_Analysis = Top_populars_Analysis[["Item ID", "Item Name", "Purchase Count", "Price", "Total Purchase Value"]]
Top_populars_Analysis

```

    <pandas.core.groupby.DataFrameGroupBy object at 0x10e0725f8>





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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>3</td>
      <td>3.64</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>1</th>
      <td>60</td>
      <td>Wolf</td>
      <td>2</td>
      <td>2.70</td>
      <td>5.40</td>
    </tr>
    <tr>
      <th>2</th>
      <td>64</td>
      <td>Fusion Pummel</td>
      <td>2</td>
      <td>2.42</td>
      <td>4.84</td>
    </tr>
    <tr>
      <th>3</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>2</td>
      <td>4.12</td>
      <td>8.24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>2</td>
      <td>4.49</td>
      <td>8.98</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
# find top 5 profitable item IDs
top_profitable = grouped_itemname["Price"].sum().nlargest(5)

# top_profitables variables of item ID
first_profitable = top_profitable.index[0]
second_profitable = top_profitable.index[1]
third_profitable = top_profitable.index[2]
fourth_profitable = top_profitable.index[3]
fifth_profitable = top_profitable.index[4]

# extract first profitable dataframes and calculations
first_profitable_data = hero_data.loc[hero_data["Item ID"] == first_profitable]
first_profitableprice = first_profitable_data["Price"].mean()
first_profitablecount = first_profitable_data["Price"].count()
first_profitabletotal = first_profitable_data["Price"].sum()
first_profitablename = first_profitable_data["Item Name"].max()

# extract second profitable dataframes and calculations
second_profitable_data = hero_data.loc[hero_data["Item ID"] == second_profitable]
second_profitableprice = second_profitable_data["Price"].mean()
second_profitablecount = second_profitable_data["Price"].count()
second_profitabletotal = second_profitable_data["Price"].sum()
second_profitablename = second_profitable_data["Item Name"].max()

# extract third profitable dataframes and calculations
third_profitable_data = hero_data.loc[hero_data["Item ID"] == third_profitable]
third_profitableprice = third_profitable_data["Price"].mean()
third_profitablecount = third_profitable_data["Price"].count()
third_profitabletotal = third_profitable_data["Price"].sum()
third_profitablename = third_profitable_data["Item Name"].max()

# extract fourth profitable dataframes and calculations
fourth_profitable_data = hero_data.loc[hero_data["Item ID"] == fourth_profitable]
fourth_profitableprice = fourth_profitable_data["Price"].mean()
fourth_profitablecount = fourth_profitable_data["Price"].count()
fourth_profitabletotal = fourth_profitable_data["Price"].sum()
fourth_profitablename = fourth_profitable_data["Item Name"].max()

# extract fifth profitable dataframes and calculations
fifth_profitable_data = hero_data.loc[hero_data["Item ID"] == fifth_profitable]
fifth_profitableprice = fifth_profitable_data["Price"].mean()
fifth_profitablecount = fifth_profitable_data["Price"].count()
fifth_profitabletotal = fifth_profitable_data["Price"].sum()
fifth_profitablename = fifth_profitable_data["Item Name"].max()

# new table
Top_profitable_Analysis = pd.DataFrame({"Item ID":[first_profitable,second_profitable, third_profitable, fourth_profitable, fifth_profitable ],
    "Item Name":[first_profitablename, second_profitablename, third_profitablename, fourth_profitablename, fifth_profitablename],
    "Purchase Count":[first_profitablecount, second_profitablecount, third_profitablecount, fourth_profitablecount, fifth_profitablecount], 
    "Price":[first_profitableprice, second_profitableprice, third_profitableprice, fourth_profitableprice, fifth_profitableprice], 
    "Total Purchase Value":[first_profitabletotal, second_profitabletotal, third_profitabletotal, fourth_profitabletotal, fifth_profitabletotal]})

Top_profitable_Analysis = Top_profitable_Analysis.round(2)
Top_profitable_Analysis = Top_profitable_Analysis[["Item ID", "Item Name", "Purchase Count", "Price", "Total Purchase Value"]]
Top_profitable_Analysis

Top_profitable_Analysis = Top_profitable_Analysis.round(2)
Top_profitable_Analysis = Top_profitable_Analysis[["Item ID", "Item Name", "Purchase Count", "Price", "Total Purchase Value"]]
Top_profitable_Analysis

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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>94</td>
      <td>Mourning Blade</td>
      <td>3</td>
      <td>3.64</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>1</th>
      <td>117</td>
      <td>Heartstriker, Legacy of the Light</td>
      <td>2</td>
      <td>4.71</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>2</td>
      <td>4.49</td>
      <td>8.98</td>
    </tr>
    <tr>
      <th>3</th>
      <td>90</td>
      <td>Betrayer</td>
      <td>2</td>
      <td>4.12</td>
      <td>8.24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>2</td>
      <td>4.11</td>
      <td>8.22</td>
    </tr>
  </tbody>
</table>
</div>



## Three Observable Trends


```python
# Three observable trends
# 1. Mostly males purchased items
# 2. 15-19 year old purchased most, then 25-29. large dropoff in 20-24 year olds, possible due to college students not affording or having time
# 3. Purchases decline at age 30 and above, also decline at 15 and below

```
# pandas-challenge
