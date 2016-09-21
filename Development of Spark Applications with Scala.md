#Development of Spark Applications with Scala  

至官網下載　scala ide
http://scala-ide.org/download/sdk.html

~$ wget https://dl.bintray.com/sbt/native-packages/sbt/0.13.9/sbt-0.13.9.tgz
~$ gunzip sbt-0.13.9.tgz
~$ tar -xvf sbt-0.13.9.tar
~$ export PATH=$PATH:~/sbt/bin


~$ mkdir -p ~/.sbt/0.13/plugins # mkdir -p creates all necessary directories in the path in the given order
~$ cat >> ~/.sbt/0.13/plugins/plugins.sbt
addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "4.0.0")
<ctrl>+D
~$

