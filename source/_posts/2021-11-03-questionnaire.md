---
title: questionnaire
date: 2021-11-03 20:14:55
tags: questionnaire
---

一个做问卷的项目，准备用 nodejs+mongodb+vue

# mongodb

数据库采用 mongodb，原因是与 nodejs 能较好的结合，工具或者示例要多一些

## 对应概念
![](/public/images/questionnaire1.png)

## 常用命令
* 新建数据库： use testdb
* 显示全部数据库： show dbs
* 删除数据库： db.dropDatabase()
* 创建集合： 
    * db.createCollection(name, options)
    * db.testConllection.insert 时没有集合会自动创建
* 显示全部集合： show collections
* 删除集合： db.collection.drop()
* 插入文档
    ```
        * db.testConllection.insert(document)
        * db.collection.insertOne(
            \<document\>,
            {
                writeConcern: \<document\>
            }
        )
        * db.collection.insertMany(
            [ \<document 1> , \<document 2>, ... ],
            {
                writeConcern: \<document>,
                ordered: \<boolean>
            }
        )
    ```
* 更新文档
    ```
        * db.collection.update(
            <query>,
            <update>,
            {
                upsert: <boolean>,
                multi: <boolean>,
                writeConcern: <document>
            }
        )
    ```
* 
