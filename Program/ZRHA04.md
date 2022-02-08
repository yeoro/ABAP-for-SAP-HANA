```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_03
*&---------------------------------------------------------------------*
*& ZVDDL04_05를 Open SQL로 구현
*&---------------------------------------------------------------------*
REPORT zrha04_04.

DATA go_salv TYPE REF TO cl_salv_table.
DATA BEGIN OF gs_list.
        INCLUDE TYPE zvddl04_05.
        DATA: currcode TYPE scarr-currcode,
     END OF gs_list,
     gt_list LIKE TABLE OF gs_list.

* Association을 사용하기 위해서는 New Open SQL 사용해야 함
START-OF-SELECTION.
  SELECT carrid, connid, carrname, \_airline-currcode "Association Field
    FROM zvddl04_05
    INTO CORRESPONDING FIELDS OF TABLE @gt_list.

* 클래스=>스태틱메소드
  cl_salv_table=>factory(
    IMPORTING
      r_salv_table = go_salv
    CHANGING
      t_table = gt_list
  ).
  go_salv->display( ).
```

