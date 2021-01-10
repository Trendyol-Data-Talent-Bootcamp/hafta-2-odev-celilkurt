```SQL

with multi_medal_athletes as(
  select 
      athlete 
   from 
      abdulcelil_kurt.summer_medals 
   where
      year >= 1980
   group by 
      athlete 
   having 
      count(athlete) >= 3
)
select 
  distinct athlete,
  year_one,
  year_two,
  year_three
from(
  select 
    multi_medal_athletes.athlete as athlete,
    lead(year,0) over(partition by sum_medals.athlete order by year) as year_one,
    lead(year,1) over(partition by sum_medals.athlete order by year) as year_two,
    lead(year,2) over(partition by sum_medals.athlete order by year) as year_three
  from 
     abdulcelil_kurt.summer_medals as sum_medals
  inner join multi_medal_athletes on multi_medal_athletes.athlete = sum_medals.athlete
  order by 
    athlete)
where year_one = year_two-4 and year_two = year_three-4

;

```
