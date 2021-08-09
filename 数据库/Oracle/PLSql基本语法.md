# oracle 学习笔记 - PL/SQL基本语法

> 开启输出功能

```sql
SQL> set serveroutput on
```

## 基本语法

```sql
declare
    -- 声明部分 (变量说明、游标说明、异常说明)    
begin
    --语句部分 （DML语句）
exception
    --例外处理语句
end;
```
以``hello word``为例。

```sql
declare
  --说明部分
begin
  --程序
  dbms_output.put_line('Hello World');
end;

-- 结果  
hello word

PL/SQL 过程已成功完成。
```

## 变量和常量

```sql
说明变量 （char,varchar2,date,number,boolean,long）

例如：

var1 char(15);  #说明变量名，数据类型和长度，并以分号结束

married boolean := true;    #赋值的一种写法

psal number(7,2);

name emp.ename%type;    #引用型变量，和empty表中的ename列的类型一样。

rec emp%rowtype;    #记录型变量，类似数组。
rec.ename:='abc';
```
记录型变量的使用。

```sql
-- 查询并打印7839的姓名和薪水
declare 
  -- 定义记录型变量：代表一行
  emp_rec emp%rowtype;
begin
  select * into emp_rec from emp where empno=7839;  

  dbms_output.put_line(emp_rec.ename||'的薪水是'||emp_rec.sal);  
end;
```

## ``if``语句

```sql
if 条件 then 语句1；

    语句2；

end if;
-------------------------------------------------
if 条件 then 语句1;

    else 语句2;

end if;
-------------------------------------------------
if 条件 then 语句1;

    elsif 语句2;

    else 语句3;

end if;
```

>  判断用户从键盘输入的数字

```sql
--接受键盘输入
--num: 地址值，在该地址上保存了输入的值
accept num prompt '请输入一个数字:';

declare 
  -- 定义变量保存数字
  pnum number := &num; # 得到键盘上的输入值
begin
  if pnum = 0 then dbms_output.put_line('您输入的是0');
     elsif pnum = 1 then dbms_output.put_line('您输入的是1');
     elsif pnum = 2 then dbms_output.put_line('您输入的是2');
     else dbms_output.put_line('其他数字');
  end if;
end;
```

## 循环

```sql
when toal<=100
loop 
    ...
    total:=total+salary;
end loop;

--------------------------------------------------
loop
    exit [when 条件]
    ...
end log

--------------------------------------------------
for i int 1..3
loop
    语句;
end loop;

--------------------------------------------------
```
> 简单的例子，输出1-10。

```sql
declare 
  pnum number := 1;
begin
  loop
       --循环
       --退出条件
       exit when pnum > 10;

       dbms_output.put_line(pnum);
       --加一
       pnum := pnum + 1;
  end loop;
end;
```
## ``exception``

这里的``when``相当于``java``里面的``catch { }`` 花括号内可以写多条语句；

### 系统异常

```sql
-- 被0除
declare 
  pnum number;
begin
  pnum := 1/0; 

exception
  when zero_divide then dbms_output.put_line('1:0不能做分母');
                        dbms_output.put_line('2:0不能做分母');
  when value_error then dbms_output.put_line('算术或者转换错误');
  when others then dbms_output.put_line('其他例外');
end;
```
### 自定义异常

```sql
-- 查询50号部门的员工姓名
declare 
  --定义光标：代表50号部门的员工
  cursor cemp is select ename from emp where deptno=50;
  pename emp.ename%type;

  --自定义例外
  no_emp_found exception;
begin
  open cemp;

  --取第一条记录
  fetch cemp into pename;

  if cemp%notfound then
     --抛出例外
     raise no_emp_found;
  end if;

  --pmon进程: process monitor
  close cemp;

exception 
  when no_emp_found then dbms_output.put_line('没有找到员工');  
  when others then dbms_output.put_line('其他例外');  
end;
```

## 游标（``Cursor``）== ``ResultSet``

游标（``Cursor``）相当于``RssultSet``是一个结果集，是集合。

> **语法**

```sql
-- 说明游标

cursor 游标名称 [ ( 参数名 数据类型 [,参数名 数据类型]... ) ] is select 语句;

例如:

cursor c is select ename from emp;

-- 打开游标
    open c; ( 打开游标执行查询 )
-- 读取游标
    feth c into pname; ( 取一行到变量中 )
-- 关闭游标
    close c; ( 关闭游标释放资源 )
```

