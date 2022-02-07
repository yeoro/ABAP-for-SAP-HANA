```ABAP
*&---------------------------------------------------------------------*
*& Report ZRHA04_01
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT ZRHA04_01.

DATA: GS_LIST TYPE SFLIGHT,
      GT_LIST LIKE TABLE OF GS_LIST.
DATA: GT_SCARR TYPE TABLE OF SCARR.

* exit이 없고 무분별하게 endselect 사용하는 경우
* * 안쓰고 from과 into 구조가 같으면 문제가 됨
SELECT *
  FROM SFLIGHT
  INTO GS_LIST.
  WRITE:/ GS_LIST-CARRID, GS_LIST-CONNID, GS_LIST-FLDATE.
ENDSELECT.

CLEAR GT_LIST.
SELECT CARRID CONNID FLDATE
  FROM SFLIGHT
  INTO CORRESPONDING FIELDS OF TABLE GT_LIST.

* 팝업 창으로 결과 띄우기
CL_DEMO_OUTPUT=>DISPLAY_DATA( GT_LIST ).

SELECT CARRID
  FROM SCARR
  INTO CORRESPONDING FIELDS OF TABLE GT_SCARR.

* 주석 : ctrl + , / 주석 풀기 : ctrl + .
CLEAR GT_LIST.
SORT GT_SCARR BY CARRID.
DELETE ADJACENT DUPLICATES FROM GT_SCARR COMPARING CARRID.
IF GT_SCARR IS INITIAL.
  RETURN.
ENDIF.

* for all entries in : 오른쪽 인터널테이블이 비어있으면 왼쪽 테이블의 데이터를 모두 가져오므로 조심해야함. 즉, 인터널 테이블의 중복값이나 비어있음을 확인해야함
* 현실적으로 안쓰는걸 권장함. 실제로 new opensql에서는 지원이 중단되었다.
* 에러가 발생하는게 아니라 인터널 테이블에 데이터를 넣는 작업을 하므로 메모리 잡아먹는다.
SELECT CARRID CONNID FLDATE
  INTO CORRESPONDING FIELDS OF TABLE GT_LIST
  FROM SFLIGHT FOR ALL ENTRIES IN GT_SCARR
  WHERE CARRID = GT_SCARR-CARRID.

CL_DEMO_OUTPUT=>DISPLAY_DATA( GT_LIST ).

* for all entries & into corresponding => open sql 전용
```