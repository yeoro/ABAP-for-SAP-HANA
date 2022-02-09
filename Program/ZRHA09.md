```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_09
*&---------------------------------------------------------------------*
*& HANA Calculation View 호출
*&---------------------------------------------------------------------*
REPORT zrha04_09.

TRY.
    DATA go_con TYPE REF TO cl_sql_connection.

    DATA go_sql TYPE REF TO cl_sql_statement.
    CREATE OBJECT go_sql
      EXPORTING
        con_ref = go_con.
    DATA gv_sql TYPE string.

    DATA go_result TYPE REF TO cl_sql_result_set.

    DATA: BEGIN OF gs_list,
            id             TYPE scustom-id,
            name           TYPE scustom-name,
            avg_days_ahead TYPE i,
          END OF gs_list,
          gt_list LIKE TABLE OF gs_list.
    DATA go_data TYPE REF TO data.

    DATA go_ex TYPE REF TO cx_root.
    DATA gv_mesg TYPE string.

* 1. Get DB Connection
    go_con = cl_sql_connection=>get_connection( ).

* 2. SQL
    gv_sql = |SELECT id, name, sum("AVG_DAYS_AHEAD") AS "AVG_DAYS_AHEAD" | &&
             |FROM "_SYS_BIC"."ha400.primdb/CA_CUST_WITH_AVG_DAYS_AHEAD" | &&
*             |('PLACEHOLDER' = ('$$FLIGHTS_BEFORE$$', '20220209')) | &&
             |('PLACEHOLDER' = ('$$FLIGHTS_BEFORE$$', '{ sy-datum }')) | && "파라미터 처리
             |GROUP BY "ID", "NAME" |.

* 3. SQL 실행
    go_result = go_sql->execute_query( gv_sql ).

* 4. 결과 가져오기
    GET REFERENCE OF gt_list INTO go_data.
    go_result->set_param_table( go_data ).
    go_result->next_package( ).

* 5. 연결 종료
    go_result->close( ).

  CATCH cx_root INTO go_ex.
    gv_mesg = go_ex->get_text( ).
    WRITE gv_mesg.
ENDTRY.

* 6. 결과 출력
cl_demo_output=>display_data( gt_list ).
```

