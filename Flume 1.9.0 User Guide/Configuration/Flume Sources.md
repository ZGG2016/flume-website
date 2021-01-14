# Flume Sources

[TOC]

## 1、Avro Source

> Listens on Avro port and receives events from external Avro client streams. When paired with the built-in Avro Sink on another (previous hop) Flume agent, it can create tiered collection topologies. Required properties are in bold.

监听 Avro 端口，并从外部 Avro 客户端流接收 events。

当与另一个(上一跳) Flume agent 上的内置 Avro Sink 配对时，它可以创建分层集合拓扑。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channels**     |     –	   |  
**type**	     |     –	   |    The component type name, needs to be `avro`【组件类型名称，必须是`avro`】
**bind**	     |     –	   |    hostname or IP address to listen on【监听的主机名或ip地址】
**port**	     |     –	   |    Port # to bind to 【绑定的端口】
threads  	     |     –	   |    Maximum number of worker threads to spawn【要生成的工作线程的最大数目】
selector.type    |             |	 	 
selector.*	 	 |             |
interceptors	 |     –	   |    Space-separated list of interceptors【空格分隔的拦截器列表】
interceptors.*	 |             | 	 
compression-type |	  none	   |    This can be “none” or “deflate”. The compression-type must match the compression-type of matching AvroSource【可以是`none`或`deflate`，compression-type必须与匹配的AvroSource的compression-type匹配】
ssl	             |    false	   |    Set this to true to enable SSL encryption. If SSL is enabled, you must also specify a “keystore” and a “keystore-password”, either through component level parameters (see below) or as global SSL parameters (see [SSL/TLS support section](http://flume.apache.org/FlumeUserGuide.html#ssl-tls-support)).【为true时，启用ssl加密。如果启用了ssl，必须也指定一个`“keystore”`和`“keystore-password”`，要么通过组件级的参数，要么作为全局的ssl参数。】
keystore	     |      –	   |    This is the path to a Java keystore file. If not specified here, then the global keystore will be used (if defined, otherwise configuration error).【这是一个java keystore文件的路径。如果不在这指定，将使用全局的keystore（如果定义的话，否则出现配置错误）】
keystore-password|	    –	   |     The password for the Java keystore. If not specified here, then the global keystore password will be used (if defined, otherwise configuration error).【Java keystore密码。如果不在这指定，将使用全局的keystore密码（如果定义的话，否则出现配置错误）】
keystore-type	 |      JKS	   |      The type of the Java keystore. This can be “JKS” or “PKCS12”. If not specified here, then the global keystore type will be used (if defined, otherwise the default is JKS).【Java keystore类型。可以是`“JKS”`或`“PKCS12”`。如果不在这指定，将使用全局的keystore类型（如果定义的话，否则出现默认是JKS）】
exclude-protocols|	   SSLv3   |      Space-separated list of SSL/TLS protocols to exclude. SSLv3 will always be excluded in addition to the protocols specified.【排除的空格分隔的SSL/TLS协议的列表。除了指定的协议，SSLv3总是被排除在外。】
include-protocols|	     –	   |      Space-separated list of SSL/TLS protocols to include. The enabled protocols will be the included protocols without the excluded protocols. If included-protocols is empty, it includes every supported protocols.【包含的空格分隔的SSL/TLS协议的列表。启用的协议将是要包含的协议，没有排除的协议，如果`included-protocols`为空，它包含每一个支持的协议。】
exclude-cipher-suites|	 –	   |      Space-separated list of cipher suites to exclude. 【排除的空格分隔的cipher suites的列表。】
include-cipher-suites|	 –	   |      Space-separated list of cipher suites to include. The enabled cipher suites will be the included cipher suites without the excluded cipher suites. If included-cipher-suites is empty, it includes every supported cipher suites.【包含的空格分隔的cipher suites的列表。启用的cipher suites将是要包含的cipher suites，没有排除的cipher suites，如果`included-cipher-suites`为空，它包含每一个支持的cipher suites。】
ipFilter         |	    false  |      Set this to true to enable ipFiltering for netty【设为true，来为netty启用ip过滤】
ipFilterRules	 |        –	   |      Define N netty ipFilter pattern rules with this config.【使用这个配置可以定义n个netty ipFilter规则】

> Example for agent named a1:

	a1.sources = r1
	a1.channels = c1
	a1.sources.r1.type = avro
	a1.sources.r1.channels = c1
	a1.sources.r1.bind = 0.0.0.0
	a1.sources.r1.port = 4141
	
ipFilterRules 的示例：

> ipFilterRules defines N netty ipFilters separated by a comma a pattern rule must be in this format.

ipFilterRules 定义 N 个 netty ipFilter，使用逗号分隔，匹配规则必须是这个格式。

`<'allow' or deny>:<'ip' or 'name' for computer name>:<pattern>` 或 `allow/deny:ip/name:pattern`

例如: `ipFilterRules=allow:ip:127.*`,`allow:name:localhost,deny:ip:*`

> Note that the first rule to match will apply as the example below shows from a client on the localhost

注意，要匹配的第一个规则将应用于本地主机上的客户端，如下面的示例所示

This will Allow the client on localhost be deny clients from any other ip `"allow:name:localhost,deny:ip:"`.

This will deny the client on localhost be allow clients from any other ip `"deny:name:localhost,allow:ip:"`

## 2、Thrift Source

> Listens on Thrift port and receives events from external Thrift client streams. When paired with the built-in ThriftSink on another (previous hop) Flume agent, it can create tiered collection topologies. Thrift source can be configured to start in secure mode by enabling kerberos authentication. agent-principal and agent-keytab are the properties used by the Thrift source to authenticate to the kerberos KDC. Required properties are in bold.

监听 Thrift 端口，并从外部 Thrift 客户端流接收 events。

当与另一个(上一跳) Flume agent 上的内置 ThriftSink 配对时，它可以创建分层集合拓扑。

Thrift source 可以在安全模式下通过启用 kerberos 身份验证配置为开始。

agent-principal 和 agent-keytab 属性被 Thrift source 用来授权 kerberos KDC。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channels**	 |      –	   | 
**type**	     |      –	   |    The component type name, needs to be `thrift`【组件类型名称，必须是`thrift`】
**bind**	     |      –	   |    hostname or IP address to listen on【监听的主机名或ip地址】
**port**	     |      –	   |    Port # to bind to【绑定的端口】
threads	         |      –	   |    Maximum number of worker threads to spawn【要生成的工作线程的最大数目】
selector.type	 |             |	 
selector.*	 	 |             |
interceptors	 |      –	   |    Space separated list of interceptors【空格分隔的拦截器列表】
interceptors.*	 |             |	 
ssl	             |    false	   |    Set this to true to enable SSL encryption. If SSL is enabled, you must also specify a “keystore” and a “keystore-password”, either through component level parameters (see below) or as global SSL parameters (see SSL/TLS support section)【为true时，启用ssl加密。如果启用了ssl，必须也指定一个“keystore”和“keystore-password”，要么通过组件级的参数，要么作为全局的ssl参数。】
keystore	     |      –	   |    This is the path to a Java keystore file. If not specified here, then the global keystore will be used (if defined, otherwise configuration error).【这是一个java keystore文件的路径。如果不在这指定，将使用全局的keystore（如果定义的话，否则出现配置错误）】
keystore-password|	    –	   |    The password for the Java keystore. If not specified here, then the global keystore password will be used (if defined, otherwise configuration error).【Java keystore密码。如果不在这指定，将使用全局的keystore密码（如果定义的话，否则出现配置错误）】
keystore-type	 |     JKS	   |    The type of the Java keystore. This can be “JKS” or “PKCS12”. If not specified here, then the global keystore type will be used (if defined, otherwise the default is JKS).【Java keystore类型。可以是“JKS”或“PKCS12”。如果不在这指定，将使用全局的keystore类型（如果定义的话，否则出现默认是JKS）】
exclude-protocols|	   SSLv3   |    Space-separated list of SSL/TLS protocols to exclude. SSLv3 will always be excluded in addition to the protocols specified.【排除的空格分隔的SSL/TLS协议的列表。除了指定的协议，SSLv3总是被排除在外。】
include-protocols|	     –	   |    Space-separated list of SSL/TLS protocols to include. The enabled protocols will be the included protocols without the excluded protocols. If included-protocols is empty, it includes every supported protocols.【包含的空格分隔的SSL/TLS协议的列表。启用的协议将是要包含的协议，没有排除的协议，如果included-protocols为空，它包含每一个支持的协议。】
exclude-cipher-suites|	 –	   |    Space-separated list of cipher suites to exclude.【排除的空格分隔的cipher suites的列表。】
include-cipher-suites|	 –	   |    Space-separated list of cipher suites to include. The enabled cipher suites will be the included cipher suites without the excluded cipher suites.【包含的空格分隔的cipher suites的列表。启用的cipher suites将是要包含的cipher suites，没有排除的cipher suites】
kerberos	         | false   | 	Set to true to enable kerberos authentication. In kerberos mode, agent-principal and agent-keytab are required for successful authentication. The Thrift source in secure mode, will accept connections only from Thrift clients that have kerberos enabled and are successfully authenticated to the kerberos KDC.【设置为true，来启用kerberos身份验证，在kerberos模式下，对成功的身份验证，要求`agent-principal`和`agent-keytab`。在安全模式下，Thrift source将仅接收来自启用kerberos的，并成功给kerberos KDC身份验证的Thrift客户端的连接】
agent-principal      |	 –	   |     The kerberos principal used by the Thrift Source to authenticate to the kerberos KDC.【Thrift Source使用的kerberos principal对kerberos KDC进行身份验证。】
agent-keytab	     |   —	   |     The keytab location used by the Thrift Source in combination with the agent-principal to authenticate to the kerberos KDC.【Thrift Source与agent-principal一起使用的keytab位置，用于对kerberos KDC进行身份验证。】

> Example for agent named a1:

	a1.sources = r1
	a1.channels = c1
	a1.sources.r1.type = thrift
	a1.sources.r1.channels = c1
	a1.sources.r1.bind = 0.0.0.0
	a1.sources.r1.port = 4141

## 3、Exec Source

> Exec source runs a given Unix command on start-up and expects that process to continuously produce data on standard out (stderr is simply discarded, unless property logStdErr is set to true). If the process exits for any reason, the source also exits and will produce no further data. This means configurations such as `cat [named pipe]` or `tail -F [file]` are going to produce the desired results where as `date` will probably not - the former two commands produce streams of data where as the latter produces a single event and exits.Required properties are in bold.

Exec source 在启动时，运行一个给定的 Unix 命令，那个进程会在标准输出上产生数据。(除非属性 logStdErr 设置为 true，否则 stderr 将被丢弃)

如果这个进程退出，那么 source 也会退出，不再产生数据。这就意味着，像 `cat [named pipe]` 或者 `tail -F [file]` 的这种配置会产生期望的结果，而 `date` 命令不会产生。

前两个命令产生数据流，而后一个命令产生单个事件并退出。

必需的属性以粗体显示。

Property Name    |   Default   | 	Description
---|:---|:---
**channels**	 |     –	   | 
**type**	     |     –	   |    The component type name, needs to be `exec`【组件类型名称，必须是`exec`】
**command**	     |     –	   |    The command to execute【要执行的命令】
shell	         |     –	   |    A shell invocation used to run the command. e.g. `/bin/sh -c`. Required only for commands relying on shell features like wildcards, back ticks, pipes etc.【用于运行命令的shell调用。如`/bin/sh -c`。仅用于依赖于shell特性的命令，如通配符、反引号、管道等。】
restartThrottle  |	  10000	   |    Amount of time (in millis) to wait before attempting a restart【在尝试重启前，等待的时间(毫秒)】
restart	         |     false   |    Whether the executed cmd should be restarted if it dies【如果执行的命令终止，是否应该重新启动】
logStdErr	     |     false   |    Whether the command’s stderr should be logged【命令的stderr是否应该输出】
batchSize	     |      20	   |    The max number of lines to read and send to the channel at a time【一次读取并发送给channel的最大行数，】
batchTimeout	 |      3000   |    Amount of time (in milliseconds) to wait, if the buffer size was not reached, before data is pushed downstream【在数据推到下游前，如果没有达到缓存大小，等待的时间(毫秒)】
selector.type	 |  replicating|	replicating or multiplexing
selector.*	     | 	           |    Depends on the selector.type value
interceptors	 |      –	   |    Space-separated list of interceptors【空格分隔的拦截器列表】
interceptors.*	 |             |

> Warning：The problem with ExecSource and other asynchronous sources is that the source can not guarantee that if there is a failure to put the event into the Channel the client knows about it. In such cases, the data will be lost. As a for instance, one of the most commonly requested features is the tail -F [file]-like use case where an application writes to a log file on disk and Flume tails the file, sending each line as an event. While this is possible, there’s an obvious problem; what happens if the channel fills up and Flume can’t send an event? Flume has no way of indicating to the application writing the log file that it needs to retain the log or that the event hasn’t been sent, for some reason. If this doesn’t make sense, you need only know this: Your application can never guarantee data has been received when using a unidirectional asynchronous interface such as ExecSource! As an extension of this warning - and to be completely clear - there is absolutely zero guarantee of event delivery when using this source. For stronger reliability guarantees, consider the Spooling Directory Source, Taildir Source or direct integration with Flume via the SDK.

警告：ExecSource 和其他异步源的问题是，source 不能保证，客户端知道有一个失败的事件放入了 Channel。

在这种情况下，数据将丢失。例如，最常见的请求特性之一是 `tail -F [file]` 用例，应用程序写入磁盘上的日志文件，Flume tail 文件，将每一行作为 event 发送。

虽然这是可能的，但有一个明显的问题；如果 channel 被填满了，Flume 不能发送一个事件怎么办？

Flume 无法向编写日志文件的应用程序指示它需要保留日志，或者由于某些原因不发送 event。

如果这没有意义，你只需要知道:当使用单向异步接口(比如ExecSource)时，你的应用程序永远不能保证数据已经被接收到!

作为此警告的扩展，并且要完全清楚：在使用此 source 时，event 传输绝对没有任何保证。

为了更强的可靠性保证，可以考虑 Spooling Directory Source、Taildir Source 或 通过 SDK 直接与 Flume 集成。

> Example for agent named a1:

	a1.sources = r1
	a1.channels = c1
	a1.sources.r1.type = exec
	a1.sources.r1.command = tail -F /var/log/secure
	a1.sources.r1.channels = c1

> The 'shell' config is used to invoke the 'command' through a command shell (such as Bash or Powershell). The 'command' is passed as an argument to 'shell' for execution. This allows the 'command' to use features from the shell such as wildcards, back ticks, pipes, loops, conditionals etc. In the absence of the ‘shell’ config, the 'command' will be invoked directly. Common values for 'shell' : '/bin/sh -c', '/bin/ksh -c', 'cmd /c', 'powershell -Command', etc.

'shell' 配置用来通过一个命令行（Bash or Powershell）来调用 'command'。为了执行 'command'， 它作为一个参数传递给 'shell'。

它允许 'command' 使用 shell 特性，如通配符、反引号、管道、循环、条件语句等。

在 'shell' 配置中，直接调用 'command'。'shell' 常用的值：'/bin/sh -c'、'/bin/ksh -c'、 'cmd /c'、'powershell -Command' 等

	a1.sources.tailsource-1.type = exec
	a1.sources.tailsource-1.shell = /bin/bash -c
	a1.sources.tailsource-1.command = for i in /path/*.txt; do cat $i; done

## 4、JMS Source 【待做】

## 5、Spooling Directory Source

> This source lets you ingest data by placing files to be ingested into a “spooling” directory on disk. This source will watch the specified directory for new files, and will parse events out of new files as they appear. The event parsing logic is pluggable. After a given file has been fully read into the channel, completion by default is indicated by renaming the file or it can be deleted or the trackerDir is used to keep track of processed files.

这个 source 可以让你将要接收的文件放置到磁盘上的 spooling 目录下来接收数据。

该 source 会监控指定目录下的新文件，然后当新文件出现时，就解析其中的 events。解析 events 的逻辑是可插拔的。

在将一个给定的文件完全读入到 channel 后，默认是通过重命名文件来表示完成，或者可以删除该文件，或者使用 trackerDir 追踪已处理的文件。

> Unlike the Exec source, this source is reliable and will not miss data, even if Flume is restarted or killed. In exchange for this reliability, only immutable, uniquely-named files must be dropped into the spooling directory. Flume tries to detect these problem conditions and will fail loudly if they are violated:

不同于 Exec source，spooling source 是可靠的，即使 flume 被重启或被杀死时，不会失去数据。

为了获得这种可靠性，必须只将不可变的、唯一命名的文件放入 spooling 目录。

Flume 试图检测这些问题条件，如果违反，将会提示失败：

> If a file is written to after being placed into the spooling directory, Flume will print an error to its log file and stop processing.

（1）如果一个文件在放到 spooling 目录后，又写入了数据，flume 将会在日志文件中打印错误，停止处理。

> If a file name is reused at a later time, Flume will print an error to its log file and stop processing.

（2）如果一个文件名在后面被重复使用，Flume 将会在日志文件中打印错误，停止处理。 

> To avoid the above issues, it may be useful to add a unique identifier (such as a timestamp) to log file names when they are moved into the spooling directory.

为了避免上述情况，在移动到 spooling 目录下时，给日志文件名添加一个唯一的标识符（如时间戳）。

> Despite the reliability guarantees of this source, there are still cases in which events may be duplicated if certain downstream failures occur. This is consistent with the guarantees offered by other Flume components.

尽管该 source 具有可靠性保证，但如果某些下游出现故障，仍然存在 events 可能重复的情况。这与其他 Flume 组件提供的保证是一致的。

Property Name    |   Default   | 	Description
---|:---|:---
**channels**	 |      –	   |
**type**	     |      –	   |    The component type name, needs to be `spooldir`.【组件类型名称，必须是`spooldir`】
**spoolDir**	 |      –	   |    The directory from which to read files from.【读取文件的目录】
fileSuffix	     | .COMPLETED  |	Suffix to append to completely ingested files【完成后，追加到接收文件的后缀】
deletePolicy	 |    never	   |    When to delete completed files: `never` or `immediate`【什么时候删除完成的文件】
fileHeader	     |    false	   |    Whether to add a header storing the absolute path filename.【是否添加一个存储绝对路径的文件名的header】
fileHeaderKey	 |     file	   |    Header key to use when appending absolute path filename to event header.【当追加一个存储绝对路径的fileHeaderKey时，使用的Header key】
basenameHeader	 |     false   |    Whether to add a header storing the basename of the file.【是否添加一个存储文件basename的header】
basenameHeaderKey|	 basename  | 	Header Key to use when appending basename of file to event header.【当追加一个文件basename给事件header时，使用的Header key】
includePattern	 |   `^.*$`    |	Regular expression specifying which files to include. It can used together with `ignorePattern`. If a file matches both `ignorePattern` and `includePattern` regex, the file is ignored.【指定要包含文件的正则表达式。和`ignorePattern`一起使用。如果一个文件同时匹配`ignorePattern`和`includePattern`正则，那么该文件就被忽略。】
ignorePattern	 |     `^$`	   |    Regular expression specifying which files to ignore (skip). It can used together with `includePattern`. If a file matches both `ignorePattern` and `includePattern` regex, the file is ignored.【指定要忽略\跳过的文件的正则表达式。和`includePattern`一起使用。如果一个文件同时匹配`ignorePattern`和`includePattern`正则，那么该文件就被忽略。】
trackerDir	     | .flumespool |	Directory to store metadata related to processing of files. If this path is not an absolute path, then it is interpreted as relative to the spoolDir.【存储与文件处理相关的元数据的目录。如果这个路径不是一个绝对路径，那么就将其当作相对spoolDir的相对路径】
trackingPolicy	 |    rename   |    The tracking policy defines how file processing is tracked. It can be “rename” or “tracker_dir”. This parameter is only effective if the deletePolicy is “never”. “rename” - After processing files they get renamed according to the fileSuffix parameter. “tracker_dir” - Files are not renamed but a new empty file is created in the trackerDir. The new tracker file name is derived from the ingested one plus the fileSuffix.【追踪策略定义了如何追踪正在处理的文件。可以是`rename`或`tracker_dir`。只有`deletePolicy`为`never`时，此参数才有效。`rename`是在处理文件后，根据`filesufix`参数对文件进行重命名。`tracker_dir`是文件不被重命名，但在`trackerDir`中创建一个新的空文件。新的追踪文件名来自于输入的文件名加上`filesufix`。】
consumeOrder     |	oldest	   |    In which order files in the spooling directory will be consumed `oldest`,`youngest` and `random`. In case of `oldest` and `youngest`, the last modified time of the files will be used to compare the files. In case of a tie, the file with smallest lexicographical order will be consumed first. In case of `random` any file will be picked randomly. When using `oldest` and `youngest` the whole directory will be scanned to pick the oldest/youngest file, which might be slow if there are a large number of files, while using `random` may cause old files to be consumed very late if new files keep coming in the spooling directory.【spooling目录中的文件被消费的顺序，有`oldest`、`youngest`和`random`。对于`oldest`和`youngest`情况，使用文件的最后修改时间来比较文件。在出现并列的情况下，字典顺序最小的文件将首先被消费。在`random`情况下，任何文件将被随机选取。当使用`oldest`和`youngest`，将扫描整个目录选择最古老/最小的文件，如果有大量的文件，这可能比较慢，在使用`random`时，可能导致如果新文件不断在出现在spooling目录中，旧文件会很晚才被消费。】
pollDelay	     |     500	   |     Delay (in milliseconds) used when polling for new files.【轮询新文件时使用的延迟(毫秒)。】
recursiveDirectorySearch|false |     Whether to monitor sub directories for new files to read.【对于读取的新文件，是否监控子目录。】
maxBackoff	     |     4000	   |     The maximum time (in millis) to wait between consecutive attempts to write to the channel(s) if the channel is full. The source will start at a low backoff and increase it exponentially each time the channel throws a ChannelException, upto the value specified by this parameter.【如果channel满了，在两次连续尝试写入的最大时间间隔。source将从较低的回退开始，并在channel每次抛出ChannelException时以指数方式增加回退，直到达到此参数指定的值】
batchSize 	     |      100	   |     Granularity at which to batch transfer to the channel【批量传输到channel的粒度】
inputCharset     |     UTF-8   |     Character set used by deserializers that treat the input file as text.【反序列化器使用的将输入文件视为文本的字符集。】
decodeErrorPolicy|	   `FAIL`  |     What to do when we see a non-decodable character in the input file. `FAIL`: Throw an exception and fail to parse the file. `REPLACE`: Replace the unparseable character with the “replacement character” char, typically Unicode U+FFFD. `IGNORE`: Drop the unparseable character sequence.【当我们在输入文件中看到一个不可解码的字符时该怎么做。`FAIL`:抛出异常，无法解析文件。`REPLACE`:用"replacement character"替换不可解析字符，通常是Unicode U+FFFD。`IGNORE`:删除不可解析的字符序列。】
deserializer	 |     `LINE`  |      Specify the deserializer used to parse the file into events. Defaults to parsing each line as an event. The class specified must implement `EventDeserializer.Builder`.【指定用来解析文件成事件的反序列化器。默认解析每行为一个事件。指定的类必须实现`EventDeserializer.Builder`】
deserializer.*	 |    	       |      Varies per event deserializer.
bufferMaxLines	 |      –	   |      (Obselete) This option is now ignored.
bufferMaxLineLength|  5000	   |      (Deprecated) Maximum length of a line in the commit buffer. Use deserializer.maxLineLength instead.
selector.type	 | replicating |	  replicating or multiplexing
selector.*	     |    	       |      Depends on the selector.type value
interceptors	 |     –	   |      Space-separated list of interceptors
interceptors.*	 | 	           |

> Example for an agent named agent-1:

	a1.channels = ch-1
	a1.sources = src-1

	a1.sources.src-1.type = spooldir
	a1.sources.src-1.channels = ch-1
	a1.sources.src-1.spoolDir = /var/log/apache/flumeSpool
	a1.sources.src-1.fileHeader = true

### 5.1、Event Deserializers

> The following event deserializers ship with Flume.

事件反序列化器

#### 5.1.1、LINE

> This deserializer generates one event per line of text input.

对文本输入的每行生成一个 event。

Property Name    |   Default   | 	Description
---|:---|:---
deserializer.maxLineLength  |  2048   |  Maximum number of characters to include in a single event. If a line exceeds this length, it is truncated, and the remaining characters on the line will appear in a subsequent event.【在一个事件中，包含的字符的最大数量。如果一行超过了这个长度，就会被截断，这行的剩余字符将出现在后面】
deserializer.outputCharset  |  UTF-8  |  Charset to use for encoding events put into the channel.【用于编码事件的字符集，然后放入channel】

#### 5.1.2、AVRO

> This deserializer is able to read an Avro container file, and it generates one event per Avro record in the file. Each event is annotated with a header that indicates the schema used. The body of the event is the binary Avro record data, not including the schema or the rest of the container file elements.

这个反序列化器能够读取一个 Avro 容器文件，并把文件中的每个 Avro 记录生成一个 event。

每个 event 都有一个 header，表示使用的 schema。

event 的主体是二进制 Avro 记录数据，不包括 schema 或容器文件元素的其余部分。

> Note that if the spool directory source must retry putting one of these events onto a channel (for example, because the channel is full), then it will reset and retry from the most recent Avro container file sync point. To reduce potential event duplication in such a failure scenario, write sync markers more frequently in your Avro input files.

如果 spool directory source 必须重新尝试将其中的一个 events 放入一个 channel 中（比如，channel满了），那么它会重新设置，并从最新的 Avro 容器文件同步点开始重试。

为了减少这种失败场景中的潜在 event 重复，可以在 Avro 输入文件中更频繁地写入同步标记。

Property Name    |   Default   | 	Description
---|:---|:---
deserializer.schemaType	 |  HASH   | How the schema is represented. By default, or when the value `HASH` is specified, the Avro schema is hashed and the hash is stored in every event in the event header “flume.avro.schema.hash”. If `LITERAL` is specified, the JSON-encoded schema itself is stored in every event in the event header “flume.avro.schema.literal”. Using LITERAL mode is relatively inefficient compared to HASH mode.【如果表示schema。默认情况下，当指定了`HASH`，Avro schema就会被哈希，哈希值被存储在事件header“flume.avro.schema.hash”中的每个事件中。如果指定了`LITERAL`，json编码的schema本身将存储在事件header“flume.avro.schema.literal”中的每个事件中。与`HASH`模式相比，使用`LITERAL`模式效率相对较低】

#### 5.1.1、BlobDeserializer

> This deserializer reads a Binary Large Object (BLOB) per event, typically one BLOB per file. For example a PDF or JPG file. Note that this approach is not suitable for very large objects because the entire BLOB is buffered in RAM.

这个反序列化器读取每个 event 的一个二进制大对象(BLOB)，通常每个文件是一个 BLOB。例如一个 PDF 或 JPG 文件。

注意，这种方法不适用于非常大的对象，因为整个 BLOB 都缓存在 RAM 中。

Property Name    |   Default   | 	Description
---|:---|:---
deserializer	 |      –	   |   The FQCN of this class: `org.apache.flume.sink.solr.morphline.BlobDeserializer$Builder`
deserializer.maxBlobLength |	100000000 |  The maximum number of bytes to read and buffer for a given request【对一个给定的请求，读取和缓存的字节的最大数量】

## 6、Taildir Source

> Note This source is provided as a preview feature. It does not work on Windows.

> Watch the specified files, and tail them in nearly real-time once detected new lines appended to the each files. If the new lines are being written, this source will retry reading them in wait for the completion of the write.

注意：此 source 作为预览功能。 它不适用于 Windows。

监控指定的文件，并且，一旦有新的行添加到每个文件，近实时的读取它们。如果新行正在写入，这个 source 将在写操作完成时重新尝试读取它们。

> This source is reliable and will not miss data even when the tailing files rotate. It periodically writes the last read position of each files on the given position file in JSON format. If Flume is stopped or down for some reason, it can restart tailing from the position written on the existing position file.

该 source 是可靠的，即使在循环读取文件时，也不会丢失数据。

它定期以 JSON 格式将每个文件的最后读取位置写入到给定的位置文件。

如果 Flume 由于某种原因停止或关闭，则可以从现有位置文件上的写入位置重新开始读取。【断点续传】

> In other use case, this source can also start tailing from the arbitrary position for each files using the given position file. When there is no position file on the specified path, it will start tailing from the first line of each files by default.

该 source 也可以使用给定的位置文件从任意位置开始读取每个文件。当在指定路径上没有位置文件时，默认情况下，将从每个文件的第一行开始读取。

> Files will be consumed in order of their modification time. File with the oldest modification time will be consumed first.

文件将按其修改时间顺序消费。使用最老的修改时间的文件将首先被消费。

> This source does not rename or delete or do any modifications to the file being tailed. Currently this source does not support tailing binary files. It reads text files line by line.

此 source 不会对正在读取的文件重命名，或删除，或做任何修改。当前不支持读取二进制文件。它一行行地读取文本文件。

Property Name    |   Default   | 	Description
---|:---|:---
**channels**	 |     –	   | 
**type**	     |     –	   |    The component type name, needs to be `TAILDIR`.【组件类型名称，必须是`TAILDIR`】
**filegroups**	 |     –	   |    Space-separated list of file groups. Each file group indicates a set of files to be tailed.【空格分隔的文件分组列表。每个文件分组表示要读取的一组文件。】
**`filegroups.<filegroupName>`** |	–	|Absolute path of the file group. Regular expression (and not file system patterns) can be used for filename only.【文件分组的绝对路径。只能对文件名使用正则表达式(而不是文件系统模式)。】
positionFile     | `~/.flume/taildir_position.json`	 |  File in JSON format to record the inode, the absolute path and the last position of each tailing file.【记录inode、绝对路径和每个读取文件的最后位置的JSON格式的文件。】
`headers.<filegroupName>.<headerKey>` |	–	|  Header value which is the set with header key. Multiple headers can be specified for one file group.【Header值，它是用Header键设置的。可以为一个文件分组指定多个Header。】
byteOffsetHeader |	  false	   |    Whether to add the byte offset of a tailed line to a header called ‘byteoffset’.【是否给一个header添加一个读取行的字节偏移量，称为‘byteoffset’】
skipToEnd        |    false    | 	Whether to skip the position to EOF in the case of files not written on the position file.【如果该位置文件上没有写文件，是否跳过该位置到EOF。】
idleTimeout	     |   120000	   |    Time (ms) to close inactive files. If the closed file is appended new lines to, this source will automatically re-open it.【关闭非活跃文件的时间(ms)。如果关闭的文件追加了新行，此源将自动重新打开它。】
writePosInterval |	  3000	   |    Interval time (ms) to write the last position of each file on the position file.【在位置文件上，写每个文件的最后位置的间隔时间。】
batchSize	     |     100	   |    Max number of lines to read and send to the channel at a time. Using the default is usually fine.【同时读取并发送给channel的最大行数。通常使用默认的即可】
maxBatchCount	 | Long.MAX_VALUE | 	Controls the number of batches being read consecutively from the same file. If the source is tailing multiple files and one of them is written at a fast rate, it can prevent other files to be processed, because the busy file would be read in an endless loop. In this case lower this value.【控制从同一文件连续读取批次的数量。如果源正在读取多个文件，其中一个以较快的速度写入，它可以阻止其他文件被处理，因为繁忙的文件会无终止地读取。在这种情况下，减低这个值。】
backoffSleepIncrement  |	1000  |	The increment for time delay before reattempting to poll for new data, when the last attempt did not find any new data.【当上次尝试没有发现任何新数据时，在重新尝试轮询新数据之前的时间延迟的增量。】
maxBackoffSleep	 |     5000	      | The max time delay between each reattempt to poll for new data, when the last attempt did not find any new data.【当上次尝试没有找到任何新数据时，每次重新尝试轮询新数据之间的最大时间延迟。】
cachePatternMatching   |   true   |	Listing directories and applying the filename regex pattern may be time consuming for directories containing thousands of files. Caching the list of matching files can improve performance. The order in which files are consumed will also be cached. Requires that the file system keeps track of modification times with at least a 1-second granularity.【对于包含成千上万个文件的目录来说，列出目录并应用文件名正则表达式模式可能非常耗时。缓存匹配的文件列表可以提高性能。消费文件的顺序也将被缓存。要求文件系统以至少1秒的粒度跟踪修改时间。】
fileHeader       |     false      |  Whether to add a header storing the absolute path filename.【是否添加一个存储绝对路径的文件名的header】
fileHeaderKey    |      file      |  Header key to use when appending absolute path filename to event header.【当追加一个存储绝对路径的fileHeaderKey时，使用的Header key】

> Example for agent named a1:

	a1.sources = r1
	a1.channels = c1
	a1.sources.r1.type = TAILDIR
	a1.sources.r1.channels = c1
	a1.sources.r1.positionFile = /var/log/flume/taildir_position.json
	a1.sources.r1.filegroups = f1 f2
	a1.sources.r1.filegroups.f1 = /var/log/test1/example.log
	a1.sources.r1.headers.f1.headerKey1 = value1
	a1.sources.r1.filegroups.f2 = /var/log/test2/.*log.*
	a1.sources.r1.headers.f2.headerKey1 = value2
	a1.sources.r1.headers.f2.headerKey2 = value2-2
	a1.sources.r1.fileHeader = true
	a1.sources.ri.maxBatchCount = 1000

## 7、Twitter 1% firehose Source (experimental) 【待做】

> Warning:This source is highly experimental and may change between minor versions of Flume. Use at your own risk.

> Experimental source that connects via Streaming API to the 1% sample twitter firehose, continously downloads tweets, converts them to Avro format and sends Avro events to a downstream Flume sink. Requires the consumer and access tokens and secrets of a Twitter developer account. Required properties are in bold.

Property Name     |   Default   | 	Description
---|:---|:---
**channels**	  |     –	   | 
**type**	      |     –	   |    The component type name, needs to be org.apache.flume.source.twitter.TwitterSource
**consumerKey**	  |     –	   |    OAuth consumer key
**consumerSecret**|	    –	   |    OAuth consumer secret
**accessToken**	  |     –	   |    OAuth access token
**accessTokenSecret**	–	   |    OAuth token secret
maxBatchSize	  |    1000	   |    Maximum number of twitter messages to put in a single batch
maxBatchDurationMillis |  1000	|   Maximum number of milliseconds to wait before closing a batch

> Example for agent named a1:

	a1.sources = r1
	a1.channels = c1
	a1.sources.r1.type = org.apache.flume.source.twitter.TwitterSource
	a1.sources.r1.channels = c1
	a1.sources.r1.consumerKey = YOUR_TWITTER_CONSUMER_KEY
	a1.sources.r1.consumerSecret = YOUR_TWITTER_CONSUMER_SECRET
	a1.sources.r1.accessToken = YOUR_TWITTER_ACCESS_TOKEN
	a1.sources.r1.accessTokenSecret = YOUR_TWITTER_ACCESS_TOKEN_SECRET
	a1.sources.r1.maxBatchSize = 10
	a1.sources.r1.maxBatchDurationMillis = 200

## 8、Kafka Source