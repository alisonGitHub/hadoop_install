# hadoop install
steps and nececry files for installing hadoop + yarn 2.6 on ubuntu 14.10

Three big steps: install packages and config them and hadoop xml files.

1, install packages for ssh, java and hadoop.
+ creat hadoop user and user group
+ 


install ssh
 
* sudo apt-get intall openssh-server 

install java 1.7 for all machines. we select 1.7 because it is reported on http://wiki.apache.org/hadoop/HadoopJavaVersions

* sudo add-apt-repository ppa:webupd8team/java
* sudo apt-get update
* sudo apt-get install oracle-java7-installer
* sudo update-java-alternatives -s java-7-oracle

1.2 install hadoop for all machines

2, config path for java and hadoop

3, config hadoop xml files.

refs:
for hadoop instllation
http://www.rohitmenon.com/index.php/how-to-install-hadoop-on-ubuntulinux-mint/ (very good for single node but no yarn)

http://disi.unitn.it/~lissandrini/notes/installing-hadoop-on-ubuntu-14.html (vert clear and easy to follow)

http://dogdogfish.com/2014/04/26/installing-hadoop-2-4-on-ubuntu-14-04/

http://dongxicheng.org/mapreduce-nextgen/hadoop-yarn-install/(abour yarn installation)

http://www.highlyscalablesystems.com/3597/hadoop-installation-tutorial-hadoop-2-x/

http://blog.csdn.net/zhu_xun/article/details/42077311

http://www.linuxidc.com/Linux/2015-01/111258.htm

http://blog.csdn.net/stark_summer/article/details/42424279

http://www.hadoopor.com/redirect.php?tid=5473&goto=lastpost (best one in Chinese)




http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-multi-node-cluster/(classic but kind of old)

from Apache
single node
http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html
cluster
http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html



