```ABAP
@AbapCatalog.sqlViewName: 'ZVSQL04_03'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Join'
define view ZVDDL04_03 as select from spfli as a
inner join scarr as b

    // Native SQL 이기 때문에 '.' 사용
    on a.carrid = b.carrid {
    key a.carrid,
    key a.connid,
    a.countryfr,
    a.countryto,
    b.carrname,
    
    // 조건문은 ABAP과 같이 case 사용
    case a.countryfr
        when a.countryto then 'Domestic'
        else 'International'
    end as cal_ftype
}
```