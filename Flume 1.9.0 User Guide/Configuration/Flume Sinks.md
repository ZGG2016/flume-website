# Flume Sinks

[TOC]

## 1、HDFS Sink

> This sink writes events into the Hadoop Distributed File System (HDFS). It currently supports creating text and sequence files. It supports compression in both file types. The files can be rolled (close current file and create a new one) periodically based on the elapsed time or size of data or number of events. It also buckets/partitions data by attributes like timestamp or machine where the event originated. The HDFS directory path may contain formatting escape sequences that will replaced by the HDFS sink to generate a directory/file name to store the events. Using this sink requires hadoop to be installed so that Flume can use the Hadoop jars to communicate with the HDFS cluster. Note that a version of Hadoop that supports the sync() call is required.

这个 sink 将 events 写入 HDFS。它目前支持创建 text 和 sequence 文件。这两种文件类型都支持压缩。

可以根据运行时间、数据大小或 events 数量周期性地滚动文件(关闭当前文件并创建一个新文件)。

它还根据时间戳或 event 来源的机器等属性对数据进行分桶/分区。

HDFS 目录路径可能包含格式化转义序列，这些转义序列将被 HDFS sink 替换，生成一个用于存储 events 的目录/文件名。

使用这个 sink 需要安装 hadoop，这样 Flume 才能使用 Hadoop jar 与 HDFS 集群通信。

注意，需要一个支持 sync() 调用的 Hadoop 版本。

> The following are the escape sequences supported:

支持的转义序列如下:

Alias   |     Description
---|:---
%{host} |     Substitute value of event header named “host”. Arbitrary header names are supported.
%t      |     Unix time in milliseconds
%a      |     locale’s short weekday name (Mon, Tue, ...)
%A      |     locale’s full weekday name (Monday, Tuesday, ...)
%b      |     locale’s short month name (Jan, Feb, ...)
%B      |     locale’s long month name (January, February, ...)
%c      |     locale’s date and time (Thu Mar 3 23:05:25 2005)
%d      |     day of month (01)
%e      |     day of month without padding (1)
%D      |     date; same as %m/%d/%y
%H      |     hour (00..23)
%I      |     hour (01..12)
%j      |     day of year (001..366)
%k      |     hour ( 0..23)
%m      |     month (01..12)
%n      |     month without padding (1..12)
%M      |     minute (00..59)
%p      |     locale’s equivalent of am or pm
%s      |     seconds since 1970-01-01 00:00:00 UTC
%S      |     second (00..59)
%y      |     last two digits of year (00..99)
%Y      |     year (2010)
%z      |     +hhmm numeric timezone (for example, -0400)
%[localhost]  |  Substitute the hostname of the host where the agent is running【替换agent运行所在的主机的主机名】
%[IP]         |  Substitute the IP address of the host where the agent is running
%[FQDN]       |  Substitute the canonical hostname of the host where the agent is running

> Note: The escape strings %[localhost], %[IP] and %[FQDN] all rely on Java’s ability to obtain the hostname, which may fail in some networking environments.

注意:转义字符串 `%[localhost]`，`%[IP]` 和 `%[FQDN] `都依赖于 Java 获取主机名的能力，这在一些网络环境中可能会失败。

> The file in use will have the name mangled to include ”.tmp” at the end. Once the file is closed, this extension is removed. This allows excluding partially complete files in the directory. Required properties are in bold.

正在使用的文件的名称将被修改为在末尾添加 `.tmp`。一旦文件被关闭，这个扩展名将被删除。这允许排除目录中部分完整的文件。

必需的属性以粗体显示

> Note For all of the time related escape sequences, a header with the key “timestamp” must exist among the headers of the event (unless hdfs.useLocalTimeStamp is set to true). One way to add this automatically is to use the TimestampInterceptor.

对于所有与时间相关的转义序列，带有关键字 `timestamp` 的 header 必须存在于事件的 header 中(除非`hdfs.useLocalTimeStamp`设置为true)。一种方法是使用 TimestampInterceptor 自动添加它。

