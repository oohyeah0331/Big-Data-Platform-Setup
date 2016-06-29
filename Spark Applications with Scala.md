## Development and deployment of Spark Applications with Scala  
sbt, and Eclipse

####install sbt  
1.$ wget https://dl.bintray.com/sbt/native-packages/sbt/0.13.8/sbt-0.13.8.tgz  
2.$ gunzip sbt-0.13.8.tgz  
3.$ tar -xvf sbt-0.13.8.tar  
4.$ sudo vim ~/.bashrc  
```
export PATH=$PATH:~/sbt/bin
```
####sbteclipse plugin  
5.$ mkdir -p ~/.sbt/0.13/plugin  
6.$ cat >> ~/.sbt/0.13/plugins/plugins.sbt  
```
addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "4.0.0")
```
7." ctrl "+D  
####Creating sample sbt project  
8.$ mkdir SampleApp  
9.$ cd SampleApp  
10.$ mkdir -p src/main/scala  
11.$ vim src/main/scala/SampleApp.scala  
```
/* SampleApp.scala:
   This application simply counts the number of lines that contain "val" from itself
 */
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf
 
object SampleApp {
  def main(args: Array[String]) {
    val txtFile = "/home/osboxes/SampleApp/src/main/scala/SampleApp.scala"
    val conf = new SparkConf().setAppName("Sample Application")
    val sc = new SparkContext(conf)
    val txtFileLines = sc.textFile(txtFile , 2).cache()
    val numAs = txtFileLines .filter(line => line.contains("val")).count()
    println("Lines with val: %s".format(numAs))
  }
}
```
12.$ vim build.sbt  
```
name := "Sample Project"
 
version := "1.0"
 
scalaVersion := "2.11.7"
 
libraryDependencies += "org.apache.spark" %% "spark-core" % "1.6.1"
```
13.$ find .
```
.
./sample.sbt
./src
./src/main
./src/main/scala
./src/main/scala/SampleApp.scala
```
14.$ sbt package  
15.$ sbt eclipse  
16.$ spark-submit --class "SampleApp" --master local target/scala-2.11/sample-project_2.11-1.0.jar  


