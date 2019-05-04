# InfluxDB 功能介绍(二)

## 功能

### Retention policy (保留策略)

设置数据保存周期。默认安装的InfluxDB，在创建数据库的时候都会自动创建一个名为'autogen'的保留策略，表示数据一直保留。可以在系统的配置文件中，关闭这个功能。

语法：

```
```

### Continues Query (CQ)

CQ是预先配置的命令，我们使用CQ可以实现数据的downsample。

语法：

```
CREATE CONTINUOUS QUERY <cq_name> ON <database_name>
BEGIN
    SELECT <function[s]> INTO <destination_measurement> FROM <measurement> [WHERE <stuff>] GROUP BY time(<interval>)[,<tag_key[s]>]
END
```

样例：
```
CREATE CONTINUOUS QUERY "cq_basic" ON "transportation"
BEGIN
  SELECT mean("passengers") INTO "average_passengers" FROM "bus_data" GROUP BY time(1h)
END
```

#### 支持的功能

1. 数据自动将精度
```
CREATE CONTINUOUS QUERY "cq_basic" ON "transportation"
BEGIN
  SELECT mean("passengers") INTO "average_passengers" FROM "bus_data" GROUP BY time(1h)
END
```

2. 数据降精度转存到另外一个保留策略中
```
CREATE CONTINUOUS QUERY "cq_basic_rp" ON "transportation"
BEGIN
  SELECT mean("passengers") INTO "transportation"."three_weeks"."average_passengers" FROM "bus_data" GROUP BY time(1h)
END
```

3. 支持整个数据库
```
CREATE CONTINUOUS QUERY "cq_basic_br" ON "transportation"
BEGIN
  SELECT mean(*) INTO "downsampled_transportation"."autogen".:MEASUREMENT FROM /.*/ GROUP BY time(30m),*
END
```

4. 支持指定时间边界触发CQ
```
CREATE CONTINUOUS QUERY "cq_basic_offset" ON "transportation"
BEGIN
  SELECT mean("passengers") INTO "average_passengers" FROM "bus_data" GROUP BY time(1h,15m)
END

```

#### 降精度支持的function：


### Shard/Shard Group

Shard和我们常见的NoSQL的分区功能一致，都是将数据按照特定的规则分隔开来

## 核心存储 - TSM


