### 相关sql语句
```sql
CREATE TABLE CTEST
( ID INTEGER,
  C_DATE DATE)
  PARTITION BY RANGE(C_DATE ) INTERVAL(NUMTOYMINTERVAL(1, 'YEAR')) (
  PARTITION PART_MIN VALUES LESS THAN (TO_DATE('2015-01-01', 'YYYY-MM-DD'))
  );
INSERT INTO CTEST VALUES(1, TO_DATE('2013-11-01', 'YYYY-MM-DD'));
INSERT INTO CTEST VALUES(1, TO_DATE('2014-11-01', 'YYYY-MM-DD'));
INSERT INTO CTEST VALUES(1, TO_DATE('2015-11-01', 'YYYY-MM-DD'));
INSERT INTO CTEST VALUES(1, TO_DATE('2016-11-01', 'YYYY-MM-DD'));
SELECT * FROM CTEST;
        ID C_DATE
---------- -------------
         1 01-11月-14
         1 01-11月-13
         1 01-11月-15
         1 01-11月-16
SELECT * FROM CTEST PARTITION (PART_MIN);
        ID C_DATE
---------- -------------
         1 01-11月-14
         1 01-11月-13
SELECT TABLE_NAME, PARTITION_NAME FROM USER_TAB_PARTITIONS T WHERE T.TABLE_NAME = 'CTEST'
TABLE_NAME                     PARTITION_NAME
------------------------------ ----------------------------
CTEST                          PART_MIN
CTEST                          SYS_P21
CTEST                          SYS_P22
```

### 说明
```sql
interval使用之后，就不用手工增加分区，来适应数据的增长。 
根据年月 INTERVAL(NUMTOYMINTERVAL(1,'YEAR')) 
INTERVAL(NUMTOYMINTERVAL(1,'MONTH')) 
根据天 INTERVAL(NUMTODSINTERVAL(1,'DAY'))
```