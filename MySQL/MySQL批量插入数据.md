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
                -- 调用存储过程插入1万条数据
                call test_insert(6);

                -- 查看、删除存储过程
                show procedure status like'test_insert';
                drop procedure if exists test_insert;
```

