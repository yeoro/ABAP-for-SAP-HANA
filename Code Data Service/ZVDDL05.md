```ABAP
@AbapCatalog.sqlViewName: 'ZVSQL04_05'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Association'

// Association => spfli와 scarr를 join하는 과정에서 관계를 설정하는 것을 의미한다.
define view ZVDDL04_05 as select from spfli as a
association [1] to scarr as _Airline // Association 대상의 별칭은 _로 시작해야 함
on $projection.carrid = _Airline.carrid { // $projection 대신 spfli 혹은 a도 가능
    key carrid, // Association에서는 필드가 중복되어도 테이블을 명시하지 않아도 됨
    key connid,
    _Airline.carrname,
    _Airline // Make association public => 자신을 참조하는 뷰에서 사용하도록 선언, 자기 자신은 사용하지 않음
}
```