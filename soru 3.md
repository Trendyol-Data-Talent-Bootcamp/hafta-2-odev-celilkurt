#SORU
5 dakika içerisindeki en yüksek aktif kullanıcı sayısı

-her dakika için kullanıcı sayısı hesaplanıp 'pageview_minute' tablosuna kaydedildi,
-Kayıtlar saatine göre sıralı bir şekilde 5'er 5'er toplandı.
-En yüksek sayı, herhangi bir 5 dakikalık zaman dilimindeki en yüksek aktif kullanıcı sayıdır.

-Bu tarz bir çözüm için 'Maximum subarray problem' çözümlerine bakılabilir.


```SQL 
create table abdulcelil_kurt.pageview_minute
as 
  select timestamp_trunc(pv.view_ts,hour) view_minute
        ,hll_count.init(deviceid,15) approx_dist_user
  from abdulcelil_kurt.pageview pv
  group by 1;



select max(five_min_sum)
from (
  select sum(pv_m.dist_user_cnt) over(order by pv_m.view_minute rows between 4 preceding and current row) as five_min_sum
  from (
    select view_minute, hll_count.merge(approx_dist_user) as dist_user_cnt
    from abdulcelil_kurt.pageview_minute
    group by view_minute
      )as pv_m
);
```


