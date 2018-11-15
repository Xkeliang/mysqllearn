### mysqllearn
mysql学习

MYSQL学习     Server version: 8.0.11
##一、数据管理
    1、人工管理阶段
    2、文件系统阶段
    3、数据库系统阶段
##二、概念
    1、数据库（DB）
    2、数据库管理系统（DBMS）
    3、数据库系统（DBS）
##三、数据库阶段
    1、层次数据库和网状数据库
    2、关系数据库（逻辑性）
    3、后关系数据库（抽象性）
##四、mysql
        SQL
        struct query language
        Oracle，DB2，SQL Server，PostgreSQL
    1、数据库定义语言（DDL）  create，alter，drop
    2、数据操作语言（DML）  insert，update，delete
    3、数据库控制语言（DCL） grant，reboke，omcmit，rollback

    #一、安装和配置 （版本：8.0.11）
        1、社区版、企业版
            软件版本：
            GA、RC、Alpha、Bean
            官网：https://www.mysql.com/cn/      下载
        2、启动
            计算机管理，服务
            net start MYSQL
            net stop mysql
        3、登录
            mysql -uroot -p

    #二、数据库、数据库对象、数据
        数据库对象：表、视图、存储
        数据库：存储数据库对象

        show databases；   查看
        create database $test;  创建
        drop database database_name;    删除
        use database_name;    //use $test    选择

    #三、存储引擎、数据类型
        存储引擎：
            指定了表的类型，如何存储和索引数据，是否支持事务，决定了表在计算机中的存储方式
            show engines；show engines\G   //查看引擎
            show variables like '%storage_engine%';   //查看默认存储引擎

            修改默认存储引擎：
                手动  ---MYsql配置文件my.ini
            常用三种存储引擎（区别）
                MyISAM  不支持事务   表锁
                InnoDB  支持事务  存储64TB  行锁
                MEMORY  不支持事务  表锁
        数据类型：
            整数：tinyint  1字节，smalunt 2字节，mediumint 3字节，int和integer 4字节，bigint 8字节    -0
            浮点数类型:  float 4字节  ，double  8字节
            定点数类型： dec（M,D）   decimal(M,D)   字节M+2
            位类型: Bit(M)    1~8 字节
            日期和时间类型
                date  4字节
                datatime  8字节
                timestamp  4字节
                time 3字节
                year 1字节
            字符串类型
                char（M）  M字节
                varchar（M）M字节
                tinytext
                text
                mediumtext
                longtext
                binary（M）
                varbinary（M）
                tinyblob
                blob
                mediumblol
                longblob
        原码、反码、补码
    #四、表的操作
        1、表的概念
            包含数据库所有数据的数据库对象
            列、索引、触发器
            show tables；
        2、创建表
            create table table_name(属性名 数据类型，属性名 数据类型)engine=存储引擎名
                create table employee(empno int,ename varchar(20),job varchar(20),mgr int,hiredate date,sal dec(5,2),comm dec(5,2));
        3、查看表
            desc table_name          //查看表结构
            show create table table_name    //查看表结构（创建）
            select * from table_name  //查看表内容
        4、删除表
            drop table table_name
        5、修改表  （alter）
            修改表名
                alter table old_table_name rename （to） new_table_name;
            修改字段
                alter table table_name add column_name 类型;
                alter table table_name add column_name 类型 first;
                alter table table_name add column_name 类型 after column_name;   //增加字段

                alter table table_name drop column_name;     //删除字段

                alter table table_name modify column_name 类型；  //修改字段类型
                alter table table_name change old_column_name new_column_name 类型；  //修改字段名字
                alter table table_name modify column_name 类型 first(after column_name)  //修改字段位置

        6、完整性约束
            Not null 非空   create table table_name(column_name 类型 not null)
            default 默认
            unique key 唯一约束
            primary key 主键约束   多字段主键约束 create table table_name （column_name 类型，......[constraint 约束名] primary key （column_name1，column_name2.....））;
            auto_increment  自动增加
            foreign key   //create table table_name(column_name 类型 [constraint 外键约束名]foreign key（属性名）references 表名（属性名））；// 表名（属性名）类型一致，为主键

             alter table table_name drop foreign key 外键约束名 ; 
             alter table table_name add constraint 外键约束名 foreign key(属性名)references 表明(属性名) ; 

    #五、索引
        创建、修改、删除
        提高查询速度、占据磁盘空间
            普通索引、唯一索引、全文索引、单列索引、多列索引、空间索引

        1、创建普通索引
            创建表时创建
                create table table_name （属性名 类型.........,index | key [索引名]（属性名[(长度)][ASC|DESC]））;
            已经存在表中
                create index 索引名 on table_name(属性[(长度)][ASC|DESC]);
                alter table table_name add index | key 索引名 (属性名[(长度)][ASC|DESC]);

        2、创建唯一索引
            创建表时创建
                create table table_name （属性名 类型.........,unique index | key [索引名]（属性名[(长度)][ASC|DESC]））;
            已经存在表中
                create unique index 索引名 on table_name(属性[(长度)][ASC|DESC]);
                alter table table_name add unique index | key 索引名 (属性名[(长度)][ASC|DESC]);
        3、全文索引
            只能在存储引擎WieMyISAM的数据库
            创建表时创建
                create table table_name （属性名 类型.........,fulltext index | key [索引名]（属性名[(长度)][ASC|DESC]））;
            已经存在表中
                create fulltext index 索引名 on table_name(属性[(长度)][ASC|DESC],);
                alter table table_name add fulltext index | key 索引名 (属性名[(长度)][ASC|DESC]);
        4、多列索引   
            创建表时创建
                create table table_name （属性名 类型.........,index | key [索引名]（属性名[(长度)][ASC|DESC],属性[(长度)][ASC|DESC]...));
            已经存在表中
                create index 索引名 on table_name(属性[(长度)][ASC|DESC],属性[(长度)][ASC|DESC]...);
                alter table table_name add index | key 索引名 (属性名[(长度)][ASC|DESC],属性[(长度)][ASC|DESC]...);
        5、查看索引
            show index from table_name；
        6、删除索引
            drop index index_name on table_name;
        
    #六、视图
        创建
            create view view_name AS 查询语句
        查看
            show tables;
            desc view_name;
            show table status [from db_name][like 'pattern']
            show create view view_name;
            select *  from views where table_name ='view_name'
        修改
            create or replace view view_name as 查询语句;
            alter view viewname as 查询语句;
        视图操作表
            select、insert、delete、update

    #七、触发器（trigger）
        delete
        insert
        update
        1、创建触发器：
        delimiter $
            create trigger trigger_name 
                before|after trigger_event on table_name for each row 
                begin
                trigger_STMT1;
                trigger_STMT2;
            END$
        2、查看
            show triggers
        3、删除
            drop trigger trigger_name;
    
    #八、数据操作
        insert into
            insert into table_name () values();
            insert into table_name () select()from()where...;   //插入查询结果
        update
            update table_name set  field1=value1,field2=value2  where condition;
        delete from
            delete from table_name where condition;
        select
    
    #九、查询
        1、单表
            简单数据记录查询
                select field1,field2 ... from table_name  //简单数据记录查询
                select distinct field1  from table_name    //去重查询
                select field1,field2*12+5 from table_name    //实现数学运算   select concat(field,'连接',field) as newname  from table_name
            条件数据记录查询
                select field1，....from table_name where condition  
                    select field1，....from table_name where field between value1 and value2
                    select field1，....from table_name where field not between value1 and value2
                    select field1，....from table_name where field in (value1,value2...)      //or
                    select field1, ....from table_name where field like value;    通配符"_" 匹配一个单字符 “%”匹配任意字符
            排序数据记录查询
                select field1....from table_name where condition order by field1 asc|desc,field2 asc|desc
                select field1....from table_name where condition limit offset_start，row_count
            统计数据记录查询
                select count（） as name from table_name where condition
                avg()
                sum() 
                max()
                min()
            分组数据记录查询
                select group_concat (field) from table_name where condition group by field;   
                having  查询分组选择
        2、多表
            内连接查询
                select field1 field2 ...from join_tablename1 inner join join_tablename2  [inner join join_tablename] on join_condition [where] condition;
                自连接
                等值连接
                不等连接
            外连接查询
                左外连接
                select field1，field2 ...from join_tablename1 left join join_tablename2 on join_condition
                右外连接
                select field1，field2 ...from join_tablename1 right join join_tablename2 on join_condition
            合并查询
                union 、union all
            子查询  
                where子句的子查询
                    any all exists in
                from子句的子查询  
    #十、运算符
        算术运算符
        比较运算符
            regexp
        逻辑运算符
        位运算符

    #十一、字符串函数
    #十二、日期和时间函数
        now()、curdate()、curtime()

    #十三、存储过程和函数的操作
        1、概念
            函数必须有返回值，而存储过程则没有（不能使用return），存储过程的参数类型远远多于函数参数类型
                允许标准组件式编程，提高sql语句的重用性、共享性和可移植性
                实现较快执行速度、减少网络流量
                可以被作为一种安全机制来利用
                ————————————————————
                需要更高的技能
                创建这些数据库对象的权限
            创建、查看、更新、删除
        2、创建存储过程
            create procedure procedure_name(procedure_parameter[,...])[characteristic...] routine_body  ____________可以用begin...END 来标记SQL语句的开始结束
                procedure_parameter    参数
                        [in|out|inout]  parameter_name type
                characteristic  特性
                        language SQL
                        | [not]deterministic
                        | {contains sql| no sql |reads sql data | modifies sql data}
                        | sql security {definer|invoker}
                        | comment 'string'
        3、创建函数
             create function function_name(function_parameter[,...])[characteristic...] routine_body  ____________可以用begin...END 来标记SQL语句的开始结束

        4、操作变量
            声明： declare var_name [,...] type [default value]
            赋值: set var_name = expr[,...]
                  select field_name[,...] into var_name[,...] from table_name where condition
            操作: 定义条件
                    declare condition_name condition for condition_value
                  定义处理程序
                    declare handler_action handler for condition_value[,...] statement
                        handler_action
                            continue
                            | exit
                        condition_value
                            sqlstate[value]  sqlstate_value
                            | condition_name
                            |sqlwarning
                            |not found
                            |sqlexception
                            |mysql_error_code
        5、使用游标
            游标遍历记录结果
                    看做一种类型，遍历结果集，相当于指针或者数组中的下标
            声明
                declare cursor_name cursor for select_statement
            打开
                open cursor_name
            使用
                fetch cursor_name into var_name[,var_name]...
            关闭
                close cursor_name
            
            遍历
                while ... end while
                loop ... end loop
                repeat ... end repeat

        6、流程控制
            if  then  else  then   end if
            case  
                when  then
            end case

        7、调用存储过程
            call   procedure_name()
        8、查看存储过程和方法
            show procedure [function] status [like  ]
            通过系统表
             use information_schema
             desc routines
             select * from foutines where 
            
            show create procedure procedure_name
        9、修改存储过程和函数
            alter procedure procedure_name [characteristic...]
            alter function function_name [characteristic...]

        10、删除存储过程和函数
            drop proceedure procedure_name;
            drop function function_name;
    #十四、MYSQL安全机制
        root 用户
        普通用户
            系统表：mysql.user
                用户字段
                权限字段
                安全列
                资源控制列
            系统表： mysql.db
        用户管理机制
            登录：
                mysql -h hostname|hostIP -pport -u usename -p DattabaseName -e "SQL语句"
                -h:  指定所连接mysql服务器的地址    主机名、主机IP
                -p：默认3306
                -u：用户
                -p：输入密码
                databasename：登录到那个数据库  系统默认数据库mysql
                -e: 指定执行的sql语句
                出现问题
                    登录时Access denied for user ‘root’@'localhost' (using password :YES)
                解决
                    1、打开目录下my.ini,在最后一行添加“skip-grant-tables”，保存关闭
                    2、重启mysql服务
                    3、dos 下  输入“mysql -uroot -p”（不用输入密码），回车进入数据库
                    4、执行 use mysql
                    5、执行 update user set password=PASSWOED("rootadmin") where user = "root";    新密码
                    6、打开my.ini,删除最后一行的“skip-grant-tables” ，保存关闭
                    7、重启mysql服务
                    8、重新登录用新密码
            创建普通用户
                create user username[indentified by  'password']   //方式一
                insert into user(host,username,password) values(('hostname','username',PASSWORD('password')) //方式二
                grant priv_type on databasename.tablename to username@hostname indentified by 'password';
            修改密码
                mysqladmin -u username -p password "newpassword"
                grant priv_type on databasename.tablename to username@hostname indentified by 'newpassword' with grant option;
                set passsword for 'username'@'hostname'=password('newpassword');   (好用)
            删除用户
                drop user usename@hostname;
                delete from user where user='username' and host='hostname';
            权限管理
                grant（赋予）
                    grant select,create,drop on *.*  to 'username'@'hostname';
                revoke（收回）
                    revoke all privileges on *.* from 'usename'@'localhost'

    #十五、数据备份还原
            备份文件一般以.sql为扩展名，也可以使用其他扩展名
            mysqldump -u username -p dbname table1,table2... >D:\ backupname.sql  //备份表
            mysqldump -u username -p --all -databases >D:\ backupname.sql  //备份数据库

            还原数据
            mysql -u username -p dbname <D:\ backupname.sql
        
        





        