Property Name      |    Default     | 	Description
---|:---|:---
**channel**	       |       –	    | 
**type**           |       –	    |   The component type name, needs to be `hdfs`【组件类型名称，必须是`hdfs`】
**hdfs.path**	   |       –	    |   HDFS directory path (eg hdfs://namenode/flume/webdata/)【hdfs目录路径】
hdfs.filePrefix	   |   FlumeData	|   Name prefixed to files created by Flume in hdfs directory 【在hdfs目录中，flume创建的文件的前缀名字】
hdfs.fileSuffix	   |       –	    |   Suffix to append to file (eg `.avro` - NOTE: period is not automatically added)【追加到文件的后缀。如`.avro`，点号不是自动添加的】
hdfs.inUsePrefix   |       –	    |   Prefix that is used for temporal files that flume actively writes into【flume正在写入的临时文件的前缀】
hdfs.inUseSuffix   |      .tmp	    |   Suffix that is used for temporal files that flume actively writes into【flume正在写入的临时文件的后缀】
hdfs.emptyInUseSuffix  |	false	|   If false an hdfs.inUseSuffix is used while writing the output. After closing the output hdfs.inUseSuffix is removed from the output file name. If true the hdfs.inUseSuffix parameter is ignored an empty string is used instead.【如果是false，当写输出时，使用hdfs.inUseSuffix。在关闭输出后，hdfs.inUseSuffix从输出文件名中移除。如果是true，shdfs.inUseSuffix被忽略，使用一个空字符串替代。】
hdfs.rollInterval  |	   30	    |   Number of seconds to wait before rolling current file (0 = never roll based on time interval)【在滚动当前文件前，等待的秒数（0=不要基于时间间隔滚动）（滚动就是当达到一定条件，比如本条件，关闭此文件，当仍有数据传来时，新建一个新文件来存储。）】
hdfs.rollSize	   |       1024	    |   File size to trigger roll, in bytes (0: never roll based on file size)【触发滚动的文件大小，字节表示。（0 = 不要基于文件大小滚动）】
hdfs.rollCount	   |        10	    |   Number of events written to file before it rolled (0 = never roll based on number of events)【在滚动前，写入文件的事件数量。（0=不要基于事件数量滚动）】
hdfs.idleTimeout   |        0	    |   Timeout after which inactive files get closed (0 = disable automatic closing of idle files)【关闭非活动文件的超时时间（0=禁用自动关闭空闲文件功能）】
hdfs.batchSize	   |       100	    |   number of events written to file before it is flushed to HDFS【在刷新到hdfs前，写入文件的事件数量（从channel中一次取事件的数量）】
hdfs.codeC	       |         –	    |   Compression codec. one of following : `gzip`, `bzip2`, `lzo`, `lzop`,`snappy`【压缩的编解码器】
hdfs.fileType	   |   SequenceFile	|   File format: currently SequenceFile, DataStream or CompressedStream (1)DataStream will not compress output file and please don’t set codeC (2)CompressedStream requires set hdfs.codeC with an available codeC【文件格式：当前`SequenceFile`、`DataStream`、`CompressedStream`。（1）DataStream 将不压缩输出文件，请不要设置CodeC。（2）CompressedStream 要求使用一个可用的 codeC 设置 hdfs.codeC 】
hdfs.maxOpenFiles  |       5000	    |   Allow only this number of open files. If this number is exceeded, the oldest file is closed.【允许的打开文件的数量。如果超过了限制，最旧的的文件关闭。】
hdfs.minBlockReplicas |	    –	    |   Specify minimum number of replicas per HDFS block. If not specified, it comes from the default Hadoop config in the classpath.【指定每个hdfs块的最小副本数量。如果不指定，使用类路径中的hadoop默认配置】
hdfs.writeFormat   |      Writable	|   Format for sequence file records. One of `Text` or `Writable`. Set to `Text` before creating data files with Flume, otherwise those files cannot be read by either Apache Impala (incubating) or Apache Hive.【序列文件记录的格式。`Text`或`Writable`的一种。在flume创建数据文件前设为`Text`，否则这些文件不能被impala或hive读取】
hdfs.threadsPoolSize  |	    10	    |   Number of threads per HDFS sink for HDFS IO ops (open, write, etc.)【对HDFS IO操作，每个 hdfs sink的线程数量】
hdfs.rollTimerPoolSize|	    1	    |   Number of threads per HDFS sink for scheduling timed file rolling【用于调度定时文件滚动的每个 HDFS sink的线程数】
hdfs.kerberosPrincipal|	     –	    |   Kerberos user principal for accessing secure HDFS
hdfs.kerberosKeytab	  |      –	    |   Kerberos keytab for accessing secure HDFS
hdfs.proxyUser	 	  |             | 
hdfs.round	          |    false	|   Should the timestamp be rounded down (if true, affects all time based escape sequences except %t)【时间戳是否应该被向下取整(如果为true，将影响除%t之外的所有基于转义序列的时间)】
hdfs.roundValue	      |      1	    |   Rounded down to the highest multiple of this (in the unit configured using hdfs.roundUnit), less than current time.【向下取整到该值的最高倍数(在使用`hdfs.roundUnit`配置的单位中)，小于当前时间。】
hdfs.roundUnit	      |    second	|   The unit of the round down value - `second`, `minute` or `hour`.【向下取整值的单位】
hdfs.timeZone	      |  Local Time	|   Name of the timezone that should be used for resolving the directory path, e.g. America/Los_Angeles.【用来解析目录路径的时区的名字】
hdfs.useLocalTimeStamp|	     false	|   Use the local time (instead of the timestamp from the event header) while replacing the escape sequences.【当替换转移序列时，使用本地时间（而不是来事件header的时间戳）】
hdfs.closeTries	      |      0	    |   Number of times the sink must try renaming a file, after initiating a close attempt. If set to 1, this sink will not re-try a failed rename (due to, for example, NameNode or DataNode failure), and may leave the file in an open state with a .tmp extension. If set to 0, the sink will try to rename the file until the file is eventually renamed (there is no limit on the number of times it would try). The file may still remain open if the close call fails but the data will be intact and in this case, the file will be closed only after a Flume restart.【在发起一个关闭尝试后，sink必须尝试重命名一个文件的次数。如果设为1，这个sink不会重试一个失败的重命名（如NameNode or DataNode故障），可能会让这个文件保持为打开状态，后缀是`.tmp`。如果设为0，sink会尝试重命名文件，直到在文件被最终重命名（没有重试次数的限制）。如果调用关闭失败，文件会仍保持打开状态。但数据仍是原封不动的，文件会在flume重启后关闭。】
hdfs.retryInterval	  |      180	|   Time in seconds between consecutive attempts to close a file. Each close call costs multiple RPC round-trips to the Namenode, so setting this too low can cause a lot of load on the name node. If set to 0 or less, the sink will not attempt to close the file if the first attempt fails, and may leave the file open or with a ”.tmp” extension.【连续尝试关闭文件的时间间隔(秒)。每次关闭调用都要花费到Namenode的多次RPC往返，因此设置得太低可能会导致name node节点上的大量负载。如果设置为0或更小，sink在第一次尝试失败时不会尝试关闭文件，并且可能会让文件保持打开状态或使用`.tmp`的扩展。】
serializer	          |    `TEXT`	|    Other possible options include `avro_event` or the fully-qualified class name of an implementation of the `EventSerializer.Builder`interface.【其他可能的选项包括`avro_event`或`EventSerializer.Builder`接口实现类的完全限定类名】
serializer.*	      |             | 	 

> Deprecated Properties

Property Name      |    Default     | 	Description
---|:---|:---
hdfs.callTimeout   |     30000      |   Number of milliseconds allowed for HDFS operations, such as open, write, flush, close. This number should be increased if many HDFS timeout operations are occurring. 

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = hdfs
	a1.sinks.k1.channel = c1
	a1.sinks.k1.hdfs.path = /flume/events/%y-%m-%d/%H%M/%S
	a1.sinks.k1.hdfs.filePrefix = events-
	a1.sinks.k1.hdfs.round = true
	a1.sinks.k1.hdfs.roundValue = 10
	a1.sinks.k1.hdfs.roundUnit = minute

> The above configuration will round down the timestamp to the last 10th minute. For example, an event with timestamp 11:54:34 AM, June 12, 2012 will cause the hdfs path to become /flume/events/2012-06-12/1150/00.

上面的配置将把时间戳向下取整到最后10分钟。例如，一个时间戳为 `11:54:34 AM, June 12, 2012`的事件将 hdfs 路径变成 `/flume/events/2012-06-12/1150/00`。

---------------------------------------------------------------------

参考理解：

[https://blog.csdn.net/weixin_42102379/article/details/88942934](https://blog.csdn.net/weixin_42102379/article/details/88942934)

![hdfs-sink01](./image/hdfs-sink01.png)

----------------------------------------------------------------------

## 2、Hive Sink

> This sink streams events containing delimited text or JSON data directly into a Hive table or partition. Events are written using Hive transactions. As soon as a set of events are committed to Hive, they become immediately visible to Hive queries. Partitions to which flume will stream to can either be pre-created or, optionally, Flume can create them if they are missing. Fields from incoming event data are mapped to corresponding columns in the Hive table.

该 sink 将包含分隔文本或 JSON 数据的 events 直接流到 Hive 表或分区中。

Events 是使用 Hive 事务写的。一旦一组事件提交给 Hive，它们就会立即对 Hive 查询可见。

flume 将流到的分区可以预先创建，或者，如果缺少分区，flume 也可以创建分区。

传入 event 数据的字段映射到 Hive 表中相应的列。

Property Name      |    Default     | 	Description
---|:---|:---
**channel**	       |       –	    |
**type**	       |       –	    |   The component type name, needs to be `hive`【组件类型名称，必须是`hive`】
**hive.metastore** |       –	    |   Hive metastore URI (eg thrift://a.b.com:9083 )
**hive.database**  |       –	    |   Hive database name
**hive.table**	   |       –	    |   Hive table name
hive.partition	   |       –	    |   Comma separate list of partition values identifying the partition to write to. May contain escape sequences. E.g: If the table is partitioned by (continent: string, country :string, time : string) then ‘Asia,India,2014-02-26-01-21’ will indicate continent=Asia,country=India,time=2014-02-26-01-21【逗号分隔的分区值的列表，分区值表示要写入的分区。可能包含转移序列，如：如果表被(continent: string, country :string, time : string)分区，那么`Asia,India,2014-02-26-01-21` 表示`continent=Asia,country=India,time=2014-02-26-01-21`】
hive.txnsPerBatchAsk|	 100	    |   Hive grants a batch of transactions instead of single transactions to streaming clients like Flume. This setting configures the number of desired transactions per Transaction Batch. Data from all transactions in a single batch end up in a single file. Flume will write a maximum of batchSize events in each transaction in the batch. This setting in conjunction with batchSize provides control over the size of each file. Note that eventually Hive will transparently compact these files into larger files.【Hive将一批事务而不是单个事务授权像Flume这样的流客户端。此设置配置每个事务批次中所需的事务数量。来自单个批次中所有事务的数据最终保存在单个文件中。Flume将在批次中的每个事务中写入最大数量的batchSize事件。这个设置与batchSize一起提供了对每个文件大小的控制。请注意，最终Hive会透明地将这些文件压缩成更大的文件。】
heartBeatInterval |	      240	   |    (In seconds) Interval between consecutive heartbeats sent to Hive to keep unused transactions from expiring. Set this value to 0 to disable heartbeats.【发送到hive的两次连续心跳间的间隔（秒），来防止未使用的事务过期。将该值设置为0表示禁用心跳。】
autoCreatePartitions|	 true	   |    Flume will automatically create the necessary Hive partitions to stream to【flume将自动创建必要的hive分区，使数据流入。】
batchSize	      |      15000	   |    Max number of events written to Hive in a single Hive transaction【在一次hive事务中，写入hive事件的最大数量】
maxOpenConnections|       500	   |   Allow only this number of open connections. If this number is exceeded, the least recently used connection is closed.【允许打开的连接的最大数据量。如果超过这个限制，关闭最近最少使用的连接。】
callTimeout	      |      10000	   |   (In milliseconds) Timeout for Hive & HDFS I/O operations, such as openTxn, write, commit, abort.【对Hive & HDFS I/O的IO操作的超时时长】
**serializer**    |	 	           |   Serializer is responsible for parsing out field from the event and mapping them to columns in the hive table. Choice of serializer depends upon the format of the data in the event. Supported serializers: DELIMITED and JSON【Serializer负责解析出event中的字段，并将其映射到hive表中的列。序列化程序的选择取决于event中数据的格式。支持的序列化器:`DELIMITED`和`JSON`】
roundUnit	      |      minute	   |   The unit of the round down value - `second`, `minute` or `hour`.【向下取整值的单位】
roundValue	      |         1	   |   Rounded down to the highest multiple of this (in the unit configured using hive.roundUnit), less than current time【向下取整到该值的最高倍数(在使用hdfs.roundUnit配置的单位中)，小于当前时间。】
timeZone	      |    Local Time  |   Name of the timezone that should be used for resolving the escape sequences in partition, e.g. America/Los_Angeles.【用来解析目录路径的时区的名字】
useLocalTimeStamp |	     false	   |   Use the local time (instead of the timestamp from the event header) while replacing the escape sequences.【当替换转移序列时，使用本地时间（而不是来事件header的时间戳）】

> Following serializers are provided for Hive sink:

Hive sink 提供了如下的序列器：

> **JSON**: Handles UTF8 encoded Json (strict syntax) events and requires no configration. Object names in the JSON are mapped directly to columns with the same name in the Hive table. Internally uses org.apache.hive.hcatalog.data.JsonSerDe but is independent of the Serde of the Hive table. This serializer requires HCatalog to be installed.

**JSON**: 处理 UTF8 编码的Json(严格语法)events，不需要配置。JSON 中的对象名称直接映射到 Hive 表中同名的列。在内部使用`org.apache.hive.hcatalog.data.JsonSerDe`。但独立于 Hive 表的 Serde。这个序列化器需要安装 HCatalog。

> **DELIMITED**: Handles simple delimited textual events. Internally uses LazySimpleSerde but is independent of the Serde of the Hive table.

**DELIMITED**: 处理简单的 DELIMITED 文本 events。内部使用 LazySimpleSerde，但独立于 Hive 表的 Serde。

Property Name              |    Default     | 	Description
---|:---|:---
serializer.delimiter	   |       ,	    |   (Type: string) The field delimiter in the incoming data. To use special characters, surround them with double quotes like “\t”【（类型：字符串）在输入数据中字段的分隔符。为了使用特殊字符，使用双引号包围它，如“\t”】
**serializer.fieldnames**  |       –	    |   The mapping from input fields to columns in hive table. Specified as a comma separated list (no spaces) of hive table columns names, identifying the input fields in order of their occurrence. To skip fields leave the column name unspecified. Eg. ‘time,,ip,message’ indicates the 1st, 3rd and 4th fields in input map to time, ip and message columns in the hive table.【从输入字段映射到hive表中的列。hive表中各列的名称的列表，以逗号分隔(不支持空格)，按列的出现顺序标识输入字段。若要跳过字段，请保留未指定的列名。如‘time,,ip,message’输入中的第1，第3和第4个字段到hive表中的time, ip和message列的映射。】
serializer.serdeSeparator  |     Ctrl-A	    |    (Type: character) Customizes the separator used by underlying serde. There can be a gain in efficiency if the fields in serializer.fieldnames are in same order as table columns, the serializer.delimiter is same as the serializer.serdeSeparator and number of fields in serializer.fieldnames is less than or equal to number of table columns, as the fields in incoming event body do not need to be reordered to match order of table columns. Use single quotes for special characters like ‘\t’. Ensure input fields do not contain this character. NOTE: If serializer.delimiter is a single character, preferably set this to the same character【（类型：字符）自定义底层serde使用的分隔符。如果`serializer.fieldnames`中的字段与表中的列的顺序相同，`serializer.delimiter`和`serializer.serdeSeparator`相同，`serializer.fieldnames`中的字段数量小于或等于表的列的数量，可以提高效率。因为传入事件主体中的字段不需要重新排序以匹配表列的顺序。对于特殊字符，如‘\t’，请使用单引号。确保输入字段不包含此字符。注意:如果`serializer.delimiter`是单个字符，最好将其设置为相同的字符】

> The following are the escape sequences supported:

支持下列转义字符：

Alias   |     Description
---|:---
%{host} |	  Substitute value of event header named “host”. Arbitrary header names are supported.
%t	    |     Unix time in milliseconds
%a	    |     locale’s short weekday name (Mon, Tue, ...)
%A	    |     locale’s full weekday name (Monday, Tuesday, ...)
%b	    |     locale’s short month name (Jan, Feb, ...)
%B	    |     locale’s long month name (January, February, ...)
%c	    |     locale’s date and time (Thu Mar 3 23:05:25 2005)
%d	    |     day of month (01)
%D	    |     date; same as %m/%d/%y
%H	    |     hour (00..23)
%I	    |     hour (01..12)
%j	    |     day of year (001..366)
%k	    |     hour ( 0..23)
%m	    |     month (01..12)
%M	    |     minute (00..59)
%p	    |     locale’s equivalent of am or pm
%s	    |     seconds since 1970-01-01 00:00:00 UTC
%S	    |     second (00..59)
%y	    |     last two digits of year (00..99)
%Y	    |     year (2010)
%z	    |     +hhmm numeric timezone (for example, -0400)

> Note For all of the time related escape sequences, a header with the key “timestamp” must exist among the headers of the event (unless useLocalTimeStamp is set to true). One way to add this automatically is to use the TimestampInterceptor.

对于所有与时间相关的转义序列，带有关键字 timestamp 的 header 必须存在于事件的 header 中(除非hdfs.useLocalTimeStamp设置为true)。一种方法是使用 TimestampInterceptor 自动添加它。

> Example Hive table :

```sql
create table weblogs ( id int , msg string )
    partitioned by (continent string, country string, time string)
    clustered by (id) into 5 buckets
    stored as orc;
```

> Example for agent named a1:

	a1.channels = c1
	a1.channels.c1.type = memory
	a1.sinks = k1
	a1.sinks.k1.type = hive
	a1.sinks.k1.channel = c1
	a1.sinks.k1.hive.metastore = thrift://127.0.0.1:9083
	a1.sinks.k1.hive.database = logsdb
	a1.sinks.k1.hive.table = weblogs
	a1.sinks.k1.hive.partition = asia,%{country},%y-%m-%d-%H-%M
	a1.sinks.k1.useLocalTimeStamp = false
	a1.sinks.k1.round = true
	a1.sinks.k1.roundValue = 10
	a1.sinks.k1.roundUnit = minute
	a1.sinks.k1.serializer = DELIMITED
	a1.sinks.k1.serializer.delimiter = "\t"
	a1.sinks.k1.serializer.serdeSeparator = '\t'
	a1.sinks.k1.serializer.fieldnames =id,,msg

> The above configuration will round down the timestamp to the last 10th minute. For example, an event with timestamp header set to 11:54:34 AM, June 12, 2012 and ‘country’ header set to ‘india’ will evaluate to the partition (continent=’asia’,country=’india’,time=‘2012-06-12-11-50’). The serializer is configured to accept tab separated input containing three fields and to skip the second field.

上面的配置将把时间戳向下取整到最后10分钟。例如，一个 timestamp header 为 11:54:34 AM, June 12, 2012，‘country’ header 为 ‘india’ 的 event 将计算分区`(continent=’asia’,country=’india’,time=‘2012-06-12-11-50’)`。序列化器配置为包含 3 个字段的接受 tab 分隔的输入，跳过了第二个字段。

## 3、Logger Sink

> Logs event at INFO level. Typically useful for testing/debugging purpose. Required properties are in bold. This sink is the only exception which doesn’t require the extra configuration explained in the Logging raw data section.

日志输出 event，在 INFO 级别。通常用于测试/调试目的。

必需的属性以粗体显示。

这个 sink 是唯一的例外，它不需要日志记录原始数据部分中解释的额外配置。

Property Name      |    Default     | 	Description
---|:---|:---
**channel**	       |       –	    |
**type**	       |       –	    |   The component type name, needs to be `logger`【组件类型名称，必须是`logger`】
maxBytesToLog	   |       16	    |   Maximum number of bytes of the Event body to log【日志输出的事件主体的最大的字节数量】

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = logger
	a1.sinks.k1.channel = c1

## 4、Avro Sink

> This sink forms one half of Flume’s tiered collection support. Flume events sent to this sink are turned into Avro events and sent to the configured hostname / port pair. The events are taken from the configured Channel in batches of the configured batch size. Required properties are in bold.

发送到该 sink 的 Flume events 被转换为 Avro events，并发送到配置好的 hostname/port 对。

events 按配置的批次大小分批从配置的 Channel 获取。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channel**	     |      –	   |
**type**	     |      –	   |    The component type name, needs to be `avro`.【组件类型名称，必须是`avro`】
**hostname**	 |      –	   |    The hostname or IP address to bind to.【绑定的主机名或ip地址】
**port**	     |      –	   |    The port # to listen on.【监听的端口】
batch-size	     |     100	   |    number of event to batch together for send.【一个批次发送的事件数量】
connect-timeout	 |    20000	   |    Amount of time (ms) to allow for the first (handshake) request.【允许第一个(握手)请求的时间(ms)。】
request-timeout	 |    20000	   |    Amount of time (ms) to allow for requests after the first.【在第一个(握手)请求之后，允许的请求的时间(ms)。】
reset-connection-interval|none |	Amount of time (s) before the connection to the next hop is reset. This will force the Avro Sink to reconnect to the next hop. This will allow the sink to connect to hosts behind a hardware load-balancer when news hosts are added without having to restart the agent.【连接到下一跳之前被重置的时间。这将迫使Avro Sink重新连接到下一跳。这将允许sink在添加新主机时连接到硬件负载均衡器后的主机，而不必重新启动代理】
compression-type |	   none	   |    This can be “none” or “deflate”. The compression-type must match the compression-type of matching AvroSource【可以是“none或“deflate”。压缩类型必须匹配与之匹配的AvroSource的压缩类型】
compression-level|	     6	   |    The level of compression to compress event. 0 = no compression and 1-9 is compression. The higher the number the more compression【压缩事件的压缩等级。0表示不压缩。1-9数字越高，压缩越多。】
ssl	             |     false   |    Set to true to enable SSL for this AvroSink. When configuring SSL, you can optionally set a “truststore”, “truststore-password”, “truststore-type”, and specify whether to “trust-all-certs”.【为true时，为这个AvroSink启用ssl。当配置了ssl，可以可选地也设置一个“truststore”、“truststore-password”、“truststore-type”，并指定是否“trust-all-certs”。】
trust-all-certs	 |     false   |    If this is set to true, SSL server certificates for remote servers (Avro Sources) will not be checked. This should NOT be used in production because it makes it easier for an attacker to execute a man-in-the-middle attack and “listen in” on the encrypted connection.【如果这个设为true，对远程服务器(Avro Sources)，将不会检查其ssl服务证书。这不应该在生成中使用。】
truststore	     |       –	   |     The path to a custom Java truststore file. Flume uses the certificate authority information in this file to determine whether the remote Avro Source’s SSL authentication credentials should be trusted. If not specified, then the global keystore will be used. If the global keystore not specified either, then the default Java JSSE certificate authority files (typically “jssecacerts” or “cacerts” in the Oracle JRE) will be used.【自定义Java truststore文件的路径。Flume使用此文件中的证书授权信息来确定远程Avro Source’s的SSL身份验证凭据是否应该受信任。如果未指定，则将使用全局keystore。如果没有指定全局keystore，则将使用默认的Java JSSE证书授权文件(通常是Oracle JRE中的“jssecacerts”或“cacerts”)。】
truststore-password|	 –	   |     The password for the truststore. If not specified, then the global keystore password will be used (if defined).
truststore-type	 |      JKS	   |     The type of the Java truststore. This can be “JKS” or other supported Java truststore type. If not specified, then the global keystore type will be used (if defined, otherwise the defautl is JKS).
exclude-protocols|	   SSLv3   |      Space-separated list of SSL/TLS protocols to exclude. SSLv3 will always be excluded in addition to the protocols specified.
maxIoWorkers	 | 2 * the number of available processors in the machine  |  The maximum number of I/O worker threads. This is configured on the NettyAvroRpcClient NioClientSocketChannelFactory.【I/O worker线程的最大数量。在NettyAvroRpcClient NioClientSocketChannelFactory上配置。】

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = avro
	a1.sinks.k1.channel = c1
	a1.sinks.k1.hostname = 10.10.10.10
	a1.sinks.k1.port = 4545

## 5、Thrift Sink

> This sink forms one half of Flume’s tiered collection support. Flume events sent to this sink are turned into Thrift events and sent to the configured hostname / port pair. The events are taken from the configured Channel in batches of the configured batch size.

发送到这个 sink 的 Flume events 被转换成 Thrift events，并发送到配置好的 hostname/port 对。events 按配置的批次大小分批从配置的 Channel 获取。

> Thrift sink can be configured to start in secure mode by enabling kerberos authentication. To communicate with a Thrift source started in secure mode, the Thrift sink should also operate in secure mode. client-principal and client-keytab are the properties used by the Thrift sink to authenticate to the kerberos KDC. The server-principal represents the principal of the Thrift source this sink is configured to connect to in secure mode. Required properties are in bold.

通过启用 kerberos 身份验证，可以将 Thrift sink 配置为以安全模式启动。

要与在安全模式下启动的 Thrift source 通信，Thrift sink 也应该在安全模式下运行。

client-principal 和 client-keytab 是 Thrift sink 用于对 kerberos KDC 进行身份验证的属性。server-principal 表示此接 sink 被配置为以安全模式连接到的 Thrift source 的主体。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channel** 	 |      –	   |
**type** 	     |      –	   |    The component type name, needs to be `thrift`.【组件类型名称，必须是`thrift`】
**hostname** 	 |      –	   |    The hostname or IP address to bind to.【绑定的主机名或ip地址】
**port** 	     |      –	   |    The port # to listen on.【监听的端口】
batch-size	     |     100	   |    number of event to batch together for send.【一个批次发送的事件数量】
connect-timeout	 |    20000	   |    Amount of time (ms) to allow for the first (handshake) request.【允许第一个(握手)请求的时间(ms)。】
request-timeout	 |    20000	   |    Amount of time (ms) to allow for requests after the first.在第一个(握手)请求之后，允许的请求的时间(ms)。】
connection-reset-interval|none |	Amount of time (s) before the connection to the next hop is reset. This will force the Thrift Sink to reconnect to the next hop. This will allow the sink to connect to hosts behind a hardware load-balancer when news hosts are added without having to restart the agent.【连接到下一跳之前被重置的时间。这将迫使Thrift Sink重新连接到下一跳。这将允许sink在添加新主机时连接到硬件负载均衡器后的主机，而不必重新启动代理】
ssl	             |    false	   |    Set to true to enable SSL for this ThriftSink. When configuring SSL, you can optionally set a “truststore”, “truststore-password” and “truststore-type”【为true时，为这个ThriftSink启用ssl。当配置了ssl，可以可选地也设置一个“truststore”、“truststore-password”、“truststore-type”。】
truststore	     |      –	   |    The path to a custom Java truststore file. Flume uses the certificate authority information in this file to determine whether the remote Thrift Source’s SSL authentication credentials should be trusted. If not specified, then the global keystore will be used. If the global keystore not specified either, then the default Java JSSE certificate authority files (typically “jssecacerts” or “cacerts” in the Oracle JRE) will be used.【自定义Java truststore文件的路径。Flume使用此文件中的证书授权信息来确定远程Thrift Source’s的SSL身份验证凭据是否应该受信任。如果未指定，则将使用全局keystore。如果没有指定全局keystore，则将使用默认的Java JSSE证书授权文件(通常是Oracle JRE中的“jssecacerts”或“cacerts”)。】
truststore-password|	–	   |     The password for the truststore. If not specified, then the global keystore password will be used (if defined).
truststore-type	 |     JKS	   |     The type of the Java truststore. This can be “JKS” or other supported Java truststore type. If not specified, then the global keystore type will be used (if defined, otherwise the defautl is JKS).
exclude-protocols|	  SSLv3	   |      Space-separated list of SSL/TLS protocols to exclude
kerberos	     |    false	   |      Set to true to enable kerberos authentication. In kerberos mode, client-principal, client-keytab and server-principal are required for successful authentication and communication to a kerberos enabled Thrift Source.
client-principal |	   —-	   |      The kerberos principal used by the Thrift Sink to authenticate to the kerberos KDC.
client-keytab	 |     —-	   |       The keytab location used by the Thrift Sink in combination with the client-principal to authenticate to the kerberos KDC.
server-principal |     –	   |       The kerberos principal of the Thrift Source to which the Thrift Sink is configured to connect to.

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = thrift
	a1.sinks.k1.channel = c1
	a1.sinks.k1.hostname = 10.10.10.10
	a1.sinks.k1.port = 4545

## 6、IRC Sink

> The IRC sink takes messages from attached channel and relays those to configured IRC destinations. Required properties are in bold.

IRC sink 从附加的 channel 接收消息，并将这些消息转发到配置的 IRC 目的地。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channel**	     |      –	   |
**type**	     |      –	   |    The component type name, needs to be `irc`【组件类型名称，必须是`irc`】
**hostname**	 |      –	   |    The hostname or IP address to connect to【连接的主机名或ip地址】
port	         |     6667	   |    The port number of remote host to connect【连接的远程主机的端口号】
**nick**	     |      –	   |    Nick name
user	         |      –	   |    User name
password	     |      –	   |    User password
**chan**	     |      –	   |    channel
name	 	     |      –	   |
splitlines	     |      –	   |    (boolean)
splitchars	     |      n	   |    line separator (if you were to enter the default value into the config file, then you would need to escape the backslash, like this: “\n”)【行分隔符（如果要在配置文件中输入默认值，则需要转义反斜杠，如：“\n”）】

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = irc
	a1.sinks.k1.channel = c1
	a1.sinks.k1.hostname = irc.yourdomain.com
	a1.sinks.k1.nick = flume
	a1.sinks.k1.chan = #flume

## 7、File Roll Sink

> Stores events on the local filesystem. Required properties are in bold.

将 events 存储在本地文件系统。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channel**	     |      –	   |
**type**	     |      –	   |    The component type name, needs to be `file_roll`.【组件类型名称，必须是`file_roll`】
**sink.directory**|	    –	   |    The directory where files will be stored【文件存储的目录】
sink.pathManager |    DEFAULT  | 	The PathManager implementation to use.【使用的PathManager实现】
sink.pathManager.extension|	–  |    The file extension if the default PathManager is used.【如果使用了默认的PathManager，文件的扩展】
sink.pathManager.prefix|–	   |    A character string to add to the beginning of the file name if the default PathManager is used【如果使用了默认的PathManager，一个字符字符串添加到文件名字的开始。】
sink.rollInterval|	    30	   |    Roll the file every 30 seconds. Specifying 0 will disable rolling and cause all events to be written to a single file.【每30秒滚动一次文件。指定0将禁用滚动，并导致所有事件被写入单个文件。】
sink.serializer  |    `TEXT`   |    Other possible options include `avro_event` or the FQCN of an implementation of `EventSerializer.Builder` interface.【其他可能的选项包括`avro_event`，或`EventSerializer.Builder`接口的实现类的完全限定类名】
sink.batchSize	 |     100	   | 

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = file_roll
	a1.sinks.k1.channel = c1
	a1.sinks.k1.sink.directory = /var/log/flume


## 8、Null Sink

> Discards all events it receives from the channel. Required properties are in bold.

丢弃从 channel 接收的所有 events。必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channel**	     |      –	   |
**type**	     |      –	   |    The component type name, needs to be `null`.【组件类型名称，必须是`null`】
batchSize	     |     100	   |

> Example for agent named a1:

	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = null
	a1.sinks.k1.channel = c1

## 9、HBaseSinks