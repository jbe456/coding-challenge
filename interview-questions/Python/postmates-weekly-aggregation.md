# Weekly Aggregation

This question was asked by: Postmates.

Source: [Interview Query](https://www.interviewquery.com/)

## Description

Given a list of timestamps in sequential order, return a list of lists grouped by week (7 days) using the first timestamp as the starting point.

Example:

```python
ts = [
    '2019-01-01',
    '2019-01-02',
    '2019-01-08',
    '2019-02-01',
    '2019-02-02',
    '2019-02-05',
]
```

```python
output = [
    ['2019-01-01', '2019-01-02'],
    ['2019-01-08'],
    ['2019-02-01', '2019-02-02'],
    ['2019-02-05'],
]
```

## Solution 1: using reduce

```python
# Python v3.6
import functools
from datetime import datetime

ts = [
    '2019-01-01',
    '2019-01-02',
    '2019-01-08',
    '2019-02-01',
    '2019-02-02',
    '2019-02-05',
]

def getWeekNb(d):
    return datetime.strptime(d, '%Y-%m-%d').isocalendar()[1]

def reduceCb(result, newDate):
    weekNumber = getWeekNb(newDate)
    current = result[weekNumber]
    # best practice: avoid mutating array
    result[weekNumber] = ([] if current is None else current) + [newDate]
    return result

# reduce ts into an array of size 52 (= weeks of year) then filter out empty ones
result = filter(lambda x: x is not None,
             functools.reduce(reduceCb, ts, [None] * 52))
print(*result, sep='\n')
```

## Solution 2: using pandas

```python
# Python v3.6
import pandas as pd

ts = [
    '2019-01-01',
    '2019-01-02',
    '2019-01-08',
    '2019-02-01',
    '2019-02-02',
    '2019-02-05',
]

df = pd.DataFrame({'date': ts})
df['date'] = pd.to_datetime(df['date'])
dfg = df.groupby(df['date'].dt.week)
res = dfg['date'].apply(list)

print(*res.values.tolist(), sep='\n')
```
