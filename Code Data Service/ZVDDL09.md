```abap
@AbapCatalog.sqlViewName: 'ZVSQL04_09'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Parameters Use'
define view ZVDDL04_09 
with parameters p_rate : abap.dec( 5, 1 )
as select from ZVDDL04_08 (
    // ABAP에서는 값을 바로 할당하는 것이 가능하지만, CDS에서는 변환해줘야 함
    p_rate: $parameters.p_rate,
    p_carrid: 'AA'
) {
    key carrid,
    key connid,
    key fldate,
    price,
    currency,
    price_new
}

```