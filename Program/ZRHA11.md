```abap
*&---------------------------------------------------------------------*
*& Report ZRHA04_11
*&---------------------------------------------------------------------*
*& Procedure Proxy 호출
*&---------------------------------------------------------------------*
REPORT zrha04_11.

DATA gt_data TYPE TABLE OF zif_zproxy04_01=>et_late.
DATA gt_early TYPE TABLE OF zif_zproxy04_01=>et_early.

CALL DATABASE PROCEDURE zproxy04_01
  EXPORTING
    iv_mandt = sy-mandt
    iv_number = 5
  IMPORTING
    et_late = gt_data
    et_early = gt_early.

cl_demo_output=>display_data( gt_data ).
```

