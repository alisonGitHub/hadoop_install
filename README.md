# hadoop install
steps and nececry files for installing hadoop + yarn 2.6 on ubuntu 14.10

I collected many instructions as I could (see the refs below) but select the steps I like and put them here (It is kind of like cherry pick). Those steps are tested on my hadoop cluster. It works perfect.
Three big steps: install packages and config them and hadoop xml files.

##machines
* pocoyo-1 192.168.1.72
* pocoyo-2 192.168.1.52
* pocoyo-3 192.168.1.44

##creat hadoop user and user group for each machine
* sudo addgroup hadoop
* sudo adduser --ingroup hadoop hduser
* sudo adduser hduser sudo
* sudo chown -R hduser:hadoop /usr/local/

##install ssh
 
* su - hduser
* sudo apt-get intall openssh-server 
* ssh-keygen -t rsa -P ""
* cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
* ssh localhost
##disable ipv6

* sudo vi /etc/sysctl.conf
 
add following lines

** net.ipv6.conf.all.disable_ipv6 = 1
** net.ipv6.conf.default.disable_ipv6 = 1
** net.ipv6.conf.lo.disable_ipv6 = 1
###run
* sudo service networking restart 
##download hadoop
* su - hduser
* cd /usr/local
* wget http://mirror.reverse.net/pub/apache/hadoop/common/stable2/hadoop-2.6.0.tar.gz 
* tar -xzf hadoop-2.6.0.tar.gz
* ln -s /usr/local/hadoop-2.6.0 /usr/local/hadoop

##install java 1.7 for all machines. we select 1.7 because it is reported on http://wiki.apache.org/hadoop/HadoopJavaVersions

* sudo add-apt-repository ppa:webupd8team/java
* sudo apt-get update
* sudo apt-get install oracle-java7-installer
* sudo update-java-alternatives -s java-7-oracle
### antoher way
* su - hduser
* cd cd /usr/local
* wget http://download.oracle.com/otn-pub/java/jdk/7u75-b13/jdk-7u75-linux-x64.tar.gz
* tar -xzf jdk-7u75-linux-x64.tar.gz
* ln -s /usr/local/jdk-7u75-linux-x64 /usr/local/jdk

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



