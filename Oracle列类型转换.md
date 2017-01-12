###CHAR型转为INTEGER
原来为char型，现需要改为integer型，当列中有数据后不能直接更改类型，需要先清空再更改类型，但这无疑会丢掉原来的数据。做法如下：

###第一步：添加TYPE1字段
```sql
ALTER TABLE TABLE_NAME ADD TYPE1 INTEGER DEFAULT 0;
```

###第二步：UPDATE TYPE1的值为TYPE的值
```sql
UPDATE TABLE_NAME T SET T.TYPE1 = NVL(T.TYPE, 0);
```

###第三步：删除原TYPE列
```sql
ALTER TABLE TABLE_NAME DROP COLUMN TYPE;
```

###第四步：重命名TYPE1为TYPE
```sql
ALTER TABLE TABLE_NAME RENAME COLUMN TYPE1 TO TYPE;
```