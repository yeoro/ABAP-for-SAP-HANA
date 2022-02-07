```ABAP
@AbapCatalog.sqlViewName: 'ZVSQL04_02'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'DDL View'
define view ZVDDL04_02 as select from scustom {
    id,
    form,
    name,
    street,
    postcode,
    city,
    country,
    postbox,
    custtype
} where postbox <> ' '
/*
    - union : 중복 제거 / union all : 중복 허용
    - union 사용시 양쪽 테이블의 필드가 같아야 함
*/
union 
select from scustom {
    id,
    form,
    name,
    street,
    postcode,
    city,
    country,
    postbox,
    custtype
} where custtype = 'B'
```

