



```SQL 
create or replace table dsmbootcamp.abdulcelil_kurt.content_category

as

select * from dsmbootcamp.sample.content_category;


create or replace table dsmbootcamp.abdulcelil_kurt.content_category_20201222_00_59

as

select * from dsmbootcamp.sample.content_category_20201222_00_59;


MERGE abdulcelil_kurt.content_category T
USING abdulcelil_kurt.content_category_20201222_00_59 S
ON T.id = S.id
WHEN NOT MATCHED THEN
  INSERT VALUES(S.cdc_date,S.is_deleted,S.id,S.category)
WHEN MATCHED AND S.is_deleted != T.is_deleted THEN
    UPDATE SET cdc_date = S.cdc_date,is_deleted = S.is_deleted
WHEN MATCHED AND NOT T.is_deleted AND T.cdc_date != S.cdc_date THEN
    UPDATE SET cdc_date = S.cdc_date, category = S.category
;


select count(1) = 0 as is_equals
from abdulcelil_kurt.content_category as s
inner join abdulcelil_kurt.content_category_target as t on s.id = t.id
where farm_fingerprint(to_json_string(s)) != farm_fingerprint(to_json_string(t));

```
