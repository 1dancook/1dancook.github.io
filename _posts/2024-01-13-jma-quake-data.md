---
title: An Overview of the Available Quake Data from JMA
layout: post
---

In light of the issue from the other day where I discovered that NHK had different numbers than I did (for aftershocks) I made a recent discovery.

I happened to be browsing the main page for JMA - something that I have never done simply because I bookmark certain areas of the site that I want to use frequently. The main thing I look at is the radar for my area to see if it's going to rain - and it usually does.

The home page for JMA lists a number of articles / news updates. I clicked on a random update about the Noto earthquakes expecting to see roughly information that I already knew - but it was different! To my suprise, they had reported *more* aftershocks as well. Now it was clear to me that NHK wasn't lying and they in fact had some different information from JMA.

The question I have now is *why doesn't JMA show everything in the earthquake map?*

Now, the report didn't have a table of data and it was still not obvious where to find this information. So I went digging on the Japanese version of the website. In the English version it's not possible to find this information (or, so it seems).

The following are some of the ways to access quake data that I have found.

### Epicenter Data

The [各種デ一夕 • 資料](https://www.jma.go.jp/jma/menu/menureport.html) tab gives access to a great amount of data for different domains. Under the heading **地震・津波・火山** there is a link to [震源リスト](https://www.data.jma.go.jp/eqev/data/daily_map/index.html). I think I had looked at this before but the actual list itself is buried a little. 

The page itself displays a list of months like this:

```
震源リスト
2024年01月（クリックするとリストが開閉します）
2023年12月
2023年11月
2023年10月
...
```

When you click on a month, you are given the dates of the month to click on. 

When you click on a day, you are taken to another page with a link like this: `https://www.data.jma.go.jp/eqev/data/daily_map/20240109.html`.

That page displays a map of Japan with *all* of the earthquakes for that date. This is much more than what is reported on the user-friendly interactive earthquake map. It's actually suprising.

But underneath the map, there is yet another thing to click on:

```
震源リスト
クリックするとリストが開閉します
```

After click that you are presented with a list of epicenters. Here's an entry:

```
2024  1  9 00:05 51.3  35°50.6'N 137° 8.5'E   13     0.1  岐阜県飛騨地方
```

You can see the `0.1` value - that's the recorded magnitude and this is possible why it isn't available in the user-friendly interactive earthquake map.

It wouldn't be too difficult to gather this data from each site for each day and so far I haven't found another way that these verbose observations are made available.

One discovery was that the map for the day has an easy link: 

```
https://www.data.jma.go.jp/eqev/data/daily_map/20240109japan.png
```

There are also different parts you can click on the map as revealed in the HTML but these lead to close-ups of the map, not any more data.

The observation data is baked into the page itself inside a `<pre>` HTML tag. It's not even in a structured format - it's almost like the output of some kind of script just pasted into the HTML.

It's not complicated to grab that data, but, ugh.

Another issue is that this data is limited. At the time of writing it goes back to sometime in 2022, and is only posted up to two days ago.

### Intensity Database

The url:

```
https://www.data.jma.go.jp/eqdb/data/shindo/index.html
```

This provides a map and form interface to see earthquakes for any range of time dating back to 1923.

Digging into the site, it seems entirely possible to use the API and get a JSON response. This is nice but I wondered if it gives *all* of the results even those that are very small earthquakes (i.e. M0.1).

After checking just briefly, I don't think it does. Each dot on the map has a recorded intensity and it's possible to filter the intensity in the form by anything *more than or equal to* 1. So it seems that anything quake that wasn't strong enough to register an intensity level isn't going to be in this data.

That's unfortunate.

### XML Data

There is XML data available from JMA. This gives an overview of what is available, but no links:

```
https://www.data.jma.go.jp/add/suishin/cgi-bin/catalogue/make_product_page.cgi?id=Jishin
```

There is another page [http://xml.kishou.go.jp/xmlpull.html](http://xml.kishou.go.jp/xmlpull.html) that has links to ATOM feeds:

- [High frequency feed](https://www.data.jma.go.jp/developer/xml/feed/eqvol.xml) is updated every minute and the most recent incoming call for at least 10 minutes is posted.
- [Long-term feed](https://www.data.jma.go.jp/developer/xml/feed/eqvol_l.xml) is updated every hour, and all incoming calls for several days are posted.

Taking a look at the high frequency feed, there are numeries entries for both earthquakes and volcanic activity. Here's an earthquake entry:

```XML
<entry>
    <title>震源・震度に関する情報</title>
    <id>
    https://www.data.jma.go.jp/developer/xml/data/20240110230013_0_VXSE53_010000.xml
    </id>
    <updated>2024-01-10T23:00:13Z</updated>
    <author>
        <name>気象庁</name>
    </author>
    <link type="application/xml" href="https://www.data.jma.go.jp/developer/xml/data/20240110230013_0_VXSE53_010000.xml"/>
    <content type="text">【震源・震度情報】１１日０７時５７分ころ、地震がありました。</content>
</entry>
```

Exploring one of the linked xml files it has earthquake information like this:

```XML
<Earthquake>
    <OriginTime>2024-01-11T07:57:00+09:00</OriginTime>
    <ArrivalTime>2024-01-11T07:57:00+09:00</ArrivalTime>
    <Hypocenter>
        <Area>
            <Name>石川県能登地方</Name>
            <Code type="震央地名">390</Code>
            <jmx_eb:Coordinate description="北緯３７．５度　東経１３７．２度　深さ　１０ｋｍ" datum="日本測地系">+37.5+137.2-10000/</jmx_eb:Coordinate>
        </Area>
    </Hypocenter>
    <jmx_eb:Magnitude type="Mj" description="Ｍ３．４">3.4</jmx_eb:Magnitude>
</Earthquake>
```

Along with intensity observations:

```XML
<Intensity>
    <Observation>
    <CodeDefine>
        <Type xpath="Pref/Code">地震情報／都道府県等</Type>
        <Type xpath="Pref/Area/Code">地震情報／細分区域</Type>
        <Type xpath="Pref/Area/City/Code">気象・地震・火山情報／市町村等</Type>
        <Type xpath="Pref/Area/City/IntensityStation/Code">震度観測点</Type>
    </CodeDefine>
    <MaxInt>1</MaxInt>
    <Pref>
        <Name>石川県</Name>
        <Code>17</Code>
        <MaxInt>1</MaxInt>
        <Area>
            <Name>石川県能登</Name>
            <Code>390</Code>
            <MaxInt>1</MaxInt>
            <City>
                <Name>珠洲市</Name>
                <Code>1720500</Code>
                <MaxInt>1</MaxInt>
                <IntensityStation>
                    <Name>珠洲市三崎町</Name>
                    <Code>1720500</Code>
                    <Int>1</Int>
                </IntensityStation>
            </City>
        </Area>
    </Pref>
    </Observation>
</Intensity>
```

The long feed has a lot of information. The question is - does it contain *more* information than the user-friendly map interface?

I can load it up in pandas with `pd.read_xml`. It takes a URL which is useful, but it's important to specify an xpath to all of the entries:

```python
import pandas as pd

namespaces = {'atom': 'http://www.w3.org/2005/Atom'}

# Define the XPath expression
xpath_expression = "//atom:entry"

# Read XML using pandas read_xml
df = pd.read_xml(
    "https://www.data.jma.go.jp/developer/xml/feed/eqvol_l.xml",
    xpath=xpath_expression,
    namespaces=namespaces
    )

# Filter only earthquake info:
df = df.loc[df.title == "震源・震度に関する情報"]

# Sort by date/time
df = df.sort_values("updated")
```

I'm accessing this on January 11, and the first entry is dated `2024-01-03T23:08:30Z` the last entry is dated `2024-01-10T23:00:13Z` and there are a total of **334** rows. That means this feed definitely does *not* contain verbose data. It's actually specifically for situations that may require some kind of emergency response.

### Earthquake Information (Interactive Map)

This can be found here:

```
https://www.jma.go.jp/bosai/map.html#11/&elem=int&contents=earthquake_map
```

There is a json file provided that I have used to gather the information I tried to analyze before. This is limited to information within one month prior to the current date, and there is some kind of threshold applied to the observations. I believe the threshold is related to measured intensity -- if the quake isn't strong enough to register an intensity level *on land* it will not be included.

One issue I have noticed with this data is that it has a problem with the GPS coordinates of the quake and the depth measurement. The GPS and depth measurement in the epicenter datasets is more accurate and it's possible they have done some post-processing and manual checking in order to get better results.

## Summary

There are a few sources:

1. Epicenter Data (unfiltered, limited to start at ~2022)
2. Intensity Database (all data, filtered)
2. XML (only emergency)
4. Interactive Map (1 month of data, filtered)

The best approach to analyze recent data may be to combine both the epicenter data (1) and the intensity database data (2). It would require identifying the `eid` and then doing some merging, and etc. 

To analyze most recent data the interactive map data (4) is best - but it simply doesn't contain anything. I believe this is meant for people to look at so they can see why their house is shaking.
