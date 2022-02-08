```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_03
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zrha04_06.

DATA gt_list TYPE zcl_amdp04_01=>tt_list. "local => static(global)
DATA go_salv TYPE REF TO cl_salv_table.
DATA go_amdp TYPE REF TO zcl_amdp04_01.

START-OF-SELECTION.
  CREATE OBJECT go_amdp.
  go_amdp->get_amdp(
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

