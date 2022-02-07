```ABAP
@AbapCatalog.sqlViewName: 'ZVSQL04_01' // SAP 단에서 생성(transparent table), 한 번 생성되면 이름 수정 불가능
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'CDS View'

/*
    - CDS는 ADT를 통해서만 생성 및 수정이 가능하다.
    - CDS에서 DML은 지원하지 않는다.
    - cds(ddl) view와 sql view는 이름이 달라야 한다.
*/
define view ZVCDS04_01 as select from sflight {
    
    // Key 설정
    key carrid, 
    key connid,
    key fldate,
    
    // 이미 sflight에 사용되었지만, 한 번 더 사용
    @Semantics.amount.currencyCode: 'WAERS'
    price as price_t,
    @Semantics.currencyCode: true
    currency as waers
} where carrid = 'KA'
```