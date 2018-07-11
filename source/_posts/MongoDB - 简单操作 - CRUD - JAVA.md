---
title: MongoDB - 简单操作 - CRUD - JAVA
keywords: [MongoDB,MongoDB CRUD]
date: 2017/7/11 23:55
tags: [mongodb]
categories: sql
---
之前我记得写过简单的测试类，但是忘了，现在重新写一个

顺便用博客记录下，mongodb系列应该会有几篇记录

本篇具体代码：<a href="https://github.com/hisen-yuan/IDEAPractice/blob/master/src/main/java/com/hisen/databases/mongodb/SampleMongoTestNo1.java" target="_blank">SampleMongoTestNo1.java</a>


1、环境介绍

mongodb安装教程：<a href="/20170219-Ubuntu16-04使用阿里云镜像安装Mongodb-配置/" target="_blank">点击查看</a>
```
DataBase：MongoDB V 3.2
Driver　：3.4.1
maven：
    <dependency>
      <groupId>org.mongodb</groupId>
      <artifactId>mongo-java-driver</artifactId>
      <version>3.4.1</version>
    </dependency>
```
２、初始化连接
```
  private Mongo mg = null;
  private DB db;
  private DBCollection dbCollection;

  @Before
  public void initDB(){
    //建立连接
    mg = new MongoClient("127.0.0.1",27017);
    //获取要操作的数据库实例，没有会创建
    db = mg.getDB("hisen");
    //获取要操作的集合实例，没有会创建
    dbCollection = db.getCollection("emp");
  }
```
3、关闭连接
```
@After
  public void destoryDB(){
    if (mg == null) {
      mg.close();
      mg=null;
      db=null;
      dbCollection=null;
    }
  }
```
4、CRUD操作
<!--more-->
```
/**
   * 插入数据
   */
  @Test
  public void testCreate(){
    DBObject obj = null;
    for (int i = 1; i <= 10; i++) {
      obj = new BasicDBObject("_id",i).append("name","hisen"+i).append("age",i*5);
      dbCollection.save(obj);
    }
  }

  /**
   * 查询所有
   */
  @Test
  public void testReadAll(){
    DBCursor dbCursor = dbCollection.find();
    while (dbCursor.hasNext()){
      System.out.println(dbCursor.next());
    }
  }

  /**
   * 修改记录
   */
  @Test
  public void testUpdate(){
    BasicDBObject condition = new BasicDBObject("_id",10);
    BasicDBObject res = new BasicDBObject("name", "hisen10_new");
    //若没有此语句，直接调用下面的语句，返回结果{ "_id" : 10 , "name" : "hisen10_new"}
    BasicDBObject res2 = new BasicDBObject("$set", res);
    dbCollection.update(condition,res2);
    System.out.println(dbCollection.findOne(new BasicDBObject("_id",10)));
  }

  /**
   * 删除记录
   */
  @Test
  public void testDelete(){
    dbCollection.remove(new BasicDBObject("_id",10));
    testReadAll();
  }

  /**
   * 根据主键查询
   */
  @Test
  public void testReadOneWithId(){
    DBObject object = dbCollection.findOne(new BasicDBObject("_id",1));
    System.out.println(object);
  }

  /**
   * 模糊查询 - 使用正则
   */
  @Test
  public void testReadPuzzy(){
    Pattern pattern = Pattern.compile("^hisen1");
    BasicDBObject basicDBObject = new BasicDBObject("name",pattern);
    DBCursor dbCursor = dbCollection.find(basicDBObject);
    while (dbCursor.hasNext()){
      System.out.println(dbCursor.next());
    }
  }

  /**
   * 清空集合
   */
  @Test
  public void testDrop(){
    dbCollection.drop();
    testReadAll();
  }
```