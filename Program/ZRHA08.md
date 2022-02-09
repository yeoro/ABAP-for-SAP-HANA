```ABAP
*&---------------------------------------------------------------------*
*& Report ZRHA04_08
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zrha04_08.

TRY. "예외처리
* 1. Get DB Connection : DB 연결 객체 생성 및 메소드 반환 값 할당
    DATA go_con TYPE REF TO cl_sql_connection.
    go_con = cl_sql_connection=>get_connection( ). "연결할 DB 이름 입력, 공백이면 Primary DB로 연결

* 2. SQL
    DATA go_sql TYPE REF TO cl_sql_statement.
    CREATE OBJECT go_sql
      EXPORTING
        con_ref = go_con. "연결한 DB에 SQL을 실행시킬 객체 변수 생성
    DATA gv_sql TYPE string.

* CONCATENATE ... => 이전 방법
*    gv_sql = |SELECT carrid, carrname | && "쿼리 사이에 띄어쓰기 해야함.
*             |FROM scarr | &&
*             |WHERE mandt = '{ sy-mandt }' |. "WHERE절 변수처리

    gv_sql = |SELECT carrid, carrname, connid, cal_ftype | &&
             |FROM "_SYS_BIC"."student04/AT_STD04" |.

* 3. SQL 실행 : 실행은 ABAP 단에서 하지만, 결과는 DB 단에 존재한다.
    DATA go_result TYPE REF TO cl_sql_result_set.
    go_result = go_sql->execute_query( gv_sql ). "cl_sql_connection의 methods를 사용할 때 '->'를 사용한다.

* 4. 결과 가져오기
    DATA: BEGIN OF gs_list, "결과를 받을 인터널 테이블 생성
            carrid   TYPE scarr-carrid,
            carrname TYPE scarr-carrname,
            connid TYPE spfli-connid,
            cal_ftype,
          END OF gs_list,
          gt_list LIKE TABLE OF gs_list.

    "gt_list에 결과를 바로 받을 수도 있지만, 쿼리가 변하면 gt_list의 구조도 바꿔야 하기 때문에 번거롭다.
    "이를 위해서 go_data라는 DATA 타입 객체를 만들어 쿼리 결과를 담고, 동적으로 gt_list에 값을 담는다.
    DATA go_data TYPE REF TO data.

    "try-catch를 위한 변수 선언
    DATA go_ex TYPE REF TO cx_root. "cx_sql_exception.
    DATA gv_mesg TYPE string.

    GET REFERENCE OF gt_list INTO go_data. "go_data가 gt_list를 참조한다.
    go_result->set_param_table( go_data ). "go_data가 값을 받으면 gt_list에 값이 채워진다.

    go_result->next_package( ). "실행된 결과 가져오기

* 5. 연결 종료
    go_result->close( ).

  CATCH cx_root INTO go_ex. "cx_sql_exception INTO go_ex.
    gv_mesg = go_ex->get_text( ).
    WRITE gv_mesg.
ENDTRY.

* 6. 결과 출력
cl_demo_output=>display_data( gt_list ).
```

