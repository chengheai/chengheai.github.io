---
title: MongoDB导入与导出
date: 2018-10-24 15:50:48
tags: mongoDB
---

## 导出

### 概念

mongoDB 中的 mongoexport 工具可以把一个 collection 导出成 JSON 格式或 CSV 格式的文件。可以通过参数指定导出的数据项，也可以根据指定的条件导出数据。

### 语法

``` JSON
mongoexport -d dbname -c collectionname -o file --type json/csv -f field
        参数说明：
            -d ：数据库名
            -c ：collection名
            -o ：输出的路径和文件名
            --type ： 输出的格式，默认为json
            -f ：输出的字段，如果-type为csv，则需要加上-f "字段名"
```

### 例子

``` bash
➜  mongodb-vue git:(master) ✗ mongoexport -d lolTest -c myhero -o /Users/chengheai/Desktop/a.json --type json
2018-10-24T15:41:56.489+0800	connected to: localhost
2018-10-24T15:41:56.496+0800	exported 12 records
```

## 导入

### 语法

``` JSON
    mongoimport -d dbname -c collectionname --file filename --headerline --type json/csv -f field
   参数说明：
            -d ：数据库名
            -c ：collection名
            --type ：导入的格式默认json
            -f ：导入的字段名
            --headerline ：如果导入的格式是csv，则可以使用第一行的标题作为导入的字段
            --file ：要导入的文件
```

### 例子

``` bash
➜  mongodb-vue git:(master) ✗ mongoimport -d mgDbs -c myhero --file /Users/chengheai/Desktop/a.json  --type json
2018-10-24T15:47:09.209+0800	connected to: localhost
2018-10-24T15:47:09.273+0800	imported 12 documents
```
## 备份
```
mongodump -h host -d dbname -o directory
mongodump -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -o 文件存在路径
Eg： # mongodump -d test -o /data/
参数说明：
如果想导出所有数据库，可以去掉-d
-h：MongDB所在服务器地址，如：127.0.0.1，也可以指定端口号：127.0.0.1:27017
-d：需要备份的数据库名称，如：db_test
-o：备份的数据存放位置，如：~\dump，当然该目录需要提前建立，在备份完成后，系统自动在dump目录下建立一个db_test目录，这个目录里面存放该数据库实例的备份数据。
```
## 恢复
### 概念
mongorestore是Mongodb从备份中恢复数据的工具，它主要用来获取mongodump的输出结果，并将备份的数据插入到运行的Mongodb中。
```
mongorestore -h host -d dbname --directoryperdb dbdirectory
Eg:  # mongorestore --host=10.0.0.25 --port=27017 --db ztjy --dir=ztjy/
参数说明：
-h：MongoDB所在服务器地址
-d：需要恢复的数据库名称，如：db_test，当然这个名称可以不同于备份的时候，比如new_db
--directoryperdb：备份数据文件所在位置，如：~\dump\db_test（这里之所以要加db_test子目录，从mongoretore的help中的--directoryperdb，可以读出“每一个db在一个单独的目录”。）
```
