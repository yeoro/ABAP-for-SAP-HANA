```abap
@AbapCatalog.sqlViewName: 'ZVSQL04_07'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Association 0 : N'
define view ZVDDL04_07 as select from scarr 
association [*] to spfli as _Connections // [*] => spfli 결과로 여러건이 있다
on $projection.carrid = _Connections.carrid {
    key carrid,
    carrname,
    
    // 필터에 맞지 않는 결과는 null 값이므로 조건을 많이 해줘야 한다. 일대다 관계는 사용할 때 고민을 해야 함.
    _Connections[ carrid = 'AA' and connid = '0017' ].countryfr as countryfr,
    
    _Connections
}

```