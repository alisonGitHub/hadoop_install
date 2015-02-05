# hadoop_install
steps and nececry files for installing hadoop + yarn 2.6 on ubuntu 14.10

Three big steps: install packages and config them and hadoop xml files.

1, install packages for ssh, java and hadoop and 
install java 1.7 for all checines. we select 1.7 because it is reported on http://wiki.apache.org/hadoop/HadoopJavaVersions

sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
sudo update-java-alternatives -s java-7-oracle

2, config path for java and hadoop

3, config hadoop xml files.

refs:
for hadoop instllation
http://www.rohitmenon.com/index.php/how-to-install-hadoop-on-ubuntulinux-mint/ (very good for single node but no yarn)

from Apache
single node
http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html
cluster
http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html



