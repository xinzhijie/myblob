hadoop  checknative

报错：
```
19/07/12 09:34:39 ERROR snappy.SnappyCompressor: failed to load SnappyCompressor
java.lang.UnsatisfiedLinkError: Cannot load libsnappy.so.1 (libsnappy.so.1: cannot open shared object file: No such file or directory)!
	at org.apache.hadoop.io.compress.snappy.SnappyCompressor.initIDs(Native Method)
	at org.apache.hadoop.io.compress.snappy.SnappyCompressor.<clinit>(SnappyCompressor.java:61)
	at org.apache.hadoop.io.compress.SnappyCodec.isNativeCodeLoaded(SnappyCodec.java:82)
	at org.apache.hadoop.util.NativeLibraryChecker.main(NativeLibraryChecker.java:82)
Native library checking:
hadoop:  true /root/apps/hadoop-2.6.0-cdh5.14.0/lib/native/libhadoop.so.1.0.0
zlib:    true /lib64/libz.so.1
snappy:  false 
lz4:     true revision:10301
bzip2:   true /lib64/libbz2.so.1
openssl: false Cannot load libcrypto.so (libcrypto.so: cannot open shared object file: No such file or directory)!



```
需要安装
```
yum install snappy libsnappy*
yum install openssl-devel 
```



修改core-site.xml文件
```
<configuration>
	<!-- hadoop默认访问nameNode元数据的路径 -->
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://node01:8020</value>
	</property>
	<!-- hadoop的临时文件夹位置 -->
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/tempDatas</value>
	</property>
	<!--  缓冲区大小，根据服务器性能动态调整 -->
	<property>
		<name>io.file.buffer.size</name>
		<value>4096</value>
	</property>
	<!--  开启hdfs的垃圾桶机制，删除掉的数据可以从垃圾桶中回收，单位分钟 -->
	<property>
		<name>fs.trash.interval</name>
		<value>10080</value>
	</property>
</configuration>
```
修改hadoop-env.sh文件
```
export JAVA_HOME=/export/servers/jdk1.8.0_144
```

修改hdfs-site.xml文件
```
<configuration>
	<!-- NameNode存储元数据信息的路径，一般先确定磁盘的挂载目录，然后多个目录用，进行分割 --> 
	<!--   集群动态上下线 
	<property>
		<name>dfs.hosts</name>
		<value>/export/servers/hadoop-2.6.0-cdh5.14.0/etc/hadoop/accept_host</value>
	</property>
	
	<property>
		<name>dfs.hosts.exclude</name>
		<value>/export/servers/hadoop-2.6.0-cdh5.14.0/etc/hadoop/deny_host</value>
	</property>
	 -->
	
	<!-- nameNode的访问路径和端口号 -->
	<property>
		<name>dfs.namenode.secondary.http-address</name>
		<value>node01:50090</value>
	</property>
	<!-- nameNode的外部访问路径和端口号 -->
	<property>
		<name>dfs.namenode.http-address</name>
		<value>node01:50070</value>
	</property>
	<!-- 配置nameNode的存放位置 -->
	<property>
		<name>dfs.namenode.name.dir</name>
		<value>file:///export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/namenodeDatas</value>
	</property>
	<!--  定义dataNode数据存储的节点位置，一般先确定磁盘的挂载目录，然后多个目录用，进行分割  -->
	<property>
		<name>dfs.datanode.data.dir</name>
		<value>file:///export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/datanodeDatas</value>
	</property>
	<!-- edits的存储位置 -->
	<property>
		<name>dfs.namenode.edits.dir</name>
		<value>file:///export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/dfs/nn/edits</value>
	</property>
	<!-- 元数据信息检查点的存储位置 -->
	<property>
		<name>dfs.namenode.checkpoint.dir</name>
		<value>file:///export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/dfs/snn/name</value>
	</property>
	<!-- edits的检查点的存储位置 -->
	<property>
		<name>dfs.namenode.checkpoint.edits.dir</name>
		<value>file:///export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/dfs/nn/snn/edits</value>
	</property>
	<!-- 副本数量 -->
	<property>
		<name>dfs.replication</name>
		<value>3</value>
	</property>
	<!-- hdfs的权限 -->
	<property>
		<name>dfs.permissions</name>
		<value>false</value>
	</property>
	<!-- hdfs存储的block大小, 128M -->
	<property>
		<name>dfs.blocksize</name>
		<value>134217728</value>
	</property>
</configuration>
```

修改mapred-site.xml文件
```
<configuration>
	<!-- 指定MapReduce运行在yarn集群上 -->
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
	<!-- 开启MapReduce的小任务模式 -->
	<property>
		<name>mapreduce.job.ubertask.enable</name>
		<value>true</value>
	</property>
	<!-- 配置jobhistory的访问路径和端口号, jobhistory是执行完成的任务日志 -->
	<property>
		<name>mapreduce.jobhistory.address</name>
		<value>node01:10020</value>
	</property>
	<!-- 配置jobhistory的浏览器访问路径和端口号 -->
	<property>
		<name>mapreduce.jobhistory.webapp.address</name>
		<value>node01:19888</value>
	</property>
</configuration>
```

修改slaves文件
```
node01
node02
node03
```

修改yarn-site.xml文件
```
<configuration>
	<!-- 配置resourcemanager运行在哪台机器上 -->
	<property>
		<name>yarn.resourcemanager.hostname</name>
		<value>node01</value>
	</property>
	<!-- 配置nodemanager上运行的附属服务为shuffle -->
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<!-- 开启聚合日志 -->
	<property>
		<name>yarn.log-aggregation-enable</name>
		<value>true</value>
	</property>
	<!-- 配置聚合日志的保持时长 -->
	<property>
		<name>yarn.log-aggregation.retain-seconds</name>
		<value>604800</value>
	</property>
</configuration>
```
修改mapred-env.sh文件 (可选)
```
export JAVA_HOME=/export/servers/jdk1.8.0_144
```

```
mkdir -p /export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/tempDatas
mkdir -p /export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/namenodeDatas
mkdir -p /export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/datanodeDatas 
mkdir -p /export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/dfs/nn/edits
mkdir -p /export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/dfs/snn/name
mkdir -p /export/servers/hadoop-2.6.0-cdh5.14.0/hadoopDatas/dfs/nn/snn/edits
```


node01 启动：
```
 NodeManager
 SecondaryNameNode
 NameNode
 DataNode
 ResourceManager
 JobHistoryServer
```
node02，node03 启动:
```
 NodeManager
 datanode
```
