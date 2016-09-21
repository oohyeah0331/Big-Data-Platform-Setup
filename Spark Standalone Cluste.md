#Spark Standalone Cluste
  
直接先至官網下載合適版本
1.$tar -zxvf spark-2.0.0-bin-hadoop2.7.tgz
2.$sudo mv spark-2.0.0-bin-hadoop2.7 spark
3.$cd spark/conf/
4.$cp slaves.template slaves
5.$sudo vim slaves
```
slave01
slave02
slave03
```

6.cp spark-env.sh.template spark-env.sh
7.sudo vim spark-env.sh
```
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
export HADOOP_HOME=/home/spark/hadoop   
SPARK_MASTER_IP=master
SPARK_LOCAL_DIRS=/home/spark/spark
```
8.$cd
9.$scp -r /home/spark/spark spark@slave01:~/
10.$scp -r /home/spark/spark spark@slave02:~/
11.$scp -r /home/spark/spark spark@slave03:~/
  
12.$spark/sbin/start-all.sh
13.$spark/bin/run-example SparkPi 10 --master local[*]
14.$spark/bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://master:7077 spark/examples/jars/spark-examples_2.11-2.0.0.jar 100


