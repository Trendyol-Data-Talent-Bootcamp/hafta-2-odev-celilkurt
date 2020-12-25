```SQL

select 
  sum_medals.athlete
from 
  (select 
      athlete 
   from 
      abdulcelil_kurt.summer_medals 
   where
      year >= 1980
   group by 
      athlete 
   having 
      count(athlete) >= 3) as athletes
inner join 
    abdulcelil_kurt.summer_medals as sum_medals on sum_medals.athlete = athletes.athlete
group by
  athlete
order by 
  athlete
;
```
