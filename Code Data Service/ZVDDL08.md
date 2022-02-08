```abap
@AbapCatalog.sqlViewName: 'ZVSQL04_08'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Parameters'
define view ZVDDL04_08
    with parameters p_rate: abap.dec( 5, 1 ), // 사용자가 입력한 값으로 parameters가 선언된 필드에 적용됨
                    p_carrid: s_carr_id
as select from sflight {
    key carrid,
    key connid,
    key fldate,
    price,
    price * $parameters.p_rate as price_new, // $parameters.p_rate == :p_rate
    currency
} where carrid = $parameters.p_carrid

```