# GPS Data Processing Code Snippet

```python

# UTC time to datetime with UTC timezone 
df_gps['UTC.time'] = pd.to_datetime(df_gps['UTC.time'], utc = True)

# minute rounding 
df_gps['UTC.time'] = df_gps['UTC.time'].dt.floor('min')
df_gps= df_gps.groupby(['patient_ids', 'UTC.time'])[['latitude', 'longitude', 'accuracy']].mean().reset_index()
```