> 例：涨工资，总裁1000 经理800 其他400

```sql
-- 涨工资，总裁1000 经理800 其他400
declare 
  -- 定义光标
  cursor cemp is select empno,job from emp;
  pempno emp.empno%type;
  pjob   emp.job%type;
begin
  rollback;  

 --打开光标 
 open cemp;

 loop
      --取一个员工
      fetch cemp into pempno,pjob;
      exit when cemp%notfound;

      --判断职位
      if pjob = 'PRESIDENT' then update emp set sal=sal+1000 where empno=pempno;
         elsif pjob = 'MANAGER' then update emp set sal=sal+800 where empno=pempno;
         else update emp set sal=sal+400 where empno=pempno;
      end if;
 end loop;

 --关闭光标
 close cemp;

 --提交  ---> ACID
 commit;

 dbms_output.put_line('完成');
end;
```
## **带参数的游标**

```sql
corsor c2 ( pjob varchar2 ) is select ename,sal from emp where job = pjob;

--执行语句

open c2 ( 'clerk' );
```
实例：
```sql
-- 查询某个部门的员工姓名
declare 
  cursor cemp(dno number) is select ename from emp where deptno=dno;
  pename emp.ename%type;
begin
  open cemp(20);
  loop
       fetch cemp into pename;
       exit when cemp%notfound;

       dbms_output.put_line(pename);

  end loop;
  close cemp;
end;
```
## **异常``exception``**

类似 ``java`` 中 ``exception``，但是这里没有 ``finally`` 语句。所以必须列出所有的异常情况。

```sql
No_data_found ( 没有找到数据 )
Too_many_rows ( select ... into 语句匹配多个行 )
Zero_Divide ( 被零除 )
Value_error ( 算数或者转化错误 )
Timeout_on_resource ( 等待资源超时 )
```

```sql
-- 被0除
declare 
  pnum number;
begin
  pnum := 1/0; 

exception
  # when相当于catch语句
  when zero_divide then dbms_output.put_line('1:0不能做分母');
                        dbms_output.put_line('2:0不能做分母');
  when value_error then dbms_output.put_line('算术或者转换错误');
  #没有finally 语句。所以必须列出所有的异常情况。
  when others then dbms_output.put_line('其他例外'); 
end;
```

### 自定义异常

在``declare``中定义异常

1. ``myexcp exception``  在执行语句中抛出异常
2. ``raise　myexcp``  在``exception``中处理异常
3. ``when myexcp then …``

```sql
-- 查询50号部门的员工姓名
declare 
  --定义光标：代表50号部门的员工
  cursor cemp is select ename from emp where deptno=50;
  pename emp.ename%type;

  --自定义例外
  no_emp_found exception;
begin
  open cemp;

  --取第一条记录
  fetch cemp into pename;

  if cemp%notfound then
     --抛出例外
     raise no_emp_found;
  end if;

  --pmon进程: process monitor,类似finally功能，这里process monitor进行自动的善后操作会关闭资源。
  close cemp;

exception 
  when no_emp_found then dbms_output.put_line('没有找到员工');  
  when others then dbms_output.put_line('其他例外');  
end;
```

## 存储过程

存储在数据库中供所有用户程序调用的子程序叫做存储过程或者存储函数。

### 创建存储过程

```sql
create [ or replace] procedure 名称 ( 参数列表 ) 

as

    plsql子程序体:
```

举例：

```sql
/*
给指定的员工涨100，并且打印涨前和涨后的薪水
*/
create or replace procedure raiseSalary(eno in number)
as
   --定义变量保存涨前的薪水
   psal emp.sal%type;
begin
   --得到涨前的薪水
   select sal into psal from emp where empno=eno;

   --涨100
   update emp set sal=sal+100 where empno=eno;

   --要不要commit？最好不要commit，把commit留给存储过程的调用者

   dbms_output.put_line('涨前：'||psal||'  涨后:'||(psal+100));
end;
```
### 调用存储过程

```sql
# 方法一
# 开启输出功能
set serveroutput on
begin
    raisesalary(7369);
    commit;
end;

# 方法二
# 开启输出功能
set serveroutput on
exec raisesalary(7369);
commit;
```

