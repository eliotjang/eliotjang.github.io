---
title: "[Hadoop] Demonstration of hadoop command"
excerpt: "Tutorials Point (India) Ltd."

categories:
  - 컴퓨터공학
tags:
  - hadoop

last_modified_at: 2020-02-11T18:00:00+09:00
---

**Make temp folder and files**  

  - `$ mkdir ~/tmpFolder`
  - `$ touch ~/tmpFolder/sample_file.txt`
  - `$ touch ~/tmpFolder/student_info.csv`  

![Test](https://eliotjang.github.io/assets/images/hadoop/demo-1.png){: .align-center}  

**Starting the Hadoop File System:**  
  - `$ ./hadoop/sbin/start-all.sh`  

  
![](https://eliotjang.github.io/assets/images/hadoop/demo-2.png){: .align-center}  

**Open the browser and go to the link**  
  - <http://localhost:50070/>  
  - select **Utilities** menu
  - check **Browse the file system**


![](https://eliotjang.github.io/assets/images/hadoop/demo-3.png){: .align-center}  

**Make directory on HDFS**  
  - `$ hdfs dfs -mkdir /hadoopFolder`  


![](https://eliotjang.github.io/assets/images/hadoop/demo-4.png){: .align-center}  

**Put files on HDFS**  
  - `$ hdfs dfs -put ~/tmpFolder/sample_file.txt /hadoopFolder/hadoop_file.txt`
  - `$ hdfs dfs -put ~/tmpFolder/student_info.csv /hadoopFolder/hadoop_info.csv`  


![](https://eliotjang.github.io/assets/images/hadoop/demo-5.png){: .align-center}  

  > It is same as `$ hadoop fs -copyFromLocal <path1> <path2>` syntax.

**Get files on HDFS**
  - `$ hdfs dfs -get /hadoopFolder/hadoop_file.txt ~/tmpFolder/get_file.txt`
  - `$ hdfs dfs -get /hadoopFolder/hadoop_info.csv ~/tmpFolder/get_info.csv`  


![](https://eliotjang.github.io/assets/images/hadoop/demo-6.png){: .align-center}  

  > It is same as `$ hadoop fs -copyToLocal <path1> <paht2>` syntax.

**List all files**
  - `$ hdfs dfs -ls /hadoopFolder`  


![](https://eliotjang.github.io/assets/images/hadoop/demo-7.png){: .align-center}  

  > It is same as `$ hadoop fs -ls /hadoopFolder` syntax.

**Count directories, files**  
  - `$ hdfs dfs -count /hadoopFolder`  


![](https://eliotjang.github.io/assets/images/hadoop/demo-8.png){: .align-center}  

**Remove all directories and files**  
  - `$ hdfs dfs -rm -r /hadoopFolder`  


![](https://eliotjang.github.io/assets/images/hadoop/demo-9.png){: .align-center}  


