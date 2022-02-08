```abap
@AbapCatalog.sqlViewAppendName: 'ZVSQL04_01_EX'
@EndUserText.label: 'Extension'
extend view ZVCDS04_01 with ZVCDS04_01_EX {
    seatsocc // Extension view에서 선언한 필드는 원래 view에서 선언할 수 없다.
}
```