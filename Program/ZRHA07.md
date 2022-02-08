```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_03
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zrha04_07.

DATA gt_list TYPE zcl_amdp04_02=>tt_list. "local => static(global)
DATA go_salv TYPE REF TO cl_salv_table.

START-OF-SELECTION.
  "AMDP에서 class-method로 선언했기 때문에 객체를 안 만들어도 된다.
  zcl_amdp04_02=>get_conn(
    EXPORTING
      iv_mandt = sy-mandt
    IMPORTING
      et_list = gt_list
  ).

  cl_salv_table=>factory(
    IMPORTING
      r_salv_table = go_salv
    CHANGING
      t_table = gt_list
  ).
  go_salv->display( ).
```

