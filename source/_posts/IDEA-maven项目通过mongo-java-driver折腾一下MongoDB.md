---
title: IDEA maven项目通过mongo-java-driver折腾一下MongoDB
keywords: [java简单的操作mongodb,mongo-java-driver]
tags: [mongo,java]
date: 2017-02-20 15:21:26
categories: java
---
这几天折腾ubuntu然后安装了下mongodb

通过Oracle VM VirtualBox端口转发，连接了虚拟机的MongoDB

1.用idea创建一个maven项目

2.在pom.xml中添加mongodb java驱动
```
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb-driver</artifactId>
        <version>3.2.2</version>
    </dependency>
```
3.参考官方：<a href="http://mongodb.github.io/mongo-java-driver/3.2/driver/getting-started/quick-tour/" target="_blank">MongoDB Driver Quick Tour</a>

本用例github地址：<a href="https://github.com/hisen-yuan/mongodbTest/blob/master/src/main/java/com/hisen/jdbc/QuickTour.java" target="_blank">mongodbTest</a>

贴下代码：
<!--more-->
```
package com.hisen.jdbc;

import com.mongodb.Block;
import com.mongodb.MongoClient;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoCursor;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.result.DeleteResult;
import com.mongodb.client.result.UpdateResult;
import org.bson.Document;

import java.util.ArrayList;
import java.util.List;

import static com.mongodb.client.model.Accumulators.sum;
import static com.mongodb.client.model.Aggregates.group;
import static com.mongodb.client.model.Filters.*;
import static com.mongodb.client.model.Projections.excludeId;
import static com.mongodb.client.model.Sorts.descending;
import static com.mongodb.client.model.Updates.inc;
import static com.mongodb.client.model.Updates.set;
import static java.util.Collections.singletonList;

/**
 * Created by hisenyuan on 2017/2/20 at 13:42.
 */
public class QuickTour {
    public static void main(String[] args) {
        MongoClient mongoClient = new MongoClient("localhost", 27017);
        MongoDatabase database = mongoClient.getDatabase("mydb");
        //gets the collection test
        MongoCollection<Document> collection = database.getCollection("test");
        //插入数据
        //insert(collection);
        //插入大量数据
        insertMany(collection);
        //统计条数
        count(collection);
        //打印查询到的第一条数据
        print(collection);
        //打印所有数据
        findAll(collection);
        //按条件查询一条数据
        findOneWithFilter(collection);
        //按条件查询一个Set集合
        getSet(collection);
        //排序，输出第一条
        sortDocuments(collection);
        //获取指定的field
        projectingFields(collection);
        aggregations(collection);
        //更新
        update(collection);
        //删除数据
        delete(collection);
    }

    /**
     * 插入单个文档
     * @param collection
     */
    public static void insert(MongoCollection<Document> collection) {
        //create document
        Document doc = new Document("name", "MongoDB")
                .append("type", "database")
                .append("count", 1)
                .append("info", new Document("x", 203).append("y", 102));
        //insert the document into the collection
        collection.insertOne(doc);
    }

    /**
     * 一次性插入多条数据，用ArrayList拼装
     * @param collection
     */
    public static void insertMany(MongoCollection<Document> collection) {
        //Create the documents in a loop
        List<Document> documents = new ArrayList<Document>();
        for (int i = 0; i < 100; i++) {
            documents.add(new Document("i", i));
        }
        collection.insertMany(documents);
    }

    /**
     * 统计集合中的总条数
     * @param collection
     */
    public static void count(MongoCollection<Document> collection) {
        //Count Documents in A Collection
        System.out.println("总条数：" + collection.count());
    }

    /**
     * 打印查询到的第一条数据
     * @param collection
     */
    public static void print(MongoCollection<Document> collection) {
        //prints the first document found in the collection
        Document myDoc = collection.find().first();
        System.out.println(myDoc.toJson());
    }

    /**
     * 查询集合中所有的数据
     * 不推荐使用foreach循环
     * @param collection
     */
    public static void findAll(MongoCollection<Document> collection) {
        System.out.println("----------------输出所有数据----------------");
        MongoCursor<Document> cursor = collection.find().iterator();
        try {
            while (cursor.hasNext()) {
                System.out.println(cursor.next().toJson());
            }
        } finally {
            cursor.close();
        }
    }

    /**
     * 条件查询单条数据
     * @param collection
     */
    public static void findOneWithFilter(MongoCollection<Document> collection) {
        //Get A Single Document with a Query Filter
        Document myDoc = collection.find(eq("i", 71)).first();
        System.out.println(myDoc.toJson());
    }

    /**
     * 获得一个Set集合的数据
     * @param collection
     */
    public static void getSet(MongoCollection<Document> collection) {
        // now use a range query to get a larger subset
        Block<Document> printBlock = new Block<Document>() {
            public void apply(final Document document) {
                System.out.println(document.toJson());
            }
        };
        //i的值大于98的数据
        System.out.println("----------------i大于98的数据----------------");
        collection.find(gt("i", 98)).forEach(printBlock);
        //50 - 51 gt:大于 lte:小于等于
        System.out.println("----------------50<i<=51的数据----------------");
        collection.find(and(gt("i", 50), lte("i", 51))).forEach(printBlock);
    }

    /**
     * 排序
     * @param collection
     */
    public static void sortDocuments(MongoCollection<Document> collection) {
        //根据i的值进行排序
        Document myDoc = collection.find(exists("i")).sort(descending("i")).first();
        System.out.println("----------------排序，输出第一条----------------");
        System.out.println(myDoc.toJson());
    }

    /**
     * 获取想要的field(字段)
     * @param collection
     */
    public static void projectingFields(MongoCollection<Document> collection) {
        System.out.println("----------------获取指定field，输出第一条----------------");
        //排除ID字段
        Document myDoc = collection.find().projection(excludeId()).first();
        System.out.println(myDoc.toJson());
    }

    public static void aggregations(MongoCollection<Document> collection) {
        /*
        collection.aggregate(asList(
                match(gt("i", 0)),
                project(Document.parse("{ITimes10: {$multiply: ['$i', 10]}}")))
        ).forEach(printBlock);
        */
        //求和
        Document myDoc = collection.aggregate(singletonList(group(null, sum("total", "$i")))).first();
        System.out.println(myDoc.toJson());
    }

    /**
     * 更新document的方法
     * @param collection
     */
    public static void update(MongoCollection<Document> collection) {
        //更新一个，如果不引入下面这个包，set方法会报错
        //import static com.mongodb.client.model.Updates.*;
        collection.updateOne(eq("i", 10), set("i", 110));
        //更新多个，小于100的都加100
        UpdateResult updateResult = collection.updateMany(lt("i", 190), inc("i", 100));
        System.out.println("----------------更新条数----------------");
        System.out.println(updateResult.getModifiedCount());
    }

    /**
     * 删除数据的方法
     * @param collection
     */
    public static void delete(MongoCollection<Document> collection) {
        collection.deleteOne(eq("i", 210));
        //gte 大于等于100的都删除
        DeleteResult deleteResult = collection.deleteMany(gte("i", 100));
        System.out.println("----------------删除条数----------------");
        System.out.println(deleteResult.getDeletedCount());
    }
    /*
    public static void bulk(MongoCollection<Document> collection) {
        // 2. Ordered bulk operation - order is guarenteed
        collection.bulkWrite(
                Arrays.asList(new InsertOneModel<>(new Document("_id", 4)),
                        new InsertOneModel<>(new Document("_id", 5)),
                        new InsertOneModel<>(new Document("_id", 6)),
                        new UpdateOneModel<>(new Document("_id", 1),
                                new Document("$set", new Document("x", 2))),
                        new DeleteOneModel<>(new Document("_id", 2)),
                        new ReplaceOneModel<>(new Document("_id", 3),
                                new Document("_id", 3).append("x", 4))));
        // 2. Unordered bulk operation - no guarantee of order of operation
        collection.bulkWrite(
                Arrays.asList(new InsertOneModel<>(new Document("_id", 4)),
                        new InsertOneModel<>(new Document("_id", 5)),
                        new InsertOneModel<>(new Document("_id", 6)),
                        new UpdateOneModel<>(new Document("_id", 1),
                                new Document("$set", new Document("x", 2))),
                        new DeleteOneModel<>(new Document("_id", 2)),
                        new ReplaceOneModel<>(new Document("_id", 3),
                                new Document("_id", 3).append("x", 4))),
                new BulkWriteOptions().ordered(false));
    }
    */
}
```