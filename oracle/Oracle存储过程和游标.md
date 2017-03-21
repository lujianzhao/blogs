### [参考资料](http://www.live-in.org/archives/2171.html)

### 第一种写法
```sql
-- CREATED ON 2016/8/26 BY SERICAL 
DECLARE
  -- LOCAL VARIABLES HERE
  CURSOR C IS
    SELECT * FROM TB_DEPT_INFO;
  T C%ROWTYPE;
BEGIN
  -- TEST STATEMENTS HERE
  OPEN C;

  LOOP
    FETCH C INTO T;
    EXIT WHEN(C%NOTFOUND);
    DBMS_OUTPUT.PUT_LINE(T.DEPT_CN_NAME);
  END LOOP;

  CLOSE C;
END;
```

### 第二种写法
```sql
DECLARE

  CURSOR C IS
    SELECT * FROM TB_DEPT_INFO;
  T C%ROWTYPE;
BEGIN
  OPEN C;
  
  FETCH C INTO T; -- 第一处
  WHILE (C%FOUND) LOOP
    DBMS_OUTPUT.PUT_LINE(T.DEPT_CN_NAME);
    FETCH C INTO T; -- 第二处
  END LOOP;

  CLOSE C;
END;
```

### 第三种写法
```sql
DECLARE

  CURSOR C IS
    SELECT * FROM TB_DEPT_INFO;
  T C%ROWTYPE;
BEGIN
  FOR T IN C LOOP
    DBMS_OUTPUT.put_line(T.DEPT_CN_NAME);
  END LOOP;
END;

说明：
1）for循环里，cursor不需要打开，for循环自动帮你打开。for循环结束时会自动关闭cursor。
2）for循环自动帮你fetch到v_emp中。
3）for循环和游标搭配，简单而且不容易出错。
```

### 游标使用存储过程参数
```sql
-- 这种方式为错误的，不能直接使用，原因是参数名与字段名一样，可以换个参数名或者下面的方法
CREATE OR REPLACE PROCEDURE SYNC_DEPOSIT_CUSTOMER(SJXF_DATE IN DATE) IS
  -- 查询数据
  CURSOR V_ROW IS
    SELECT ...
       WHERE T.SJXF_DATE = SJXF_DATE;
  -- 中间变量
  V_PARAM V_ROW%ROWTYPE;
BEGIN
  DBMS_OUTPUT.put_line('START : ' || SJXF_DATE);

  FOR V_PARAM IN V_ROW LOOP
    DBMS_OUTPUT.put_line(V_PARAM.KHXM);
  END LOOP;
END;

-- 解决方法，给游标加参数，然后调用游标时传入存储过程的参数
CREATE OR REPLACE PROCEDURE SYNC_DEPOSIT_CUSTOMER(SJXF_DATE IN DATE) IS
  -- 查询数据
  CURSOR V_ROW(SJ DATE) IS
    SELECT ...
       WHERE T.SJXF_DATE = SJ;
  -- 中间变量
  V_PARAM V_ROW%ROWTYPE;
BEGIN
  DBMS_OUTPUT.put_line('START : ' || SJXF_DATE);

  FOR V_PARAM IN V_ROW(SJXF_DATE) LOOP
    DBMS_OUTPUT.put_line(V_PARAM.KHXM);
  END LOOP;
END;
```
### 存储过程、游标参数变量切记不能重复，与参与的字段也不能相同！
### 存储过程、游标参数变量切记不能重复，与参与的字段也不能相同！
### 存储过程、游标参数变量切记不能重复，与参与的字段也不能相同！