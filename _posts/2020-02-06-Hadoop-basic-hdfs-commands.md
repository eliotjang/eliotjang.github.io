---
title: "[Hadoop] Basic HDFS Commands"
excerpt: "Tutorials Point (India) Ltd."

categories:
  - 컴퓨터공학
tags:
  - Hadoop

last_modified_at: 2020-02-06T20:00:00+09:00
---

**Some basic HDFS Commands:**
  - HDFS commands are very similar with the UNIX commands. Some syntax and output format may differ for the commands.  

  | Command | Description |
  | :---: | :--- |
  | `ls` | List all files with permissions and other details. |
  | `mkdir` | Create a directory |
  | `rm` | Remove File or Directory |
  | `put` | Store file/folder from local disk to HDFS |
  | `cat` | Concatenate text/display file content |
  | `get` | Store file/folder from HDFS to local disk |
  | `count` | Count number of directory, number of files and file size. |  
  
  - **NOTE:** Start Hadoop first to run HDFS commands.   
  - **The ls command**:
    - Syntax: `hdfs dfs -ls <path>`  
    - The ls command will show all files and folders are the given path. It also displays the file permissions, owner, group, modification time and file-size.  
  - **The mkdir command**:
    - Syntax: `hdfs dfs -mkdir <path>/dir_name`
    - The mkdir command is used to create a new directory inside the given location.  
  - **The rm command**:
    - Syntax: `hdfs dfs -rm <file_path>`
    - Syntax: `hdfs dfs -rm -r <directory_name>`
    - The rm command is used to remove a file or directory. If the directory has some elements we need to use rm -r option to recursively delete all internal elements.  
  - **The put command**:
    - Syntax: `hdfs dfs -put <path1> <path2> <dest>`
    - Put command is used to store data into HDFS from local disk. It can take multiple arguments. All are source except the last one. That is destination path in the HDFS  
  - **The get command**:
    - Syntax: `hdfs dfs -get <path1> <path2> <dest>`
    - Get command is used to store data from HDFS to local disk. It can take multiple arguments. All are source locations in HDFS except the last one. That is destination path in the local disk.  
  - **The count command**:
    - Syntax: `hdfs dfs -count <path>`
    - Count command is used to count the number of directories, files inside the given directory. It also shows the file size of the directory.  


