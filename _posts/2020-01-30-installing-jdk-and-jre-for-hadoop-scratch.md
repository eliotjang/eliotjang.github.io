---
title: "[Hadoop] Installing JDK and JRE for Hadoop"
excerpt: "Tutorials Point (India) Pvt. Ltd."

categories:
  - scratch
tags:
  - scratch
  - big data
  - hadoop
last_mdodified_at: 2020-01-30T18:50:00+09:00
---

**How to Download JDK on Lunux?**  
  - To download JDK, please follow this link: <http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>  
  - Accept the License Agreement and go for Linux x64 version(ending with tar.gz)  

**How to Download JRE on Linux?**  
  - To download JRE, please follow this link: <http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html>
  - Accept the License Agreement and to for Linux x64 jre download(ending with tar.gz)  

>If you interested in the latest JDK version(i.e. 13 version), it could be hard for installing that jdk 13 version is not required JRE.  
It should download **TimeZone data**.  
So in this part, jdk 8 would easy to learning hadoop.  

**Installation of JRE and JDK on Linux:**  

![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/mkdir-java.png){: .align-center}

  - Open up the terminal by pressing Ctrl + option(Alt) + T
  - Create a java folder at /usr/local/ using root permission. It will ask for the password for the Ubuntu system.  
  `$ sudo mkdir -p /usr/local/java`
  - Now copy and paste the downloaded JDK and JRE tar.gz from the Download location to the newly created java directory.  
  `$ cd Download/`  
  `$ sudo cp -r jre*.tar.g /usr/local/java`  
  `$ sudo cp -r jdk*.tar.gz /usr/local/java`
  - Now go to the java folder, then un-tar JDK and JRE  
  `$ cd /usr/local/java`  
  `$ sudo tar xvzf jdk*.tar.gz`  
  `$ sudo tar xvzf jre*.tar.gz`
  - Now go to the /etc/profile and edit the profile using the lines metioned below.  
  - Open /etc/profile using gedit:  
  `$ sudo gedit /etc/profile`  
  - Then write these lines into the profile document.  
  ![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/java-path.png){: .align-center}  
  `export JAVA_HOME=/usr/local/java/jdk1.8.0_241`  
  `PATH=$PATH:$JAVA_HOME/bin`  
  `export JRE_HOME=/usr/local/java/jre1.8.0_241`  
  `PATH=$PATH:$JRE_HOME/bin`  
  `export PATH`  
  - Now wirte these lines onto the termianl one by one.  
  `sudo update-alternatives --install /usr/bin/java java /usr/local/java/jdk1.8.0_241/bin/java 2`  
  `sudo update-alternatives --install /usr/bin/javac javac /usr/local/java/jdk1.8.0_241/bin/javac 2`  
  `sudo update-alternatives --install /usr/bin/jar jar /usr/local/java/jdk1.8.0_241/bin/jar 2`  
  - Now write these lines onto the terminal one by one.  
  `sudo update-alternatives --set java /usr/local/java/jdk1.8.0_241/bin/java`  
  `sudo update-alternatives --set javac /usr/local/java/jdk1.8.0_241/bin/javac`  
  `sudo update-alternatives --set jar /usr/local/java/jdk1.8.0_241/bin/jar`  
  >**NOTE**: If you are using the smae Java version, the given lines will work fine. For different versions you need to edit the version details in the lines.  
  - Now refresh the profile to take up the changes we have done.  
  `$ . /etc/profile`  
  - Then please reboot your system.  
  `$ reboot`  
  - To check whether the java is installed correctly or not, paste the command in your terminal. If it returns the java version, then java is installed correctly.  
  `$ java -version`  

![](https://eliotjang.github.io/assets/images/hadoop/virtualbox/installed-java.png){: .align-center}  

Thank you for visting my blog :D  


