#SORU
5 dakika içerisindeki en yüksek aktif kullanıcı sayısı

-her dakika için kullanıcı sayısı hesaplanıp 'pageview_minute' tablosuna kaydedildi,
-Kayıtlar saatine göre sıralı bir şekilde 5'er 5'er toplandı.
-En yüksek sayı, herhangi bir 5 dakikalık zaman dilimindeki en yüksek aktif kullanıcı sayıdır.



```SQL 
create or replace table abdulcelil_kurt.pageview_minute
as 
  select timestamp_trunc(pv.view_ts,minute) view_minute
        ,hll_count.init(deviceid,24) approx_dist_user
  from abdulcelil_kurt.pageview pv
  group by 1;


 select 
    start_time || '  -  ' || TIMESTAMP_ADD(start_time, INTERVAL 5 MINUTE) as time_range, 
    (select
        hll_count.merge(approx_dist_user)
     from
        unnest(approx_user_array) approx_dist_user) as max_count
from 
    (select 
        array_Agg(approx_dist_user) over(order by view_minute rows between current row and 4 following) as approx_user_array,
        view_minute  as start_time
     from abdulcelil_kurt.pageview_minute
     order by view_minute asc)
order by start_time;
/*2020-03-03 20:38:00+00  -  2020-03-03 20:43:00+00  ---  230302 */

```


