```sql
--1.取整(大)  　　    
select ceil(-1.501), ceil(1.501) from dual;


--2.取整（小） 　　
select floor(-1.501), floor(1.501) from dual;

--3.取整（截取）　 
select trunc(-1.502), trunc(1.502) from dual;      

--4.取整(舍入)           
select round(-1.501), round(1.501) from dual;
```