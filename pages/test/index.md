<script>
    import Timeline from '$lib/Timeline.svelte';
</script>

This is a test.

[test2](test/test2)

```sleeps
select
  bedtime_start as start,
  total_sleep_duration/60/60 as total
-- from raw.sleeps
from read_csv_auto('../../sources/sleeps.csv')
where start > '2023-01-01'
``` 

```sleeps2
select
  *
-- from raw.sleeps
from read_csv_auto('../../sources/sleeps.csv')
where bedtime_start > '2025-07-08'
```

<Timeline 
   data={sleeps2}
   height=80 
   legend
   min=1
   max="YYYY-MM-DD"
/>

Sometimes you need something *bigger*: 
<BigValue data={sleeps} value=total />

<BarChart
    title='Total sleep'
    subtitle='Hours'
    data={sleeps}
    x=start
    y=total
/>
