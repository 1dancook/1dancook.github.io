---
layout: post
title: Some Analysis of Ishikawa Earthquakes
tags: python code pandas earthquakes japan
---

There are still a number of afterschocks (current time is almost 9am, January 3rd). But, are the aftershocks keeping at the same pace?

Continuing the code from a previous post and using the same `quakes` dataframe, I first filter by time (everything since 4pm on January 1) and the location (Ishikawa prefecture, code `390`).


```python
# make a datetime mask
start = datetime.datetime.fromisoformat("2024-01-01 16:00") # 4pm, new years
dt_mask = (quakes.date_time > start)

# make a location mask
location_mask = quakes.area_code == 390

# filter the dataframe and make a copy (so we're not dealing with a dataframe slice later)
iquakes = quakes[location_mask & dt_mask].copy()

# Then make a few more columns - just day and hour will be enough
iquakes["date"] = iquakes.date_time.dt.date
iquakes["hour"] = iquakes.date_time.dt.hour

# We'll also get the total number
total_quakes = iquakes.shape[0]
```


```python
iquakes.groupby(["date", "hour"]).date_time.count().plot(
    kind="bar",
    title=f"Number of Earthquakes per hour in Ishikawa (202401011600-202401030900); Total: {total_quakes}",
    figsize=(15,8),
    grid=True,
    ylabel="Count"
)
```

![png]({{site.url}}/assets/20240103-quakesperhour.png)

It looks like there were quite a few quakes last night between 2-5am.

Using the same grouping idea, this time lets see the average magnitude per hour.


```python
iquakes.groupby(["date", "hour"]).mag.mean().plot(
    kind="bar",
    title=f"Mean Magnitude of Quakes per hour in Ishikawa (202401011600-202401030900)",
    figsize=(15,8),
    grid=True,
    ylabel="Mean"
)
```


![png]({{site.url}}/assets/20240103-meanmagperhour.png)


But, the actual intensity is important as well. This is a measurement used in Japan that gives people a good idea of how intense the earthquake was. The scale ranges from 1 to 7, 7 being the worst. But it has a few other things like 5 Lower, 5 Upper, 6 Lower, 6 Upper. So really in total there are 9 possible values.

This kind of information gives you a better idea of what kind of impact a quake may have.

The data can be grouped by the intensity and from there it's trivial to get descriptive statistics:


```python
intensity_index = ["1","2","3","4","5-","5+","6-","6+","7"]
iquakes.groupby("max_intensity").mag.describe().reindex(intensity_index)
```



<div class="dataframe-container">
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>max_intensity</th>
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
      <th>1</th>
      <td>56.0</td>
      <td>2.837500</td>
      <td>0.385917</td>
      <td>1.9</td>
      <td>2.575</td>
      <td>2.9</td>
      <td>3.025</td>
      <td>3.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>59.0</td>
      <td>3.359322</td>
      <td>0.449566</td>
      <td>2.4</td>
      <td>3.100</td>
      <td>3.3</td>
      <td>3.700</td>
      <td>4.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>44.0</td>
      <td>3.822727</td>
      <td>0.447662</td>
      <td>2.8</td>
      <td>3.575</td>
      <td>3.8</td>
      <td>4.200</td>
      <td>4.8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>4.547059</td>
      <td>0.291800</td>
      <td>4.0</td>
      <td>4.400</td>
      <td>4.5</td>
      <td>4.700</td>
      <td>5.2</td>
    </tr>
    <tr>
      <th>5-</th>
      <td>2.0</td>
      <td>5.100000</td>
      <td>0.707107</td>
      <td>4.6</td>
      <td>4.850</td>
      <td>5.1</td>
      <td>5.350</td>
      <td>5.6</td>
    </tr>
    <tr>
      <th>5+</th>
      <td>4.0</td>
      <td>5.625000</td>
      <td>0.457347</td>
      <td>5.0</td>
      <td>5.525</td>
      <td>5.7</td>
      <td>5.800</td>
      <td>6.1</td>
    </tr>
    <tr>
      <th>6-</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6+</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1.0</td>
      <td>7.600000</td>
      <td>NaN</td>
      <td>7.6</td>
      <td>7.600</td>
      <td>7.6</td>
      <td>7.600</td>
      <td>7.6</td>
    </tr>
  </tbody>
</table>
</div>



You will notice that there is no `6-` or `6+`. Those intensities *did* occur but they were at the same time as the M7.6 quake - and this is showing the maximum intensity per quake, and for that one it was 7. In other words, there were no quakes that had a maximum intensity of `6-` or `6+`, at least within the last few days.

A few other interesting things to note:

- There are slightly more quakes that result in a level 2 intensity
- Quakes that could potentially result in damage (`5+` or greater intensity) are not many -- 6 in total.
- There were 4 quakes that resulted in 5 Upper intensity - depending on where those occured, that could have caused further damage to already broken houses.

Probably a particularly useful and informative data point would now require loading all of the attached JSON files, flattening the data, and getting the maximum intensity for each city or station. Then it would be possible to see which cities may have been affected the worst. Those cities need aid.

That makes me wonder if agencies are using that kind of aggregated information or are strictly relying on other data like phone-ins, etc.
