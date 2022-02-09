```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_13
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zrha04_13.

DATA: go_salv TYPE REF TO if_salv_gui_table_ida.
*      go_full TYPE REF TO if_salv_gui_fullscreen_ida.

START-OF-SELECTION.

  go_salv = cl_salv_gui_table_ida=>create( iv_table_name = 'SCARR' ).

*  go_full = go_salv->fullscreen( ).
*  go_full->display( ).

  go_salv->fullscreen( )->display( ).
```