## 存储函数

和存储过程类似，但是必须有一个``return``字句，用来返回一个返回值（这是和存储过程的唯一区别）。也可以用``out``参数返回多个结果，如果用``out``类型参数返回值，存储过程和存储函数几乎无差别。

### 创建语法
```sql
create [or replace] function 函数名 ( 参数列表 )

return 函数值类型

as

    plsql子程序体。
```

例子：
```sql
--查询某个员工的年收入
create or replace function queryEmpIncome(eno in number)
return number
as
--月薪和奖金
psal emp.sal%type;
pcomm emp.comm%type;
begin
--得到月薪和奖金
select sal,comm into psal,pcomm from emp where empno=eno;

--返回年收入
return psal*12+nvl(pcomm,0);
end;
```

### 调用

```sql
declare
    v_sal number;
begin
    v_sal:=queryEmpSalary(7934);
    dbms_output.put_line('salary is:'||v_sal);
end;    
```

```sql
begin
    dbms_output.put_line('salary is:'||queryEmpSalary(7934));
end;
```

## ``java``语言调用存储过程和存储函数

``Jdbcutil.java``

```java
package demo;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCUtils {

    private static String driver = "oracle.jdbc.OracleDriver";
    private static String url = "jdbc:oracle:thin:@192.168.137.129:1521/orcl";
    private static String user = "scott";
    private static String password = "tiger";

    static{
        //注册驱动
        try {
            Class.forName(driver);
            //DriverManager.registerDriver(driver);
        } catch (ClassNotFoundException e) {
            throw new ExceptionInInitializerError(e);
        }
    }

    public static Connection getConnection(){
        try {
            return DriverManager.getConnection(url, user, password);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    /*
     * 运行Java程序：
     * java -Xms100M -Xmx200M HelloWorld
     * 
     * 技术方向：
     * 1. 性能优化
     * 2. 故障诊断:死锁 ThreadDump
     */
    public static void release(Connection conn,Statement st,ResultSet rs){
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }finally{
                rs = null;//-----> Java GC
            }
        }
        if(st != null){
            try {
                st.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }finally{
                st = null;
            }
        }
        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }finally{
                conn = null;
            }
        }       
    }
}
```

``TestOracle`` 测试连接，并调用。``conn.commit()``用来提交事务;

测试调用存储过程和存储函数

```java
package demo;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.ResultSet;

import oracle.jdbc.OracleCallableStatement;
import oracle.jdbc.OracleTypes;

import org.junit.Test;

public class TestOracle {
/*
 * create or replace procedure queryEmpInformation(eno in number,
                                                pename out varchar2,
                                                psal   out number,
                                                pjob   out varchar2)
 */
    @Test
    public void testProcedure(){ // 测试调用存储过程
        // {call <procedure-name>[(<arg1>,<arg2>, ...)]}
        String sql = "{call queryEmpInformation(?,?,?,?)}";

        Connection conn = null;
        CallableStatement call = null;
        try {
            conn = JDBCUtils.getConnection();
            call = conn.prepareCall(sql);

            //对于in参数，赋值
            call.setInt(1, 7839);

            //对于out参数，申明
            call.registerOutParameter(2, OracleTypes.VARCHAR);
            call.registerOutParameter(3, OracleTypes.NUMBER);
            call.registerOutParameter(4, OracleTypes.VARCHAR);

            //执行
            call.execute();

            //取出结果
            String name = call.getString(2);
            double sal = call.getDouble(3);
            String job = call.getString(4);

            System.out.println(name+"\t"+sal+"\t"+job);         
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            JDBCUtils.release(conn, call, null);
        }
    }

/*
 *  create or replace function queryEmpIncome(eno in number)
 return number
 */
    @Test
    public void testFunction(){ // 测试调用存储函数
        //{?= call <procedure-name>[(<arg1>,<arg2>, ...)]}
        String sql = "{?=call queryEmpIncome(?)}";

        Connection conn = null;
        CallableStatement call = null;
        try {
            conn = JDBCUtils.getConnection();
            call = conn.prepareCall(sql);

            //对于out参数，申明
            call.registerOutParameter(1, OracleTypes.NUMBER);

            //对于in参数，赋值
            call.setInt(2, 7839);

            call.execute();

            //取出结果
            double income = call.getDouble(1);
            System.out.println(income);
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            JDBCUtils.release(conn, call, null);
        }       
    }
}
```
## 包和包体
### 在``out``参数中使用游标

