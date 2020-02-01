---
title: "[Hadoop] Installing Hadoop for Single Node Cluster"
excerpt: "Tutorials Point (India) Pvt. Ltd."

categories:
  - scratch
tags:
  - scratch
  - hadoop
  - big data
last_modified_at: 2020-02-02T02:00:00+09:00
---
**SSH Setup:**  
  - Install SSH and rsync
    - (**NOTE:** <u>Secure Shell (SSH)</u> is a protocol for cryptographic network for operating network services securely over an unsecured network.)
    - (**NOTE:** <u>rsync (Remote Sync)</u> is a remote and local file synchronization tool. It uses an algorithm that minimizes the amount of data copied by only moving the portions of files that have changed.)
    - (**NOTE:** A <u>passphrase</u> is a sequence of words or other text used to control access to a computer system, program or data. A passphrase is similar to a password in usage, but in generally required for added security.)
      >`$ sudo apt-get install ssh`  
      >`$ sudo apt-get install rsync`
    - SSH without Passphrase setup:
      >`$ ssh-keygen -t rsa`  

      Press 'Enter' three times without entering anything.  
      ![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/ssh-keygen.png){: .align-center}  

      >`$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`  
      >$ ssh localhost`

      ![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/local-host.png){: .align-center}  

**How to Download Hadoop?**  
  - Open the browser and go to this link <https://archive.apache.org/dist/hadoop/core/hadoop-2.4.1/>
  - Then click **hadoop-2.4.1.tar.gz**  

![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/hadoop-download.png){: .align-center}  

**How to install Hadoop?**  
  - Now open up the terminal (Press ctrl + option + T) and create a directory called 'hadoop' at the home directory
  >`$ mkdir hadoop`  
  >`$ sudo edit /etc/profile`
  - Edit the /etc/profile and add these lines:
    >`HADOOP_INSTALL=/home/eliotjang/hadoop`  
    >`PATH=$PATH:$HADOOP_INSTALL/bin`  
    >`export PATH`
  >`$ . /etc/profile`  
  >`$ reboot`
  - Copy the downloaded Hadoop (hadoop-2.4.1.tar.gz) into the hadoop directory, and go to this directory using terminal, then un-tar the downloaded tar.gz file.
  - (**NOTE:** you should changed name **eliotjang** to your own name. check **$ cd ./home** and **$ ls**. you can know your own folder name.)
  - ![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/moved-hadoop-targz.png){: .align-center}
  - Follow the lines to go to the hadoop directory from home directory.
    >`$ tar xzf hadoop`
    >`$ mv hadoop-2.4.1 hadoop`
    >`$ rm hadoop-2.4.1.tar.gz`
