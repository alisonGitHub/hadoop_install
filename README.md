# hadoop install
steps and nececry files for installing hadoop + yarn 2.6 on ubuntu 14.10

I collected many instructions as I could (see the refs below) but select the steps I like and put them here (It is kind of like cherry pick). Those steps are tested on my hadoop cluster. It works perfect.
Three big steps: install packages and config them and hadoop xml files. I used tmux with the function of synchronize-panes for setting all the machines.

##machines
* pocoyo-1 192.168.1.72 (master)
* pocoyo-2 192.168.1.52 (data node)
* pocoyo-3 192.168.1.44 (data node)

## edit host
* vi /etc/hostname
  - check machine name, for each machine, for example, you can modify them if you want
    - pocoyo-1

* sudo vi /etc/hosts
  - add folowing lines, for  each machine or use scp to others
    - 127.0.0.1 localhost
    - pocoyo-1 192.168.1.72 # nameNode
    - pocoyo-2 192.168.1.52 # secondary namdNode
    - pocoyo-3 192.168.1.44 # data node

##creat hadoop user and user group for each machine, for each machine
* sudo addgroup hadoop
* sudo adduser --ingroup hadoop hduser
* sudo adduser hduser sudo
* sudo chown -R hduser:hadoop /usr/local/

##install ssh for each machine 
(the following is not a secure way but it faster for test purpose)

* su - hduser
* sudo apt-get intall openssh-server 

##### on master

* ssh-keygen -t rsa -P ""
* cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
* ssh localhost
* scp ~/.ssh/id_rsa hduser@pocoyo-2:/home/hduser/.ssh/ (do the same for pocoyo-3)
* scp ~/.ssh/id_rsa.pub hduser@pocoyo-2:/home/hduser/.ssh/ (do the same for pocoyo-3)

##### on slaves (pocoyo-2 and pocoyo-3)
* cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
* ssh localhost

##### on master
* ssh hduser@pocoyo-2
* ssh hduser@pocoyo-3

##disable ipv6 for each machine

* sudo vi /etc/sysctl.conf
  * add following lines
    - net.ipv6.conf.all.disable_ipv6 = 1
    - net.ipv6.conf.default.disable_ipv6 = 1
    - net.ipv6.conf.lo.disable_ipv6 = 1

##### run
* sudo service networking restart 

##download hadoop for each machine 
(once one dowloaded you can use scp to copy to others)
* su - hduser
* cd /usr/local
* wget http://mirror.reverse.net/pub/apache/hadoop/common/stable2/hadoop-2.6.0.tar.gz 
* tar -xzf hadoop-2.6.0.tar.gz
* ln -s /usr/local/hadoop-2.6.0 /usr/local/hadoop

##install java 1.7 for all machines. 

(once one dowloaded you can use scp to copy to others)

we select 1.7 because it is reported on http://wiki.apache.org/hadoop/HadoopJavaVersions

* su - hduser
* cd cd /usr/local
* wget http://download.oracle.com/otn-pub/java/jdk/7u75-b13/jdk-7u75-linux-x64.tar.gz
* tar -xzf jdk-7u75-linux-x64.tar.gz
* ln -s /usr/local/jdk-7u75-linux-x64 /usr/local/jdk

## edit /etc/profile for master

* sudo vi /etc/profile
  * add following lines
    * export HADOOP_HOME=/usr/local/hadoop
    * export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
    * export JAVA_HOME=/usr/local/jdk
    * export CLASSPATH=$JAVA_HOME/lib/tools.jar
    * export PATH=$JAVA_HOME/bin:$PATH

##config hadoop xml files.
#### modify $HADOOP_HOME/etc/hadoop/hadoop-env.sh for all machines add
* export JAVA_HOME=/usr/local/jdk

#### modify $HADOOP_HOME/etc/hadoop/slaves for all machines
* add
  * pocoyo-1
  * pocoyo-2
  * pocoyo-3

#### copy xml files first

* cd $HADOOP_HOME
* cp ./share/doc/hadoop/hadoop-project-dist/hadoop-common/core-default.xml ./etc/hadoop/core-site.xml
* cp ./share/doc/hadoop/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml ./etc/hadoop/hdfs-site.xml
* cp ./share/doc/hadoop/hadoop-yarn/hadoop-yarn-common/yarn-default.xml ./etc/hadoop/yarn-site.xml
* cp ./share/doc/hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml ./etc/hadoop/mapred-site.xml

#### modify core-site.xml
property | value | machines
-------- | ------ | -------
fs.defaultFS | hdfs://pocoyo-1:9000 | all 
hadoop.tmp.dir | /usr/local/hadoop/tmp | all
dfs.datanode.data.dir | /usr/local/hadoop/data | dataNodes

#### modify hdfs-site.xml
property | value | machines
-------- | ------ | -------
dfs.namenode.rpc-address | pocoyo-1:9001 | all
dfs.namenode.secondary.http-address | pocoyo-2:50090 | namenode and seconday nameNode
dfs.namenode.name.dir | /usr/local/hadoop/dfs/name | namenode and seconday nameNode
dfs.datanode.data.dir | /usr/local/hadoop/data | datanodes

#### modify mapered-site.xml
property | value | machines
-------- | ------ | -------
mapreduce.framework.name | yarn | all

#### modify yarn-site.xml
property | value | machines
-------- | ------ | -------
yarn.resourcemanager.hostname | pocoyo-1 | resource manager and nodeManager
yarn.nodemanager.hostname | 0.0.0.0 | nodemanager

## start HDFs
* cd $HADOOP_HOME/sbin
* ./start-dfs.sh
* jps ( for all machines to check)

## start yarn
* cd $HADOOP_HOME/sbin
* ./start-yarn.sh

##refs
###for hadoop instllation
http://www.rohitmenon.com/index.php/how-to-install-hadoop-on-ubuntulinux-mint/ (very good for single node but no yarn)

http://disi.unitn.it/~lissandrini/notes/installing-hadoop-on-ubuntu-14.html (vert clear and easy to follow)

http://www.hadoopor.com/redirect.php?tid=5473&goto=lastpost (best one in Chinese, I really like this one)

http://dogdogfish.com/2014/04/26/installing-hadoop-2-4-on-ubuntu-14-04/

http://dongxicheng.org/mapreduce-nextgen/hadoop-yarn-install/ (about yarn installation)

http://www.highlyscalablesystems.com/3597/hadoop-installation-tutorial-hadoop-2-x/

http://blog.csdn.net/zhu_xun/article/details/42077311

http://www.linuxidc.com/Linux/2015-01/111258.htm

http://blog.csdn.net/stark_summer/article/details/42424279

http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/(classic but kind of old)

##from Apache

###single node

http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html

###cluster

http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html



