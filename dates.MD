# Dates and Times

## Converting a date string to a datetime

```python
df = pd.DataFrame({
    'date': ['July 10, 2020', 'May 1, 2020']
})

df['datetime'] = pd.to_datetime(df['date'])
df['month'] = df['datetime'].apply(lambda dt: dt.month)
```

```
            date   datetime  month
0  July 10, 2020 2020-07-10      7
1    May 1, 2020 2020-05-01      5
```
