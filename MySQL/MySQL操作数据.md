- 批量插入数据


```mysql
drop procedure if exists test_insert;
        delimiter $$
        create procedure test_insert(n int)
        begin
        declare v int default 1;
        while v<n
        do
                INSERT INTO mt_data.ib_collect_order_trace         (id,ibo_no,ibo_type,order_status,collecting_time,collect_finish_time,cancel_time,down_tms_time,down_wms_time,create_time,update_time,create_user,update_user,yn,ts,version,reserve1,reserve2,test)
                VALUES('',v+2305011000007,0,0,'2023-04-20 21:51:39',NULL,NULL,NULL,NULL,'2023-04-20 21:51:39','2023-04-20 21:51:39','mt',NULL,1,'2023-04-20 21:51:39',0,NULL,NULL,0);
                set v=v+1;
                end while;
                END $$
                -- 重定义结束符
                delimiter;
                -- 调用存储过程插入1000条数据
                call test_insert(1001);

                -- 查看、删除存储过程
                show procedure status like'test_insert';
                drop procedure if exists test_insert;
                
                                
drop procedure if exists afs_service_info;
        delimiter $$
        create procedure afs_service_info(n int)
        begin
        declare v int default 1;
        while v<n
        do
        		INSERT INTO `afs_service_info` (`id`, `org_id`, `afs_service_id`, `pickware_code`, `pickware_id`, `question_type_cid1`, `question_type_cid2`, `customer_pin`, `customer_name`, `customer_grade`, `customer_contact_name`, `customer_tel`, `customer_mobile_phone`, `customer_email`, `customer_postcode`, `is_has_invoice`, `invoice_code`, `customer_expect`, `question_desc`, `is_need_detection_report`, `question_pic`, `afs_service_state`, `approve_notes`, `approve_result`, `pickware_type`, `waybill_code`, `afs_apply_time`, `afterService_Type`, `afs_apply_id`, `sales_platform`, `identity`, `create_user`, `create_name`, `update_user`, `update_name`, `create_time`, `update_time`, `sys_version`, `is_delete`, `ts`, `original_source`, `order_type`, `afsService_flag`, `five_star_flag`, `question_type_cid1Txt`, `question_type_cid2Txt`, `audit_basisText`, `factory_direct_cover`, `special_property_value`) 
				VALUES (NULL, 3, 50000000+v, '', 2150000900, 10039, 10040, 'lijiaxiong5', '我我哦我', -10000, '我我哦我', '13412341234', '13412341234', '', '', 0, '', 10, '测试', -128, '', 1033, '', 33, 40, '', '2023-12-19 16:39:46', 10, 2150000858, 298, '', 'system', 'system', '', '', '2023-12-19 17:02:22', '1970-01-01 00:00:00', 1, 0, '2023-12-19 17:02:22', 0, 0, 0, '', '质量不太行', '质量不行', 'b', '10_207472_NULL', '');
                set v=v+1;
                end while;
                END $$
call afs_service_info(2);


drop procedure if exists loop_bin_container;
        delimiter $$
        create procedure loop_bin_container(n INT,containerCode bigint)
        begin
        declare v int default 1;
        declare cc int default containerCode;
        while v<n
        do
        		INSERT INTO rd_wms.bin_container
					(container_code, org_id, store_id, is_lock, create_user, create_name, create_time, update_user, update_name, update_time, sys_version, is_delete)
					VALUES(cc, 3, 1, 100, 'BJADMIN', 'admin', now(), 'BJADMIN', 'admin', now(), 1, 0);
					SET v=v+1;
					SET cc=cc+1;
                end while;
                END $$
call loop_bin_container(5,123);
```



- 更改某字段的值为另一个字段的值


```mysql
UPDATE `rd_wms`.`storage_inventory`
SET part_code=id
WHERE id between 3029725859 and 3029730858;
```



- 将某字段的值增加固定前缀


```mysql
UPDATE `rd_wms`.`storage_inventory`
SET part_code=CONCAT('DK', part_code)
WHERE id between 3029725859 and 3029730858;
```



- 按模更改某字段的值


```mysql
UPDATE `rd_wms`.`storage_inventory`
SET id_location= case
                     when id%6=0 then 'BN_YC-YJ-0404'
                     when id%6=1 then 'BN_YC-YJ-0403'
                     when id%6=2 then 'BN_YC-YJ-0402'
                     when id%6=3 then 'BN_YC-YJ-0401'
                     when id%6=4 then 'BN_YC-YJ-0404'
                     when id%6=5 then 'BN_YC-YJ-0403'
    END
WHERE id between 3029725859 and 3029730858;
```



- 插入随机生成的字母和数字组合


```sql
INSERT INTO your_table_name (column_name) VALUES (CONCAT(SUBSTRING(MD5(RAND()) FROM 1 FOR 10), LPAD(FLOOR(RAND() * 1000000), 6, '0')));

RAND() 函数生成一个随机数。

MD5(RAND()) 生成一个随机的MD5散列值。

SUBSTRING(MD5(RAND()) FROM 1 FOR 10) 从这个散列值中提取前10个字符。

LPAD(FLOOR(RAND() * 1000000), 6, '0') 生成一个6位数的随机数字，不足6位前面补零。

CONCAT 函数将字母和数字组合在一起。
```



- 创建表的代码

```mysql
show create table  default_unpack_instore_reason_config;
```



- 按条件查询某一天普通拆包任务和主赠拆包任务数量

```mysql
SELECT COUNT(*) as once_occurrences
FROM (
    SELECT afs_service_id
    FROM afs_service_info
    where create_time >'2023-04-18 00:00:00' and create_time <'2024-04-18 23:59:59' AND org_id=6
    GROUP BY afs_service_id
    HAVING COUNT(*) = 1
) as subquery;

SELECT COUNT(*) as two_occurrences
FROM (
    SELECT afs_service_id
    FROM afs_service_info
    where create_time >'2023-04-18 00:00:00' and create_time <'2024-04-18 23:59:59' AND org_id=6
    GROUP BY afs_service_id
    HAVING COUNT(*) >= 2
) as subquery;
```



- 截取某字段的值

```mysql
使用REPLACE函数去掉特定字符串：
UPDATE your_table_name
SET your_column_name = REPLACE(your_column_name, '要去掉的部分', '')
WHERE 条件;

使用REPLACE函数将特定字符串替换为其他字符串：
UPDATE your_table_name
SET your_column_name = REPLACE(your_column_name, '要去掉的部分', '要替换的部分')
WHERE 条件;

使用SUBSTRING函数截取字符串的一部分：
UPDATE your_table_name
SET your_column_name = SUBSTRING(your_column_name, 1, 保留的长度)
WHERE 条件;
```



- 统计一个容器内明细区间总数

```mysql
SELECT elt(interval(t.cc, 1, 5, 10, 50, 100, 200, 300, 500), "a-1-5", "b-5-10", "c-10-50", "d-50-100", "e-200-300", "f-300-500", "g-其他") AS region,count(*)
FROM
  (SELECT unpack_box_no,count(1) cc FROM rd_unpack.rd_part_receive
   WHERE create_time >'2024-04-15' AND create_time <'2024-04-19' AND org_id=6
   GROUP BY unpack_box_no) t
GROUP BY region
ORDER BY region;
```



- 使用`CONCAT`函数进行左匹配

```mysql
SELECT * FROM your_table WHERE your_column LIKE CONCAT('pattern', '%');
这里，'pattern'是你想要匹配的字符串，'%'是通配符，表示任意数量的字符。
```
