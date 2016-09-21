#Hadoop Multi Node
在伺服器上使用虛擬機模擬叢集     
安裝完畢後更改虛擬網路卡設定
每一台節點的路徑和使用者都必須一樣
  
以下每一台電腦均要做  
將原先的網路卡從 NAT 改為 橋接介面卡  
0.sudo gedit /etc/network/interfaces
```
auto eth0
iface eth0 inet static
address 140.119.164.78
netmast 255.255.255.0
gateway 140.119.164.254
dns-nameservers 140.119.1.110
```
設定完網路必須重新開機

接著設定ip對應主機的名稱  
```
#127.0.0.1	localhost
#127.0.1.1	master

140.119.164.78 master
140.119.164.85 slave01
140.119.164.86 slave02
140.119.164.87 slave03
```

1.$sudo apt-get update  
2.$sudo apt-get upgrade  
3.$sudo apt-get install vim  
4.$sudo apt-get install openjdk-7-jdk (http://openjdk.java.net/install)  
5.$sudo apt-get install openssh-server  

在設定面ssh免密碼登錄時僅需將master的金鑰傳送至其他節點  
6.$ssh-keygen -t rsa -P "" (and just press enter)  
7.$cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys  
$ssh-copy-id -i ~/.ssh/id_rsa.pub spark@slave01  
$ssh-copy-id -i ~/.ssh/id_rsa.pub spark@slave02  
$ssh-copy-id -i ~/.ssh/id_rsa.pub spark@slave03  

8.$ssh master (type "yes")  login without key 
ssh slave01　/ slave02  /slave03  
 
9.$exit    

僅master做，全不設定完成在複製到其他節點    
wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz  

11.$tar xfz hadoop-2.7.3.tar.gz  
12.$sudo mv hadoop-2.7.3 hadoop  

15.$cd hadoop/etc/hadoop  
16.$sudo vim slaves
```
slave01
slave02
slave03
```
16.$sudo vim hadoop-env.sh  
```
#export JAVA_HOME={JAVA_HOME}
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64 
```
17.$sudo vim core-site.xml
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://master:9000/</value>
    </property>
    <property>
         <name>hadoop.tmp.dir</name>
         <value>file:/home/spark/hadoop/hadoop_store</value>
    </property>
</configuration>
```

18.$sudo vim hdfs-site.xml
```
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/spark/hadoop/hadoop_store/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/spark/hadoop/hadoop_store/dfs/data</value>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
</configuration>
```
19.$cp mapred-site.xml.template mapred-site.xml
20.$sudo vim mapred-site.xml
```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
21.$sudo vim yarn-site.xml
```
<configuration>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>master</value>
    </property>
    <property>
        <name>yarn.nodemanager.hostname</name>
        <value>0.0.0.0</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
22.$cd
scp -r ~/hadoop spark@slave01:~/
scp -r ~/hadoop spark@slave02:~/
scp -r ~/hadoop spark@slave03:~/

sudo vim ~/.bashrc //注意:這也必須在每一台機器添加
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
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
#HADOOP VARIABLES END
```
23.$hadoop namenode -format
24.$start-dfs.sh (type “yes”)
25.$start-yarn.sh
26.$jps

27.在master上會有
SecondaryNameNode
Jps
NameNode
ResourceManager
28.在slave上會有
Jps
NodeManager
DataNode


參考網站  
http://woodysclin.blogspot.tw/2014/05/hadoop-240-cluster.html
http://wuchong.me/blog/2015/04/04/spark-on-yarn-cluster-deploy
