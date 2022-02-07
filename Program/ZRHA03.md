```ABAP
*&---------------------------------------------------------------------*
*& Report ZRHA04_03
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zrha04_03.

DATA gt_list TYPE TABLE OF zvddl04_02. "zvsql04_02
DATA go_salv TYPE REF TO cl_salv_table.

* DDL 소스를 사용하기 위해서는 New Open SQL 사용해야 함
SELECT *
  FROM zvddl04_02 "zvsql04_02
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
