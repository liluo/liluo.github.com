---
layout: post
title: "非 DBA 也要勇敢的 Online(!) DDL"
date: 2015-12-30 14:14
comments: true
categories: [MySQL, DDL, Rails]
---

目前我司没有专职 DBA，所以 Online DDL 需要自己动手。最近操作了几次不同数量级（单表 5 million, 20 million, 80 million）的 Online DDL，这里做下记录，希望这些数据能够对其他和我一样非 DBA 的同学提供点儿参考价值。

<!-- more -->

### Production Environments

``` bash
Database: MySQL
Version: 5.6.19
Storage Engine: InnoDB
Migrate Tool: Active Record Migrations(Ruby on Rails)
```

### Round 1（5 million）

第一个回合是给 500W+ 记录的表增加两个字段和一个索引：

``` bash
ALTER TABLE `table` ADD COLUMN `foo` INT DEFAULT 0;
ALTER TABLE `table` ADD COLUMN `bar` DATETIME DEFAULT NULL;
ALTER TABLE `table` ADD INDEX `index_table_on_bar` (`bar`);
```

首次操作这么“大规模”（让 DBA 们贱笑了）的 Online(!!!) DDL 内心是有点小复杂……

当时的心理预期是 10 分钟，但是结果却很乐观：

``` bash
== AddFooToTable: migrating - Shard: master =============
-- add_column(:table, :foo, :integer, {:default=>0})
-> 95.8153s
== AddFooToTable: migrated (95.8155s) - Shard: master ===
== AddBarToTable: migrating - Shard: master =============
-- add_column(:table, :bar, :datetime)
-> 94.1172s
-- add_index(:table, :bar)
-> 10.6757s
== AddBarToTable: migrated (104.7929s) -- Shard: master =
```

### Round 2（20 million）

第二回合是 2000W+ 记录的表，更改其中一字段类型(varchar -> integer)：

``` bash
ALTER TABLE `table` MODIFY `foo` INTEGER;
```

有了上次的经历之后，这次已然是云淡风轻，耗时也不出意外：

``` bash
== ChangeFooTypeInTable: migrating - Shard: master ==============
-- change_column(:table, :foo, :integer)
-> 513.9101s
== ChangeFooTypeInTable: migrated (513.9102s) - Shard: master ===
```

### Round 3（80 million）

第三回合是 8000W+ 记录的表，增加一个字段和一个索引：

``` bash
ALTER TABLE `table` ADD COLUMN `foo` DATETIME DEFAULT NULL;
ALTER TABLE `table` ADD INDEX `index_table_on_foo` (`foo`);
```

这次的耗时远超预期……因为要避开高峰时段在凌晨操作，我是靠着 Xbox 的《Just Dance》和《Sports Rivals》才勉强坚持住。

好吧，来看一下耗时：

``` bash
== 20151228112810 AddFooToTable: migrating - Shard: master ===============
-- add_column(:table, :foo, :datetime)
-> 4313.0770s
-- add_index(:table, :foo)
-> 248.9602s
== 20151228112810 AddFooAtToTable: migrated (4562.0374s) - Shard: master =
```

### 其他

- 如果使用 MySQL 的 master-slave 模式, 同步到 slave 大概需要花费同样的时间（这块儿我们没有做特别具体的统计，只是 master 完成后人工检查 slave 情况得出的一个比较粗暴的结论）。

- MySQL 官方文档：[Overview of Online DDL](https://dev.mysql.com/doc/refman/5.6/en/innodb-create-index-overview.html)

__END__
