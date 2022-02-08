```abap
* ADT에서 class를 생성하면 내부는 ABAP처럼 되어있기 때문에 'like ABAP source code'라고도 한다.
CLASS zcl_amdp04_01 DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    "이 클래스를 AMDP 클래스로 사용하려면 Interface를 상속받아야 한다.
    INTERFACES if_amdp_marker_hdb.

    TYPES: BEGIN OF ts_list, "local structure 타입
            carrid TYPE scarr-carrid,
            carrname TYPE scarr-carrname,
           END OF ts_list,
           tt_list TYPE TABLE OF ts_list.

    METHODS get_data IMPORTING iv_carrid TYPE s_carr_id
                     EXPORTING et_list TYPE spfli_tab.

    "HANA 영역의 메소드는 call by reference이므로 call by value인 SAP에 맞춰줘야 한다.
    METHODS get_amdp EXPORTING value(et_list) TYPE tt_list.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_amdp04_01 IMPLEMENTATION.
    METHOD get_data.
        SELECT *
          FROM spfli
          INTO CORRESPONDING FIELDS OF TABLE et_list
         WHERE carrid = iv_carrid.
    ENDMETHOD.

    "이 메소드는 일반적인 메소드가 아니라, HANA DB에 프로시져를 만들기 위해 쓸 것이다."
    METHOD get_amdp BY DATABASE PROCEDURE FOR HDB LANGUAGE SQLSCRIPT
                    "사용하지 않는 테이블을 선언할 경우 에러, 꼭 필요한 것만 사용
                    USING scarr.

       -- 위에 ts_list에 정의된 필드 외의 필드를 select할 경우 오류
       -- 필드 순서를 지켜야 함
       -- AMDP에서는 mandt 처리가 필수
       et_list = select carrid, carrname
                   from scarr;
    ENDMETHOD.
ENDCLASS.
```

