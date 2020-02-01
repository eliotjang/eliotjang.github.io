---
title: "[Hadoop] Final Steps for Hadoop Installation"
excerpt: "Tutorials Point (India) Pvt. Ltd."

categories:
  - scratch
tags:
  - scratch
  - hadoop
  - big data
last_modified_at: 2020-02-02T05:30:00+09:00
---

**NameNode Setup:**
  - After setting up all the xml and sh files, format the namenode to activate the setup  
  `$ hadoop namenode -format`  

**Starting the Hadoop File system:**  
  - Now run the start-dfs.sh and start-all.sh scripts to start the Hadoop File-system.  
  `$ ./hadoop/sbin/start-all.sh`  
  - Open the browser and go to this link: <http://localhost:50070/>  


![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/localhost.png){: .align-center}  

If you see this image, you successfully well done.  
