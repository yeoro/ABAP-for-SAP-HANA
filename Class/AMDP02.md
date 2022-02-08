```abap
CLASS zcl_amdp04_02 DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_amdp_marker_hdb.
    TYPES: BEGIN OF ts_list,
            carrid TYPE spfli-carrid,
            connid TYPE spfli-connid,
            carrname TYPE scarr-carrname,
            cal_ftype(20),
           END OF ts_list,
           tt_list TYPE TABLE OF ts_list.

    CLASS-METHODS get_conn IMPORTING VALUE(iv_mandt) TYPE mandt
                           EXPORTING VALUE(et_list) TYPE tt_list.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_amdp04_02 IMPLEMENTATION.
    METHOD get_conn BY DATABASE PROCEDURE FOR HDB LANGUAGE SQLSCRIPT
                    USING spfli scarr zcl_amdp04_01=>get_amdp.

        -- 참조하는 AMDP의 프로시저 호출
        call "ZCL_AMDP04_01=>GET_AMDP"( lt_scarr );

        lt_temp = select carrid, carrname
                    from scarr;

        lt_temp2 = SELECT a.carrid, a.connid, b.carrname
                    FROM spfli as a inner join :lt_scarr as b
                      ON a.carrid = b.carrid
                   WHERE a.mandt = :iv_mandt;

        et_list = select carrid, connid, carrname, cal_ftype
                    -- 같은 HANA에 존재하기 때문에 Using에 선언하지 않아도 된다.
                    from "_SYS_BIC"."student04/AT_STD04" ;
    ENDMETHOD.
ENDCLASS.
```

