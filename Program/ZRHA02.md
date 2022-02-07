```ABAP
*&---------------------------------------------------------------------*
*& Report ZRHA04_02
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zrha04_02.

*DATA gs_list TYPE sflight.

DATA: BEGIN OF gs_list.
        INCLUDE TYPE spfli.
        DATA: cal_type(1),
        carrname    TYPE scarr-carrname,
      END OF gs_list,
      gt_list LIKE TABLE OF gs_list.

PARAMETERS pa_car LIKE gs_list-carrid.

* select 절에서 *는 성능이 안 좋아질 수 있으므로 필드를 선택한다.
* Open SQL => carrid connid fldate
* New Open SQL => carrid, connid, fldate + @ (변수 앞에 무조건 선언)

*START-OF-SELECTION.
*  SELECT SINGLE CARRID, CONNID, FLDATE
*    FROM SFLIGHT
*    INTO CORRESPONDING FIELDS OF @GS_LIST
*    WHERE CARRID = @PA_CAR.
*
*  CL_DEMO_OUTPUT=>DISPLAY_DATA( GS_LIST ).

* New Open SQL => case 문법
START-OF-SELECTION.
  SELECT a~carrid, a~connid, a~countryfr, a~countryto, b~carrname,
    CASE a~countryfr
      WHEN a~countryto THEN 'Domestic'
      ELSE 'International'
    END AS cal_type
  FROM spfli AS a INNER JOIN scarr AS b
    ON a~carrid = b~carrid
  INTO CORRESPONDING FIELDS OF TABLE @gt_list
  WHERE a~carrid = @pa_car.

  cl_demo_output=>display_data( gt_list ).
```
