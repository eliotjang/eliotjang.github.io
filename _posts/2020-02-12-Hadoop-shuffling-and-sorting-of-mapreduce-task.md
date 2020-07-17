---
title: "[Hadoop] Shuffle & Sorting of MapReduce Task"
excerpt: "Tutorials Point (India) Ltd."

categories:
  - 컴퓨터공학
tags:
  - Hadoop

last_modified_at: 2020-02-12T20:50:00+09:00
---  


**What is Shuffling?**
  - The Mapper creates the intermediate key-value pair and transfer them to the Reducer task. This procedure is known as Shuffling.
  - Using the Shuffling procedure the system can sort the data using key values.
  - The shuffling task begins when some of the mapping tasks are done. So this is the faster process. It will not wait for completion of the mapper task.  

**What is Sorting?**
  - The MapReduce Framework automatically sorts the data on key values on the output of Mapper. So before sending it to the reducer, all key-value pairs are sorted.
  - The Reducer can easily understand when a new reducing task will be started by the sorted key-value pairs.
  - If the user set no reducer task, the shuffling and sorting phase will not take place. The task will over after the mapper task.  


