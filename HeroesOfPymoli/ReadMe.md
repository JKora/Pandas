

```python
# import pandas and numpy libraries
import pandas as pd
import numpy as np
import os
```


```python
import json as js # import JSON to read json files
```


```python
file_path = input ("Enter the file path: ") # user input for file path
file_name = input("Enter the file name: ") # user input for file name
file = os.path.join(file_path, file_name) 
datafile = js.load(open(file)) # open and load the required file
purch_data_df = pd.DataFrame(datafile) # pass the data from datafile into data frame
purch_data_df.head(5)
```

    Enter the file path: /Users/janakidevikora/Python_GIT/Pandas/HeroesOfPymoli
    Enter the file name: purchase_data.json





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
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#player count:
player_count_df = pd.DataFrame({"Total Players" : [len(purch_data_df["SN"].unique())]})
player_count_df
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
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total) : Number of Unique Items, Average Purchase Price, Total Number of Purchases,Total Revenue

Unique_items = len(purch_data_df["Item Name"].unique())
#'${:.2f}'.format(cash) - sets the display format to show $ symbol and 2 decimal places
Avg_Pur_Price = '${:.2f}'.format(purch_data_df["Price"].mean())
Total_purchases = purch_data_df["Price"].count()
Total_Rev = '${:.2f}'.format(purch_data_df["Price"].sum())

purchasing_analy_df = pd.DataFrame({"Number of Unique Items" : [Unique_items],
                                   "Average Price" : [Avg_Pur_Price],
                                   "Number of Purchases": [Total_purchases],
                                   "Total Revenue" : [Total_Rev]})

Org_purc_anal_df = purchasing_analy_df[["Number of Unique Items", "Average Price",
                                        "Number of Purchases", "Total Revenue" ]]
Org_purc_anal_df
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
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics: # Percentage and Count of Male Players, # Percentage and Count of Female Players
# Percentage and Count of Other / Non-Disclosed
gender_demo = purch_data_df.groupby("Gender")# group purchase data by Gender
# send the grouped data into Data Frame with total counts of players per gender
gender_demo_df = pd.DataFrame({"Total Count" : gender_demo["SN"].nunique()}) 
# percentage of players 
percent_players = round((gender_demo_df["Total Count"]/gender_demo_df["Total Count"].sum())*100,2)
# add Percentage of players to the grouped data frame
gender_demo_df["Percentage of Players"] = percent_players
# set the column sequence for the data frame
gender_demo_df_arranged = gender_demo_df[["Percentage of Players", "Total Count"]]
gender_demo_df_arranged
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Purchasing Analysis (Gender)**
purchase_count = gender_demo["Item ID"].count() # Total Number of purchases per Gender
purchase_value = gender_demo["Price"].sum()# Total Purchase value per Gender
Avg_Pur_Price = gender_demo["Price"].mean() # Total Avg Purchase value per Gender
SN_Count = gender_demo["SN"].nunique() # total unique screen names per gender
#normalized total of a gender = total purchase price of the gender / unique number of users (or SN) of that gender
normalized_total = gender_demo["Price"].sum()/gender_demo["SN"].nunique() 

gender_analysis_df = pd.DataFrame({"Total Purchase Value" : purchase_value.map('${:,.2f}'.format),
                                  "Average Purchase Price" : Avg_Pur_Price.map('${:,.2f}'.format),
                                  "Purchase Count" : purchase_count,
                                  "Normalized Totals" : normalized_total.map('${:,.2f}'.format)})

gender_analysis_df_arranged = gender_analysis_df[["Purchase Count","Average Purchase Price", 
                                                  "Total Purchase Value","Normalized Totals"]]

gender_analysis_df_arranged
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Age Demographics**  # * The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.) 

age_bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]
age_labels = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
purch_data_df[""]=pd.cut(purch_data_df["Age"], age_bins, labels = age_labels) # bins the data per column specified
age_demo = purch_data_df.groupby("") # grouping the data by age_bins
age_demo_df = pd.DataFrame({"Total Count" : age_demo["SN"].nunique(),
                           "Percentage of Players" : ((age_demo["SN"].nunique()/purch_data_df["SN"].nunique())*100).map('{:.2f}'.format)})
age_demo_df


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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Purchasing Analysis (Age)**
# * Purchase Count , * Average Purchase Price, * Total Purchase Value, * Normalized Totals

age_anal_df = pd.DataFrame({"Purchase Count":age_demo["Price"].count(),
                            "Average Purchase Price" : age_demo["Price"].mean().map('${:.2f}'.format),
                            "Total Purchase Value" : age_demo["Price"].sum()})

age_anal_df["Normalized Total"] = round(age_anal_df["Total Purchase Value"]/age_demo["SN"].nunique(),2)
age_anal_df["Total Purchase Value"]=age_anal_df["Total Purchase Value"].map('${:.2f}'.format)

age_anal_df_org = age_anal_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Total"]]
age_anal_df_org
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Total</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>4.89</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Top Spenders**

# * Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
Item = purch_data_df.groupby("SN") # group purchases by Screen Name
# data frame : purchases per user, avg. purchase price/user, total purchase value/user
Item_df = pd.DataFrame({"Purchase Count" : Item["Item Name"].count(),
                       "Average Purchase Price" : Item["Price"].mean().map('${:.2f}'.format),
                        "Total Purchase Value" : Item["Price"].sum().map('${:.2f}'.format)})
# sort the data frame in decending order of Purchase count
Item_df_sorted = Item_df.sort_values("Purchase Count", ascending= False)
Item_df_sorted_org = Item_df_sorted[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
Item_df_sorted_org.head(5)

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Sondastan54</th>
      <td>4</td>
      <td>$2.56</td>
      <td>$10.24</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Most Popular Items**
# group purchase data on Item Name and Item ID
Item_demo = purch_data_df.groupby(["Item Name", "Item ID"])

popular_item_df = pd.DataFrame({"Purchase Count": Item_demo["Item Name"].count(),
                               "Total Purchase Value" : Item_demo["Price"].sum(),
                               "Item Price" : Item_demo["Price"].sum()/Item_demo["Item Name"].count()})
# sort the grouped data by Purchase Count in descending order and display the first 5 rows
popular_item_df_sorted = popular_item_df.sort_values("Purchase Count", ascending=False)
popular_item_df_sorted.head(5)



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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <th>39</th>
      <td>2.35</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <th>84</th>
      <td>2.23</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <th>34</th>
      <td>4.14</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Trickster</th>
      <th>31</th>
      <td>2.07</td>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>Serenity</th>
      <th>13</th>
      <td>1.49</td>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Most Profitable Items**

profitable_item_df = popular_item_df.sort_values("Total Purchase Value", ascending = False)
profitable_item_df.head(5)

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
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Retribution Axe</th>
      <th>34</th>
      <td>4.14</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <th>115</th>
      <td>4.25</td>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <th>32</th>
      <td>4.95</td>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>Singed Scalpel</th>
      <th>103</th>
      <td>4.87</td>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>Splitter, Foe Of Subtlety</th>
      <th>107</th>
      <td>3.61</td>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


