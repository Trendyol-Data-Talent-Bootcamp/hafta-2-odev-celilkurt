
```SQL 
with lead_board as(
  select 
    Country,
    Sport,
    count(1) as medal_cnt
  from abdulcelil_kurt.summer_medals 
  where 
    Country is not null and 
    Sport is not null and 
    year >= 1980
  group by 
    Country,
    Sport
  order by 
    Sport, 
    medal_cnt desc
 )
select 
    sport,
    first_champ,
    third_champ,
    fifth_champ
from(
    select 
        sport,
        country as first_champ,
        lead(country,2) over(partition by sport order by medal_cnt desc) as third_champ,
        lead(country,4) over(partition by sport order by medal_cnt desc) as fifth_champ,
        ROW_NUMBER() OVER (PARTITION BY sport ORDER BY medal_cnt DESC) AS row_number
     from lead_board
     order by sport)
where
  row_number = 1
;

```

