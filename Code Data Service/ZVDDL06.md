```ABAp
@AbapCatalog.sqlViewName: 'ZVSQL04_06'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Association Use'
define view ZVDDL04_06 as select from ZVDDL04_05 { // Nested View => 테이블이 아닌 뷰를 참조하는 것
    key carrid,
    key connid,
    carrname,
    _Airline.currcode
}

```