---
title: Ishikawa Quakes boxplot
layout: post
---

```python
iquakes.shape[0]
```

```
247
```

A quick post. It's almost 8:30am, January 4th. There have been 247 quakes since the big one.

Let's see what the magnitude is like as a boxplot (for every hour):

```python
iquakes.boxplot(
    by=["date", "hour"],
    column=["mag"],
    rot=90,
    figsize=(20,20),
    ylabel="Magnitude",
)

```

![ishikawa aftershocks boxplot]({{site.url}}/assets/20240104-quakeboxplotimg.png)