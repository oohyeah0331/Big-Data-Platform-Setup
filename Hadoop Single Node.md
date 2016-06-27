# Hadoop Single Node

virtualbox version  >>>  4.3.30   ubuntu version  >>>  14.04.3
  
1.$sudo apt-get update  
2.$sudo apt-get upgrade  
3.$sudo apt-get install vim  
4.$sudo apt-get install openjdk-7-jdk (http://openjdk.java.net/install)  
5.$sudo apt-get install openssh-server  
6.$ssh-keygen -t rsa -P "" (and just press enter)  
7.$cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys  
8.$ssh localhost (type "yes")  login without key  
9.$exit  
10.$sudo vim /etc/sysctl.conf  
```
#disable ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```  
11.$sudo sysctl -p  
12.$cat /proc/sys/net/ipv6/conf/all/disable_ipv6 (return 1)  
13.$wget http://www.motorlogy.com/apache/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz   
   (All version : http://www.motorlogy.com/apache/hadoop/)  
14.$tar xfz hadoop-2.6.0.tar.gz  
15.$sudo mv hadoop-2.6.0 / hadoop  
16.$sudo vim ~/.bashrc  
```
#HADOOP VARIABLES START
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
export HADOOP_INSTALL=/home/spark/hadoop 
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
export HADOOP_OPTS=”-Djava.library.path=$HADOOP_INSTALL/lib”
#HADOOP VARIABLES END
```  
17.$source ~/.bashrc  
18.$cd hadoop/etc/hadoop  
19.$sudo vim hadoop-env.sh  
```  
#export JAVA_HOME={JAVA_HOME}
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64 
```  
20.$sudo vim core-site.xml  
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
21.$sudo vim hdfs-site.xml  
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```  
22.$cp mapred-site.xml.template mapred-site.xml  
23.$sudo vim mapred-site.xml  
```
<configuration>
    <property>
        <name>mapreduce.framwork.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
24.$sudo vim yarn-site.xml  
```
<configuration>
    <property>
        <name>yarn.nodmanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```  
25.$hadoop namenode -format  
26.$start-dfs.sh (type “yes”)  
27.$start-yarn.sh  
28.$jps  
  
If it returns ResourceManager、Jps、DataNode、SecondaryNameNode、NodeManager and NameNode, then you are done.