　　如何解决下面的问题，我们不能通过不断的为存储函数或者存储过程添加``out``类型的参数。应该用一个集合类型的参数作为接收值。

```sql
思考：
1. 查询某个员工的所有信息 ---> out参数太多
2. 查询某个部门中的所有员工信息  ---> 返回集合
*/
```

### 创建包头和包体

解决办法：在``out``参数中使用游标

```sql
# 2. 查询某个部门中的所有员工信息  ---> 返回集合

包头

CREATE OR REPLACE PACKAGE MYPACKAGE AS 

  type empcursor is ref cursor; # 通过type自定义类型。
  procedure queryEmpList(dno in number,empList out empcursor);

END MYPACKAGE;


包体
CREATE OR REPLACE PACKAGE BODY MYPACKAGE AS

  procedure queryEmpList(dno in number,empList out empcursor) AS
  BEGIN

    open empList for select * from emp where deptno=dno; # 手动打开了游标，之后会自动关闭游标。

  END queryEmpList;

END MYPACKAGE;
```
### ``java``调用

```java
@Test
public void testCursor(){
    String sql = "{call MYPACKAGE.QUERYEMPLIST(?,?)}";
    Connection conn = null;
    CallableStatement call = null;
    ResultSet rs = null;
    try {
        conn = JDBCUtils.getConnection();
        call = conn.prepareCall(sql);

        //对于in参数，赋值
        call.setInt(1, 20);

        //对于out参数，申明
        call.registerOutParameter(2, OracleTypes.CURSOR);

        //执行
        call.execute();

        //取出结果
        rs = ((OracleCallableStatement)call).getCursor(2); // 需要将call转为针对oracle数据库的实现类。
        while(rs.next()){
            String name = rs.getString("ename");
            double sal = rs.getDouble("sal");
            String job = rs.getString("job");
            System.out.println(name+"\t"+sal+"\t"+job);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        JDBCUtils.release(conn, call, rs); # 这里会自动关闭游标，通过rs。
    }       

}
```

## 触发器
### 概述

　　数据库触发器是一个与表相关联的、存储在表上的``PL/SQL``程序。每当一个特定的数据操作语句``（insert、update、delete）``作用在指定的表上时，``oracle``会自动的执行触发器中定义的语句序列。

### 触发器的类型
- 语句级触发器
    - 在指定的操作语句操作之前或之后执行一次，不管这条语句影响了多少行。
- 行级触发器
    - 触发语句作用的每一条记录都会触发触发器。在行级触发器中使用：``old``和：``new``伪记录变量，识别值的状态。

**触发语句与伪记录变量的值。**

触发语句 |	``:old`` |	``:new``
---- | --- | ---
``insert`` |	所有字段都是空（``null``） |	将要插入的数据
``update`` |	更新以前该行的值 |	更新后的值
``delete`` |	删除以前该行的值 |	所有字段都是（``null``）

### 创建触发器
```sql
create [or replace] trigger 触发器名称

{before|alter}

{delete|insert|update[of 列名]}

on 表名

[for eache row [when (条件)]]

PLSQL块
```
例子：
```sql
/*
触发器应用一: 禁止在非工作时间插入新员工

1. 周末: to_char(sysdate,'day') in ('星期六','星期日')
2. 上班前 下班后：to_number(to_char(sysdate,'hh24')) not between 9 and 17 
*/
create or replace trigger securityemp
before insert
on emp
begin
  if to_char(sysdate,'day') in ('星期六','星期日') or
     to_number(to_char(sysdate,'hh24')) not between 9 and 17  then
     --禁止insert操作
     raise_application_error(-20001,'禁止在非工作时间插入新员工');
  end if;

end;
```

```sql
/*
触发器应用二：数据的确认
涨后的薪水不能少于涨前的薪水
*/
create or replace trigger checksalary
before update
on emp
for each row
begin
  --if 涨后的薪水 < 涨前的薪水 then
  if :new.sal < :old.sal then
    raise_application_error(-20002,'涨后的薪水不能少于涨前的薪水.涨前:'||:old.sal||'   涨后:'||:new.sal);
  end if;
end;
```
















