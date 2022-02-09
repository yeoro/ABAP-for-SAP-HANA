```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_10
*&---------------------------------------------------------------------*
*& HANA Procedure 호출
*&---------------------------------------------------------------------*
REPORT zrha04_10.

TRY.
    DATA go_con TYPE REF TO cl_sql_connection.

    DATA go_sql TYPE REF TO cl_sql_statement.
    CREATE OBJECT go_sql
      EXPORTING
        con_ref = go_con.
    DATA gv_sql TYPE string.

    DATA go_result TYPE REF TO cl_sql_result_set.

    DATA: BEGIN OF gs_list,
            param TYPE string, "variable
            value TYPE string, "table
          END OF gs_list,
          gt_list LIKE TABLE OF gs_list.

    DATA: BEGIN OF gs_data,
            id   TYPE scustom-id,
            name TYPE scustom-name,
          END OF gs_data,
          gt_data LIKE TABLE OF gs_data.
    DATA go_data TYPE REF TO data.

    DATA go_ex TYPE REF TO cx_root.
    DATA gv_mesg TYPE string.

* 1. 프로시저 호출 후 임시테이블 가져오기
    go_con = cl_sql_connection=>get_connection( ).

    gv_sql = |call "_SYS_BIC"."ha400.primdb/SP_EARLY_BIRD_AND_LAST_MINUTE"| &&
             |( '5', '{ sy-mandt }', null, null ) | &&
             |with overview |.

    go_result = go_sql->execute_query( gv_sql ).

    GET REFERENCE OF gt_list INTO go_data.
    go_result->set_param_table( go_data ).
    go_result->next_package( ).

    cl_demo_output=>display_data( gt_list ).

* 2. 가져온 임시테이블에서 값 꺼내기
    CLEAR gs_list.
    READ TABLE gt_list WITH KEY param = 'ET_LATE' INTO gs_list.

    CLEAR gv_sql.
    gv_sql = |SELECT id, name | &&
             |FROM { gs_list-value } |.

    go_result = go_sql->execute_query( gv_sql ).

    GET REFERENCE OF gt_data INTO go_data.
    go_result->set_param_table( go_data ).
    go_result->next_package( ).

    cl_demo_output=>display_data( gt_data ).

* 3. 연결 종료
    go_result->close( ).

  CATCH cx_root INTO go_ex.
    gv_mesg = go_ex->get_text( ).
    WRITE gv_mesg.
ENDTRY.
```

