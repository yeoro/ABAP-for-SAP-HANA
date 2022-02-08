```ABAP
@AbapCatalog.sqlViewName: 'ZVSQL04_04'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Currency'
define view ZVDDL04_04 as select from sbook {
    key carrid,
    key connid,
    key fldate,
    key bookid,
    customid,
    loccurkey,
    loccuram,
    
    /* 
     - CDS에서 사용하는 통화 변환 메소드
     - F1 도움말에 나오는 파라미터 사용
    */
    currency_conversion(
        amount => loccuram,
        source_currency => loccurkey,
        
        // 'EUR'은 char로 인식되기 때문에, cuky로 변환해줘야 함
        target_currency => cast ( 'EUR' as abap.cuky( 5 ) ),
        exchange_rate_date => fldate,
        error_handling => 'SET_TO_NULL'
    ) as loccuram_eur @<Semantics.amount.currencyCode: 'TARGET_WAERS',
    
    // 위와 마찬가지로 char 타입을 cuky 타입으로 변환
    @Semantics.currencyCode: true
    cast ( 'EUR' as waers ) as target_waers
    
} where carrid = 'AA'
    and connid = '0017'
    and fldate = '20180221'

```