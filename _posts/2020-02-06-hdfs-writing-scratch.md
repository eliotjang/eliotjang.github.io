---
title: "[Hadoop] HDFS Writing"
excerpt: "Tutorials Point (India) Ltd."

categories:
  - scratch
tags:
  - scratch
  - hadoop
  - big data
last_modified_at: 2020-02-06T19:20:00+09:00
---

**The Writing operation in HDFS?**  
  - Writing is another important operation in HDFS.
  - To write in HDFS, at first, the client sends requests for metadata to the NameNode. When the NameNode responds, then it sends a response with the number of blocks, thier location, and some other details.
  - After getting this information, the client splits the data into different blocks, and then start sending to the DataNodes.
  - The client sends the data block with other DataNode details. When one block is received, DataNodes create the replica and store them into another DataNodes.

![](https://eliotjang.github.io/assets/images/hadoop/hdfs-writing.png){: .align-center}  

  - This is the pictorial representation of the Data Writing procedure.  


