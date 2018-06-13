

```python
#import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
%matplotlib inline
```


```python
#import data
city_data = pd.read_csv('city_data.csv')
ride_data = pd.read_csv('ride_data.csv')
joined_df = pd.merge(city_data, ride_data, how = "outer", on = 'city')
joined_df.head()
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
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Bubble Plot
urbanCities=joined_df[joined_df["type"]=="Urban"]
suburbanCities=joined_df[joined_df["type"]=="Suburban"]
ruralCities=joined_df[joined_df["type"]=="Rural"]

urbanRideCount=urbanCities.groupby(["city"]).count()["ride_id"]
urbanFareAvg=urbanCities.groupby(["city"]).mean()["fare"]
urbanDriverCount=urbanCities.groupby(["city"]).mean()["driver_count"]

suburbanRideCount=suburbanCities.groupby(["city"]).count()["ride_id"]
suburbanFareAvg=suburbanCities.groupby(["city"]).mean()["fare"]
suburbanDriverCount=suburbanCities.groupby(["city"]).mean()["driver_count"]

ruralRideCount=ruralCities.groupby(["city"]).count()["ride_id"]
ruralFareAvg=ruralCities.groupby(["city"]).mean()["fare"]
ruralDriverCount=ruralCities.groupby(["city"]).mean()["driver_count"]

col_list = ["lightskyblue", "gold", "lightcoral"]

plt.scatter(urbanRideCount, 
            urbanFareAvg, 
            s=10*urbanDriverCount,
            edgecolor="black", linewidths=1, color=col_list[2], marker="o",
            alpha=0.8, label="Urban")

plt.scatter(suburbanRideCount, 
            suburbanFareAvg, 
            s=10*urbanDriverCount,
            edgecolor="black", linewidths=1, color=col_list[1], marker="o",
            alpha=0.8, label="Suburban")

plt.scatter(ruralRideCount, 
            ruralFareAvg, 
            s=10*urbanDriverCount,
            edgecolor="black", linewidths=1, color=col_list[0], marker="o",
            alpha=0.8, label="Rural")

plt.title("Pyber Ride Sharing Data")
plt.ylabel("Average Fare")
plt.xlabel("Total Number of Rides")
plt.xlim((0,40))
plt.grid(True)


lgnd = plt.legend(fontsize="small", mode="Expanded", 
                  numpoints=1, scatterpoints=1, 
                  loc="best", title="City Types", 
                  labelspacing=0.5)
lgnd.legendHandles[0]._sizes = [30]
lgnd.legendHandles[1]._sizes = [30]
lgnd.legendHandles[2]._sizes = [30]

plt.show()

```


![png](output_2_0.png)



```python
#Total Fares by City Type Pie Chart
typeFare = joined_df.groupby('type')["fare"].sum().reset_index()
fig1, ax1 = plt.subplots()
ax1.pie(typeFare["fare"], labels = typeFare["type"], shadow = True, explode = (0,0,0.1), 
        startangle=90, colors= col_list, autopct = "%1.1f%%",)
ax1.axis('equal')
plt.title('Fares by City Type', fontsize = 14).axes.get_yaxis().set_visible(False)
plt.show()
```


![png](output_3_0.png)



```python
#Total Rides by City Type Pie Chart
typeRide = joined_df.groupby('type')["ride_id"].count().reset_index()
fig1, ax1 = plt.subplots()
ax1.pie(typeRide["ride_id"], labels = typeDriver["type"], shadow = True, explode = (0,0,0.1), 
        startangle=90, colors= col_list, autopct = "%1.1f%%")
ax1.axis('equal')
plt.title('Rides by City Type', fontsize = 14).axes.get_yaxis().set_visible(False)
plt.show()
```


![png](output_4_0.png)



```python
#Total Drivers by City Type Pie Chart
typeDriver = joined_df.groupby('type')["driver_count"].sum().reset_index()
fig1, ax1 = plt.subplots()
ax1.pie(typeDriver["driver_count"], labels = typeDriver["type"], shadow = True, explode = (0,0,0.1), 
        startangle=90, colors= col_list, autopct = "%1.1f%%")
ax1.axis('equal')
plt.title('Drivers by City Type', fontsize = 14).axes.get_yaxis().set_visible(False)
plt.show()
```


![png](output_5_0.png)


3 Observable Trends:
1. Average fare is higher in rural cities.
2. Highest number of riders are in urban cities.
3. Highest number of drivers are in urban cities.
