### 问题
```sql
DROP TABLESPACE ZHMYRP_2016  INCLUDING CONTENTS AND DATAFILES CASCADE CONSTRAINTS;
ORA-14404: 分区表包含不同表空间中的分区
```

### 解决方法
```sql
# 查找分区对象
SELECT * FROM DBA_SEGMENTS T WHERE T.TABLESPACE_NAME = 'ZHMYRP_2016';
# 删除分区表和分区索引，再执行上面sql即可删除
DROP TABLE LDSB.TB_CRM_ZHMYRP
```