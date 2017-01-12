```sql

-- 两个日期之间的月数
SELECT 
    ROUND(
        MONTHS_BETWEEN(
            TO_DATE('2015-03-31', 'YYYY-MM-DD'), 
            TO_DATE('2015-01-01', 'YYYY-MM-DD')
        )
    ) FROM DUAL;

-- 两个日期之间的天数
SELECT TO_DATE('2015-03-31', 'YYYY-MM-DD')-TO_DATE('2015-03-29', 'YYYY-MM-DD') FROM DUAL;

--显示当前时间
select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual;  

--截取到年（本年的第一天）
select trunc(sysdate,'year') from dual; 

--截取到季度（本季度的第一天）
select trunc(sysdate,'q') from dual; 

--截取到月（本月的第一天）
select trunc(sysdate,'month') from dual; 

--本月最后一天
SELECT TRUNC(LAST_DAY(SYSDATE)) FROM DUAL; 

--空
select trunc(sysdate,'') from dual; 

--默认截取到日（当日的零点零分零秒）
select to_char(trunc(sysdate),'yyyymmdd hh24:mi:ss') from dual; 

-- 离当前时间最近的周四，若当天为周四则返回当天，否则返回上周四
select trunc(sysdate-1,'w') from dual;  

--截取到上周末（上周周六）
select trunc(sysdate,'ww') from dual;  

--截取到周（本周第一天，即上周日）
select trunc(sysdate,'day') from dual; 

--本周第2天，即本周一
select trunc(sysdate,'iw') from dual; 

--截取到日（当日的零点零分零秒）
select to_char(trunc(sysdate,'dd'),'yyyymmdd hh24:mi:ss') from dual;

--截取到小时（当前小时，零分零秒）
select trunc(sysdate,'hh24') from dual;  

--截取到分（当前分，零秒）
select trunc(sysdate,'mi') from dual; 

--报错，没有精确到秒的格式
select trunc(sysdate,'ss') from dual ;

--Oracle英文日期转换，指定nls_date_language参数
select to_date('01-sep-2009','dd-mon-yyyy','nls_date_language=American') from dual;
```