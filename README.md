# custom-hive-parquet
This project is for making jar what used parquet lib of cdh version in AWS EMR.
There is a issue when we read parquet file what made in cdh cluster on aws EMR cluster.

Cause of parquet library's bug, NullPointException can cause when the column statistics data reads.
Recent version of parquet has patched. (e.g. parquet library in cdh5.5.2 or qubole)
But EMR's library is not patched and it's included in hive-exe library. (e.g. hive-exec-1.0.0-amzn-2.jar)

For avoid this confliction, we made a jar what have parquet library file having different version to amazon's library.
We used maven shade plug-in for this conveting in batches.
For example, "parquet.hadoop.ParquetFileReader" is converted to "custom.parquet.hadoop.ParquetFileReader".
Actually, all the classes in "parquet" package is changed it's package name to "custom.parquet".

And for making hive parquet serde and file format using these class, conveted "org.apache.hadoop.hive.ql.io.parquet" package to "custom.org.apache.hadoop.hive.ql.io.parquet"

Below is step if build this library.


1. **Install hive-exec-1.0.0-amzn-2.jar library to local maven repository.**

Amazon aws don't have public maven repository, So, you have to install jar file itself to local repository for maven can find it.

```
mvn clean
```

2. **packaging with shade plugin**

shade plugin has setup of activation when "package" phase.

```
mvn package
```


After all the these process, you can find final jar file in target directory.
There are 2 file of jar, "original" prefixed file is not shaded library.
