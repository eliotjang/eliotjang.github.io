---
title: "[Hadoop] Installing Hadoop on Pseudo Distributed Mode"
excerpt: "Tutorials Point (India) Pvt. Ltd."

categories:
  - 컴퓨터공학
tags:
  - Hadoop
last_modified_at: 2020-02-02T04:00+09:00
---  


**Installing Hadoop in Pseudo Distributed mode**  
  - To setup hadoop for Pseudo distributed mode Set up the following XML files located at /home/eliotjang/hadoop/etc/hadoop/
    1. core-site.xml
    2. hdfs-site.xml
    3. mapred-site.xml
  - (**NOTE:** eliotjang folder is my folder, so you should input your own folder)  

**Setup core-site.xml**  
  - Open core-site.xml from the given path /home/eliotjang/hadoop/etc/hadoop/core-site.xml
  - Put the Following lines in the xml file.  
    `$ sudo gedit /home/eliotjang/hadoop/etc/hadoop/core-site.xml`  
    ```xml
    <configuration>
      <property>
	<name>fs.defaultFS</name>
	<value>hdfs://localhost:9000</value>
      </property>
      <property>
	<name>hadoop.tmp.dir</name>
	<value>/home/eliotjang/hadoop/temp</value>
      </property>
    <configuration>
    ```
    - (**NOTE:** you should change eliotjang to your own folder name)

![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/core-site.png){: .align-center}

**Setup hdfs-site.xml**  
  - Open hdfs-site.xml from the given path /home/eliotjang/hadoop/etc/hadoop/hdfs-site.xml
  - Put the Following lines in the xml file  
    `$ sudo gedit /home/eliotjang/hadoop/etc/hadoop/hdfs-site.xml`  
    ```xml
    <configuration>
      <property>
	<name>dfs.replication</name>
	<value>1</value>
      </property>
    </configuration>
    ```  
![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/hdfs-site.png){: .align-center}  


**Setup  mapred-site.xml**  
  - Open mapred-site.xml from the giben path /home/eliotjang/hadoop/etc/hadoop/mapred-site.xml
  - If the file is not present copy the mapred-site.xml.template file and rename the copied file with mapred-site.xml  
    `$ cd /home/eliotjang/hadoop/etc/hadoop/`  
    `$ cp mapred-site.xml.template mapred-site.xml`  
    `$ sudo gedit mapred-site.xml`  
  - Or the file is exist, Put the Follwoing line in the xml file  
    `$ sudo gedit /home/eliotjang/hadoop/etc/hadoop/mapred-site.xml`  
    ```xml
    <configuration>
      <property>
	<name>mapred.job.tracker</name>
	<value>localhost:9001</value>
      </property>
    <configuration>
    ```  
![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/mapred-site.png){: .align-center}  

**Setup hadoop-env.sh**  
  - Open hadoop-env.sh from the given path /home/eliotjang/hadoop/etc/hadoop/hadoop-env.sh
    - just **double click** the file.
  - Export the JAVA_HOME environment variable in that file.
  - Put the Following line in that file.  
    `export JAVA_HOME=/usr/local/java/jdk1.8.0_241/`
![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/direct-java-home.png){: .align-center}    
  - put name node format in Termianl
    `$ hadoop namenode -format`  

**Start the Hadoop:**  
  - put Following command in termianl  
    `$ ./hadoop/sbin/start-all.sh`
  - put Following command in termianl  
    `$ jps`  
![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/jps.png){: .align-center}
  - in the brower go to localhost:50070/ to get the hadoop overview page
![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/localhost.png){: .align-center}
