---
title: "[Hadoop] HDFS Reading"
excerpt: "Tutorials Point (India) Ltd."

categories:
  - 컴퓨터공학
tags:
  - Hadoop

last_modified_at: 2020-02-06T19:00:00+09:00
---

**The Reading operation in HDFS?**  
  - In HDFS the main two operations are Reading and Writing data using HDFS.
  - To read from HDFS, at first, the client sends requests for metadata to the NameNode. When the NameNode responds, then it sends a response with the number of blocks, their location, different information about replicas and some other details.
  - Now the client communicates with the DataNodes. The client can read data in the parallel fashion. When the whole data is read, it combines the blocks as the original file.  

![](https://eliotjang.github.io/assets/images/hadoop/hdfs-reading.png){: .align-center}  

  - This is the pictorial representation of the Data Reading procedure.  


