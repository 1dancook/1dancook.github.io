---
title: Wrong NHK Numbers about Ishikawa Quakes?
layout: post
---

Earlier this morning I check NHK's website for any updates about Ishikawa and the post-quake situation. I came across one sentence in an article that immediately stood out to me since I have been wrangling the quake data from JMA in pandas for the past few days. Something didn't seem right.

Here's the section from [the article](https://www3.nhk.or.jp/news/html/20240105/k10014309461000.html):

> 今月1日午後4時10分ごろに発生した能登半島地震では、石川県の志賀町で震度7の激しい揺れを観測したほか、震度6強の揺れを七尾市と輪島市、珠洲市、穴水町で観測しました。  
> 
> また、新潟県と石川県、富山県、福井県、長野県、岐阜県で震度6弱から5弱を観測しました。  
> 
> **能登地方やその周辺を震源とする地震はその後も相次ぎ、震度1以上の揺れを観測した地震は5日午前4時までに786回にのぼっています。**
> 
> 気象庁は、揺れの強かった地域では、家屋の倒壊や土砂災害などの危険性が高まっているため、1日の地震から1週間ほどは最大震度7程度の揺れに注意するよう呼びかけています。

A rough English translation of the sentence in question: *"Earthquakes with an epicenter in and around the Noto region have continued after that, and earthquakes with a seismic intensity of 1 or more have occured 786 times up to 4 a.m. on the 5th."*

Yesterday morning when I had checked according to filtered data from the JMA the number was **247** - that was about 8:30am, January 4th.

The number is so different that it made me wonder if I had something wrong.

There are a few possibilities:

1. Something about my method was flawed.
2. JMA is providing NHK with data that is not publicly available.
3. NHK has made an error in how they analyzed data.

The first thing that I checked was the actual number of entries in the dataset, starting from January 1st at 4:09pm until now (current time is 8:56pm, January 5th). It's reasonable that the total number of entries would now be more than 768. Keep in mind that there are a number of other things in that data - earthquakes outside of Japan, outside of Noto and the surrounding area, "information" (which is not information about an epicenter), etc. Those are things that I filtered out before because they are not relevant to analyzing quake data from Noto peninsula.

Here's the code that I ran:

```python
import pandas as pd
import datetime

# the json url
quakes_json_url = "https://www.jma.go.jp/bosai/quake/data/list.json"

# load it into a dataframe
quakes = pd.read_json(quakes_json_url)

# convert to datetime but first will drop the offset
quakes["at"] = quakes["at"].apply(lambda x: x.replace("+09:00", "").replace("T", " "))
quakes["at"] = pd.to_datetime(quakes["at"])

# change the 'at' column to date_time
quakes = quakes.rename(columns={"at": "date_time"})

# make a mask to have only those entries with a date_time starting after 4:09pm, January 1st
dt_mask = quakes.date_time > datetime.datetime.fromisoformat("2024-01-01 16:09")

# Apply the mask and get the number of entries
quakes.loc[dt_mask].shape[0]
```

**Result:**

```text
687
```

*Note: the data available in that JSON file is limited to one month prior to the current date - so if you are trying this in the distant future it won't work.*

*Note: I didn't account for duplicated data, and there are some where the `eid` is the same.*

**I don't even need to dig any further.** There just isn't enough data to support the claim from NHK.

Well, just to show in more detail what kind of entries there are -- there are several kinds of "titles" (column = `ttl`) in the dataset. Here are the value counts for each with the same `date_time` mask applied from above:

```python
quakes.loc[dt_mask].ttl.value_counts()
```

**Result:**

```text
震源・震度情報                         537
震度速報                              105
震源に関する情報                        41
顕著な地震の震源要素更新のお知らせ          4
```

I see a large number of actual epicenter entries (537). Let's see where those might be. The `en_anm` column is English area name. Let's see the value counts for those, but just the epicenter:

```python
quakes.loc[dt_mask & (quakes.ttl == "震源・震度情報")].en_anm.value_counts()
```

**Result:**

```text
Noto, Ishikawa Prefecture                                  318
Off the Coast of Noto Peninsula                            145
Adjacent Sado                                               35
Off the Coast of Joetsu and Chuetsu, Niigata Prefecture     18
Off the west Coast of Ishikawa Prefecture                    6
Toyama Bay                                                   4
Northwestern Chiba Prefecture                                1
Adjacent Sea of Amami-Oshima Island                          1
Off the Coast of Iwate Prefecture                            1
Northern Wakayama Prefecture                                 1
Adjacent Sea of Ishigakijima Island                          1
Northern Ibaraki Prefecture                                  1
Adjacent Sea of Tokara Islands                               1
Northern Miyagi Prefecture                                   1
Eastern Region · Fuji Five Lakes, Yamanashi Prefecture       1
Off the east Coast of Osumi Peninsula                        1
Iyonada Sea                                                  1
```

Just to double check another thing - each area has a code. I had been using `390` since that seems to have been the code for Noto. Let's check the counts for those codes according to the English name, with the same filter from the above:

```python
quakes.loc[dt_mask & (quakes.ttl == "震源・震度情報")].groupby(["acd", "en_anm"]).en_anm.count()
```

**Result:**

```text
acd  en_anm
220  Northern Miyagi Prefecture                                   1
286  Off the Coast of Iwate Prefecture                            1
300  Northern Ibaraki Prefecture                                  1
341  Northwestern Chiba Prefecture                                1
379  Off the Coast of Joetsu and Chuetsu, Niigata Prefecture     18
390  Noto, Ishikawa Prefecture                                  318
412  Eastern Region · Fuji Five Lakes, Yamanashi Prefecture       1
494  Off the west Coast of Ishikawa Prefecture                    6
495  Off the Coast of Noto Peninsula                            145
497  Toyama Bay                                                   4
498  Adjacent Sado                                               35
550  Northern Wakayama Prefecture                                 1
680  Iyonada Sea                                                  1
793  Adjacent Sea of Amami-Oshima Island                          1
798  Adjacent Sea of Tokara Islands                               1
820  Off the east Coast of Osumi Peninsula                        1
854  Adjacent Sea of Ishigakijima Island                          1
```

I'm a little rusty on my Japanese geography, but I do know that at least 6 of these entries are relevant:

```text
acd  en_anm
390  Noto, Ishikawa Prefecture                                  318
495  Off the Coast of Noto Peninsula                            145
379  Off the Coast of Joetsu and Chuetsu, Niigata Prefecture     18
498  Adjacent Sado                                               35
494  Off the west Coast of Ishikawa Prefecture                    6
497  Toyama Bay                                                   4
```

Adding those together, I get a total of **536**. The counts from other areas are so small they make little difference. Remember, NHK's number was *786* -- and that was almost 16 hours ago.

### Wrapping up

Now with this information I *do* understand that my method of just checking for quakes from `390` was not adequate since there are other areas that are related and likely have an impact (as far as an aftershock is concerned). I believe part of my misunderstanding was due to how those codes work- and this bit of work from above helps to make that more clear.

In a follow up at some point it would be good to actually use coordinates as a window. Another interesting angle would be to see how many earthquakes had a registered intensity on Noto peninsula.

However, the number here is less than what NHK reported. It seems there was either an error on NHK's part, or JMA shares non-public data with them - but it can't be an intentional error... the government run media would never lie to us.
