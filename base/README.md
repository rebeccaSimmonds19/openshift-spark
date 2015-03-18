# Apache Spark base image for Docker

This image is the base for master and worker images, and a CLI
environment for interacting with a Spark cluster.

# Build

* ```docker build -t <name>/spark-base .```

# Use

This assumes that the master knows itself as "spark-master"

* ```docker run -it <name>/spark-base sh```
* ```export SPARK_LOCAL_HOSTNAME=$(hostname -i)```
* ```echo "<IP OF MASTER> spark-master"```
* ```MASTER=spark://spark-master:7077 pyspark```