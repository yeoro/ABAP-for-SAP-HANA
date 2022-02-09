```sql
-- hana view : 기존에 구성된 필드 + calculation view
-- view : output 1개
-- procedure(function) : output/input 여러 개

-- "스탠다드 스키마"."패키지명(대소문자 구분)/프로시저명"( 파라미터 (input, input, output, output) )
call "_SYS_BIC"."ha400.primdb/SP_EARLY_BIRD_AND_LAST_MINUTE"( '5', '001', null, null ); -- ABAP에서는 결과를 1개만 받을 수 있어 결과가 여러 개면 모두 받지 못할 수 있다. 

call "_SYS_BIC"."ha400.primdb/SP_EARLY_BIRD_AND_LAST_MINUTE"( '5', '001', null, null ) with overview; -- 결과를 갖는 임시테이블을 생성하고, ABAP에서 이 임시테이블을 호출한다.

-- Full Text Search
-- score() => 검색하려는 키워드와 근접하면 높은 값
select id, name, score() as score from scustom where contains(name, 'sch', fuzzy(0.5));
```

