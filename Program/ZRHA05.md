```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_03
*&---------------------------------------------------------------------*
*& ZVDDL04_05를 Open SQL로 구현
*&---------------------------------------------------------------------*
REPORT zrha04_05.

DATA gt_list TYPE TABLE OF zvddl04_08.
DATA go_salv TYPE REF TO cl_salv_table.

PARAMETERS: pa_car  TYPE scarr-carrid,
            pa_rate TYPE p LENGTH 5 DECIMALS 1.

START-OF-SELECTION.
  SELECT *
    FROM zvddl04_08(
      p_rate = @pa_rate,
      p_carrid = @pa_car
    )
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

