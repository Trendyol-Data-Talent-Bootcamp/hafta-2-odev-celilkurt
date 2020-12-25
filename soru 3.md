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
    sum(dist_user_cnt) over(order by view_minute rows between current row and 4 following) as five_min_sum,
    view_minute || '  -  ' || TIMESTAMP_ADD(view_minute, INTERVAL 5 MINUTE) time
  from (
    select view_minute, hll_count.merge(approx_dist_user) as dist_user_cnt
    from abdulcelil_kurt.pageview_minute
    group by view_minute
    order by view_minute asc
      )
   order by five_min_sum desc
   limit 1
;
/*476612 | 2020-03-03 23:02:00+00  -  2020-03-03 23:07:00+00 */

```


