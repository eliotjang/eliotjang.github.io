---
title: "[Hadoop] Example of MapReduce Program"
excerpt: "Tutorials Point (India) Ltd."

categories:
  - 컴퓨터공학
tags:
  - hadoop

last_modified_at: 2020-02-12T23:00:00+09:00
---  

**The practical demonstration of WordCount Problem:**

  - To run a MapReduce Program, we need to start Hadop demons first.  

![](https://eliotjang.github.io/assets/images/hadoop/hadoop-daemons.png){: .align-center}  

  - Make temp folder and file.  


![](https://eliotjang.github.io/assets/images/hadoop/make-folder.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/make-file.png){: .align-center}  

  - Create and copy a file into HDFS. Put the file inside a separate directory. In this case, it is /input


![](https://eliotjang.github.io/assets/images/hadoop/make-input.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/put-sample-file.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/show-hadoop-file.png){: .align-center}  

  - The MapReduce will create another directory, in this case it is /output. In that directory there will be a part file. That file will hold the final MapReduce output.

  - The WordCount program is one of the default programs, that comes with Hadoop. We can use the jar file to execute the MapReduce Task.
  - The syntax of using the Jar file to start MapReduce task:
    - `$ hadoop jar <jar_path> <class_name> <parameters>`  
  - In this case the parameters are the input and output directory.  

![](https://eliotjang.github.io/assets/images/hadoop/show-hadoop-mapreduce-example-jar.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/hadoop-wordcount-jar.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/show-output-folder.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/final-output1.png){: .align-center}  

![](https://eliotjang.github.io/assets/images/hadoop/final-output2.png){: .align-center}  





