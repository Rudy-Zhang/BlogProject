title: "Hadoop Streaming学习文档"
date: 2015-5-24 9:30
category: [Hadoop]
tags: [Hadoop]
---

##Hadoop Streaming是什么

Hadoop MapReduce和HDFS使用Java实现，默认是Java接口，另外提供了C++编程接口和Streaming框架。

Streaming框架允许任何程序语言实现的程序（只要该程序支持标准输入输出即可），在Hadoop MapReduce中使用，方便已有程序向Hadoop平台移植。

##Streaming原理

使用Java实现了一个包装**用户程序**（mapper，reducer方法）的MapReduce程序，该程序调用MapReduce的Java接口，创建一个**新的进程**包装用户程序，将数据通过**管道**传递给包装的用户程序。

![](http://i.imgur.com/dlebclb.jpg)

##考虑的问题

在实际的开发过程中，开发人员需要考虑下面几个方面的问题，其中1,2是必须实现的：

1. Mapper程序：对输入key/value数据进行处理；
2. Reducer程序：对mapper的输出进行归并处理；
3. Combiner：在本地对一个计算节点上的mapper输出进行归并；
4. Partitioner：将mapper的输出分配到reducer；（Map的中间结果通常用”hash(key) mod R”这个结果作为标准）
5. InputFormat/OutputFormat：对输入数据进行切分，保存输出数据。

##执行

	$HADOOP_HOME/bin/hadoop streaming \
		-input /user/test/input -output /user/test/output \
		-mapper “cat” \
		-reducer “cat” \
		-jobconf mapred.reduce.tasks=1\
		-jobconf mapred.job.name=”dist-sort”

这是一个实现了分布式排序的程序。可以通过shell脚本，shell命令，或者其它语言的**可执行程序**来进行mapper和reducer程序的运行

##常用参数的使用

例1：

	$hadoop streaming \
	    -D stream.num.map.output.key.fields=2 \
	    -D mapred.job.name="jobname" \
	    -D mapred.job.priority=VERY_HIGH \
	    -D mapred.job.map.capacity=500 \
	    -D mapred.job.reduce.capacity=300 \
	    -D mapred.map.tasks=500 \
	    -D mapred.reduce.tasks=300 \
	    -mapper "Python/bin/python $app/mapper.py" \
	    -reducer "Python/bin/python $app/reducer.py" \
	    -input $input \
	    -output $output \
	    -cacheArchive $archive

-D表示配置参数，剩余项是和输入输出相关的文件和可执行程序

`-D mapred.job.name="jobname" \`,作业名

`-D mapred.job.priority=VERY_HIGH \` 作业优先级

`-D mapred.job.map.capacity=500 \`,最多同时运行map任务数

`-D mapred.job.reduce.capacity=300 \`最多同时运行reduce任务数

`-D mapred.map.tasks=500 \`,map任务个数

`-D mapred.reduce.tasks=300 \`,reduce任务个数

capacity是同时运行任务的个数，tasks是运行任务的总的个数，一般task更大，capacity是同时运行任务的上限

`-D stream.num.map.output.key.fields=2 \`,表示在第2个分隔符（默认为\t）之前作为key，之后作为value，也可以使用参数`-D stream.map.output.field.separator=. \`指定分隔符为'.'或者其它字符。

`-mapper "Python/bin/python $app/mapper.py" \`指定map任务，需要可执行，读入标准输入流，输出标准输出流

`-reducer "Python/bin/python $app/reducer.py" \`指定reducer任务

`-input $input \`指定输入文件

`-output $output \`指定输出文件

`-cacheArchive $archive`分发压缩包。`$archive`格式为`hdfs://host:port/path/to/archivefile#linkname`，表示hdfs上这个压缩包的路径为`hdfs://host:port/path/to/archivefile`,可以使用`linkname/children/path`访问这个压缩包的内容

例2：

	$hadoop streaming \
	    -D stream.num.map.output.key.fields=3 \
	    -D mapred.text.key.partitioner.options="-k1,1" \
	    -D mapred.text.key.comparator.options="-k1,1 -k3,3nr" \
	    -D mapred.job.name="jobname" \
	    -D mapred.job.priority=VERY_HIGH \
	    -D mapred.job.map.capacity=500 \
	    -D mapred.job.reduce.capacity=300 \
	    -D mapred.map.tasks=500 \
	    -D mapred.reduce.tasks=300 \
	    -mapper "cat" \
	    -reducer "cat" \
	    -input $input \
	    -output $output \
	    -cacheArchive $archives

`-D mapred.text.key.partitioner.options="-k1,1" \`表示对key进行分割，分割符默认为\t，取分割后的1,1部分作为主key，剩下的作为辅key

`-D mapred.text.key.comparator.options="-k1,1 -k3,3nr" \`指定排序依据，主key按第一部分排字母序，辅key按第三部分数字序倒序排

例3：

	$HADOOP_HOME/bin/hadoop streaming \
		-D stream.map.output.field.separator=. \
		-D stream.num.map.output.key.fields=4 \
		-D map.output.key.field.separator=. \
		-D num.key.fields.for.partition=2 \ 
		-input /user/test/input -output /user/test/output \
		-mapper “mymapper.sh” -reducer “ myreducer.sh” \
		-partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner \
		-file /home/work/mymapper.sh \
		-file /home/work/myreducer.sh \
		-jobconf mapred.job.name=”jobname”

`-D stream.map.output.field.separator=. \`
`-D stream.num.map.output.key.fields=4 \`
表示输出分隔符为'.'，并且第4个.之后为value，之前为key

`-D map.output.key.field.separator=. \`
`-D num.key.fields.for.partition=2 \ `
表示key内的分隔符为'.'，第2个'.'之前作为主key，之后作为辅key

`-partitioner org.apache.hadoop.mapred.lib.KeyFieldBasedPartitioner \`指定要使用KeyFieldBasedPartitioner，也就是key域内的partioner。这样的话会把主key的内容作为partition的依据，相同的主key分配到同一个reducer中。

