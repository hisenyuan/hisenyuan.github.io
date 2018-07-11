---
title: Spark第一个java程序 - 字词统计 - Spark java idea
keywords: [Spark java idea,spark java开发,spark java api 入门]
date: 2017/8/2 17:01
tags: [java,spark,BigData]
categories: spark
---
在本地利用idea，java开发spark程序

原来并不用安装spark什么的这些东西

这样就不会那么繁琐，门槛也低了点

具体过程如下

一、创建工程
--
1. idea -> new Project -> maven -> create from archetype -> maven-archetype-quickstart
2. pom.xml添加依赖
```
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-core_2.11</artifactId>
      <version>2.0.1</version>
    </dependency>
```
3. 编写java代码
```
package com.hisen.spark;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.api.java.function.Function;

/**
 * 第一个spark程序
 * Created by hisenyuan on 2017/8/2 at 16:36.
 */
public class SimpleApp {

  public static void main(String[] args) {
    // Should be some file on your system
    String logFile = "D:\\logs\\boss_debug.log";
    //设置本地运行，设置名称（在spark web ui上显示）
    SparkConf sparkConf = new SparkConf().setMaster("local").setAppName("Simple Application");
    JavaSparkContext javaSparkContext = new JavaSparkContext(sparkConf);
    JavaRDD<String> logData = javaSparkContext.textFile(logFile).cache();

    //统计包含a的次数
    long countA = logData.filter(new Function<String, Boolean>() {
      public Boolean call(String s) throws Exception {
        return s.contains("a");
      }
    }).count();

    //统计包含b的次数
    long countB = logData.filter(new Function<String, Boolean>() {
      public Boolean call(String s) throws Exception {
        return s.contains("b");
      }
    }).count();
    System.out.printf("Lines with a: %d, lines with b: %d\n", countA, countB);
    //Lines with a: 11146, lines with b: 10760
  }
}
```
4. 运行main方法：结果：Lines with a: 11146, lines with b: 10760 
5. 日志如下：
```
C:\hisenwork\soft\jdk8\bin\java -Dspark.master=local "-javaagent:C:\hisenwork\IntelliJ IDEA 2017.1.1\lib\idea_rt.jar=59366:C:\hisenwork\IntelliJ IDEA 2017.1.1\bin" -Dfile.encoding=UTF-8 -classpath C:\hisenwork\soft\jdk8\jre\lib\charsets.jar;C:\hisenwork\soft\jdk8\jre\lib\deploy.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\access-bridge-64.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\cldrdata.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\dnsns.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\jaccess.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\jfxrt.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\localedata.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\nashorn.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\sunec.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\sunjce_provider.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\sunmscapi.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\sunpkcs11.jar;C:\hisenwork\soft\jdk8\jre\lib\ext\zipfs.jar;C:\hisenwork\soft\jdk8\jre\lib\javaws.jar;C:\hisenwork\soft\jdk8\jre\lib\jce.jar;C:\hisenwork\soft\jdk8\jre\lib\jfr.jar;C:\hisenwork\soft\jdk8\jre\lib\jfxswt.jar;C:\hisenwork\soft\jdk8\jre\lib\jsse.jar;C:\hisenwork\soft\jdk8\jre\lib\management-agent.jar;C:\hisenwork\soft\jdk8\jre\lib\plugin.jar;C:\hisenwork\soft\jdk8\jre\lib\resources.jar;C:\hisenwork\soft\jdk8\jre\lib\rt.jar;C:\hisenwork\code\rmitec\SparkTest\target\classes;C:\hisenwork\soft\maven\org\apache\spark\spark-core_2.11\2.0.1\spark-core_2.11-2.0.1.jar;C:\hisenwork\soft\maven\org\apache\avro\avro-mapred\1.7.7\avro-mapred-1.7.7-hadoop2.jar;C:\hisenwork\soft\maven\org\apache\avro\avro-ipc\1.7.7\avro-ipc-1.7.7.jar;C:\hisenwork\soft\maven\org\apache\avro\avro\1.7.7\avro-1.7.7.jar;C:\hisenwork\soft\maven\org\apache\avro\avro-ipc\1.7.7\avro-ipc-1.7.7-tests.jar;C:\hisenwork\soft\maven\org\codehaus\jackson\jackson-core-asl\1.9.13\jackson-core-asl-1.9.13.jar;C:\hisenwork\soft\maven\org\codehaus\jackson\jackson-mapper-asl\1.9.13\jackson-mapper-asl-1.9.13.jar;C:\hisenwork\soft\maven\com\twitter\chill_2.11\0.8.0\chill_2.11-0.8.0.jar;C:\hisenwork\soft\maven\com\esotericsoftware\kryo-shaded\3.0.3\kryo-shaded-3.0.3.jar;C:\hisenwork\soft\maven\com\esotericsoftware\minlog\1.3.0\minlog-1.3.0.jar;C:\hisenwork\soft\maven\org\objenesis\objenesis\2.1\objenesis-2.1.jar;C:\hisenwork\soft\maven\com\twitter\chill-java\0.8.0\chill-java-0.8.0.jar;C:\hisenwork\soft\maven\org\apache\xbean\xbean-asm5-shaded\4.4\xbean-asm5-shaded-4.4.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-client\2.2.0\hadoop-client-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-common\2.2.0\hadoop-common-2.2.0.jar;C:\hisenwork\soft\maven\commons-cli\commons-cli\1.2\commons-cli-1.2.jar;C:\hisenwork\soft\maven\org\apache\commons\commons-math\2.1\commons-math-2.1.jar;C:\hisenwork\soft\maven\xmlenc\xmlenc\0.52\xmlenc-0.52.jar;C:\hisenwork\soft\maven\commons-io\commons-io\2.1\commons-io-2.1.jar;C:\hisenwork\soft\maven\commons-lang\commons-lang\2.5\commons-lang-2.5.jar;C:\hisenwork\soft\maven\commons-configuration\commons-configuration\1.6\commons-configuration-1.6.jar;C:\hisenwork\soft\maven\commons-collections\commons-collections\3.2.1\commons-collections-3.2.1.jar;C:\hisenwork\soft\maven\commons-digester\commons-digester\1.8\commons-digester-1.8.jar;C:\hisenwork\soft\maven\commons-beanutils\commons-beanutils\1.7.0\commons-beanutils-1.7.0.jar;C:\hisenwork\soft\maven\commons-beanutils\commons-beanutils-core\1.8.0\commons-beanutils-core-1.8.0.jar;C:\hisenwork\soft\maven\com\google\protobuf\protobuf-java\2.5.0\protobuf-java-2.5.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-auth\2.2.0\hadoop-auth-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\commons\commons-compress\1.4.1\commons-compress-1.4.1.jar;C:\hisenwork\soft\maven\org\tukaani\xz\1.0\xz-1.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-hdfs\2.2.0\hadoop-hdfs-2.2.0.jar;C:\hisenwork\soft\maven\org\mortbay\jetty\jetty-util\6.1.26\jetty-util-6.1.26.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-mapreduce-client-app\2.2.0\hadoop-mapreduce-client-app-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-mapreduce-client-common\2.2.0\hadoop-mapreduce-client-common-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-yarn-client\2.2.0\hadoop-yarn-client-2.2.0.jar;C:\hisenwork\soft\maven\com\google\inject\guice\3.0\guice-3.0.jar;C:\hisenwork\soft\maven\javax\inject\javax.inject\1\javax.inject-1.jar;C:\hisenwork\soft\maven\aopalliance\aopalliance\1.0\aopalliance-1.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-yarn-server-common\2.2.0\hadoop-yarn-server-common-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-mapreduce-client-shuffle\2.2.0\hadoop-mapreduce-client-shuffle-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-yarn-api\2.2.0\hadoop-yarn-api-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-mapreduce-client-core\2.2.0\hadoop-mapreduce-client-core-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-yarn-common\2.2.0\hadoop-yarn-common-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-mapreduce-client-jobclient\2.2.0\hadoop-mapreduce-client-jobclient-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\hadoop\hadoop-annotations\2.2.0\hadoop-annotations-2.2.0.jar;C:\hisenwork\soft\maven\org\apache\spark\spark-launcher_2.11\2.0.1\spark-launcher_2.11-2.0.1.jar;C:\hisenwork\soft\maven\org\apache\spark\spark-network-common_2.11\2.0.1\spark-network-common_2.11-2.0.1.jar;C:\hisenwork\soft\maven\org\fusesource\leveldbjni\leveldbjni-all\1.8\leveldbjni-all-1.8.jar;C:\hisenwork\soft\maven\com\fasterxml\jackson\core\jackson-annotations\2.6.5\jackson-annotations-2.6.5.jar;C:\hisenwork\soft\maven\org\apache\spark\spark-network-shuffle_2.11\2.0.1\spark-network-shuffle_2.11-2.0.1.jar;C:\hisenwork\soft\maven\org\apache\spark\spark-unsafe_2.11\2.0.1\spark-unsafe_2.11-2.0.1.jar;C:\hisenwork\soft\maven\net\java\dev\jets3t\jets3t\0.7.1\jets3t-0.7.1.jar;C:\hisenwork\soft\maven\commons-codec\commons-codec\1.3\commons-codec-1.3.jar;C:\hisenwork\soft\maven\commons-httpclient\commons-httpclient\3.1\commons-httpclient-3.1.jar;C:\hisenwork\soft\maven\org\apache\curator\curator-recipes\2.4.0\curator-recipes-2.4.0.jar;C:\hisenwork\soft\maven\org\apache\curator\curator-framework\2.4.0\curator-framework-2.4.0.jar;C:\hisenwork\soft\maven\org\apache\curator\curator-client\2.4.0\curator-client-2.4.0.jar;C:\hisenwork\soft\maven\org\apache\zookeeper\zookeeper\3.4.5\zookeeper-3.4.5.jar;C:\hisenwork\soft\maven\com\google\guava\guava\14.0.1\guava-14.0.1.jar;C:\hisenwork\soft\maven\javax\servlet\javax.servlet-api\3.1.0\javax.servlet-api-3.1.0.jar;C:\hisenwork\soft\maven\org\apache\commons\commons-lang3\3.3.2\commons-lang3-3.3.2.jar;C:\hisenwork\soft\maven\org\apache\commons\commons-math3\3.4.1\commons-math3-3.4.1.jar;C:\hisenwork\soft\maven\com\google\code\findbugs\jsr305\1.3.9\jsr305-1.3.9.jar;C:\hisenwork\soft\maven\org\slf4j\slf4j-api\1.7.16\slf4j-api-1.7.16.jar;C:\hisenwork\soft\maven\org\slf4j\jul-to-slf4j\1.7.16\jul-to-slf4j-1.7.16.jar;C:\hisenwork\soft\maven\org\slf4j\jcl-over-slf4j\1.7.16\jcl-over-slf4j-1.7.16.jar;C:\hisenwork\soft\maven\log4j\log4j\1.2.17\log4j-1.2.17.jar;C:\hisenwork\soft\maven\org\slf4j\slf4j-log4j12\1.7.16\slf4j-log4j12-1.7.16.jar;C:\hisenwork\soft\maven\com\ning\compress-lzf\1.0.3\compress-lzf-1.0.3.jar;C:\hisenwork\soft\maven\org\xerial\snappy\snappy-java\1.1.2.6\snappy-java-1.1.2.6.jar;C:\hisenwork\soft\maven\net\jpountz\lz4\lz4\1.3.0\lz4-1.3.0.jar;C:\hisenwork\soft\maven\org\roaringbitmap\RoaringBitmap\0.5.11\RoaringBitmap-0.5.11.jar;C:\hisenwork\soft\maven\commons-net\commons-net\2.2\commons-net-2.2.jar;C:\hisenwork\soft\maven\org\scala-lang\scala-library\2.11.8\scala-library-2.11.8.jar;C:\hisenwork\soft\maven\org\json4s\json4s-jackson_2.11\3.2.11\json4s-jackson_2.11-3.2.11.jar;C:\hisenwork\soft\maven\org\json4s\json4s-core_2.11\3.2.11\json4s-core_2.11-3.2.11.jar;C:\hisenwork\soft\maven\org\json4s\json4s-ast_2.11\3.2.11\json4s-ast_2.11-3.2.11.jar;C:\hisenwork\soft\maven\com\thoughtworks\paranamer\paranamer\2.6\paranamer-2.6.jar;C:\hisenwork\soft\maven\org\scala-lang\scalap\2.11.0\scalap-2.11.0.jar;C:\hisenwork\soft\maven\org\scala-lang\scala-compiler\2.11.0\scala-compiler-2.11.0.jar;C:\hisenwork\soft\maven\org\scala-lang\modules\scala-parser-combinators_2.11\1.0.1\scala-parser-combinators_2.11-1.0.1.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\core\jersey-client\2.22.2\jersey-client-2.22.2.jar;C:\hisenwork\soft\maven\javax\ws\rs\javax.ws.rs-api\2.0.1\javax.ws.rs-api-2.0.1.jar;C:\hisenwork\soft\maven\org\glassfish\hk2\hk2-api\2.4.0-b34\hk2-api-2.4.0-b34.jar;C:\hisenwork\soft\maven\org\glassfish\hk2\hk2-utils\2.4.0-b34\hk2-utils-2.4.0-b34.jar;C:\hisenwork\soft\maven\org\glassfish\hk2\external\aopalliance-repackaged\2.4.0-b34\aopalliance-repackaged-2.4.0-b34.jar;C:\hisenwork\soft\maven\org\glassfish\hk2\external\javax.inject\2.4.0-b34\javax.inject-2.4.0-b34.jar;C:\hisenwork\soft\maven\org\glassfish\hk2\hk2-locator\2.4.0-b34\hk2-locator-2.4.0-b34.jar;C:\hisenwork\soft\maven\org\javassist\javassist\3.18.1-GA\javassist-3.18.1-GA.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\core\jersey-common\2.22.2\jersey-common-2.22.2.jar;C:\hisenwork\soft\maven\javax\annotation\javax.annotation-api\1.2\javax.annotation-api-1.2.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\bundles\repackaged\jersey-guava\2.22.2\jersey-guava-2.22.2.jar;C:\hisenwork\soft\maven\org\glassfish\hk2\osgi-resource-locator\1.0.1\osgi-resource-locator-1.0.1.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\core\jersey-server\2.22.2\jersey-server-2.22.2.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\media\jersey-media-jaxb\2.22.2\jersey-media-jaxb-2.22.2.jar;C:\hisenwork\soft\maven\javax\validation\validation-api\1.1.0.Final\validation-api-1.1.0.Final.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\containers\jersey-container-servlet\2.22.2\jersey-container-servlet-2.22.2.jar;C:\hisenwork\soft\maven\org\glassfish\jersey\containers\jersey-container-servlet-core\2.22.2\jersey-container-servlet-core-2.22.2.jar;C:\hisenwork\soft\maven\org\apache\mesos\mesos\0.21.1\mesos-0.21.1-shaded-protobuf.jar;C:\hisenwork\soft\maven\io\netty\netty-all\4.0.29.Final\netty-all-4.0.29.Final.jar;C:\hisenwork\soft\maven\io\netty\netty\3.8.0.Final\netty-3.8.0.Final.jar;C:\hisenwork\soft\maven\com\clearspring\analytics\stream\2.7.0\stream-2.7.0.jar;C:\hisenwork\soft\maven\io\dropwizard\metrics\metrics-core\3.1.2\metrics-core-3.1.2.jar;C:\hisenwork\soft\maven\io\dropwizard\metrics\metrics-jvm\3.1.2\metrics-jvm-3.1.2.jar;C:\hisenwork\soft\maven\io\dropwizard\metrics\metrics-json\3.1.2\metrics-json-3.1.2.jar;C:\hisenwork\soft\maven\io\dropwizard\metrics\metrics-graphite\3.1.2\metrics-graphite-3.1.2.jar;C:\hisenwork\soft\maven\com\fasterxml\jackson\core\jackson-databind\2.6.5\jackson-databind-2.6.5.jar;C:\hisenwork\soft\maven\com\fasterxml\jackson\core\jackson-core\2.6.5\jackson-core-2.6.5.jar;C:\hisenwork\soft\maven\com\fasterxml\jackson\module\jackson-module-scala_2.11\2.6.5\jackson-module-scala_2.11-2.6.5.jar;C:\hisenwork\soft\maven\org\scala-lang\scala-reflect\2.11.7\scala-reflect-2.11.7.jar;C:\hisenwork\soft\maven\com\fasterxml\jackson\module\jackson-module-paranamer\2.6.5\jackson-module-paranamer-2.6.5.jar;C:\hisenwork\soft\maven\org\apache\ivy\ivy\2.4.0\ivy-2.4.0.jar;C:\hisenwork\soft\maven\oro\oro\2.0.8\oro-2.0.8.jar;C:\hisenwork\soft\maven\net\razorvine\pyrolite\4.9\pyrolite-4.9.jar;C:\hisenwork\soft\maven\net\sf\py4j\py4j\0.10.3\py4j-0.10.3.jar;C:\hisenwork\soft\maven\org\apache\spark\spark-tags_2.11\2.0.1\spark-tags_2.11-2.0.1.jar;C:\hisenwork\soft\maven\org\scalatest\scalatest_2.11\2.2.6\scalatest_2.11-2.2.6.jar;C:\hisenwork\soft\maven\org\scala-lang\modules\scala-xml_2.11\1.0.2\scala-xml_2.11-1.0.2.jar;C:\hisenwork\soft\maven\org\spark-project\spark\unused\1.0.0\unused-1.0.0.jar com.hisen.spark.SimpleApp
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
17/08/02 17:00:09 INFO SparkContext: Running Spark version 2.0.1
17/08/02 17:00:10 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/08/02 17:00:10 INFO SecurityManager: Changing view acls to: Administrator
17/08/02 17:00:10 INFO SecurityManager: Changing modify acls to: Administrator
17/08/02 17:00:10 INFO SecurityManager: Changing view acls groups to: 
17/08/02 17:00:10 INFO SecurityManager: Changing modify acls groups to: 
17/08/02 17:00:10 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users  with view permissions: Set(Administrator); groups with view permissions: Set(); users  with modify permissions: Set(Administrator); groups with modify permissions: Set()
17/08/02 17:00:11 INFO Utils: Successfully started service 'sparkDriver' on port 59388.
17/08/02 17:00:11 INFO SparkEnv: Registering MapOutputTracker
17/08/02 17:00:11 INFO SparkEnv: Registering BlockManagerMaster
17/08/02 17:00:11 INFO DiskBlockManager: Created local directory at C:\Users\Administrator\AppData\Local\Temp\blockmgr-630d8294-e5cb-428d-8189-4c3313e46fa3
17/08/02 17:00:12 INFO MemoryStore: MemoryStore started with capacity 894.3 MB
17/08/02 17:00:12 INFO SparkEnv: Registering OutputCommitCoordinator
17/08/02 17:00:12 INFO Utils: Successfully started service 'SparkUI' on port 4040.
17/08/02 17:00:12 INFO SparkUI: Bound SparkUI to 0.0.0.0, and started at http://169.254.90.236:4040
17/08/02 17:00:12 INFO Executor: Starting executor ID driver on host localhost
17/08/02 17:00:12 INFO Utils: Successfully started service 'org.apache.spark.network.netty.NettyBlockTransferService' on port 59397.
17/08/02 17:00:12 INFO NettyBlockTransferService: Server created on 169.254.90.236:59397
17/08/02 17:00:12 INFO BlockManagerMaster: Registering BlockManager BlockManagerId(driver, 169.254.90.236, 59397)
17/08/02 17:00:12 INFO BlockManagerMasterEndpoint: Registering block manager 169.254.90.236:59397 with 894.3 MB RAM, BlockManagerId(driver, 169.254.90.236, 59397)
17/08/02 17:00:12 INFO BlockManagerMaster: Registered BlockManager BlockManagerId(driver, 169.254.90.236, 59397)
17/08/02 17:00:14 INFO MemoryStore: Block broadcast_0 stored as values in memory (estimated size 127.1 KB, free 894.2 MB)
17/08/02 17:00:14 INFO MemoryStore: Block broadcast_0_piece0 stored as bytes in memory (estimated size 14.3 KB, free 894.2 MB)
17/08/02 17:00:14 INFO BlockManagerInfo: Added broadcast_0_piece0 in memory on 169.254.90.236:59397 (size: 14.3 KB, free: 894.3 MB)
17/08/02 17:00:14 INFO SparkContext: Created broadcast 0 from textFile at SimpleApp.java:19
17/08/02 17:00:14 ERROR Shell: Failed to locate the winutils binary in the hadoop binary path
java.io.IOException: Could not locate executable null\bin\winutils.exe in the Hadoop binaries.
	at org.apache.hadoop.util.Shell.getQualifiedBinPath(Shell.java:278)
	at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:300)
	at org.apache.hadoop.util.Shell.<clinit>(Shell.java:293)
	at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:76)
	at org.apache.hadoop.mapred.FileInputFormat.setInputPaths(FileInputFormat.java:362)
	at org.apache.spark.SparkContext$$anonfun$hadoopFile$1$$anonfun$29.apply(SparkContext.scala:992)
	at org.apache.spark.SparkContext$$anonfun$hadoopFile$1$$anonfun$29.apply(SparkContext.scala:992)
	at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:176)
	at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:176)
	at scala.Option.map(Option.scala:146)
	at org.apache.spark.rdd.HadoopRDD.getJobConf(HadoopRDD.scala:176)
	at org.apache.spark.rdd.HadoopRDD.getPartitions(HadoopRDD.scala:195)
	at org.apache.spark.rdd.RDD$$anonfun$partitions$2.apply(RDD.scala:248)
	at org.apache.spark.rdd.RDD$$anonfun$partitions$2.apply(RDD.scala:246)
	at scala.Option.getOrElse(Option.scala:121)
	at org.apache.spark.rdd.RDD.partitions(RDD.scala:246)
	at org.apache.spark.rdd.MapPartitionsRDD.getPartitions(MapPartitionsRDD.scala:35)
	at org.apache.spark.rdd.RDD$$anonfun$partitions$2.apply(RDD.scala:248)
	at org.apache.spark.rdd.RDD$$anonfun$partitions$2.apply(RDD.scala:246)
	at scala.Option.getOrElse(Option.scala:121)
	at org.apache.spark.rdd.RDD.partitions(RDD.scala:246)
	at org.apache.spark.rdd.MapPartitionsRDD.getPartitions(MapPartitionsRDD.scala:35)
	at org.apache.spark.rdd.RDD$$anonfun$partitions$2.apply(RDD.scala:248)
	at org.apache.spark.rdd.RDD$$anonfun$partitions$2.apply(RDD.scala:246)
	at scala.Option.getOrElse(Option.scala:121)
	at org.apache.spark.rdd.RDD.partitions(RDD.scala:246)
	at org.apache.spark.SparkContext.runJob(SparkContext.scala:1930)
	at org.apache.spark.rdd.RDD.count(RDD.scala:1134)
	at org.apache.spark.api.java.JavaRDDLike$class.count(JavaRDDLike.scala:454)
	at org.apache.spark.api.java.AbstractJavaRDDLike.count(JavaRDDLike.scala:45)
	at com.hisen.spark.SimpleApp.main(SimpleApp.java:26)
17/08/02 17:00:14 INFO FileInputFormat: Total input paths to process : 1
17/08/02 17:00:14 INFO SparkContext: Starting job: count at SimpleApp.java:26
17/08/02 17:00:15 INFO DAGScheduler: Got job 0 (count at SimpleApp.java:26) with 1 output partitions
17/08/02 17:00:15 INFO DAGScheduler: Final stage: ResultStage 0 (count at SimpleApp.java:26)
17/08/02 17:00:15 INFO DAGScheduler: Parents of final stage: List()
17/08/02 17:00:15 INFO DAGScheduler: Missing parents: List()
17/08/02 17:00:15 INFO DAGScheduler: Submitting ResultStage 0 (MapPartitionsRDD[2] at filter at SimpleApp.java:22), which has no missing parents
17/08/02 17:00:15 INFO MemoryStore: Block broadcast_1 stored as values in memory (estimated size 3.2 KB, free 894.2 MB)
17/08/02 17:00:15 INFO MemoryStore: Block broadcast_1_piece0 stored as bytes in memory (estimated size 1966.0 B, free 894.2 MB)
17/08/02 17:00:15 INFO BlockManagerInfo: Added broadcast_1_piece0 in memory on 169.254.90.236:59397 (size: 1966.0 B, free: 894.3 MB)
17/08/02 17:00:15 INFO SparkContext: Created broadcast 1 from broadcast at DAGScheduler.scala:1012
17/08/02 17:00:15 INFO DAGScheduler: Submitting 1 missing tasks from ResultStage 0 (MapPartitionsRDD[2] at filter at SimpleApp.java:22)
17/08/02 17:00:15 INFO TaskSchedulerImpl: Adding task set 0.0 with 1 tasks
17/08/02 17:00:15 INFO TaskSetManager: Starting task 0.0 in stage 0.0 (TID 0, localhost, partition 0, PROCESS_LOCAL, 5324 bytes)
17/08/02 17:00:15 INFO Executor: Running task 0.0 in stage 0.0 (TID 0)
17/08/02 17:00:15 INFO HadoopRDD: Input split: file:/D:/logs/boss_debug.log:0+4059273
17/08/02 17:00:15 INFO deprecation: mapred.tip.id is deprecated. Instead, use mapreduce.task.id
17/08/02 17:00:15 INFO deprecation: mapred.task.id is deprecated. Instead, use mapreduce.task.attempt.id
17/08/02 17:00:15 INFO deprecation: mapred.task.is.map is deprecated. Instead, use mapreduce.task.ismap
17/08/02 17:00:15 INFO deprecation: mapred.task.partition is deprecated. Instead, use mapreduce.task.partition
17/08/02 17:00:15 INFO deprecation: mapred.job.id is deprecated. Instead, use mapreduce.job.id
17/08/02 17:00:15 INFO MemoryStore: Block rdd_1_0 stored as values in memory (estimated size 5.7 MB, free 888.4 MB)
17/08/02 17:00:15 INFO BlockManagerInfo: Added rdd_1_0 in memory on 169.254.90.236:59397 (size: 5.7 MB, free: 888.5 MB)
17/08/02 17:00:15 INFO Executor: Finished task 0.0 in stage 0.0 (TID 0). 1842 bytes result sent to driver
17/08/02 17:00:16 INFO TaskSetManager: Finished task 0.0 in stage 0.0 (TID 0) in 691 ms on localhost (1/1)
17/08/02 17:00:16 INFO TaskSchedulerImpl: Removed TaskSet 0.0, whose tasks have all completed, from pool 
17/08/02 17:00:16 INFO DAGScheduler: ResultStage 0 (count at SimpleApp.java:26) finished in 0.726 s
17/08/02 17:00:16 INFO DAGScheduler: Job 0 finished: count at SimpleApp.java:26, took 1.078428 s
17/08/02 17:00:16 INFO SparkContext: Starting job: count at SimpleApp.java:33
17/08/02 17:00:16 INFO DAGScheduler: Got job 1 (count at SimpleApp.java:33) with 1 output partitions
17/08/02 17:00:16 INFO DAGScheduler: Final stage: ResultStage 1 (count at SimpleApp.java:33)
17/08/02 17:00:16 INFO DAGScheduler: Parents of final stage: List()
17/08/02 17:00:16 INFO DAGScheduler: Missing parents: List()
17/08/02 17:00:16 INFO DAGScheduler: Submitting ResultStage 1 (MapPartitionsRDD[3] at filter at SimpleApp.java:29), which has no missing parents
17/08/02 17:00:16 INFO MemoryStore: Block broadcast_2 stored as values in memory (estimated size 3.2 KB, free 888.4 MB)
17/08/02 17:00:16 INFO MemoryStore: Block broadcast_2_piece0 stored as bytes in memory (estimated size 1967.0 B, free 888.4 MB)
17/08/02 17:00:16 INFO BlockManagerInfo: Added broadcast_2_piece0 in memory on 169.254.90.236:59397 (size: 1967.0 B, free: 888.5 MB)
17/08/02 17:00:16 INFO SparkContext: Created broadcast 2 from broadcast at DAGScheduler.scala:1012
17/08/02 17:00:16 INFO DAGScheduler: Submitting 1 missing tasks from ResultStage 1 (MapPartitionsRDD[3] at filter at SimpleApp.java:29)
17/08/02 17:00:16 INFO TaskSchedulerImpl: Adding task set 1.0 with 1 tasks
17/08/02 17:00:16 INFO TaskSetManager: Starting task 0.0 in stage 1.0 (TID 1, localhost, partition 0, PROCESS_LOCAL, 5324 bytes)
17/08/02 17:00:16 INFO Executor: Running task 0.0 in stage 1.0 (TID 1)
17/08/02 17:00:16 INFO BlockManager: Found block rdd_1_0 locally
17/08/02 17:00:16 INFO Executor: Finished task 0.0 in stage 1.0 (TID 1). 954 bytes result sent to driver
17/08/02 17:00:16 INFO DAGScheduler: ResultStage 1 (count at SimpleApp.java:33) finished in 0.100 s
17/08/02 17:00:16 INFO DAGScheduler: Job 1 finished: count at SimpleApp.java:33, took 0.139033 s
17/08/02 17:00:16 INFO TaskSetManager: Finished task 0.0 in stage 1.0 (TID 1) in 99 ms on localhost (1/1)
17/08/02 17:00:16 INFO TaskSchedulerImpl: Removed TaskSet 1.0, whose tasks have all completed, from pool 

Lines with a: 11146, lines with b: 10760

17/08/02 17:00:16 INFO SparkContext: Invoking stop() from shutdown hook
17/08/02 17:00:16 INFO SparkUI: Stopped Spark web UI at http://169.254.90.236:4040
17/08/02 17:00:16 INFO MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
17/08/02 17:00:16 INFO MemoryStore: MemoryStore cleared
17/08/02 17:00:16 INFO BlockManager: BlockManager stopped
17/08/02 17:00:16 INFO BlockManagerMaster: BlockManagerMaster stopped
17/08/02 17:00:16 INFO OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
17/08/02 17:00:16 INFO SparkContext: Successfully stopped SparkContext
17/08/02 17:00:16 INFO ShutdownHookManager: Shutdown hook called
17/08/02 17:00:16 INFO ShutdownHookManager: Deleting directory C:\Users\Administrator\AppData\Local\Temp\spark-c40f2015-170f-4633-89dd-44dcd5bacfec

Process finished with exit code 0

```