批量插入数据

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
```



批量更改某字段的值为另一个字段的值

```mysql
UPDATE `rd_wms`.`storage_inventory`
SET part_code=id
WHERE id between 3029725859 and 3029730858;
```



批量将某字段的值增加固定前缀

```mysql
UPDATE `rd_wms`.`storage_inventory`
SET part_code=CONCAT('DK', part_code)
WHERE id between 3029725859 and 3029730858;
```



按模更改某字段的值

```mysql
UPDATE `rd_wms`.`storage_inventory`
SET id_location= case

                     when id%6=0 then 'BN_YC-YJ-0404'

                     when id%6=1 then 'BN_YC-YJ-0403'

                     when id%6=2 then 'BN_YC-YJ-0402'

                     when id%6=3 then 'BN_YC-YJ-0401'

                     when id%6=4 then 'BN_YC-YJ-0404'

                     when id%6=5 then 'BN_YC-YJ-0403'

                     when id%6=6 then 'BN_YC-YJ-0402'

                     when id%6=7 then 'BN_YC-YJ-0401'

                     when id%6=8 then 'BN_YC-YJ-0402'

                     when id%6=9 then 'BN_YC-YJ-0401'
    END

WHERE id between 3029725859 and 3029730858;
```

